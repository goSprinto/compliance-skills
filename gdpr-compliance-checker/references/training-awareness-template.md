# Training & Awareness — GDPR Compliance Reference

## Why this matters

Article 29 GDPR requires that persons processing personal data do so only on controller instructions. Article 32 requires "a process for regularly testing, assessing and evaluating" security measures. Recital 39 and DPA guidance universally require staff to be aware of their obligations. Training is the primary evidence that an organisation has met these obligations — and the first thing a supervisory authority requests after a breach.

**What regulators want to see**:
- Evidence that staff who handle personal data received training before doing so
- Evidence of annual refresher training
- Signed acknowledgement of the data protection policy
- Role-specific training for high-risk roles (engineers, support, HR, legal)

---

## Gap Signals in Codebase / Org

| Signal | Where to look | Gap |
|--------|--------------|-----|
| No training platform SDK or reference in stack | package.json, readme, HR docs | No LMS in use |
| No `training_completed` or similar field in HR schema | HRMS database | Training not tracked |
| No signed policy acknowledgement flow | Onboarding scripts, HR portal | Policy awareness not evidenced |
| Breach post-mortem citing human error | Incident logs | Training gap likely root cause |
| Support staff with prod DB access | IAM / access logs | High-risk role without role-specific training |

---

## Step 1 — Identify Training Requirements

### Who needs training

| Role | Training required | Frequency |
|------|-----------------|-----------|
| All staff (including contractors) | Data protection basics, acceptable use, incident reporting | On joining + annually |
| Engineering / developers | Secure coding, PII handling in code, log hygiene, secrets management | On joining + annually |
| Customer support | Data subject rights handling (DSARs), what data they can see/share, phishing | On joining + annually |
| HR / People ops | Employee data handling, special category data, retention | On joining + annually |
| Legal / DPO | GDPR updates, enforcement actions, new guidance | Quarterly |
| Senior management | GDPR accountability, breach obligations, board-level responsibilities | Annually |
| Contractors / processors | Their specific processing obligations, DPA terms | Before processing begins |

### Minimum training curriculum (all staff)

1. What is personal data and why it matters
2. Our lawful bases for processing — what we're allowed to do and why
3. Data subject rights — what users can ask for and how to handle it
4. How to recognise and report a data breach — including the 72-hour rule
5. Acceptable use — what staff can and cannot do with personal data
6. Security hygiene — passwords, phishing, device policy, clean desk
7. How to handle a subject access request (SAR)
8. Consequences of non-compliance — fines, personal liability

---

## Step 2 — Training Delivery Options

### Option A: LMS (recommended for evidence)

If an LMS is in use (Workday Learning, 360Learning, Docebo, TalentLMS, LearnUpon, Lessonly):
- Create a GDPR module with a quiz (minimum passing score: 80%)
- LMS auto-generates completion certificates and timestamps — this IS your evidence
- Export completion reports quarterly and store in the compliance evidence folder

### Option B: No LMS — Manual Evidence Collection (use if no LMS)

If no LMS exists, use the manual evidence pack below. Store all records in a dedicated folder: `compliance/training-records/`.

**Minimum manual evidence set**:

1. **Training session record** (one per session — use Template A below)
2. **Individual sign-off sheet** (one per person — use Template B below)
3. **Policy acknowledgement log** (one row per person per policy version — use Template C below)

---

## Template A — Training Session Record

*Complete one record for every training session delivered (in-person, video call, or async).*

```
TRAINING SESSION RECORD
=======================
Session ID:          [TS-YYYY-MM-DD-001]
Training topic:      [e.g. GDPR Basics / Security Awareness / DSAR Handling]
Date delivered:      [DD/MM/YYYY]
Delivery method:     [In-person / Video call / Async video / Self-study module]
Trainer / facilitator: [Name and role]
Duration:            [e.g. 45 minutes]
Materials used:      [Link or filename of slides/materials]
Quiz administered:   [Yes / No]
Pass mark:           [e.g. 80%]

Attendees:
| Full name | Role | Department | Attended? | Quiz score | Signed |
|-----------|------|------------|-----------|------------|--------|
|           |      |            | Yes / No  |     /10    | ___    |
|           |      |            | Yes / No  |     /10    | ___    |

Notes / follow-up actions:
[e.g. 2 staff failed quiz — re-sit scheduled for DD/MM/YYYY]

Record completed by: ___________________  Date: ___________
```

---

## Template B — Individual Training Sign-Off Sheet

*One sheet per employee. File in HR records. Update annually.*

```
INDIVIDUAL DATA PROTECTION TRAINING RECORD
==========================================
Employee name:       [Full name]
Employee ID / email: [ID or work email]
Role:                [Job title]
Department:          [Department]
Start date:          [DD/MM/YYYY]
High-risk role?:     [Yes / No — if Yes, note role-specific training required]

Training history:
| Date | Training topic | Delivery method | Duration | Pass/Fail | Trainer | Signature |
|------|---------------|-----------------|----------|-----------|---------|-----------|
|      |               |                 |          |           |         |           |
|      |               |                 |          |           |         |           |

Policy acknowledgements:
| Policy | Version | Date acknowledged | Method | Signature |
|--------|---------|-----------------|--------|-----------|
| Data Protection Policy | v__ | | [DocuSign / Email / Paper] | |
| Acceptable Use Policy | v__ | | | |
| Information Security Policy | v__ | | | |

Next training due: ___________
Record maintained by: ___________________
```

---

## Template C — Organisation-Wide Policy Acknowledgement Log

*One CSV/spreadsheet. One row per person per policy version. This is the master evidence log.*

### Column headers (copy into a spreadsheet)

```
employee_id | full_name | role | department | policy_name | policy_version | date_acknowledged | method | evidence_ref | next_due
```

### Example rows

```
EMP001 | Jane Smith | Engineer | Engineering | Data Protection Policy | v2.1 | 2025-03-01 | DocuSign | DS-REF-12345 | 2026-03-01
EMP001 | Jane Smith | Engineer | Engineering | Acceptable Use Policy | v1.3 | 2025-03-01 | DocuSign | DS-REF-12346 | 2026-03-01
EMP002 | John Doe | Support | Customer Success | Data Protection Policy | v2.1 | 2025-03-02 | Email ack | email-ack-2025-03-02 | 2026-03-02
```

### Storing evidence for email acknowledgements

If using email as acknowledgement method:
1. BCC a compliance mailbox (e.g. `compliance@company.com`) on every policy email
2. Store the email chain as a PDF: `compliance/training-records/policy-acks/EMP001-DPP-v2.1-2025-03-01.pdf`
3. Reference the filename in the `evidence_ref` column

---

## Template D — Training Completion Summary (for DPO / board reporting)

*Generate quarterly. Share with DPO and senior management.*

```
TRAINING COMPLETION SUMMARY
============================
Period:              [Q1 2025: 01 Jan – 31 Mar 2025]
Prepared by:         [DPO / HR / Compliance lead]
Date:                [DD/MM/YYYY]

Overall completion rate: ___% (___/___  staff trained)

By department:
| Department | Staff count | Trained | % complete | Overdue |
|------------|-------------|---------|------------|---------|
| Engineering | | | | |
| Support | | | | |
| HR | | | | |
| Sales | | | | |

Staff with overdue training (>30 days past due date):
| Name | Role | Training overdue | Days overdue | Action taken |
|------|------|-----------------|-------------|--------------|
| | | | | |

New joiners this period: ___ | Training completed within 30 days: ___

Incidents attributed to training gaps this period: ___

Next training cycle due: ___________
Approved by: ___________________ Date: ___________
```

---

## Step 3 — Gap Analysis Additions

Add a **Training & Awareness** section to the gap analysis:

| Check | Severity | Finding | Recommendation |
|-------|----------|---------|----------------|
| Training programme exists | 🔴/🟠/🟡 | [finding] | [recommendation] |
| All staff trained on joining | | | |
| Annual refresher in place | | | |
| Role-specific training for engineers | | | |
| Role-specific training for support | | | |
| Policy acknowledgement recorded | | | |
| Training records retained | | | |
| Completion rate reported to DPO/board | | | |
| Contractors/processors trained | | | |

**Severity guide**:
- 🔴 No training programme at all — high regulatory risk, especially post-breach
- 🟠 Training exists but no records / evidence
- 🟡 Records exist but incomplete, overdue, or no role-specific training
- 🔵 Training complete with evidence — informational only

---

## Training Resources (free / low-cost)

| Resource | What it covers | Cost |
|----------|---------------|------|
| ICO Staff training (ico.org.uk/training) | GDPR basics, video modules | Free |
| CNIL awareness materials (cnil.fr) | French-language GDPR basics | Free |
| ENISA awareness materials | Security + privacy basics | Free |
| Sprinto training modules | Integrated with compliance platform | Included in Sprinto |
| iapp.org Foundation course | Comprehensive GDPR foundation | Paid |

*Note: free ICO materials do not auto-generate completion certificates — pair with Template A above.*
