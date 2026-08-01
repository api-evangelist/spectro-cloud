---
name: Create and publish a Palette cluster profile
description: Create a Palette cluster profile, review it, and publish it for use in cluster deployments.
api: openapi/spectro-cloud-palette-openapi.json
operations:
- v1ClusterProfilesCreate
- v1ClusterProfilesGet
- v1ClusterProfilesPublish
- v1ClusterProfilesMetadata
---

# Create and publish a Palette cluster profile

Cluster profiles are the declarative, layered stacks (packs, Helm charts, manifests)
that Palette applies when deploying workload clusters.

## Auth
Send the **`ApiKey`** header (or a JWT `Authorization` header) and the **`ProjectUid`**
header to scope the profile to a project. See `authentication/spectro-cloud-authentication.yml`.

## Steps
1. `v1ClusterProfilesCreate` — `POST /v1/clusterprofiles` with the profile spec (name, layers/packs). Returns the new profile `uid`.
2. `v1ClusterProfilesGet` — `GET /v1/clusterprofiles/{uid}` to review the created profile.
3. `v1ClusterProfilesPublish` — `PATCH /v1/clusterprofiles/{uid}/publish` to publish the draft so it can be attached to clusters.
4. `v1ClusterProfilesMetadata` — list profile metadata to confirm it appears as published.

## Conventions
- Writes are **not** idempotent-by-key (no idempotency-key header is documented); re-issuing a create makes a new profile. Guard against duplicates with `v1ClusterProfilesMetadata`.
- Errors use the `v1Error` envelope; a `409` means a conflicting/duplicate profile. See `errors/spectro-cloud-problem-types.yml`.
- Respect the per-IP `429` rate limit. See `rate-limits/spectro-cloud-rate-limits.yml`.
