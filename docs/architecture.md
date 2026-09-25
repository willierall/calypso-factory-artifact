# Architecture

Design agreed on 24 Sep 2026. The team version with comments lives in the Claude Doc "Calypso Config Workbench 5.8.2 — Reference for the Config Build Pipeline".

## Principles

- Agents handle steps that need judgement or read unstructured input. Workflows handle steps with one correct answer. Nothing touches a Calypso environment except a workflow running under policy.
- Agents propose: a pull request, a diagnosis or a request to run a workflow. Workflows execute. People approve.
- The pull request is the record of every change: who asked, what changed, which tests passed, who approved.

## Environments

| Environment | Who changes it | What it receives | Drift means |
| --- | --- | --- | --- |
| DEV (Andile, AWS) | The factory, plus developers experimenting | Every change first, straight from its branch | Expected; manual changes are offered back as pull requests |
| TEST (Andile, AWS) | The factory only | Merged, tagged releases | An alarm |
| UAT and production (bank) | The bank | The release pack from a validated TEST build | Outside the factory |

All factory-owned environments are hosted at Andile, so the factory server reaches them directly. Databases are Oracle or PostgreSQL on AWS with snapshots available.

## Components

| Component | Job | Technology |
| --- | --- | --- |
| Factory console | Clients, environments, runs, approvals, diffs, chat with the change agent | React web app; not needed for the hackathon |
| Factory API | The only way in for people and agents: sign-in, roles, approval policy, run records | Python, FastAPI, SSO over OpenID |
| Workflow engine | Durable runs, retries, approval waits, one lock per environment | Temporal |
| Agent service | Runs each agent with its own limited tool set | Claude Agent SDK, tools served over MCP |
| `cfgkit` | Deterministic core: explode, canonicalise, tokenise, render, pack, diff, with a CLI | Python, lxml |
| Workbench client | Typed wrapper around the Workbench REST API | Python |
| Config Workbench | Export, import, compare; one instance per Calypso version in use | Workbench 5.8.2 on the factory server |
| `host-cli` | Runs on each Calypso scheduler host: EOD start, status, collect, plus read-only probe checks | Java, reached over SSH through a forced command |
| Stores | Source of truth, run history, artefacts, secrets | GitHub, Postgres, S3, AWS Secrets Manager |

Model calls go through the Anthropic API, or through Amazon Bedrock if a client agreement requires the model to run in Andile's AWS account.

## Agents

| Agent | When it runs | What it reads | What it produces |
| --- | --- | --- | --- |
| Change agent | Chat requests; the main entry point | Repository, runs, reports | Branch and pull request with a test, answers, workflow requests through the Factory API |
| Scope analyst | Onboarding a client | Scope document, values schema | Draft `values.yaml` and a list of gaps |
| Baseline curator | After a harvest | New canonical export, current baseline | Proposed token and canonicalisation rules with reasons |
| Build doctor | A build step fails | Workbench item errors, logs, manifest | Diagnosis and a fix as a pull request or re-run request |
| Test agent | New client, product or change; any failed validation | Scope, scenarios, captured reports, diffs | Scenarios and trade files, draft expected results for sign-off, failure triage, expected-value updates |
| Drift analyst | After a drift check | Diff between expected and actual | Drift classified as intended or accidental, with a proposed fix |

## Workflows

| Workflow | Trigger | Steps | Human gate |
| --- | --- | --- | --- |
| Harvest | Run against the GCB reference environment | Export each group's package, explode, canonicalise, tokenise, open a pull request | Pull request review |
| Onboard client | Scope document uploaded | Scope analyst drafts values, schema check, pull request | Consultant approves |
| Change | Chat request | Static checks, snapshot DEV, import changed packages, compare, tests, report | Approval to merge |
| Build | Console, CLI or merge | Render, pack per group, upload and import in group order, poll, compare, verify, tests | Approval for TEST |
| Promote | A green DEV build | Import the tagged release into TEST, full validation, release pack | Approval |
| EOD test run | Change, promotion, nightly | Upload trades, run EOD, pull reports, validate | None; results feed the other gates |
| Drift check | Nightly | Export, canonicalise, diff against the expected render | None; read-only |
| Baseline impact | Baseline pull request opened | Re-render every client, compare against each client's DEV, comment | Pull request review |

## Chat change flow

1. The change agent decides where the change belongs (table below) and asks when the scope is unclear, for example "all clients, or only GCB?"
2. It edits files on a branch named `change/<client>/<short-name>`, adds at least one test, and opens a pull request with a plain-language summary.
3. Static checks run in seconds: schema, references resolve, no unreplaced tokens, import order, every affected client renders.
4. The pipeline locks the client's DEV, snapshots it, imports only the changed packages in group order, then upload-and-compares to confirm DEV matches the render.
5. The new test and the client's regression pack run; results go to the chat and the pull request.
6. A person approves and merges.
7. The tagged release is imported into TEST, the full validation pack runs, and the release pack for the bank is built. Tags look like `gcb/test-2026.10.1`.

| The change is | It lands in | Reviewed by |
| --- | --- | --- |
| A value only this client has | `clients/<client>/values.yaml` | Client lead |
| An object only this client has | `clients/<client>/overrides/` | Client lead |
| An improvement for every client | `baseline/` | Baseline owner, with an impact report across all clients |

A baseline change is not pushed into every client's DEV at once. The pipeline compares it against each client's DEV to show the impact, and each client picks it up in its next build.

## Test workflow

1. Prepare: lock the environment, load frozen market data for the fixed test date, record the commits under test.
2. Upload trades through Data Uploader, each tagged with the run ID in a trade keyword. Stop early on rejections.
3. Wait until each trade reaches the status its scenario expects.
4. Run the EOD chain through the Quartz runner for the test date (see below) and poll until it finishes.
5. Collect the report CSVs (trades, transfers, postings, messages) and task logs.
6. Validate in three layers.
7. Publish results to the pull request, chat and console; on failure, send the diff and logs to the test agent.
8. Release the lock.

| Layer | Needs expected values? | Checks |
| --- | --- | --- |
| Run health | No | All trades accepted, every EOD task completed, every report produced and not empty |
| Invariants | No | Postings balance per event and currency, transfers exist where expected, statuses valid for the workflow, no trades in error |
| Expected results | Yes | Key fields match the scenario's expected files, with amount tolerances; ID and timestamp columns ignored |

Rows are matched on the run tag plus each scenario's trade reference, never on internal trade IDs. Expected results use approval testing: capture a scenario's reports once on a validated environment, have a functional consultant approve them, then compare every later run against them.

Repeatability rules:

- A fixed test date with frozen quotes, stored in the repo.
- Run tags instead of constant resets; reset TEST from a clean snapshot on a schedule and before release validation.
- One EOD at a time per environment.
- The EOD chain and the test report templates are Workbench-exportable config, so they live in the baseline.

| Environment | Scope | When |
| --- | --- | --- |
| DEV | Scenarios touched by the change, including a short EOD | Every chat change, before approval |
| TEST | Full scenario pack with the full EOD chain | Every promotion, plus nightly regression |

## EOD on the scheduler host

The Quartz runner's API must be called on the Calypso scheduler host itself. The factory reaches it over SSH, restricted as follows:

- A dedicated `factory` user whose `authorized_keys` entry uses `restrict`, a `from=` source IP and a forced command (`infra/host/authorized_keys.template`).
- The forced command `infra/host/factory-gate` accepts only `eod start <chain> <valuation-date> <run-id>`, `eod status <run-id>` and `eod collect <run-id>`, with strict parameter formats, and logs every call to syslog.
- A sudoers rule (`infra/host/sudoers.d/factory`) lets the `factory` user run `calypso-cli` as the Calypso OS user and nothing else.
- `eod start` is idempotent per run ID and refuses a second concurrent chain on the host.
- `eod collect` streams a tar.gz of reports and logs on stdout, because the forced command blocks `scp` and `sftp`.
- One ed25519 key pair per environment in AWS Secrets Manager, loaded only inside the activity making the call. Host keys are pinned. The client is AsyncSSH inside Temporal activities, with short calls and timeouts.

## Guardrails

- One change at a time per environment, enforced by a workflow lock.
- DEV is snapshotted before every import. An RDS restore creates a new instance, so rollback means restoring and pointing Calypso at the new endpoint; script it once as a workflow step and expect minutes to tens of minutes.
- Agents never import and never hold credentials.
- Drift is checked nightly: DEV drift becomes pull requests to adopt or revert, TEST drift raises an alert.

## Repository layout

```
calypso-factory/
├── baseline/
│   └── calypso-<version>/              # one baseline per Calypso version
│       ├── manifest.yaml               # group order and package list
│       ├── tokens.yaml                 # token catalogue and XPath targets
│       ├── canonical.yaml              # volatile fields, sort rules
│       ├── packages/                   # Workbench package CSVs
│       ├── 01-core/DomainValues/…
│       ├── 02-reference-data/CurrencyPair/USD#@BASE_CCY@.xml
│       ├── 03-static-data/LegalEntity/@PO_CODE@.xml
│       ├── 04-configuration/TaskWorkflowConfig/…
│       └── 05-transactional/Trade/…    # test trade templates
├── clients/
│   └── gcb/                            # reference client
│       ├── values.yaml
│       ├── overrides/
│       ├── environments.yaml           # DEV and TEST only; no secrets
│       └── tests/                      # client-only scenarios and expected overrides
├── tests/                              # shared scenarios, tokenised
│   ├── eod-chain.yaml
│   ├── market-data/<test-date>/
│   └── scenarios/<name>/{scenario.yaml,trades.xml,expected/}
├── factory/
│   ├── cfgkit/  workbench/  workflows/  agents/  mcp/  api/  console/
├── host-cli/                           # Java CLI for scheduler hosts
├── infra/
│   └── host/                           # factory-gate, sudoers rule, authorized_keys template
├── schemas/
└── .github/workflows/                  # CI: lint, round-trip test, render all clients
```

## Build order

| Stage | Scope | Agents |
| --- | --- | --- |
| Hackathon | `cfgkit` and CLI on one server; harvest GCB; build two clients into an empty environment | Baseline curator, as a stretch goal |
| Next | Temporal workflows, Factory API, minimal console, chat change flow on DEV | Change agent, build doctor |
| Then | Promotion to TEST, EOD test runs, drift checks, SSO and roles | Test agent, drift analyst |
| Later | Scope-driven onboarding, test packs per product, a baseline per Calypso version | Scope analyst |

## Client data and models

The GCB export is a client's real configuration, and any agent call sends parts of it to a model. Check the client agreement before agents see it, and route model calls through Amazon Bedrock if the agreement requires it. The deterministic build path never needs a model.
