# Sub-Processor Notification & Validation Template

Under Article 28(2) GDPR, a processor may not engage sub-processors without prior specific or general written authorisation from the controller. When general authorisation is given (standard in most DPAs), the processor must notify the controller of any intended sub-processor changes and give the controller the opportunity to object.

This file covers:
1. The validation protocol when a new third-party domain or service is detected
2. The approved sub-processor register (maintained by the organisation as a processor)
3. Customer notification template (when the organisation adds a new sub-processor)
4. Objection window tracker
5. Code scan patterns to detect undeclared sub-processors

---

## 1. Sub-Processor Validation Protocol

Run this flow whenever the codebase scan detects a new third-party domain or service receiving personal data:

```
NEW DOMAIN / SERVICE DETECTED RECEIVING PERSONAL DATA
                    │
                    ▼
         Is this domain on the approved
         Sub-Processor List / Trust Centre?
                    │
          ┌─────────┴─────────┐
         YES                  NO
          │                   │
    Continue               ───────────────────────────
          │               Is this a processor or a
          ▼               joint controller?
    Is there a               │
    signed DPA?         ┌────┴────┐
          │           PROCESSOR  JOINT CONTROLLER
     ┌────┴────┐          │           │
    YES        NO         ▼           ▼
     │          │    Follow steps   Draft Joint
     ▼          │    2–4 below     Controller Agreement
  Continue  Trigger               (Art. 26)
             DPA workflow
                    │
                    ▼
         STEP 1: Add to sub-processor register (Template A)
         STEP 2: Obtain / verify signed DPA (Art. 28 terms)
         STEP 3: Notify customers (Template B)
         STEP 4: Wait for objection window (Template C)
         STEP 5: Record outcome and close
```

---

## 2. Template A — Approved Sub-Processor Register

*Maintained by the organisation (as a processor for its customers). Publish this or make it available via a Trust Centre URL. Update every time a sub-processor is added, changed, or removed.*

**Organisation:** {{ORGANISATION_NAME}}
**Trust Centre URL:** {{TRUST_CENTRE_URL}}
**Last updated:** {{DATE}}
**Version:** {{VERSION}}

| # | Sub-Processor Name | Parent company | Service / purpose | Data categories processed | Data location | Transfer mechanism | DPA in place? | DPA date | Sub-processor's sub-processors | Date added | Date removed |
|---|-------------------|---------------|------------------|--------------------------|--------------|-------------------|--------------|----------|-------------------------------|------------|--------------|
| 1 | {{NAME}} | {{PARENT}} | {{PURPOSE}} | {{DATA_TYPES}} | {{REGION}} | DPF / SCCs / Adequacy | Yes / No | {{DATE}} | {{SUB_SUBS}} | {{DATE}} | — |

### Minimum data categories to document per sub-processor

- Name, contact information, usage data → all sub-processors receiving user-facing data
- Payment data → payment processors only
- Health / special category → healthcare sub-processors only
- IP address, device data → analytics, security, logging sub-processors

---

## 3. Template B — Customer Notification: New Sub-Processor

*Send to all affected customers before the sub-processor becomes active (or at least 30 days before, as typically required by DPA terms). Adapt for email, in-app notification, or Trust Centre changelog.*

---

**Subject:** Notice of new sub-processor — {{NEW_SUBPROCESSOR_NAME}} — effective {{EFFECTIVE_DATE}}

Dear {{CUSTOMER_NAME / "Valued Customer"}},

In accordance with our Data Processing Agreement with you, we are writing to notify you that we intend to engage a new sub-processor.

**New sub-processor details:**

| Field | Detail |
|-------|--------|
| Sub-processor name | {{NEW_SUBPROCESSOR_NAME}} |
| Parent company | {{PARENT_COMPANY}} |
| Service provided | {{SERVICE_DESCRIPTION}} |
| Data categories | {{DATA_CATEGORIES_PROCESSED}} |
| Data location | {{DATA_LOCATION}} |
| Transfer mechanism | {{TRANSFER_MECHANISM}} |
| DPA in place | Yes — {{DPA_REFERENCE}} |
| Intended effective date | {{EFFECTIVE_DATE}} |

**Why we are adding this sub-processor:**
{{BUSINESS_REASON — e.g. "To provide improved infrastructure reliability for our EU region"}}

**Your right to object:**
Under our Data Processing Agreement, you have the right to object to this change within **{{OBJECTION_WINDOW}} days** of this notice (i.e. by **{{OBJECTION_DEADLINE}}**).

To raise an objection, please contact: {{DPO_OR_PRIVACY_EMAIL}}

Please note that if you object and we are unable to resolve the objection, either party may terminate the services agreement in accordance with its terms.

If we do not hear from you by {{OBJECTION_DEADLINE}}, we will proceed with engaging {{NEW_SUBPROCESSOR_NAME}} as a sub-processor from {{EFFECTIVE_DATE}}.

Our updated sub-processor list is available at: {{TRUST_CENTRE_URL}}

If you have any questions, please contact our Data Protection Officer:
{{DPO_NAME}}
{{DPO_EMAIL}}

Yours sincerely,
{{SIGNATORY_NAME}}
{{SIGNATORY_TITLE}}
{{ORGANISATION_NAME}}

---

## 4. Template C — Objection Window Tracker

*Maintain this log for every sub-processor change. Keep for audit purposes.*

| # | Notification ref | New sub-processor | Notification sent date | Objection deadline | Customers notified | Objections received | Objection outcome | Effective date | Status |
|---|-----------------|------------------|-----------------------|-------------------|-------------------|--------------------|--------------------|---------------|--------|
| 1 | {{REF}} | {{NAME}} | {{DATE}} | {{DATE}} | {{N}} customers | {{N}} / 0 | {{Resolved / Terminated}} | {{DATE}} | Open / Closed |

**Handling an objection:**

1. Acknowledge the objection in writing within 5 business days
2. Assess whether the objection can be resolved (e.g. offer alternative sub-processor, data isolation, additional safeguards)
3. If resolvable: document the resolution and continue
4. If not resolvable: invoke DPA termination clause; agree transition timeline
5. Record outcome in the tracker above

---

## 5. Code Scan Patterns — Detecting Undeclared Sub-Processors

When scanning the codebase, check for outbound data flows to domains not on the approved sub-processor list:

```
# New API endpoints receiving personal data
Search for: fetch(, axios.post(, axios.get(, http.request(, requests.post(
Extract all URLs and domains. Cross-reference against approved sub-processor list.

# New environment variables pointing to external services
Search for: .env* files — new API keys or base URLs not previously seen

# New SDK imports
Search for: import * from, require(' — new package names in package.json / requirements.txt

# Webhook / callback destinations
Search for: webhook_url, callback_url, notification_url — check destination domains

# Configuration-driven integrations
Search for: integrations:, providers:, connectors: in config files
```

### Automated detection rule (for CI/CD)

If a CI pipeline exists, suggest adding a check:

```bash
# Extract all external domains from fetch/axios calls
grep -rE "(fetch|axios)\(['\"]https?://[^'\"]+['\"]" src/ \
  | grep -oE "https?://[^/'\"]+[/'\"']" \
  | sort -u \
  | diff - approved-subprocessors.txt
# Exit 1 if new domains appear — triggers sub-processor review
```

---

## 6. Gap Analysis Additions

Add a **Sub-Processor Management** section to the gap analysis:

| Check | Severity | Finding | Recommendation |
|-------|----------|---------|----------------|
| Approved sub-processor list maintained | 🔴 Critical | [finding] | [recommendation] |
| All sub-processors have signed DPA | 🔴 Critical | | |
| Trust Centre / list published for customers | 🟠 High | | |
| Customer notification sent for all sub-processor changes | 🔴 Critical | | |
| Objection window honoured | 🔴 Critical | | |
| Objection log maintained | 🟡 Medium | | |
| New sub-processors detectable in CI/CD | 🟡 Medium | | |
| Sub-processor sub-processors documented | 🟡 Medium | | |
