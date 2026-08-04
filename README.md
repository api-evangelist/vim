# Vim (vim)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Vim is a United States healthcare technology company (getvim.com) operating a clinical workflow and point-of-care integration platform that connects health plans, provider organizations, and digital-health applications to physicians inside their existing electronic health records. Through the Vim Canvas developer platform and the VimOS.js JavaScript SDK, applications embed actionable clinical insights - diagnosis gaps, risk, quality, and social determinants of health - directly into supported ambulatory EHR workflows and can read and write EHR resources at the point of care. Vim is HIPAA, SOC 2, and HITRUST certified. The platform is a proprietary REST + SDK surface; it is not an HL7 FHIR API.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/vim/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/vim/refs/heads/main/apis.yml)

## Tags

- Healthcare
- United States
- Clinical AI
- EHR Integration
- Point of Care
- Interoperability
- Value-Based Care
- Care Gaps
- OAuth

## Timestamps

- **Created:** 2026-07-24
- **Modified:** 2026-07-24

## APIs

### Vim Canvas SDK (VimOS.js)

The Vim Canvas developer platform and VimOS.js JavaScript SDK for embedding applications at the point of care. Reads EHR state (Patient, Encounter, Orders, Referral, Claim, plus problem/medication/allergy/lab/vitals lists) and writes back to Encounter, Referral, and Order resources through Vim Connect's EHR connectivity layer.

- **Human URL:** [https://docs.getvim.com/vim-os-js/setting-up](https://docs.getvim.com/vim-os-js/setting-up)

### Vim Applications & Organizations API

REST endpoints for retrieving the organizations connected to an application and the users within an organization application. OAuth 2.0 client-credentials authenticated.

- **Human URL:** [https://docs.getvim.com/api](https://docs.getvim.com/api)
- **Base URL:** `https://api.getvim.com/v1`

### Vim Invitations API

REST endpoint for inviting users to access applications on Vim.

- **Human URL:** [https://docs.getvim.com/api](https://docs.getvim.com/api)
- **Base URL:** `https://api.getvim.com/v1`

### Vim Appointments API

REST endpoint returning future appointment data (a 10-day lookahead) for a Vim organization.

- **Human URL:** [https://docs.getvim.com/api](https://docs.getvim.com/api)
- **Base URL:** `https://api.getvim.com/v1`

### Vim Chart Retrieval API

REST endpoint for obtaining a download URL for a chart-retrieval request.

- **Human URL:** [https://docs.getvim.com/api](https://docs.getvim.com/api)
- **Base URL:** `https://api.getvim.com/v1`

### Vim Data Source

Ingestion surface for pushing patient-specific clinical insights and gaps (Diagnosis Gaps, Risk, Quality, Social Determinants of Health) into Vim for surfacing at the point of care, via either API or file-based connections.

- **Human URL:** [https://data-docs.getvim.com/general-information.html](https://data-docs.getvim.com/general-information.html)

## Authentication

OAuth 2.0 client-credentials grant against an Auth0-backed authorization server at `auth.getvim.com`. The OpenID Connect discovery document and OAuth 2.0 authorization-server metadata are served anonymously and are harvested verbatim under [`well-known/`](well-known/). Vim is not FHIR based - no FHIR CapabilityStatement or SMART-on-FHIR configuration is published.

## Common Properties

- [Website](https://getvim.com/)
- [Developer Portal](https://docs.getvim.com/)
- [Documentation](https://docs.getvim.com/)
- [API Reference](https://docs.getvim.com/api)
- [Getting Started](https://docs.getvim.com/vim-os-js/setting-up)
- [GitHub Organization](https://github.com/getvim)
- [Status Page](https://status.getvim.com)
- [Blog](https://getvim.com/blog)
- [Security](https://getvim.com/technology-security/)
- [Terms of Service](https://getvim.com/legal/website-terms-of-use/)
- [Privacy Policy](https://getvim.com/legal/privacy/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
