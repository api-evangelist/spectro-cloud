---
name: Authenticate and inspect Palette clusters
description: Authenticate to the Palette API and list/inspect Kubernetes clusters and their status.
api: openapi/spectro-cloud-palette-openapi.json
operations:
- v1Authenticate
- v1ProjectsList
- v1SpectroClustersMetadata
- v1SpectroClustersGet
- v1SpectroClustersUidStatus
---

# Authenticate and inspect Palette clusters

Use this skill to connect to the Spectro Cloud Palette API and read the state of
managed Kubernetes clusters.

## Auth
Palette accepts two header credentials (see `authentication/spectro-cloud-authentication.yml`):
- **`ApiKey`** — a long-lived API key generated in the Palette console (Profile > My API Keys). Preferred for automation.
- **`Authorization`** — a short-lived (~15 min) JWT obtained by `POST /v1/auth/authenticate` (`v1Authenticate`).

Scope calls to a project with the **`ProjectUid`** header (validated on all requests since 4.9.22). Omit it for tenant-scoped calls.

Base URL: `https://api.spectrocloud.com`.

## Steps
1. (Optional) `v1Authenticate` — exchange credentials for a JWT if you are not using an API key.
2. `v1ProjectsList` — list projects to pick the `ProjectUid` you want to scope to.
3. `v1SpectroClustersMetadata` — list cluster metadata for the scoped project.
4. `v1SpectroClustersGet` — fetch a single cluster by `{uid}`.
5. `v1SpectroClustersUidStatus` — read that cluster's live status.

## Conventions
- **Pagination:** list responses carry a `listMeta` object (`continue`, `count`, `limit`, `offset`); max 50 items per page — pass the `continue` token as a query param to page forward.
- **Errors:** non-2xx responses return the `v1Error` envelope (`code`, `message`, `details`, `ref`). See `errors/spectro-cloud-problem-types.yml`.
- **Rate limits:** ~10 req/s per source IP (burst 5); back off on `429`. See `rate-limits/spectro-cloud-rate-limits.yml`.
