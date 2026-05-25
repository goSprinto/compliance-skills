# Member State GDPR Supplements

GDPR is a minimum standard. Each member state adds national rules via their implementing legislation. This file covers the most commercially significant jurisdictions.

**How to use**: Identify the company's EU establishment (or lead supervisory authority jurisdiction) and where their EU users are primarily located. Load the relevant sections below and add findings to the gap analysis.

---

## Table of Contents
1. [Germany (BDSG)](#1-germany-bdsg)
2. [France (CNIL)](#2-france-cnil)
3. [Ireland (DPC)](#3-ireland-dpc)
4. [Netherlands (AP)](#4-netherlands-ap)
5. [Sweden (IMY)](#5-sweden-imy)
6. [Spain (AEPD)](#6-spain-aepd)
7. [Italy (Garante)](#7-italy-garante)
8. [Poland (UODO)](#8-poland-uodo)
9. [Denmark (Datatilsynet)](#9-denmark-datatilsynet)
10. [Belgium (APD/GBA)](#10-belgium-apdgba)
11. [United Kingdom (ICO — UK GDPR)](#11-united-kingdom-ico--uk-gdpr)
12. [How to determine lead supervisory authority](#12-how-to-determine-lead-supervisory-authority)

---

## 1. Germany (BDSG)

**Implementing law**: Bundesdatenschutzgesetz (BDSG 2018)
**Supervisory authority**: Federal: BfDI; State-level: 16 Landesdatenschutzbehörden (varies by company's registered state)

### Key additions beyond GDPR

**Employee data (§26 BDSG)**
- Processing employee data requires a legal basis specifically under §26 BDSG (not just Art. 6 GDPR)
- Employee monitoring (keyloggers, email scanning, location tracking) requires either collective agreement (Betriebsvereinbarung) or demonstrated concrete suspicion of a criminal offence
- Gap signal: Any employee monitoring features, HR system, or `employees` table in schema

**Works Council involvement**
- Employers must consult works councils (Betriebsrat) before introducing IT systems that monitor employee performance or behaviour
- Gap signal: SaaS HR or workforce management product; codebase has employee-facing monitoring features

**DPO mandatory threshold**
- Germany requires a DPO if 20+ persons regularly process personal data (stricter than GDPR's risk-based threshold)
- Gap signal: Company has 20+ staff and no DPO contact found

**Credit reporting (§31 BDSG)**
- Stricter rules on using credit scoring to affect individuals — must inform the person

**Consent for cookies**
- German courts (Planet49 judgment) require explicit opt-in for analytics cookies — pre-ticked boxes are invalid
- The Telecommunications and Telemedia Data Protection Act (TTDSG 2021) governs cookie consent in Germany specifically

**Video surveillance**
- §4 BDSG requires signage and minimisation for CCTV; separate rules for tenant buildings

**Retention**: Germany has specific retention periods for commercial records — 10 years for financial data, 6 years for business correspondence.

### Severity flags
- 🔴 Employee monitoring without Betriebsvereinbarung
- 🟠 No DPO despite 20+ data-processing staff
- 🟠 Analytics cookies without explicit opt-in (TTDSG)

---

## 2. France (CNIL)

**Implementing law**: Loi Informatique et Libertés (amended 2018, 2021)
**Supervisory authority**: CNIL (Commission Nationale de l'Informatique et des Libertés)

### Key additions beyond GDPR

**Cookies and trackers (CNIL Guidelines 2020)**
- Cookie walls (denying service if cookies refused) are generally prohibited
- Consent must be as easy to withdraw as to give — requires one-click refuse option on banner
- Legitimate interest cannot be used for advertising cookies in France
- Gap signal: Cookie banner without a "Refuse all" option at same prominence as "Accept all"

**Employee monitoring**
- Employees must be individually informed before any monitoring system is deployed
- Must be declared to employee representatives
- Gap signal: Any employee tracking, productivity monitoring, or communications scanning

**Biometrics at work (CNIL deliberation)**
- Biometric timekeeping (fingerprint scanners) requires CNIL authorisation or employee consent — consent is generally not valid in employment context
- Gap signal: Biometric fields in schema for employees

**Health data**
- Hosting of health data requires certification as a Hébergeur de Données de Santé (HDS) — a formal French certification
- Gap signal: Health-related fields; any healthtech or medtech product serving French users

**Right to object to direct marketing**
- French law provides a national opt-out register (Bloctel) for telephone marketing
- Email marketing requires prior consent (double opt-in strongly recommended by CNIL)

**CNIL enforcement pattern**
- CNIL is one of Europe's most active enforcers — particularly aggressive on cookies (Google €150M, Facebook €60M fines in 2022)

### Severity flags
- 🔴 Health data without HDS certification
- 🔴 Cookie consent without equal-prominence refuse option
- 🟠 Biometric employee data without individual informed consent

---

## 3. Ireland (DPC)

**Implementing law**: Data Protection Act 2018
**Supervisory authority**: Data Protection Commission (DPC)

### Key additions beyond GDPR

**Lead authority for major tech companies**
- Ireland is the EU establishment (and therefore lead supervisory authority) for: Meta, Google, Apple, Twitter/X, LinkedIn, Airbnb, Salesforce, HubSpot, Dropbox, and many others
- If the company has an Irish establishment and processes EU-wide data, the DPC is the lead authority under the one-stop-shop mechanism

**Children's data**
- Ireland sets the digital age of consent at **16** (not 13 as in some other states)
- Gap signal: Any service that minors may use; age verification absent

**DPC enforcement**
- DPC has become significantly more active post-2021: record €1.2B fine against Meta (2023), €405M against Instagram
- Cross-border complaints are channelled through DPC — expect scrutiny if Irish establishment

**Specific DPC guidance on**:
- Lawful basis for processing (DPC has published detailed guidance diverging slightly from EDPB on legitimate interests)
- Children's data (Fundamentals for a Child-Oriented Approach to Data Processing)
- AI and automated decision-making (ongoing guidance)

---

## 4. Netherlands (AP)

**Implementing law**: Uitvoeringswet AVG (UAVG)
**Supervisory authority**: Autoriteit Persoonsgegevens (AP)

### Key additions beyond GDPR

**Legitimate interests — stricter interpretation**
- AP has taken a strict line on legitimate interests for online tracking/advertising — it does not accept LI as a basis for third-party advertising tracking
- Gap signal: Analytics or ad-tracking SDKs with LI as stated basis

**BSN (citizen service number)**
- Processing Dutch national ID numbers (BSN) is only permitted in specific, legislated circumstances — not for general business use
- Gap signal: national_id or ssn fields in schema

**Children's services**
- AP has specific guidance for services directed at children — age-appropriate design, no profiling
- Notable enforcement: AP fined TikTok €750K for Dutch children's data

**AP enforcement pattern**
- Historically under-resourced but increasingly active; strong on children's data and online tracking

---

## 5. Sweden (IMY)

**Implementing law**: Dataskyddslagen (2018)
**Supervisory authority**: Integritetsskyddsmyndigheten (IMY)

### Key additions

**Google Analytics enforcement (2022–2023)**
- IMY issued landmark decisions finding Google Analytics illegal under GDPR for Swedish companies due to US data transfers
- Gap signal: Google Analytics (gtag, GA4) detected in codebase — Swedish users = potential violation even with consent

**Employee monitoring**
- Strong co-determination tradition; employer must negotiate with unions before implementing monitoring systems
- Swedish Work Environment Authority has separate rules on workplace surveillance

**Credit information**
- Strict rules under Kreditupplysningslagen on processing credit/financial data

---

## 6. Spain (AEPD)

**Implementing law**: Ley Orgánica 3/2018 (LOPDGDD)
**Supervisory authority**: Agencia Española de Protección de Datos (AEPD)

### Key additions

**Digital rights in employment (Arts. 87–91 LOPDGDD)**
- Employees have explicit statutory rights: right to digital disconnection, right to privacy on work devices, right to privacy against digital employee monitoring
- Gap signal: Employee monitoring, MDM (mobile device management), communications scanning

**Age of consent: 14** (not 16) for digital services

**Whistleblowing channels**
- Spain requires large companies to maintain whistleblowing channels with specific data protection safeguards (Ley 2/2023)

**AEPD enforcement**
- One of Europe's most prolific enforcers by volume of fines — fines frequent but often smaller

**Right of erasure**
- AEPD has been particularly active enforcing the right to be forgotten in search results

---

## 7. Italy (Garante)

**Implementing law**: Codice in materia di protezione dei dati personali (D.Lgs. 196/2003, as amended by D.Lgs. 101/2018)
**Supervisory authority**: Garante per la protezione dei dati personali

### Key additions

**AI and ChatGPT**
- Garante temporarily banned ChatGPT in Italy (March 2023) citing GDPR violations — age verification, transparency, legal basis
- Gap signal: Any AI features processing Italian user data without explicit legal basis and age verification

**Telemarketing**
- Strict rules; national opt-out register (Registro delle Opposizioni) must be checked before marketing calls/SMS

**Employee monitoring (Art. 4 Statuto dei Lavoratori)**
- Remote monitoring of employees requires union agreement or government authorisation
- Gap signal: Employee monitoring features

**Cookies**
- Garante issued updated cookie guidelines (2021) — requires same-prominence refusal; scroll-as-consent invalid

**Data breach**
- Italy requires breach notification within 72 hours, but Garante has been strict on the quality of breach notifications — vague notifications have attracted fines

---

## 8. Poland (UODO)

**Implementing law**: Ustawa o ochronie danych osobowych (2018)
**Supervisory authority**: Urząd Ochrony Danych Osobowych (UODO)

### Key additions

**DPO registration**
- Poland requires DPO details to be registered with UODO (unlike most member states)
- Gap signal: DPO exists but not registered with UODO

**PESEL (national ID)**
- Processing PESEL numbers has specific restrictions — not for general business use

**UODO enforcement**
- Active on healthcare and public sector; increasing activity on private sector

---

## 9. Denmark (Datatilsynet)

**Implementing law**: Databeskyttelsesloven (2018)
**Supervisory authority**: Datatilsynet

### Key additions

**CPR number (national ID)**
- Processing CPR numbers requires explicit legal basis and is restricted
- Gap signal: national_id fields for Danish users

**Employee data**
- Collective agreements may expand or restrict employee data processing rights

**Datatilsynet enforcement**
- Issued significant fines on Google Analytics (coordinated with other DPAs) and Meta pixel

---

## 10. Belgium (APD/GBA)

**Implementing law**: Wet van 30 juli 2018 betreffende de bescherming van natuurlijke personen
**Supervisory authority**: Gegevensbeschermingsautoriteit (GBA) / Autorité de Protection des Données (APD)

### Key additions

**IAB TCF enforcement**
- GBA issued a landmark decision against IAB Europe's Transparency and Consent Framework (TCF) (2022) — affects any publisher or ad-tech using TCF for cookie consent
- Gap signal: IAB TCF consent string in codebase or cookie consent implementation

**Identification numbers**
- National register number (RRN) processing is restricted by specific legislation

---

## 11. United Kingdom (ICO — UK GDPR)

**Implementing law**: UK GDPR + Data Protection Act 2018 (DPA 2018)
**Supervisory authority**: Information Commissioner's Office (ICO)

> **Post-Brexit status**: UK GDPR mirrors EU GDPR closely but has diverged and will continue to diverge. UK is an adequate country under EU GDPR. EU is an adequate country under UK GDPR (adequacy decision expires 2025 — check current status).

### Key differences from EU GDPR

**Transfer mechanism: IDTA (not SCCs)**
- Transfers from UK to non-adequate countries use the International Data Transfer Agreement (IDTA), not EU SCCs
- EU SCCs signed after 21 March 2022 are not valid for UK transfers — need UK Addendum or IDTA
- Gap signal: UK data transferred to US processors using EU SCCs only (without UK Addendum)

**Legitimate interests**
- UK ICO has published more permissive guidance on legitimate interests than EDPB — somewhat more business-friendly
- But UK courts apply the same balancing test

**Age Appropriate Design Code (Children's Code)**
- Applies to any online service likely to be accessed by under-18s (wider than EU)
- Requires: privacy by default, no profiling without explicit consent, no nudge techniques, geolocation off by default
- Gap signal: Consumer product with no age-gating or Children's Code assessment

**ICO registration**
- Most UK data controllers must register with ICO and pay a data protection fee (£40–£2,900/year)
- Gap signal: UK establishment with no ICO registration found

**ePrivacy: PECR**
- Privacy and Electronic Communications Regulations (PECR) governs cookies, email/SMS marketing, and cold calls in the UK — separate from UK GDPR
- PECR requires opt-in consent for non-essential cookies; soft opt-in for email marketing to existing customers

**UK data reform**
- Data (Use and Access) Act 2025 introduces further divergence — smart data, digital verification, AI governance. Check ICO guidance for current position.

**Fines**
- ICO maximum fine: £17.5M or 4% global turnover (mirrors EU)
- ICO has historically been less aggressive than some EU DPAs but this is changing

### Severity flags (UK-specific)
- 🔴 IDTA/UK Addendum missing for UK→US transfers
- 🟠 No ICO registration for UK controller
- 🟠 Children's Code non-compliance for consumer services
- 🟠 PECR non-compliance (cookies/email)

---

## 12. How to Determine Lead Supervisory Authority

The **lead supervisory authority (LSA)** is the DPA where the company's **main EU establishment** is located. Main establishment = where central administration is, or where decisions about processing are made.

**Decision tree**:
1. Does the company have an EU establishment? → If No: No LSA; each member state DPA has jurisdiction for their residents' data.
2. Does the company have only one EU establishment? → That country's DPA is the LSA.
3. Multiple EU establishments? → LSA is where central administration is OR where decisions about cross-border processing are made.
4. No EU establishment but has a representative (Art. 27)? → The representative's country DPA may act; no formal LSA mechanism.

**For companies outside EU processing EU data**: Each member state DPA can act for their own residents. No one-stop-shop benefit. Must designate EU representative under Art. 27.

**Common LSA assignments**:
| Company type | Common LSA |
|-------------|-----------|
| US tech with Irish HQ | DPC (Ireland) |
| US tech with Luxembourg HQ | CNPD (Luxembourg) |
| UK-headquartered EU ops | Depends on EU office location |
| German company, EU-only ops | BfDI or relevant Landesbehörde |
| French company | CNIL |

---

## Gap Analysis Addition

After running the standard gap analysis, append a **Jurisdiction-Specific Findings** section:

```
JURISDICTION FINDINGS
Establishment: [country]
Lead supervisory authority: [DPA name + contact]
Key national additions triggered:
  - [Jurisdiction]: [specific rule] → [Severity] → [Finding] → [Recommendation]
```
