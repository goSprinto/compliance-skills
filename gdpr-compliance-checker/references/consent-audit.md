# Consent Audit Checklist

Use this when `consent`, `gdpr_consent`, or similar consent mechanisms are detected in the codebase. A consent mechanism existing is not enough — this checklist validates whether it meets GDPR Art. 7 and EDPB guidelines on consent (05/2020).

---

## Table of Contents
1. [Consent record quality checks](#1-consent-record-quality-checks)
2. [Consent collection mechanism checks](#2-consent-collection-mechanism-checks)
3. [Consent withdrawal checks](#3-consent-withdrawal-checks)
4. [Children's consent checks (Art. 8)](#4-childrens-consent-checks-art-8)
5. [Consent for special category data (Art. 9)](#5-consent-for-special-category-data-art-9)
6. [Code scan patterns](#6-code-scan-patterns)
7. [Gap analysis output format](#7-gap-analysis-output-format)

---

## 1. Consent Record Quality Checks

For each consent record stored, check whether the following fields are captured:

| Field | Required? | Gap signal if missing |
|-------|----------|----------------------|
| Data subject identifier (user ID or email) | ✅ Yes | Cannot link consent to a person |
| Timestamp of consent | ✅ Yes | Cannot prove when consent was given |
| Version / reference of privacy policy shown | ✅ Yes | Cannot prove what was consented to |
| Version / reference of consent form/wording shown | ✅ Yes | Cannot prove the wording was clear |
| Specific purposes consented to | ✅ Yes | Blanket consent is invalid |
| Method of consent (checkbox, signature, etc.) | ✅ Recommended | Evidential value |
| IP address at time of consent | 🟡 Recommended | Helpful evidence |
| Consent channel (web, app, paper, phone) | 🟡 Recommended | Traceability |

**Database/schema scan**: Look for a `consents` table or consent fields on the user record. Check what columns exist.

Severe gaps:
- `marketing_opt_in BOOLEAN DEFAULT TRUE` — pre-ticked opt-in is invalid
- No timestamp on consent — cannot prove timing
- Single `gdpr_accepted` boolean with no detail — too coarse; cannot distinguish between purposes

---

## 2. Consent Collection Mechanism Checks

### Is consent freely given?

| Check | Pass condition | Gap signal |
|-------|---------------|-----------|
| Can users access the service without consenting to non-essential processing? | Yes | Consent bundled with T&Cs or access = not freely given |
| Is consent requested separately for each distinct purpose? | Yes | Bundled consent (one checkbox for marketing + analytics + profiling) is invalid |
| Is consent requested separately from other terms? | Yes | Consent buried in general T&Cs is invalid |
| Is there a clear imbalance of power (e.g. employer/employee)? | No imbalance | Employment relationship = consent generally invalid for employer processing |

### Is consent specific?

| Check | Pass condition |
|-------|---------------|
| Separate consent requests for each processing purpose? | Yes |
| Each purpose clearly described in plain language? | Yes |
| No vague purpose like "to improve our services" without specifics? | Clear + specific |

### Is consent informed?

| Check | Pass condition |
|-------|---------------|
| Controller identity disclosed? | Yes |
| Each processing purpose clearly described? | Yes |
| Types of data being processed disclosed? | Yes |
| Right to withdraw consent disclosed before consent given? | Yes |
| Privacy policy linked or provided at point of consent? | Yes |

### Is consent unambiguous?

| Check | Pass condition | Gap signal |
|-------|---------------|-----------|
| Positive affirmative action required (e.g. ticking a box)? | Yes | Pre-ticked boxes, scrolling, continued use = invalid |
| No pre-ticked checkboxes? | No pre-ticks | Pre-ticked = invalid per Planet49 (CJEU) |
| No "by using this site you agree" language? | Not present | Implicit consent via use = invalid |
| Silence or inactivity not treated as consent? | Correct | Must be active action |

---

## 3. Consent Withdrawal Checks

Article 7(3): Withdrawal must be as easy as giving consent.

| Check | Pass condition | Gap signal |
|-------|---------------|-----------|
| Withdrawal mechanism exists? | Yes | No withdrawal = Art. 7(3) violation |
| Withdrawal is as easy as giving consent? | Yes | Giving consent = one click; withdrawal = 5-step form = not equivalent |
| Withdrawal takes effect immediately (or at least promptly)? | Yes | |
| Withdrawal does not affect processing carried out before withdrawal? | Handled correctly | |
| After withdrawal, processing stops for that purpose? | Yes | Check: is the consent flag actually respected in processing logic? |
| Processors notified of withdrawal? | Yes | Mailchimp contact unsubscribed, Segment user suppressed, etc. |

**Code scan**: After a user withdraws consent, does the codebase:
1. Update the consent record flag?
2. Stop sending to marketing email platform?
3. Stop tracking events for that user?
4. Suppress the user in analytics?

---

## 4. Children's Consent Checks (Art. 8)

Applies to "information society services" offered directly to children.

| Check | Applicable? | Pass condition |
|-------|------------|---------------|
| Age verification mechanism exists? | If service may be used by children | Yes |
| For users under 16 (EU default; varies: Ireland=16, Spain=14, UK=13/18): parental consent obtained? | If under-16s may use service | Yes |
| Parental consent is verifiable (not just a "I confirm I am a parent" checkbox)? | Yes | Reasonable verification efforts |
| Privacy information presented in child-appropriate language? | If children are users | Yes |
| No profiling of children without explicit consent? | If children are users | Yes |
| No behavioural advertising targeted at children? | If children are users | Not present |

**Age of digital consent by country**:
| Country | Age | Notes |
|---------|-----|-------|
| Most EU states | 16 | GDPR default |
| Germany | 16 | |
| France | 15 | |
| Ireland | 16 | |
| Spain | 14 | |
| Italy | 14 | |
| Netherlands | 16 | |
| UK | 13 (COPPA-aligned) but Children's Code applies up to 18 | Age Appropriate Design Code |

---

## 5. Consent for Special Category Data (Art. 9)

If special category data is processed on the basis of **explicit consent** (Art. 9(2)(a)):

| Check | Pass condition |
|-------|---------------|
| Consent is "explicit" (not implied or bundled)? | Explicit separate statement or written consent |
| The specific special category type is clearly identified in the consent request? | Yes |
| Consent obtained separately from other consent? | Yes |
| Data subject can withdraw without consequence? | Yes |

**Note**: For most special category processing, consent alone is rarely sufficient — a processing condition under Art. 9(2) must also apply in national law. Legal review is strongly recommended.

---

## 6. Code Scan Patterns

```
# Consent storage — check schema
Search for: consents table, marketing_opt_in, gdpr_consent, cookie_consent, tracking_consent
Check: default value of consent fields (true = pre-ticked problem)
Check: timestamp column on consent record

# Consent enforcement — check logic
Search for: if (user.marketing_consent), if (consent.analytics), canTrack(), hasConsented()
Check: is tracking SDK initialisation guarded by a consent check?
Check: is email marketing send guarded by a consent check?

# Withdrawal — check flow
Search for: unsubscribe, opt_out, withdraw_consent, revoke_consent, delete_consent
Check: does withdrawal cascade to processors (Mailchimp, Segment, etc.)?
Check: is there a dedicated /unsubscribe or /privacy/preferences route?

# Children
Search for: age_verified, date_of_birth, dob, parental_consent, age_gate
Check: is there an age gate before account creation?
```

---

## 7. Gap Analysis Output Format

Add a **Consent Quality** section to the gap analysis:

| Check | Severity | Finding | Recommendation |
|-------|----------|---------|----------------|
| Consent records stored | 🔴/🟠/🟡 | [finding] | [recommendation] |
| Timestamp on consent | | | |
| Policy version recorded | | | |
| Per-purpose granularity | | | |
| No pre-ticked boxes | | | |
| Withdrawal mechanism | | | |
| Withdrawal as easy as giving | | | |
| Processors notified on withdrawal | | | |
| Children's consent (if applicable) | | | |
| Special category explicit consent (if applicable) | | | |
