---
name: Submit clinician feedback on a gap or insight
description: >-
  Report a clinician action (e.g. AGREE, DISMISS) back to Vim against a specific
  diagnosis gap or care insight for an identified patient.
api: openapi/vim-data-source-openapi-original.json
operations: [post-oauth-token, post-insights-feedback]
---

# Submit clinician feedback on a gap or insight

Close the loop by sending the action a clinician took on a Vim insight/gap.

## Steps

1. **Get a bearer token** — `post-oauth-token` (`POST /oauth/token`) with
   `client_credentials`. Use `Authorization: Bearer <access_token>`.

2. **Send feedback** — `post-insights-feedback` (`POST /insights/feedback`).
   The body is `oneOf(DiagnosisGapFeedback, InsightFeedback)`, discriminated by
   `data_type`:
   - Common (`FeedbackRequest`): required `data_type` (`diagnosis_gap` | `insight`),
     `patient_id`, `id` (the gap/insight id), `action` (`{ date (yyyy-mm-dd),
     type e.g. AGREE/DISMISS, reason?, notes? }`), and `organization`
     (`{ name, vim_organization_key }`).
   - `diagnosis_gap`: also `system` (ICD | HCC), `code`, `description`, `type`
     (KNOWN | SUSPECTED), `medical_codes[]`.
   - `insight`: also `title`, `code`, `category`, `description`, `type`,
     `medical_codes[]`.
   Response returns `{ update_status: "SUCCESS" }`.

## Conventions & errors
- The `id` and `patient_id` must be the stable identifiers Vim returned when the
  gap/insight was fetched (`skills/vim-identify-and-fetch-insights.md`).
- No idempotency key: re-submitting the same action id updates the same record.
- Errors return `{ code, reason }` — handle `401`/`403`/`404`/`400`/`500` as in
  `errors/vim-problem-types.yml`.
