# Breach Response Pack

Three documents in one file:
1. [Breach Response Checklist](#1-breach-response-checklist) — run this immediately when a breach is suspected
2. [Supervisory Authority Notification Template](#2-supervisory-authority-notification-template) — Article 33 GDPR
3. [Data Subject Notification Template](#3-data-subject-notification-template) — Article 34 GDPR
4. [Internal Breach Log](#4-internal-breach-log) — Article 33(5) GDPR

---

## 0. Runbook Validation — Does Your Existing Runbook Meet GDPR Requirements?

If the organisation already has an incident response runbook, validate it against these GDPR-specific requirements before relying on it. Gap signals in the runbook are as serious as gaps in the codebase.

| Requirement | What to look for in the runbook | Severity if missing |
|-------------|--------------------------------|-------------------|
| **72-hour clock** | Does the runbook explicitly state the 72-hour window starts from *awareness* (not discovery or confirmation)? | 🔴 Critical |
| **Supervisory authority identified** | Is the lead supervisory authority named, with the correct notification portal URL? | 🔴 Critical |
| **Breach threshold defined** | Does the runbook specify what constitutes a notifiable breach (likely risk to individuals)? | 🔴 Critical |
| **Incident classification** | Are incidents categorised by type (confidentiality / integrity / availability) and risk level? | 🟠 High |
| **DPO role defined** | Is the DPO explicitly named or role-referenced as the notification decision-maker? | 🟠 High |
| **Data subject notification trigger** | Does the runbook distinguish between SA-notification-only (risk) vs also-notify-individuals (high risk)? | 🔴 Critical |
| **Processor notification obligation** | Does the runbook require processors to notify the controller "without undue delay"? Is this tracked? | 🟠 High |
| **Evidence preservation** | Does the runbook require preserving logs and system state before remediation? | 🟠 High |
| **Breach log obligation** | Does the runbook reference Art. 33(5) internal logging of ALL breaches, including non-notifiable ones? | 🟡 Medium |
| **Phased notification allowed** | Does the runbook allow partial notification within 72 hours with follow-up? | 🟡 Medium |
| **Post-incident review** | Does the runbook require a root-cause analysis and policy update after each incident? | 🟡 Medium |
| **Tested recently** | Has the runbook been tested (tabletop exercise or live drill) in the past 12 months? | 🟠 High |

**How to use this**: If the existing runbook is found in the repository (e.g. `INCIDENT_RESPONSE.md`, `runbook/breach.md`, `docs/security/incidents.md`), read it and run each row above. Add findings to the gap analysis under Art. 33 and Art. 34.

**If no runbook exists**: Generate one using the Breach Response Checklist below (§1) and the notification templates (§2–3). This is a 🔴 Critical gap.

---

## 1. Breach Response Checklist

### Immediate (0–4 hours)

- [ ] **Confirm the breach**: Is this a confirmed or suspected personal data breach? Assign an incident owner.
- [ ] **Contain**: Stop the breach from continuing or worsening. Isolate affected systems if necessary. Do not delete evidence.
- [ ] **Notify DPO**: Contact DPO immediately. If no DPO, notify the data protection lead and senior management.
- [ ] **Start the clock**: Document the exact date and time the organisation *became aware* of the breach. The 72-hour notification window starts here.
- [ ] **Preserve evidence**: Preserve logs, access records, and any relevant system state. Do not overwrite or delete.
- [ ] **Preliminary scope**: Estimate the categories and approximate number of individuals affected.

### Assessment (4–24 hours)

- [ ] **Categorise the breach**:
  - Confidentiality breach (unauthorised disclosure)
  - Integrity breach (unauthorised alteration)
  - Availability breach (loss of access)
- [ ] **Identify data types involved**: What personal data is affected? Does it include special category data (Art. 9)?
- [ ] **Assess risk to individuals**: What harm could result? (Identity theft, financial loss, discrimination, physical harm, reputational damage, loss of confidentiality of professional secrets)
- [ ] **Determine notification threshold**:
  - Likely to result in *risk* to individuals → Notify supervisory authority (Art. 33)
  - Likely to result in *high risk* → Also notify affected individuals (Art. 34)
  - No risk → Document in breach log; no notification required
- [ ] **Identify cause**: Ransomware / phishing / misconfiguration / human error / third-party breach / theft / other
- [ ] **Identify affected processors**: Were any processors involved? They must notify you "without undue delay" (Art. 33(2)) — follow up with them.

### Notification (24–72 hours)

- [ ] **Prepare supervisory authority notification** (use template in §2 below)
- [ ] **Notify supervisory authority** within 72 hours of becoming aware (if threshold met). Note: if not all info is available, submit what you have and indicate further information will follow.
- [ ] **Notify affected individuals** without undue delay (if high-risk threshold met) — use template in §3 below
- [ ] **Notify joint controllers** if applicable
- [ ] **Instruct processors** to assist with investigation and notification

### Remediation (72 hours – 2 weeks)

- [ ] **Root cause analysis**: Document what caused the breach
- [ ] **Technical remediation**: Patch, reconfigure, or re-secure affected systems
- [ ] **Update internal breach log** (Art. 33(5)) — use template in §4 below
- [ ] **Review and update**: Update data protection policies, staff training, technical measures as needed
- [ ] **Follow-up with supervisory authority**: Provide additional information if initial notification was incomplete
- [ ] **Close-out report**: Document lessons learned and corrective actions

---

## 2. Supervisory Authority Notification Template

*(Article 33 GDPR — submit to lead supervisory authority within 72 hours)*

---

**TO**: {{SUPERVISORY_AUTHORITY_NAME}}
**FROM**: {{ORGANISATION_NAME}}
**DATE**: {{NOTIFICATION_DATE}}
**RE**: Personal Data Breach Notification under Article 33 GDPR
**REFERENCE**: {{INTERNAL_BREACH_REFERENCE}}

---

### Section 1 — Controller Details

| Field | Detail |
|-------|--------|
| Controller name | {{CONTROLLER_NAME}} |
| Registration number | {{COMPANY_NUMBER}} |
| Registered address | {{ADDRESS}} |
| Main EU establishment | {{ESTABLISHMENT_COUNTRY}} |
| DPO name | {{DPO_NAME}} |
| DPO contact email | {{DPO_EMAIL}} |
| DPO contact phone | {{DPO_PHONE}} |
| Contact for this notification (if different from DPO) | {{CONTACT_NAME}}, {{CONTACT_EMAIL}} |

---

### Section 2 — Nature of the Breach

**Date and time breach occurred (if known)**: {{BREACH_DATE_TIME}}
**Date and time organisation became aware**: {{AWARENESS_DATE_TIME}}
**Time elapsed between occurrence and awareness**: {{ELAPSED_TIME}}

**Type of breach** (tick all that apply):
- [ ] Confidentiality breach (unauthorised or accidental disclosure of personal data)
- [ ] Integrity breach (unauthorised or accidental alteration of personal data)
- [ ] Availability breach (accidental or unauthorised loss of access to or destruction of personal data)

**Description of the breach**:
{{BREACH_DESCRIPTION}}

**Cause of the breach** (if known):
- [ ] Ransomware / malware
- [ ] Phishing / social engineering
- [ ] Unauthorised access (internal)
- [ ] Unauthorised access (external)
- [ ] Human error (accidental disclosure)
- [ ] Misconfiguration / vulnerability
- [ ] Lost or stolen device / media
- [ ] Third-party / processor breach
- [ ] Other: {{OTHER_CAUSE}}

---

### Section 3 — Data Subjects and Personal Data Affected

| Field | Detail |
|-------|--------|
| Approximate number of data subjects affected | {{NUMBER_OF_DATA_SUBJECTS}} |
| Categories of data subjects | {{DATA_SUBJECT_CATEGORIES}} (e.g. customers, employees, users) |
| Approximate number of personal data records affected | {{NUMBER_OF_RECORDS}} |
| Categories of personal data affected | {{DATA_CATEGORIES}} |
| Does this include special category data (Art. 9)? | Yes / No — if Yes: {{SPECIAL_CATEGORY_TYPES}} |
| Does this include data relating to children? | Yes / No |
| Geographic scope | {{COUNTRIES_AFFECTED}} |

---

### Section 4 — Likely Consequences

What are the likely consequences of the breach?
{{LIKELY_CONSEQUENCES}}

Assessment of risk to data subjects:
- [ ] No risk — documenting internally, not notifying individuals
- [ ] Risk — notifying supervisory authority only
- [ ] High risk — also notifying affected individuals under Art. 34

---

### Section 5 — Measures Taken and Proposed

**Containment measures already taken**:
{{CONTAINMENT_MEASURES}}

**Measures to address adverse effects on data subjects**:
{{REMEDIATION_MEASURES}}

**Measures to prevent recurrence**:
{{_PREVENTIVE_MEASURES}}

---

### Section 6 — Information Availability

- [ ] All information is available and included in this notification
- [ ] Not all information is yet available — the following information will be provided in a follow-up notification: {{OUTSTANDING_INFORMATION}}
  Expected follow-up date: {{FOLLOWUP_DATE}}

---

*Signed on behalf of {{CONTROLLER_NAME}}*

Name: {{SIGNATORY_NAME}}
Title: {{SIGNATORY_TITLE}}
Date: {{DATE}}

---

## 3. Data Subject Notification Template

*(Article 34 GDPR — required when breach is likely to result in HIGH RISK to individuals)*

---

**Subject line**: Important notice about your personal data — [Company Name]

Dear {{DATA_SUBJECT_NAME / "Valued Customer" / "User"}},

We are writing to inform you of a data security incident that may have affected your personal information.

**What happened**

{{PLAIN_LANGUAGE_DESCRIPTION_OF_BREACH}}

This occurred on approximately {{DATE}}, and we became aware of it on {{AWARENESS_DATE}}.

**What personal data was involved**

The following categories of your personal data may have been affected:
{{LIST_OF_AFFECTED_DATA_TYPES}}

**What we have done**

We took the following steps immediately upon discovering the incident:
{{CONTAINMENT_AND_REMEDIATION_STEPS}}

We have also notified {{SUPERVISORY_AUTHORITY_NAME}}, our data protection regulator.

**What you should do**

{{RECOMMENDED_ACTIONS_FOR_DATA_SUBJECTS}}

Examples of recommended actions depending on data type:
- *Passwords*: Change your password on this service and any other services where you use the same password. Enable two-factor authentication.
- *Financial data*: Monitor your bank statements for unusual activity. Contact your bank if you see anything suspicious. Consider requesting a fraud alert from credit agencies.
- *Email*: Be alert for phishing emails claiming to be from us or other services.
- *Identity data*: Consider placing a fraud alert with credit reference agencies (Experian, Equifax, TransUnion).

**Your rights**

You have the right to:
- Access the personal data we hold about you
- Request correction or deletion of your data
- Lodge a complaint with {{SUPERVISORY_AUTHORITY_NAME}} at {{SUPERVISORY_AUTHORITY_WEBSITE}}

**Contact us**

If you have any questions about this incident, please contact our Data Protection Officer:
{{DPO_NAME}}
{{DPO_EMAIL}}
{{DPO_PHONE}}

We sincerely apologise for any concern or inconvenience this may cause.

Yours sincerely,
{{SIGNATORY_NAME}}
{{SIGNATORY_TITLE}}
{{COMPANY_NAME}}

---

## 4. Internal Breach Log

*(Article 33(5) GDPR — all breaches must be logged, including those not notified to the authority)*

Maintain a running log. Add a row for every breach or suspected breach, regardless of whether it was notified.

| # | Breach ref | Date/time occurred | Date/time discovered | Description | Data categories | # data subjects | Cause | Risk level | Notified authority? | Notified individuals? | Containment measures | Remediation | Lessons learned | Log entry date | Logged by |
|---|-----------|-------------------|---------------------|-------------|-----------------|-----------------|-------|-----------|--------------------|-----------------------|--------------------|-------------|-----------------|---------------|-----------|
| 1 | BR-2024-001 | {{DATE}} | {{DATE}} | {{DESCRIPTION}} | {{DATA}} | {{NUMBER}} | {{CAUSE}} | Low/Medium/High | Yes / No — {{DATE}} | Yes / No — {{DATE}} | {{MEASURES}} | {{REMEDIATION}} | {{LESSONS}} | {{DATE}} | {{NAME}} |

**Retention**: The breach log must be retained for at least 3 years (best practice; some DPAs recommend longer).

**Access**: Restricted to DPO, legal counsel, senior management, and those handling the specific breach. Must be available to the supervisory authority on request.
