---
name: Identify a patient and fetch their care insights
description: >-
  Authenticate to the Vim Data Source API, identify an eligible patient from
  demographics, then fetch that patient's care insights and diagnosis gaps.
api: openapi/vim-data-source-openapi-original.json
operations: [post-oauth-token, post-patient-identify, post-insights-fetch]
---

# Identify a patient and fetch their care insights

Use the Vim Data Source API to resolve a patient and pull their point-of-care insights.

## Prerequisites
- A Vim-issued `client_id` and `client_secret`.
- The customer-hosted Data Source base URL (per the server template
  `https://{environment}-{customerName}.com`).

## Steps

1. **Get a bearer token** — `post-oauth-token` (`POST /oauth/token`).
   Body: `{ client_id, client_secret, grant_type: "client_credentials" }`.
   Response returns `access_token` (JWT), `token_type: Bearer`, `expires_in: 3600`.
   Send it as `Authorization: Bearer <access_token>` on all subsequent calls.

2. **Identify the patient** — `post-patient-identify` (`POST /patient/identify`).
   Body: `PatientDetails` — required `first_name`, `last_name`, `dob` (yyyy-mm-dd);
   plus conditional `member_id` / `ehr_id` / `zip_code` per the customer's patient
   matching mechanism, and optional `insurer`, `state`.
   Response returns `is_eligible` and a stable `patient_id`. Stop if `is_eligible`
   is false.

3. **Fetch insights** — `post-insights-fetch` (`POST /insights/fetch`).
   Body: `{ patient_id }`. Response returns the patient's insights and diagnosis
   gaps (categories RISK / QUALITY / RX / SDOH / CCM / UTILIZATION / ADT /
   CLINICAL INSIGHTS), each with a stable `id`.

## Conventions & errors
- Content type `application/json` throughout.
- No idempotency key and no pagination; each call is for one patient.
- Errors return `{ code, reason }`: `401` (token missing/expired — re-run step 1),
  `403` (client not permitted for that patient), `404` (patient/path not found),
  `400` (validation), `500` (server). See `errors/vim-problem-types.yml`.
- PHI: patient demographics and identifiers are protected health information —
  handle under HIPAA. US-hosted application servers only.
