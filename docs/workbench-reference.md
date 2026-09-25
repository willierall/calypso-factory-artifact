# Config Workbench 5.8.2 reference

Findings from the 5.8.2 release zip, the 4.24.1 user guide and the 4.25.0 coverage sheet, 24 Sep 2026. Points marked "inferred" come from class and method names, not documentation.

## What it is

A Spring Boot service with a web UI and its own database (H2, Oracle or PostgreSQL). It connects to each Calypso environment by host, port, user and password; a CAMService in the Calypso DataServer performs the CalypsoML export and import (inferred). Environments must share the same Calypso version. The API is licensed separately.

## REST API

Base URL: `http://<host>:<port>/configworkbench-service/api/v1/`. The one-command build is `POST /configurationmanagement/file/upload/{fileName}?import=true`, then polling `GET /reports/{id}`. The documented import example returns 202 with status PENDING even with `sync=true`, so always poll.

| Resource | Method | Path | Parameters or body |
| --- | --- | --- | --- |
| Environment | POST, PUT | `/environments` | `name`, `host`, `port`, `username`, `password`, `secure`, `kerberos` |
| Environment | GET | `/environments`, `/environments/{name}`, `/environments/{name}/status` | None |
| Environment | DELETE | `/environments/{name}` | None |
| Folder | POST, PUT | `/folders` | `name`, `path` (example uses `#` as separator) |
| Folder | GET, DELETE | `/folders`, `/folders/{name}` | Responses include `gitEnabled` |
| Feature | POST | `/features` | `name`, `type` (CATEGORY, CAPABILITY, FEATURE), `externalReference`, `parentFeature`, `instances`, `attributes` |
| Feature | POST | `/features/upload` | Feature or item CSV |
| Feature | GET, DELETE | `/features`, `/features/{name}` | None |
| Feature | POST | `/features/{name}/items` | Items with criteria |
| Feature | POST | `/features/download?fileType&selected` | Zip of CSVs |
| Configuration | GET | `/configurationmanagement/export` | `environmentname`, `externalreference`, `folder`, `dependencies`, `sync` |
| Configuration | POST | `/configurationmanagement/import` | `folder`, `filename`, `environmentname`, `sync`, `selection` |
| Configuration | POST | `/configurationmanagement/file/upload/{fileName}` | `folder`, `force`, `sync`; plus `import=true` or `compare=true` with `environmentname` |
| Configuration | POST | `/configurationmanagement/file/download` | `folder` |
| Report | GET | `/reports?page&pageSize&filter`, `/reports/{id}` | Request status, `successItems`, `failedItems`, `itemLevelDetails` |
| Master | GET, POST, PUT | `/masters/…` | Reference lists |
| Audit | GET | `/audit?page&pageSize` | Changes to features, environments, folders |
| Entity | GET | `/entities/load/{item}/{environment}` | From the user guide: adds a missing entity |

Found only in the code, base paths unconfirmed: `import/dependency`, `exportPreview`, `identifiers/{environmentname}`, `groups/{environmentName}`, `retrigger/{configRequestId}`; a Git resource with `configurations`, `mappings` and `push`. Treat these as unsupported until Nasdaq confirms them.

## Packages (features)

A package is the unit of export: a node in a CATEGORY, CAPABILITY, FEATURE tree whose items select objects by item type and search criteria. There are 324 item types. Criteria support `%` wildcards, `!` negation, `" %"` in `processingOrg` for all processing orgs, and a delete flag that removes the object in the target.

CSV headers:

```
name,action,externalReference,type,level0,level1,level2,level3,description,comments,products,crossProduct,attributes[0].attributeName,attributes[0].attributeValue
name,externalReference,action,level0,level1,level2,level3,itemType,criteria[0].elementName,criteria[0].elementValue,criteria[1].elementName,criteria[1].elementValue
```

Item type names resolve through an alias table holding UI names ("Trading Books") and core names ("Book"); test a small upload before relying on either.

## Export zip format

`selection.xml` (one item per object with identifiers and dependencies), one CalypsoML XML file per object, `dependencyOrder.json` and `dependencyLevel.json`. The zip name carries a timestamp. File names come from type plus business key; long keys become a truncated SHA-256 hash and duplicates get a random suffix, so rename by our own convention.

## CalypsoML model

Root `calypsoDocument` with `calypsoObject` elements, namespace `http://www.calypso.com/xml`. Every object has required `version` and `action` attributes. References are `Identifiers` elements (`identifier` with `code` and `codifier`, plus `calypsoBusinessKey` entries); `LegalEntityIdentifiers` adds `role`; `DomainValueIdentifier` is either `itemValue` or `domainValue`. A Book references its processing org by `LegalEntityIdentifiers`, its base currency by `DomainValueIdentifier`, and its holidays by `Identifiers`.

## Narrow domain

| Object | Item type | Natural key | Export group |
| --- | --- | --- | --- |
| Currency | Currency Definitions (`CurrencyDefault`) | code | Reference Data |
| Currency pair | Currency Pairs | primaryCode # quotingCode | Reference Data |
| Holiday code | Holiday Codes | code | Reference Data |
| Country | Countries | name | Reference Data |
| Legal entity, including processing org | Legal Entities (`LegalEntityAdapter`) | code | Static Data |
| Book | Trading Books | name | Static Data |
| Domain value | Domain Values | key # value | Core |
| Workflow transition | Workflows (`TaskWorkflowConfigAdapter`) | processingOrg # eventClass # subtype # product # origStatus # action # resultStatus | Configuration |
| User, group | Users, Groups | name, groupName | Configuration |
| Workflow access | Workflow Access | groupName # productFamily # status # tradeAction # workflowType # messageType | Configuration |
| Trade | Trades | longId (internal ID) | Transactional |

Export with dependencies follows only the same group or Core; "export with all dependencies" is admin-only in the UI. Proposed package import order: Core, Reference Data, Static Data, Configuration, Transactional.

## Import behaviour

- Saves appear to be keyed by identifiers, so re-imports should converge (inferred; test by importing twice).
- Trades are routed to `datauploader-service/api/v2/integration/trades`, so the target needs Data Uploader. Calypso-side properties `calypso.calypsoml.trade-import-action` and `calypso.calypsoml.trade-import-status` control imported trades.
- Compare (file against environment, or environment against environment) reports new, matching and different items.
- 5.8.2 includes Git integration (repository settings, branch per environment and folder, push after export or import); it pushes raw exports and is not in the user guide.

## Where things live in the release zip

| What | Path |
| --- | --- |
| REST API reference (HTML) | `docs/configworkbenchserver/*.html` |
| CalypsoML schemas and identifier, import, export configs | `serverlib/calypsoml-impl-*.jar` (`*.xsd`, `default-identifiers-config.xml`, `configworkbench.xml`) |
| Envelope and selection schemas | `serverlib/calypsoml-core-*.jar` (`calypso-main.xsd`, `calypso-selection.xsd`) |
| Groups, aliases, entity catalogue | `server/configworkbenchserver/lib/configurationmanagement-service-*.jar` (`groups.json`, `aliases.json`, `entities.json`) |
| Sample package CSVs | `tools/configworkbench/templates/samples/` |
| Launcher and config templates | `tools/configworkbench/templates/scripts/`, `.../deploy-config/` |

## Checks on the first real export

1. Confirm the installed Workbench version and API licence; call `GET /environments/{name}/status`.
2. Find out how API calls authenticate.
3. Export a small package twice without dependencies and diff the zips for volatile fields and file names.
4. Export it with dependencies and note what crosses group boundaries.
5. Confirm the Book XML references its processing org by LE code.
6. Check whether ID-named key fields such as `bookId` come out as names.
7. On a scratch DEV, import the same zip twice and look for duplicates.
8. Change one identifier in an object and in `selection.xml`, re-zip, and import.
