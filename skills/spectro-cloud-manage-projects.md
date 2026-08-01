---
name: Manage Palette projects
description: Create and read Palette projects, the tenancy boundary that scopes clusters, profiles, and cloud accounts.
api: openapi/spectro-cloud-palette-openapi.json
operations:
- v1ProjectsCreate
- v1ProjectsList
- v1ProjectsUidGet
---

# Manage Palette projects

Projects are the scoping boundary in Palette; most resources are addressed with the
`ProjectUid` header. Use this skill to provision and read projects.

## Auth
Send the **`ApiKey`** header (or JWT `Authorization`). Project management is typically
tenant-scoped, so the `ProjectUid` header is not required for these calls.
See `authentication/spectro-cloud-authentication.yml`.

## Steps
1. `v1ProjectsCreate` — `POST /v1/projects` with the project name/metadata; returns the new `uid`.
2. `v1ProjectsList` — `GET /v1/projects` to enumerate projects (use the `listMeta.continue` token to page).
3. `v1ProjectsUidGet` — `GET /v1/projects/{uid}` to read a single project.

## Conventions
- Pagination via `listMeta` (max 50/page); errors via the `v1Error` envelope; per-IP `429` rate limiting. See `conventions/spectro-cloud-conventions.yml`.
