# Calypso configuration factory

Automates the Calypso configuration build for Andile's client environments: one tokenised baseline in Git, a small values file per client, rendered and imported through Config Workbench, then verified with test trades and end-of-day runs.

Status on 24 Sep 2026: design agreed, nothing built yet. The first real input, a Config Workbench export of the GCP reference environment, lands in `exports/gcp/` on 25 Sep 2026.

## Hackathon target

- One command takes an empty Calypso database to a configured environment.
- Changing only a client values file and running again produces a second client from the same baseline.
- Narrow domain only: currencies, currency pairs, holiday calendars, one legal entity and processing org, one book, one workflow (its statuses and actions are domain values), plus test trades.

## Decisions already made

- One tokenised baseline, many clients. Client identity lives only in `clients/<client>/`. Never create per-client copies of the baseline.
- Config Workbench 5.8.2 exports, imports and compares through its REST API under `/configworkbench-service/api/v1/`. The API is licensed separately; confirm the licence before relying on it. Details in `docs/workbench-reference.md`.
- References between objects are business-key identifiers, not database IDs. In the narrow domain only trades are keyed by internal ID.
- Export with dependencies only follows the same group or Core. So there is one Workbench package per group, imported in this order: Core, Reference Data, Static Data, Configuration, Transactional. This order is a hypothesis until confirmed on real exports. The Workbench orders objects within a package.
- Agents propose, workflows execute, people approve. Agents never import into an environment and never see credentials.
- The factory owns each client's DEV and TEST environments, hosted at Andile on AWS (Oracle or PostgreSQL, RDS snapshots available). UAT and production run at the bank and only receive a release pack.
- Config changes arrive through chat, become a branch and pull request, deploy to DEV with tests, then promote to TEST after approval.
- Test runs upload trades through Data Uploader, run EOD through the Quartz runner on the scheduler host (SSH with a forced command), pull report CSVs, and validate in three layers: run health, invariants, expected results.

Full design: `docs/architecture.md`. Natural key of every object type: `docs/object-keys.md`.

## Target repository layout

```
baseline/calypso-<version>/     tokenised canonical config, one object per file
  manifest.yaml tokens.yaml canonical.yaml packages/
  01-core/ 02-reference-data/ 03-static-data/ 04-configuration/ 05-transactional/
clients/<client>/               values.yaml, overrides/, environments.yaml, tests/
tests/                          shared scenarios, eod-chain.yaml, frozen market data
factory/                        cfgkit/, workbench/, workflows/, agents/, mcp/, api/, console/
host-cli/                       Java CLI for scheduler hosts: eod start|status|collect, probe
infra/host/                     factory-gate forced command, sudoers rule, authorized_keys template
schemas/                        JSON Schema for values, manifest, tests
exports/  vendor/               git-ignored: raw exports and the Workbench release
```

## Conventions

- Python 3.12 for factory code. Java for `host-cli/`.
- Baseline file path: `baseline/calypso-<version>/<nn>-<group>/<CalypsoType>/<natural key>.xml`, one object per file, in canonical form.
- Tokens use Calypso's `@TOKEN@` style. Substitute only at the XPath targets listed in `tokens.yaml`; never find-and-replace free text.
- Tokenise at harvest time, render at build time. Rendered output is a build artefact and is never committed.
- Every config change comes with at least one test.
- The round-trip test must always pass: rendering the baseline with GCP's values reproduces GCP's canonical export exactly.

## Safety rules

- `exports/` and `vendor/` hold confidential client data and Nasdaq software. They are git-ignored; never commit them, and never copy client data anywhere except `clients/` and `baseline/`.
- Read-only Workbench calls (list, status, export, compare) are fine against DEV. Ask before any import or delete, and never run one against a TEST environment from a session.
- Never print, log or commit credentials, SSH keys or tokens. Read them from environment variables.

## Next tasks, in order

1. Inspect the GCP export: zip layout, `selection.xml`, how references appear, and which fields differ between two exports of the same package.
2. `cfgkit explode` and `cfgkit canonicalise`, with tests on the real files.
3. `cfgkit tokenise`, extraction of `clients/gcp/values.yaml`, and the round-trip test.
4. `cfgkit render` and `cfgkit pack` (rebuild each package zip, including `selection.xml`); try one import on a scratch DEV.
5. Workbench API client and a one-command build for the narrow domain.

## Open questions

- Installed Workbench version, API licence, and how API calls authenticate.
- Does the importer accept a rebuilt `selection.xml`?
- Is import insert-or-update? Test by importing the same zip twice on a scratch DEV.
- Calypso version of the GCP environment, which names the baseline folder.
