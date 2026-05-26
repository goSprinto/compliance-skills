# Record of Processing Activities (ROPA) Template

## What is a ROPA?

Required under Article 30 GDPR for:
- Organisations with 250+ employees, OR
- Organisations whose processing is likely to result in a risk to data subjects' rights, OR
- Organisations processing special category data, OR
- Organisations processing criminal conviction data

Best practice: maintain one regardless of size.

Controllers and processors must maintain separate ROPAs. This template covers the **controller** ROPA (Art. 30(1)).

---

## How to populate this ROPA

Each row = one **processing activity** (a distinct purpose for which data is used).

Typical activities to identify from a codebase scan:
- User account registration and management
- Customer order processing and fulfilment
- Marketing email campaigns
- Analytics and product improvement
- Customer support
- Payment processing
- Session management / security logging
- Employee HR processing (if applicable)
- Third-party data sharing / advertising

---

## ROPA — Controller Record

**Organisation name:** {{ORGANISATION_NAME}}
**Organisation address:** {{ORGANISATION_ADDRESS}}
**Data Protection Officer (if applicable):** {{DPO_NAME_AND_CONTACT}}
**Date last updated:** {{DATE}}
**Version:** {{VERSION}}

---

### Processing Activities

---

#### Activity 1: {{ACTIVITY_NAME}}

| Field | Detail |
|-------|--------|
| **Activity name** | {{ACTIVITY_NAME}} |
| **Business function / department** | {{DEPARTMENT}} |
| **Description of processing** | {{DESCRIPTION}} |
| **Purpose(s) of processing** | {{PURPOSE}} |
| **Legal basis (Art. 6)** | {{LEGAL_BASIS}} — e.g. Consent (Art. 6(1)(a)) / Contract (Art. 6(1)(b)) / Legal obligation (Art. 6(1)(c)) / Legitimate interests (Art. 6(1)(f)) |
| **Legitimate interest assessment (if Art. 6(1)(f))** | {{LIA_REFERENCE_OR_N/A}} |
| **Special category data? (Art. 9)** | Yes / No — if Yes: {{SPECIAL_CATEGORY_TYPE}} and legal basis under Art. 9(2): {{ART_9_BASIS}} |
| **Categories of data subjects** | {{DATA_SUBJECT_CATEGORIES}} |
| **Categories of personal data** | {{DATA_CATEGORIES}} |
| **Source of personal data** | {{DATA_SOURCE}} — e.g. Directly from data subject / Third party: {{SOURCE_NAME}} |
| **Recipients / disclosures** | {{RECIPIENTS}} — include internal teams and external processors/controllers |
| **Processors used** | {{PROCESSORS}} — list with DPA reference |
| **Retention period** | {{RETENTION_PERIOD}} — e.g. 2 years from last login / 7 years for financial records |
| **Retention basis** | {{RETENTION_BASIS}} — e.g. Legal obligation (tax records) / Business need / Contract term |
| **Deletion method** | {{DELETION_METHOD}} — e.g. Automated purge job / Manual deletion / Anonymisation |
| **International transfers?** | Yes / No — if Yes: destination country {{TRANSFER_DESTINATION}}, transfer mechanism {{TRANSFER_MECHANISM}} |
| **Technical security measures** | {{TECH_MEASURES}} — e.g. Encryption at rest (AES-256), TLS 1.2+, access controls |
| **Organisational security measures** | {{ORG_MEASURES}} — e.g. Staff training, need-to-know access, DPA with processor |
| **DPIA required?** | Yes / No — if Yes: DPIA reference {{DPIA_REF}} |
| **Notes / Risk flags** | {{NOTES}} |

---

#### Activity 2: {{ACTIVITY_NAME}}

*(Copy the table above for each processing activity. Typical activities below — add, remove, or rename as appropriate to what the codebase reveals.)*

---

### Common Activities Starter List

Use this to seed the ROPA — confirm and populate each from the codebase scan:

| # | Activity | Likely Legal Basis | Likely Data |
|---|----------|--------------------|-------------|
| 1 | User registration & account management | Contract | Name, email, password hash |
| 2 | Order / transaction processing | Contract | Name, address, payment ref |
| 3 | Marketing emails | Consent | Email, name, preferences |
| 4 | Product analytics | Legitimate interests | IP, device ID, behaviour events |
| 5 | Customer support | Contract / Legitimate interests | Name, email, support history |
| 6 | Payment processing | Contract | Payment card data (via processor) |
| 7 | Session management & security logging | Legitimate interests | IP, session ID, timestamps |
| 8 | Fraud prevention | Legitimate interests / Legal obligation | IP, device fingerprint, transaction patterns |
| 9 | Legal compliance / record keeping | Legal obligation | Various — as required by law |
| 10 | Employee data (if applicable) | Contract / Legal obligation | Name, salary, bank details, NI/SSN |
| 11 | Cookie tracking / advertising | Consent | Cookie ID, browsing behaviour |
| 12 | Third-party data sharing (partners) | Consent / Legitimate interests | As agreed |

---

## Processor ROPA (Art. 30(2))

If this organisation also acts as a **processor** for other controllers, maintain a separate record:

| Field | Detail |
|-------|--------|
| **Controller name and contact** | {{CONTROLLER_NAME_AND_CONTACT}} |
| **Processor (this org) name and contact** | {{PROCESSOR_NAME_AND_CONTACT}} |
| **DPO contact** | {{DPO_CONTACT}} |
| **Categories of processing carried out for controller** | {{PROCESSING_CATEGORIES}} |
| **Transfers to third countries** | {{TRANSFER_DETAILS}} |
| **Security measures** | {{SECURITY_MEASURES}} |

---

## ROPA Maintenance

- Review and update this ROPA: **at least annually**, and whenever a new processing activity is introduced or materially changed.
- Owner: {{ROPA_OWNER_ROLE}}
- Review date: {{NEXT_REVIEW_DATE}}
- Approved by: {{APPROVER}}

---

*This template is based on Article 30 GDPR requirements. It is a starting point — legal review is recommended before relying on this as your organisation's official ROPA.*
