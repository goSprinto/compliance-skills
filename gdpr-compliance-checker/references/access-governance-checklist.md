# Access Governance Checklist

Access governance is the operational control that limits who can reach personal data — at the code level, database level, cloud infrastructure level, and support tooling level. Art. 25 (privacy by design) and Art. 32 (security) both require it. Regulators view uncontrolled access to production personal data as a critical finding.

---

## Table of Contents
1. [Access governance gap signals](#1-access-governance-gap-signals)
2. [Access tiers to audit](#2-access-tiers-to-audit)
3. [Template A — Access register](#3-template-a--access-register)
4. [Template B — Access review record](#4-template-b--access-review-record)
5. [Template C — Privileged access log](#5-template-c--privileged-access-log)
6. [Code and infra scan patterns](#6-code-and-infra-scan-patterns)
7. [Gap analysis additions](#7-gap-analysis-additions)

---

## 1. Access Governance Gap Signals

| Signal | Where to look | Risk |
|--------|--------------|------|
| All engineers have prod DB access | IAM policies, DB user table | Excessive access — Art. 32 violation |
| Support staff can query raw user PII | Support tool config, CRM roles | Violates need-to-know |
| Shared DB credentials in .env | Codebase, CI/CD secrets | No audit trail; impossible to revoke individual access |
| Cloud console admin access not reviewed | AWS IAM / GCP IAM / Azure AD | Over-privileged accounts |
| No offboarding access revocation process | HR/IT policy | Former employees retain access |
| No access review log | Compliance records | Cannot evidence GDPR Art. 32 compliance |
| Developer tools access production data | Dev tooling config | Test environments should use anonymised data |

---

## 2. Access Tiers to Audit

### Tier 1: Production Database

- Who has direct DB access (psql, MySQL Workbench, pgAdmin, etc.)?
- Is access role-based (read-only for most, write only for services, DBA for admins)?
- Are individual credentials used, or shared?
- Is DB access logged with user-level audit trails?
- Is there a break-glass / emergency access procedure?

### Tier 2: Cloud Console (AWS / GCP / Azure)

- Who has IAM admin roles?
- Who has roles that include access to S3/GCS/Blob storage containing personal data?
- Is MFA enforced for all cloud console access?
- Are IAM policies scoped to least privilege?
- Are access keys rotated regularly (90 days max)?
- Are unused IAM users/roles removed?

### Tier 3: Admin Tools

- Who has access to the internal admin panel?
- Can support staff export arbitrary user data?
- Is admin panel access logged per-user with action audit trail?
- Are admin actions rate-limited or require two-person approval for sensitive operations (e.g. bulk exports)?

### Tier 4: Support Tools (Zendesk, Intercom, Freshdesk)

- Which support roles can see full customer PII vs. masked data?
- Is there a tiered access model (L1 support sees less than L2)?
- Are support tool sessions logged?
- Is customer data masked in support tickets by default?

### Tier 5: Analytics / BI Tools (Metabase, Looker, Tableau, Mode)

- Can analysts run raw SQL against production tables containing PII?
- Is PII anonymised or pseudonymised before reaching BI tools?
- Are BI tool exports logged and controlled?

### Tier 6: Developer / Staging Environments

- Does staging/dev use production personal data?
- If yes: is there a documented exception with DPO sign-off?
- Are dev DB credentials separate from prod?

---

## 3. Template A — Access Register

*One row per person with any access to personal data systems. Review quarterly.*

**Organisation:** {{ORGANISATION_NAME}}
**Last reviewed:** {{DATE}}
**Reviewed by:** {{NAME, ROLE}}
**Next review:** {{DATE}}

| Employee ID | Name | Role | Department | System / tool | Access level | Business justification | Access granted date | Granted by | Last access review | Next review | MFA enabled | Notes |
|------------|------|------|------------|--------------|-------------|------------------------|--------------------|-----------|--------------------|-------------|-------------|-------|
| EMP001 | {{NAME}} | {{ROLE}} | {{DEPT}} | Prod DB | Read-only | Customer support queries | {{DATE}} | {{MANAGER}} | {{DATE}} | {{DATE}} | Yes | |
| EMP002 | {{NAME}} | {{ROLE}} | {{DEPT}} | AWS Console | Admin | Infrastructure management | {{DATE}} | {{MANAGER}} | {{DATE}} | {{DATE}} | Yes | Review due |

### Access levels reference

| Level | Definition |
|-------|-----------|
| Read-only | Can query/view data; cannot modify or export in bulk |
| Read-write | Can query and modify records; cannot delete or export in bulk |
| Admin | Full access including schema changes, bulk export, user management |
| Break-glass | Emergency elevated access; time-limited; requires approval and generates alert |
| No access | Role does not require access to this system |

---

## 4. Template B — Access Review Record

*Complete quarterly. One record per review cycle per system.*

```
ACCESS REVIEW RECORD
====================
System:              [e.g. Production PostgreSQL / AWS Console / Zendesk]
Review period:       [Q1 2025: 01 Jan – 31 Mar 2025]
Reviewer:            [Name, Role]
Review date:         [DD/MM/YYYY]
Approved by:         [Name, Role]

Summary:
Total users with access at start of period: ___
Access granted during period: ___
Access revoked during period: ___
Access modified during period: ___
Total users with access at end of period: ___

Changes made during this review:
| Name | Previous access | New access | Reason | Action date |
|------|----------------|------------|--------|-------------|
|      |                |            |        |             |

Users flagged for follow-up (access not yet confirmed appropriate):
| Name | System | Current access | Flag reason | Follow-up by |
|------|--------|---------------|-------------|-------------|
|      |        |               |             |             |

Certification:
"I confirm that access to {{SYSTEM}} has been reviewed for the period above and all 
access is appropriate and limited to what is necessary for each individual's role."

Reviewer signature: ___________________  Date: ___________
Approver signature: ___________________  Date: ___________
```

---

## 5. Template C — Privileged Access Log

*Log every instance of privileged (admin/break-glass) access to production personal data. Required for Art. 32 audit trail.*

```
PRIVILEGED ACCESS LOG
=====================
System: [Production Database / AWS Console / Admin Panel]
Period: [YYYY-MM]
```

| Log ID | Date/time | User | Access type | Systems accessed | Business reason | Duration | Approved by | Approval ref | Data accessed (summary) | Incident raised? |
|--------|-----------|------|-------------|-----------------|-----------------|----------|------------|-------------|------------------------|-----------------|
| PAL-001 | 2025-03-15 14:22 UTC | {{NAME}} | Break-glass | Prod DB — users table | Customer data recovery following support escalation | 18 min | {{MANAGER}} | TKT-4521 | Email addresses for 3 accounts | No |

**Retention**: Keep privileged access logs for minimum 3 years.
**Access to this log**: Restricted to DPO, CISO, legal counsel, and auditors.

---

## 6. Code and Infra Scan Patterns

```
# Shared credentials
Search for: DB_PASSWORD, DATABASE_URL, POSTGRES_PASSWORD in .env files
Check: are these the same across dev/staging/prod? (shared = bad)
Check: are they hardcoded in config files committed to git?

# IAM policy over-privilege (AWS)
Look for: "Action": "*" or "Resource": "*" in IAM policy JSON
Look for: PowerUserAccess, AdministratorAccess attached to non-admin roles

# Admin panel access control
Search for: isAdmin, role === 'admin', hasPermission('admin')
Check: is admin access tied to a role or just a boolean flag?

# Audit logging
Search for: audit_log, access_log, admin_action — is there a table/service logging admin actions?
Check: does each admin action record who did it, when, and what?

# Dev/prod data separation
Search for: NODE_ENV === 'production' ? prodDB : devDB
Check: does dev connect to production DB in any environment?
Search for: seed.ts, fixtures/ — do seeds use real PII or anonymised data?

# Support tool data masking
Search for: email?.replace, mask, redact, obfuscate in support-related code
Check: are PII fields masked before being sent to support ticket payloads?
```

---

## 7. Gap Analysis Additions

Add an **Access Governance** section to the gap analysis:

| Check | Severity | Finding | Recommendation |
|-------|----------|---------|----------------|
| Access register maintained | 🟠 High | [finding] | [recommendation] |
| Prod DB access limited to need-to-know | 🔴 Critical | | |
| Individual (not shared) DB credentials | 🔴 Critical | | |
| Cloud console MFA enforced | 🔴 Critical | | |
| IAM policies scoped to least privilege | 🟠 High | | |
| Admin panel access logged | 🟠 High | | |
| Support tools: PII masked / tiered access | 🟠 High | | |
| Analytics tools: no raw PII access | 🟡 Medium | | |
| Dev/staging: no production PII | 🟡 Medium | | |
| Quarterly access reviews conducted | 🟠 High | | |
| Offboarding access revocation process | 🟠 High | | |
| Privileged access log maintained | 🟡 Medium | | |
