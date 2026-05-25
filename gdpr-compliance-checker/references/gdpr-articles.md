# GDPR Articles — Gap Analysis Reference

For each article: what it requires, what signals a gap, and what to recommend.
Severity ratings: 🔴 Critical (fines up to €20M/4% turnover) | 🟠 High (€10M/2%) | 🟡 Medium | 🔵 Informational

---

## Chapter I — General Provisions

### Art. 1 – Subject matter and objectives
🔵 Informational. Sets scope. No direct compliance gap.

### Art. 2 – Material scope
🔵 Check: Does the codebase process personal data of natural persons? If yes, GDPR applies. If processing is purely anonymous, GDPR may not apply — but verify anonymisation is genuine (not just pseudonymisation).

### Art. 3 – Territorial scope
🔵 **Gap signal**: App serves EU users (language options, EU payment methods, EU domain, EU pricing currency, EU-specific content). If yes, GDPR applies regardless of where company is based.

### Art. 4 – Definitions
🔵 Reference only. Use to confirm what counts as "personal data", "processing", "controller", "processor", "consent", "pseudonymisation", etc.

---

## Chapter II — Principles

### Art. 5 – Principles of processing
🔴 **Critical — most foundational article.**

Gap signals:
- No documented lawful basis for each processing activity
- Data collected beyond what's needed (violates data minimisation)
- No retention limits defined or enforced in code
- PII logged in application logs (violates integrity/confidentiality)
- No evidence of accuracy controls (e.g. email verification)

Remediation: Document a lawful basis for every processing purpose. Audit what's collected vs what's actually used. Add log scrubbing. Define and enforce retention policies.

### Art. 6 – Lawfulness of processing
🔴 **Critical.**

Gap signals:
- No consent mechanism found (no cookie banner, no opt-in flow)
- Consent is pre-ticked or bundled with T&Cs
- Processing for purposes beyond original collection (e.g. marketing re-use of support emails)
- No legitimate interest assessment (LIA) documented for LI-based processing
- Contract-based processing but no DPA with processors

Remediation: Map every processing purpose to one of: consent, contract, legal obligation, vital interests, public task, legitimate interest. Implement or audit consent flows.

### Art. 7 – Conditions for consent
🔴 **Critical.**

Gap signals:
- Consent not freely given (tied to service access)
- No timestamp/record of when consent was given
- No withdraw-consent mechanism
- Consent for minors not verified

Remediation: Store consent records with timestamp, IP, version of policy shown. Build withdrawal flow. Decouple consent from service access.

### Art. 8 – Child's consent (information society services)
🔴 **Critical if service targets or is likely used by under-16s (or under-13 in some member states).**

Gap signals:
- No age gate or age verification
- No parental consent mechanism
- Marketing directed at children

### Art. 9 – Special category data
🔴 **Critical — highest risk category.**

Gap signals (from PII scan): health fields, biometric fields, racial/ethnic origin, religious/political beliefs, sexual orientation, trade union membership, genetic data detected in schema or forms.

**AI inference gap signal**: AI/ML models in the codebase may infer special category data even when it is not explicitly collected — e.g. sentiment analysis inferring mental health state, behaviour patterns inferring sexuality or political views, facial recognition inferring ethnicity. Flag any AI feature that processes user-generated text or images for downstream inference. Load `references/ai-vendor-checklist.md` for the full AI-specific assessment.

Remediation: Special category data requires explicit consent (Art. 9(2)(a)) or another specific exception. Must document the exception used. Consider whether this data is actually necessary.

### Art. 10 – Criminal conviction data
🔴 Gap signal: fields for criminal records, DBS checks, offence history in schema. Requires specific authorisation.

---

## Chapter III — Rights of Data Subjects

### Art. 12 – Transparent information
🟠 **High.**

Gap signals:
- No privacy notice found (no `/privacy`, `/privacy-policy` route or link)
- Privacy notice not in plain language
- No response time commitment (must respond within 1 month)

### Art. 13 – Information to be provided (data collected directly)
🟠 **High.**

Gap signals:
- Privacy policy missing any of: controller identity, DPO contact, purposes, legal basis, recipients, retention, data subject rights, right to withdraw consent, right to lodge complaint with supervisory authority, whether provision is statutory/contractual

### Art. 14 – Information to be provided (data not collected directly)
🟠 Gap signal: Third-party data sources used (bought lists, scraped data, partner feeds) without informing data subjects.

### Art. 15 – Right of access (SAR)
🟠 **High.**

Gap signals:
- No subject access request process found (no `/account/data`, no SAR email, no admin tooling to export user data)
- No way to export all data held about a user

Remediation: Build data export endpoint. Document SAR process. Ensure 1-month response SLA.

### Art. 16 – Right to rectification
🟡 Gap signal: No way for users to edit their personal data in-app or by request.

### Art. 17 – Right to erasure ("right to be forgotten")
🔴 **Critical.**

Gap signals:
- No account deletion flow
- Deletion only soft-deletes (sets `deleted_at`) without actually purging PII
- Backups not considered in deletion process
- Third-party processors not notified on deletion (Mailchimp contact not removed, Segment user not suppressed)

Remediation: Implement hard delete or anonymisation on request. Build processor notification cascade. Document backup retention and purge schedule.

### Art. 18 – Right to restriction of processing
🟡 Gap signal: No mechanism to restrict processing for a user (e.g. freeze account without deletion).

### Art. 19 – Notification obligation
🟡 Must notify third parties of rectification/erasure. Gap if no processor cascade on data changes.

### Art. 20 – Right to data portability
🟡 Gap signal: No machine-readable export (JSON/CSV) of user data available on request.

### Art. 21 – Right to object
🟠 Gap signal: No opt-out from processing based on legitimate interest or direct marketing. Must be "readily accessible."

### Art. 22 – Automated decision-making / profiling
🟠 Gap signal: Automated decisions with legal/significant effects (credit scoring, loan decisions, ad targeting that affects access) without human review option or explanation.

---

## Chapter IV — Controller and Processor

### Art. 24 – Responsibility of the controller
🟠 Gap signal: No internal data protection policies, no evidence of staff training, no DPO designated (if required).

### Art. 25 – Data protection by design and by default
🟠 **High.**

Gap signals:
- Default settings expose maximum data (e.g. public profile by default)
- More data collected than necessary for stated purpose
- PII in URL parameters
- No encryption of PII at rest detected (no reference to encryption libs or DB encryption config)

### Art. 26 – Joint controllers
🟡 Gap signal: Two or more controllers jointly determining processing purposes without a joint controller agreement.

### Art. 27 – Representatives in the Union
🟡 Gap signal: Company outside EU/EEA processing EU data with no designated EU representative.

### Art. 28 – Processor
🔴 **Critical.**

Gap signals:
- Third-party processors found (from SDK scan) with no DPA in place
- DPA doesn't cover sub-processors
- No list of approved sub-processors maintained

Remediation: Sign DPAs with every processor. Check processor's sub-processor list. Generate DPA template from this skill.

### Art. 29 – Processing under controller/processor authority
🟡 Processors must only act on documented instructions.

### Art. 30 – Records of processing activities (ROPA)
🔴 **Critical — required for orgs with 250+ employees OR processing that poses risks.**

Gap signals:
- No ROPA exists
- ROPA not kept up to date

Remediation: Generate ROPA starter kit from this skill.

### Art. 31 – Cooperation with supervisory authority
🔵 Informational.

### Art. 32 – Security of processing
🔴 **Critical.**

Gap signals:
- No HTTPS enforced (HTTP routes found)
- Passwords stored as plaintext or weak hash (MD5/SHA1 without salt)
- No evidence of encryption at rest for PII
- PII in application logs
- No mention of access controls / RBAC
- No mention of penetration testing or security audits
- API keys/secrets committed to repo (check git history if possible)
- **Token storage**: auth tokens stored in `localStorage` or `sessionStorage` (XSS-vulnerable); JWT decoded client-side exposes payload; auth cookies missing `HttpOnly` and `Secure` flags
- **SIEM/observability PII leak**: PII fields sent to Datadog, Sentry, Elastic, Splunk via `setUser()`, log interpolation, or custom attributes without scrubbing

Remediation: Enforce TLS. Use bcrypt/argon2 for passwords. Encrypt PII fields. Scrub logs and observability tool payloads. Implement RBAC. Rotate any secrets found. Store auth tokens in HttpOnly cookies, not localStorage.

### Art. 33 – Notification of breach to supervisory authority
🟠 Gap signal: No incident response / breach notification process documented. Must notify within 72 hours of becoming aware.

### Art. 34 – Communication of breach to data subject
🟠 Gap signal: No user breach notification process.

### Art. 35 – Data protection impact assessment (DPIA)
🟠 Gap signal: High-risk processing detected (biometrics, health data, systematic monitoring, large-scale profiling) with no DPIA on record.

### Art. 36 – Prior consultation
🟡 If DPIA reveals high residual risk, must consult supervisory authority before processing.

### Art. 37 – Designation of DPO
🟠 Required if: public authority; core activity = large-scale systematic monitoring; core activity = large-scale special category data.

Gap signal: Any of the above detected, no DPO contact found in privacy policy.

### Art. 38 – Position of DPO
🔵 DPO must be independent, report to highest management, not be dismissed for performing tasks.

### Art. 39 – Tasks of DPO
🔵 Informational — DPO must advise, monitor compliance, cooperate with authority, be point of contact.

---

## Chapter V — International Transfers

### Art. 44 – General principle for transfers
🔴 **Critical.**

Gap signal: Data transferred outside EU/EEA (detected via cloud region config, processor headquarters, CDN nodes) without a lawful transfer mechanism.

### Art. 45 – Transfers on basis of adequacy decision
🟡 Adequacy countries (as of 2024): Andorra, Argentina, Canada (commercial), Faroe Islands, Guernsey, Israel, Isle of Man, Japan, Jersey, New Zealand, South Korea, Switzerland, UK, Uruguay, USA (Data Privacy Framework participants only).

Gap signal: Transfer to non-adequate country without SCCs or BCRs.

### Art. 46 – Transfers subject to appropriate safeguards
🔴 Most common mechanism: Standard Contractual Clauses (SCCs — updated June 2021). Also: Binding Corporate Rules (BCRs), approved codes of conduct, certification.

Gap signal: US processors (AWS, Google, Stripe, etc.) used without checking if they are Data Privacy Framework certified OR SCCs are in place.

### Art. 47 – Binding corporate rules
🟡 BCRs for intra-group transfers. Requires supervisory authority approval.

### Art. 48 – Transfers not authorised by Union law
🔴 Gap signal: Data disclosed to foreign courts/authorities without legal basis under GDPR.

### Art. 49 – Derogations for specific situations
🟡 Narrow exceptions: explicit consent, contract performance, vital interests, public register. Cannot be used routinely.

---

## Chapter VI — Independent Supervisory Authorities
Arts. 51–59: Supervisory authority structure. No direct technical compliance gap.

---

## Chapter VII — Cooperation and Consistency
Arts. 60–76: Regulator cooperation. No direct technical compliance gap.

---

## Chapter VIII — Remedies, Liability and Penalties

### Art. 77 – Right to lodge complaint
🟡 Gap signal: No reference in privacy policy to right to complain to supervisory authority (e.g. ICO in UK, CNIL in France, BfDI in Germany).

### Art. 78 – Right to effective judicial remedy
🔵 Informational.

### Art. 79 – Right to effective judicial remedy against controller/processor
🔵 Informational.

### Art. 80 – Representation of data subjects
🔵 Informational.

### Art. 81 – Suspension of proceedings
🔵 Informational.

### Art. 82 – Right to compensation and liability
🟠 Gap signal: No liability allocation in processor contracts.

### Art. 83 – General conditions for imposing fines
🔴 **Context only — the penalty article.** Tier 1 (€10M/2%): Arts. 8, 11, 25–39, 42, 43. Tier 2 (€20M/4%): Arts. 5–7, 9, 12–22, 44–49, 58(2).

### Art. 84 – Penalties
🔵 Member state discretion for additional penalties.

---

## Chapter IX — Provisions Relating to Specific Processing Situations

### Art. 85 – Processing and freedom of expression
🔵 Journalism / academic / artistic exemptions. Informational.

### Art. 86 – Processing and public access to official documents
🔵 Public bodies. Informational.

### Art. 87 – Processing of national identification number
🟡 Gap signal: National ID numbers processed without specific safeguards.

### Art. 88 – Processing in the employment context
🟡 Gap signal: Employee data (HR systems, monitoring) without employment-specific policy.

### Art. 89 – Safeguards for archiving/research/statistics
🔵 Informational — pseudonymisation required where possible.

### Art. 90 – Obligations of secrecy
🔵 Informational.

### Art. 91 – Existing data protection rules of churches
🔵 Informational.

---

## Chapter X — Delegated and Implementing Acts
Arts. 92–93: Commission powers. No direct compliance gap.

---

## Chapter XI — Final Provisions

### Art. 94 – Repeal of Directive 95/46/EC
🔵 Informational.

### Art. 95 – Relationship with Directive 2002/58/EC
🔵 ePrivacy Directive (cookie law) is separate but related. Gap signal: No cookie consent banner.

### Art. 96 – Relation to previously concluded agreements
🔵 Informational.

### Art. 97 – Commission reports
🔵 Informational.

### Art. 98 – Review of other Union legal acts
🔵 Informational.

### Art. 99 – Entry into force and application
🔵 Informational. GDPR applies since 25 May 2018.

---

*Output format for the article compliance table is defined in the main SKILL.md under Report Structure → Appendix A. Do not define output format in this reference file.*