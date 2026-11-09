# sccm-config-changes

**Public intent repo** for the SCCM CI/CD pipeline POC.

Commit desired SCCM changes as JSON under `changes/`. On push to `main`, this repo validates the JSON and dispatches execution to the private executor [`sccm-cicd-poc`](https://github.com/SureshVijayaraghavan/sccm-cicd-poc).

```text
changes/*.json
  → validate syntax/schema
  → repository_dispatch: sccm-config-update
  → private sccm-cicd-poc (approval + dry-run / later AdminService)
```

## Change file shape

```json
{
  "action": "updateCollection",
  "objectType": "collection",
  "objectId": "LAB-COLL-001",
  "patch": {
    "@odata.type": "#sccm.pipeline.collectionUpdate",
    "name": "Lab Pilot Devices",
    "comment": "Example"
  }
}
```

Supported actions/object types in this POC:

| action | objectType |
| --- | --- |
| `updateCollection` | `collection` |
| `updateApplication` | `application` |
| `updateBaseline` | `baseline` |

`objectId` must be present in the private repo allowlist.

## Required secret

| Secret | Purpose |
| --- | --- |
| `EXECUTOR_DISPATCH_TOKEN` | Token that can create `repository_dispatch` on `sccm-cicd-poc` |

Optional variable: `EXECUTOR_REPO=SureshVijayaraghavan/sccm-cicd-poc`
