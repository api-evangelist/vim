# Vim (vim)

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
