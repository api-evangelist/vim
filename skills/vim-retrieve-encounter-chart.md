---
name: Request and download an encounter chart from Vim
description: >-
  Run the asynchronous Vim chart-retrieval flow end to end - trigger the export
  from inside the EHR session with VimOS.js, receive the status update on your
  webhook, then exchange the requestId for a presigned download URL.
api: openapi/vim-rest-api-openapi-original.json
operations:
  - POST /oauth/token
  - getChartRetrievalDownloadURL
---

# Request and download an encounter chart from Vim

This flow spans **three surfaces**. It cannot be run entirely server-side.

| Step | Surface | Where it runs |
|---|---|---|
| Trigger the export | VimOS.js SDK | Browser, inside the Vim Connect session |
| Receive status | Your webhook | Your public HTTPS endpoint |
| Download | Vim REST API | Your US-hosted server |

## Prerequisites
- `put-chart-retrieval-request` enabled in your application manifest (Vim
  Console → **EHR workflow resources → Encounter workflow**).
- A webhook URL configured in the manifest. It must be a **publicly accessible
  HTTPS endpoint** — HTTP, `localhost` and private network addresses are
  rejected.
- Vim-issued `client_id` / `client_secret` for the REST call.
- The clinic must have opted into chart data extraction review and approval.
- The encounter must be **locked (signed)**, and of type in-office, out-of-office
  or other — **not** Telephone or Video.

## Steps

1. **Trigger the export** — VimOS.js, in the browser:

   ```ts
   if (vimOS.ehr.ehrState?.encounter) {
     try {
       const { requestId } = await vimSdk.ehr.ehrState.encounter.putChartRetrievalRequest();
     } catch (error) {
       console.error('failed to request encounter chart', error);
     }
   }
   ```

   Rate limit: **no more than 10 requests per minute** (Vim publishes this as a
   DANGER callout). Persist `requestId` — it is the only correlation handle
   across all three surfaces.

2. **Wait for the webhook.** Vim POSTs status updates to your configured URL:

   ```ts
   {
     requestId: string,
     status: `Rejected` | `No data` | `Completed` | `Failed` | `Expired`
   }
   ```

   Only `Completed` proceeds to step 3. Expect a **review delay of roughly 3
   working days** unless the requester is explicitly approved (or marked
   Trusted) in the Clinical Data Exchange application.

   > Vim documents **no signing secret, HMAC header or mTLS** for this webhook.
   > Do not treat the payload as authenticated. Match `requestId` against a
   > request you actually made, and re-verify in step 3 before acting.
   > See `asyncapi/vim-webhooks.yml`.

3. **Get a bearer token** — `POST /oauth/token` (see
   `authentication/vim-authentication.yml`).

4. **Get the download URL** — `getChartRetrievalDownloadURL`
   (`GET /chart-retrieval/download-url/{requestId}`), using the **same**
   `requestId`. Returns a presigned URL. Rate limit **50/min**.

5. **Download and open the archive.** The presigned URL resolves to a
   **password-protected ZIP**, structured as:

   ```
   requestId.zip
     requestId.pdf
     requestId.json
   ```

   The ZIP password is **your `applicationId`**.

## Errors
- `400` — invalid `requestId`, missing account-id, or bad parameters.
- `401` — bearer token required/expired; re-run step 3.
- `403` — insufficient permissions.
- `429` — rate limit exceeded; no `Retry-After` is published, back off to 50/min.
- `500` — Vim failed to create the presigned URL. Retry with backoff; the
  `requestId` remains valid.

See `errors/vim-error-codes.yml` and `rate-limits/vim-rate-limits.yml`.

## Testing
Vim publishes a sandbox identifier for this operation: call it with
`requestId = a1b2c3d4e5f6a7b8c9d0` to receive a predefined de-identified ZIP.
That ZIP's password is `demo-app-id` (not your applicationId). You must still be
authorized. Full Sandbox EHR prerequisites — including setting **Self Pay:
False** and **Release of information: Yes, Release Allowed** on the patient —
are in `sandbox/vim-sandbox.yml`.

## Handling
The archive contains a full clinical chart: this is **PHI**. Handle under HIPAA,
on a US-hosted server, and do not persist the presigned URL beyond its use.
