# Sector-Specific Overlays

GDPR intersects with sector regulations that impose stricter or additional requirements. Identify the company's sector from the codebase and load the relevant sections.

**How to identify sector from codebase**:
- Health fields, MDR references, prescription data → Healthcare
- Payment processing, KYC fields, AML references, PSD2 → Financial services
- Age gates, parental consent, child-friendly UI → Children's services
- Employee records, payroll, time tracking → HR tech
- Location data at scale, smart devices, sensor data → IoT / smart infrastructure
- AI model training on personal data, automated decisions → AI / ML

---

## Table of Contents
1. [Healthcare and health tech](#1-healthcare-and-health-tech)
2. [Financial services and fintech](#2-financial-services-and-fintech)
3. [Children's services](#3-childrens-services)
4. [HR tech and employment](#4-hr-tech-and-employment)
5. [Advertising technology](#5-advertising-technology)
6. [AI and machine learning](#6-ai-and-machine-learning)
7. [Telecommunications](#7-telecommunications)
8. [Sector gap analysis additions](#8-sector-gap-analysis-additions)

---

## 1. Healthcare and Health Tech

### Relevant regulations (alongside GDPR)
- **EU Medical Device Regulation (MDR 2017/745)** — software as a medical device (SaMD)
- **EU In Vitro Diagnostic Regulation (IVDR 2017/746)**
- **Clinical Trials Regulation (EU No 536/2014)**
- **France**: Hébergeur de Données de Santé (HDS) certification required for hosting health data
- **Germany**: Digital Health Applications (DiGA) regulation for prescription apps
- **UK**: Data Security and Protection Toolkit (DSPT) for NHS-related processing

### Key additional requirements

**Data classification**: Health data is special category data (Art. 9). Processing requires:
- Explicit consent (Art. 9(2)(a)), OR
- Healthcare provision (Art. 9(2)(h)) + professional secrecy obligation, OR
- Public health (Art. 9(2)(i)), OR
- Research (Art. 9(2)(j)) + appropriate safeguards

**DPIA mandatory**: Large-scale health data processing always requires a DPIA (Art. 35(3)(b)).

**Security standards**: Health data demands higher security standards. Check for:
- Encryption at rest (AES-256 minimum)
- Role-based access (clinicians vs admin vs billing)
- Audit trails on all access to patient records
- Pseudonymisation of research data

**France (HDS)**: Any entity hosting health data (including cloud infrastructure for a health app) must be HDS-certified or use an HDS-certified host. Major certified hosts: AWS (with French healthcare option), Microsoft Azure France, OVHcloud. Non-certified hosting = serious violation.

**Retention**: Medical records have statutory retention periods that override standard GDPR minimisation — e.g. UK NHS records: 10 years post last treatment; varies significantly by country and record type.

**Gap signals**:
- Health fields detected in schema (diagnosis, medication, health_status, prescription)
- No DPIA found for health data processing
- No mention of HDS certification (France)
- No mention of professional secrecy obligations
- Health data shared with third-party analytics

---

## 2. Financial Services and Fintech

### Relevant regulations
- **PSD2 (Payment Services Directive 2)** — open banking, strong customer authentication
- **MiFID II** — financial instrument data, record keeping
- **AML Directive (AMLD)** — anti-money laundering, KYC, transaction monitoring
- **DORA (Digital Operational Resilience Act)** — ICT risk management from Jan 2025
- **UK FCA rules** — Consumer Duty, financial promotions, data handling

### Key additional requirements

**KYC / AML data retention**: AML regulations require retention of customer due diligence (KYC) data for **5 years** after the end of the business relationship. This overrides GDPR minimisation — retention is a legal obligation.

**Transaction records (MiFID II)**: Must retain transaction records for **5 years** (certain records 7 years). These are legal obligations.

**PSD2 consent**: Open banking access to payment accounts requires explicit, specific consent. Third-party providers (TPPs) must have clear authorisation.

**Strong Customer Authentication (SCA)**: PSD2 requires SCA for electronic payments — biometric or two-factor. Gap signal: payment flow without SCA.

**DORA (from Jan 2025)**:
- ICT risk management framework required
- Major ICT incident reporting (within 4 hours initial notification, 72 hours intermediate, 1 month final)
- ICT third-party risk management (assess all critical ICT providers)
- Digital operational resilience testing

**Credit data**: Strict rules on credit scoring and automated credit decisions — must inform applicant of automated decision, provide human review on request (overlaps Art. 22 GDPR).

**BNPL (Buy Now Pay Later)**: Increasingly regulated — consumer credit rules may apply; data about creditworthiness is sensitive.

**Gap signals**:
- payment, card_number, iban, sort_code fields without encryption at rest
- No 5-year retention for KYC/AML data
- Automated credit/risk decisions without Art. 22 compliance
- PSD2 open banking access without specific consent
- Post-DORA: no ICT incident response plan

---

## 3. Children's Services

### Relevant regulations
- **GDPR Art. 8** — consent for children
- **EU Better Internet for Kids / EDPB guidance on children**
- **UK Age Appropriate Design Code (Children's Code)** — applies to any service "likely to be accessed" by under-18s
- **US COPPA** — if any US children's data processed (note: outside GDPR scope but relevant for global products)

### Key requirements

**UK Children's Code (15 standards)**:
1. Best interests of the child — default to high privacy settings
2. Data protection impact assessments — DPIA required
3. Age appropriate application — tailor privacy information to child's age
4. Transparency — clear, child-appropriate privacy information
5. Detrimental use of data — no use of children's data in ways detrimental to their wellbeing
6. Policies and community standards — uphold stated policies
7. Default settings — privacy settings default to high
8. Data minimisation — collect minimum necessary
9. Data sharing — no data sharing beyond what is necessary
10. Geolocation — off by default
11. Parental controls — provide tools for parental oversight
12. Profiling — off by default; only on with compelling reason
13. Nudge techniques — no nudges that encourage children to share more data
14. Connected toys and devices — meet the Code's standards
15. Online tools — provide obvious, accessible privacy controls

**Gap signals**:
- No age verification or age estimation
- Geolocation on by default
- Profiling features without separate consent
- Nudge UX patterns (e.g. "Your friends have shared their location!")
- No child-appropriate privacy notice
- Parental consent mechanism absent

---

## 4. HR Tech and Employment

### Relevant regulations
- **GDPR Art. 88** — member states may provide more specific rules for employment context
- **National employment law** — varies significantly; see member-state-supplements.md
- **Germany**: §26 BDSG, works council requirements
- **France**: Labour Code employee monitoring rules
- **Spain**: Digital rights in employment (LOPDGDD Arts. 87–91)

### Key requirements

**Legal basis for employee data**:
- Performance management, payroll, benefits: Contract (Art. 6(1)(b))
- Legal obligations (tax, social security): Legal obligation (Art. 6(1)(c))
- Occupational health: Separate legal basis under Art. 9(2)(b) + national law
- Monitoring: Legitimate interests — but requires strict necessity test; many national laws add co-determination requirements

**Consent in employment**:
- Consent is generally **not valid** as a legal basis for employee data processing because of the power imbalance — employees cannot freely refuse
- Exception: genuinely optional benefits (e.g. optional health screening programme)

**Employee monitoring**:
- Email/internet monitoring: Must be proportionate, transparent, and notified in advance; works council/union consultation required in many states
- Location tracking: Strict necessity test; not permitted for home workers in most states without specific justification
- Keystroke/screen monitoring: Highly invasive; difficult to justify
- BYOD: Personal device policies create complexity — work data vs personal data

**HR data retention**:
- Recruitment data (unsuccessful candidates): typically 6 months–1 year max
- Employee records (current): duration of employment + statutory minimum
- Payroll records: typically 7 years (tax obligations)
- Disciplinary records: typically 12 months after completion of proceedings
- Time and attendance: typically 3–5 years

**Gap signals**:
- Employee monitoring features without transparency notice or works council reference
- Location tracking of remote employees
- Candidate data retained beyond 6 months without explicit consent
- No separate policy for employment data processing
- Consent used as legal basis for employment processing

---

## 5. Advertising Technology

### Relevant regulations
- **ePrivacy Directive** — consent for tracking (see eprivacy-checklist.md)
- **IAB TCF** — challenged in Belgium (GBA decision 2022); under scrutiny across EU
- **Digital Services Act (DSA)** — large platforms; targeting restrictions

### Key requirements

**Real-time bidding (RTB)**:
- Sharing user IDs and profile data in RTB auctions requires GDPR-compliant consent — high risk
- Belgian DPA found IAB TCF's consent mechanism systematically non-compliant (2022) — significant if TCF is used

**Profiling for advertising**:
- Profiling using special category data (health, political views, etc.) prohibited without explicit consent
- Profiling of children for advertising: prohibited

**Digital Services Act (DSA) — very large platforms (>45M EU users)**:
- Must provide option to use service without targeted advertising
- Cannot target based on sensitive attributes (health, religion, sexuality, political views)
- Must provide transparent information about recommendation systems

**Gap signals**:
- ad_profile, interest_segments, lookalike_audience fields
- User profiling for ad targeting without Art. 9 check on inferred sensitive categories
- IAB TCF integration without awareness of Belgian ruling
- No opt-out from profiling for advertising

---

## 6. AI and Machine Learning

### Relevant regulations
- **EU AI Act** — fully applicable from 2026 (tiered by risk)
- **GDPR Art. 22** — automated decision-making
- **EDPB Guidelines on automated decision-making**

### Key requirements

**EU AI Act risk tiers**:
- **Unacceptable risk** (prohibited): Real-time biometric surveillance in public spaces, social scoring by governments, subliminal manipulation
- **High risk**: Biometric categorisation, critical infrastructure management, education/employment decisions, essential services access, law enforcement, migration management, administration of justice
- **Limited risk**: Chatbots, deepfakes — transparency obligations only
- **Minimal risk**: Spam filters, AI in games — no obligations

**GDPR Art. 22 — automated decision-making**:
- Automated decisions with legal or significant effects require: human oversight option, ability to contest, explanation of the decision
- Profiling that infers sensitive attributes (even without explicit data) = Art. 9 risk

**Training data**:
- Training ML models on personal data = processing personal data; requires legal basis
- Legitimate interests commonly used — but requires LIA
- Data minimisation: consider pseudonymisation or synthetic data alternatives

**Gap signals**:
- ML model training on user data without documented legal basis
- Automated recommendations or scoring with significant effects on users
- No human review option for automated decisions
- Biometric data used in ML (face recognition, voice ID)
- Model infers sensitive attributes (political views, health, sexuality)

---

## 7. Telecommunications

### Relevant regulations
- **ePrivacy Directive** — most directly applicable to telecoms
- **Specific national telecoms data retention laws** — many EU states require mandatory retention of traffic data for law enforcement

### Key requirements

**Traffic and location data**:
- Call records, SMS metadata, IP connection data: may only be processed for billing, quality of service, security — not for profiling without consent
- Location data requires consent or strict necessity
- Law enforcement data retention: national laws vary post-CJEU rulings limiting blanket retention

---

## 8. Sector Gap Analysis Additions

After the standard gap analysis, add a **Sector-Specific Findings** section:

```
SECTOR IDENTIFIED: [healthcare / fintech / children's / HR / adtech / AI / other]
ADDITIONAL REGULATIONS TRIGGERED:
  - [Regulation]: [specific requirement] → [Severity] → [Finding] → [Recommendation]

SECTOR RISK SUMMARY:
  Critical sector gaps: N
  High sector gaps: N
  Sector compliance posture: [Not Started / Partial / Mostly Compliant / Audit-Ready]
```
