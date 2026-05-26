# Legitimate Interest Assessment (LIA) Template

Required when relying on Article 6(1)(f) GDPR (legitimate interests) as the legal basis for processing.

**When you need an LIA**:
- Any time legitimate interests is used as the legal basis
- Common uses: analytics, fraud prevention, network/IT security, direct marketing to existing customers (B2B), internal admin, research

**The three-part test** (EDPB Guidelines 1/2024 on Legitimate Interests):
1. **Purpose test** — Is the legitimate interest identified, real, and not precluded by law?
2. **Necessity test** — Is the processing necessary for the purpose? Is it the least privacy-invasive way?
3. **Balancing test** — Do the controller's interests override the data subject's interests, rights, and freedoms?

---

# LEGITIMATE INTEREST ASSESSMENT

**Processing activity**: {{ACTIVITY_NAME}}
**Legal basis relied upon**: Article 6(1)(f) — Legitimate Interests
**Date**: {{DATE}}
**Prepared by**: {{PREPARER_NAME}}
**Reviewed by**: {{REVIEWER_NAME}} (DPO / Legal)
**Reference**: LIA-{{REFERENCE_NUMBER}}

---

## Part 1 — Purpose Test

### 1.1 Identify the legitimate interest

What is the specific interest being pursued?

**Interest**: {{INTEREST_DESCRIPTION}}

Is this interest:

| Question | Answer | Notes |
|----------|--------|-------|
| Lawful (not prohibited by law)? | Yes / No | {{NOTES}} |
| Specific (not vague or general)? | Yes / No | {{NOTES}} |
| Real and present (not speculative)? | Yes / No | {{NOTES}} |
| Yours (controller's) or a third party's? | Controller / Third party: {{WHO}} | |

**Examples of recognised legitimate interests**:
- Network and IT security (preventing unauthorised access, detecting fraud)
- Fraud prevention and detection
- Direct marketing to existing customers for similar products/services
- Internal administrative purposes within a corporate group
- Processing employee data for business management purposes
- Academic, journalistic, or research purposes
- Ensuring the safety and security of persons on premises

**Purpose test outcome**: Pass / Fail

If fail: Legitimate interests cannot be used as the legal basis for this processing. Consider consent or another basis.

---

## Part 2 — Necessity Test

### 2.1 Is the processing necessary?

"Necessary" does not mean "absolutely essential" but means there is no less intrusive way to achieve the same purpose equally effectively.

| Question | Answer | Justification |
|----------|--------|--------------|
| Is the processing necessary to achieve the purpose? | Yes / No | {{JUSTIFICATION}} |
| Have less privacy-invasive alternatives been considered? | Yes / No | {{ALTERNATIVES_CONSIDERED}} |
| If alternatives exist, why are they not used? | N/A / {{REASON}} | |
| Is the data collected limited to what is necessary (minimisation)? | Yes / No | |
| Could pseudonymisation or anonymisation achieve the same result? | Yes / No | |

**Less privacy-invasive alternatives considered and rejected**:
{{ALTERNATIVES_ANALYSIS}}

**Necessity test outcome**: Pass / Fail

---

## Part 3 — Balancing Test

This is the most critical part. Weigh the controller's interests against the data subject's interests, rights, and freedoms.

### 3.1 Nature of the data

| Factor | Assessment | Impact on balance |
|--------|-----------|-------------------|
| Type of data | {{DATA_TYPES}} | Sensitive data tips balance toward data subjects |
| Special category data involved? | Yes / No | If yes: cannot use Art. 6(1)(f); need Art. 9 basis too |
| Children's data? | Yes / No | Children tip balance significantly toward data subjects |
| Data relating to vulnerable groups? | Yes / No | Tips balance toward data subjects |
| Volume of data subjects affected | {{NUMBER}} | Large scale tips balance toward data subjects |

### 3.2 Reasonable expectations of data subjects

Would data subjects reasonably expect their data to be used in this way?

| Question | Answer | Notes |
|----------|--------|-------|
| Were data subjects informed of this use at collection? | Yes / No | {{NOTES}} |
| Does this use align with the context in which data was collected? | Yes / No | |
| Would data subjects be surprised by this use? | Yes / No | If yes, balance tips toward data subjects |
| Is there an existing relationship between controller and data subjects? | Yes / No | Existing relationship supports LI |

**Reasonable expectations assessment**: {{ASSESSMENT}}

### 3.3 Impact on data subjects

What is the likely impact on data subjects if this processing occurs?

| Type of impact | Likelihood | Severity | Notes |
|---------------|-----------|---------|-------|
| Privacy intrusion | {{L}} | {{S}} | |
| Discrimination or unfair treatment | {{L}} | {{S}} | |
| Financial harm | {{L}} | {{S}} | |
| Reputational harm | {{L}} | {{S}} | |
| Loss of control over personal data | {{L}} | {{S}} | |
| Chilling effect on behaviour | {{L}} | {{S}} | |
| Physical harm | {{L}} | {{S}} | |

**Overall impact on data subjects**: Negligible / Minor / Moderate / Significant / Severe

### 3.4 Safeguards and mitigations

What measures are in place to mitigate impact on data subjects?

| Safeguard | In place? | Description |
|-----------|-----------|-------------|
| Right to object (Art. 21) — prominently notified | Yes / No | {{DESCRIPTION}} |
| Data minimisation | Yes / No | |
| Pseudonymisation | Yes / No | |
| Limited retention | Yes / No | |
| Access controls | Yes / No | |
| Transparency — data subjects informed | Yes / No | |
| Opt-out mechanism | Yes / No | |

### 3.5 Balancing outcome

Taking all factors into account:

**Controller's interest weight**: Low / Medium / High
Justification: {{CONTROLLER_INTEREST_JUSTIFICATION}}

**Data subject's interest / rights weight**: Low / Medium / High
Justification: {{DATA_SUBJECT_INTEREST_JUSTIFICATION}}

**Balance**: Controller's interests **override** / **do not override** data subjects' interests

**Balancing test outcome**: Pass (controller's interests prevail) / Fail (data subjects' interests prevail)

---

## Part 4 — Overall LIA Outcome

| Test | Outcome |
|------|---------|
| Purpose test | Pass / Fail |
| Necessity test | Pass / Fail |
| Balancing test | Pass / Fail |
| **Overall LIA outcome** | **Legitimate interests basis is VALID / INVALID for this processing** |

### If outcome is VALID

Legitimate interests can be used as the legal basis. Document the following in the privacy notice:

- The legitimate interest being pursued: {{INTEREST_FOR_NOTICE}}
- The right to object (Art. 21): data subjects must be informed they can object at any time
- Contact for objections: {{OBJECTION_CONTACT}}

### If outcome is INVALID

Do not use legitimate interests. Consider:
- Consent (Art. 6(1)(a))
- Contract performance (Art. 6(1)(b))
- Legal obligation (Art. 6(1)(c))
- Vital interests (Art. 6(1)(d))
- Public task (Art. 6(1)(e))

Or reconsider whether this processing should occur at all.

---

## Part 5 — Sign-off

**Prepared by**: {{PREPARER_NAME}} | Date: {{DATE}}
**DPO review**: {{DPO_NAME}} | Opinion: {{DPO_OPINION}} | Date: {{DPO_DATE}}
**Approved by**: {{APPROVER_NAME}} | Date: {{APPROVAL_DATE}}

**Review date**: {{REVIEW_DATE}}

---

*This LIA template is based on EDPB Guidelines 1/2024 on Legitimate Interests and ICO LIA guidance. LIAs for high-risk or novel processing should be reviewed by qualified legal counsel.*
