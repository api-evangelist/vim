---
name: Onboard an organization to a Vim application and track its users
description: >-
  Authenticate to the Vim REST API, invite a clinic into your Vim Canvas
  application, then reconcile which organizations installed the application and
  which users inside each one are active.
api: openapi/vim-rest-api-openapi-original.json
operations:
  - POST /oauth/token
  - POST /invitations
  - getApplicationOrganizations
  - getApplicationUsersForOrganization
---

# Onboard an organization to a Vim application and track its users

Base URL: `https://api.getvim.com/v1`

## Prerequisites
- A Vim-issued `client_id` and `client_secret`, read from the **My Account** tab
  of the Vim Console (`https://console.getvim.com`). There is no self-service
  signup — Vim provisions the developer account.
- Your Vim `applicationId`.
- **Your application server must be hosted in the United States.** Vim restricts
  the REST API to US-hosted instances; a VPN lets a developer read/call from
  outside the US, but production must be US-hosted.

## Steps

1. **Get a bearer token** — `POST /oauth/token`.
   Body: `{ client_id, client_secret, grant_type: "client_credentials" }`.
   Returns `access_token` (JWT), `token_type: Bearer`, `expires_in` (3600).
   Send `Authorization: Bearer <access_token>` on every later call.
   *This operation ships without an `operationId` in Vim's own spec; the docs
   anchor it as `post-oauth-token`.*

2. **Invite the organization and its first user** — `POST /invitations`.
   Body: `CreateInvitationDto` — required `invitationContexts[]` (each
   `{ type: "applications", data }`) and required `setupData`.
   Returns `userInvitationUrl` (share with the user to set up their account),
   `userId`, `organizationKey`, `organizationId`.
   *No `operationId` in Vim's spec.*

   > **This call is not idempotent.** It creates an account **and** an
   > organization. There is no `Idempotency-Key` header. If you retry after a
   > partial success you will get `409` with `errorCode` one of
   > `DUPLICATE_ORGANIZATION_NAME`, `DUPLICATE_ORGANIZATION_EHR_URL`,
   > `DUPLICATE_USER_EMAIL`. Treat that 409 as *already created* and reconcile
   > with step 3 — do not keep retrying, and do not create a second org.

   Rate limit: **10 requests per minute.**

3. **List the organizations using your application** — `getApplicationOrganizations`
   (`GET /applications/{applicationId}/organizations`).
   Confirms the organization from step 2 is installed. Rate limit **10/min**.
   `404` means the `applicationId` is unknown; `400` means it was missing.

4. **List the users in one organization** — `getApplicationUsersForOrganization`
   (`GET /applications/{applicationId}/organizations/{organizationId}/users`).
   Use this to track new users created with the app across organizations and
   monitor their status. Rate limit **50/min**.
   `403` means insufficient permissions; `404` means unknown application or
   organization.

## Conventions & errors
- `application/json` throughout. No pagination on any operation in this flow.
- **Rate limits are status-code-only.** Vim documents no `RateLimit-*`,
  `X-RateLimit-*` or `Retry-After` header. On `429` you must back off against
  the published per-operation number. See `rate-limits/vim-rate-limits.yml`.
- Error envelopes differ per operation. Only `POST /invitations` returns a
  machine-readable `errorCode`; the rest return free text, and several declared
  error responses carry no schema at all. See `errors/vim-error-codes.yml`.
- `401` on a resource call means the token expired (1 hour) — re-run step 1 once
  and retry.
- **PHI adjacency:** this flow handles clinic, provider and user identity rather
  than patient records, but it provisions access to a platform that carries PHI.
  Handle under HIPAA.

## Testing
Vim provisions a **Sandbox EHR** reachable from the Vim Console. See
`sandbox/vim-sandbox.yml`. Vim also publishes a first-party Postman collection
for the token + invitations pair:
`collections/vim-invitations.postman_collection.json`
(source: `https://docs.getvim.com/invitations-postman-collection.json`).
