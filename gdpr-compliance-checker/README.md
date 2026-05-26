# gdpr-compliance-checker

> Autonomous GDPR audit from a codebase scan — produces an article-by-article
> gap analysis and a pre-filled document pack (DPA, ROPA, DPIA, LIA, breach
> response, and more).
> Powered by [Sprinto](https://sprinto.com).

[![Sprinto](https://sprinto.com/wp-content/uploads/2025/11/sprinto-site-logo-dark.png)](https://sprinto.com)

---

## Install

```bash
# Using npx (Node.js)
npx skills add gosprinto/compliance-skills/gdpr-compliance-checker

# Using Claude Code directly
claude skills add gosprinto/compliance-skills/gdpr-compliance-checker
```

---

## What It Does

Triggered by any GDPR-adjacent phrasing — "are we GDPR compliant?", "DPA",
"ROPA", "data privacy audit", "ICO", "CNIL" — and runs end-to-end without
structured input from the user:

1. **Scans** the codebase for PII collection, storage, and sharing
2. **Researches** every third-party processor found (DPA, sub-processors,
   data regions, transfer mechanism) in parallel
3. **Assesses** all 99 GDPR articles + ePrivacy + member-state supplements +
   sector overlays + EU AI Act (if AI/ML detected)
4. **Generates** a full document pack pre-filled with company details, processors,
   data categories, and lead supervisory authority
5. **Exports** everything as `.docx` (recommended), `.xlsx`, or `.pdf`

---

## Sample Report

A real example output is bundled with the skill so you can see exactly what
the audit produces before running it:

📄 [`sample/gdpr-gap-analysis-acme-2025-05-25.docx`](./sample/gdpr-gap-analysis-acme-2025-05-25.docx)

The sample covers the full report structure: cover page with compliance posture
scorecard, 15-domain dashboard, domain-by-domain findings (what's wrong / what
to do), remediation roadmap, the complete 99-article appendix, PII inventory,
and processor research table.

---

## Document Templates Generated

Every run produces a gap analysis report **plus** a tailored set of operational
documents — pre-filled with company name, DPO contact, lead supervisory authority,
and processor details inferred from the codebase. Unknowns are flagged as
`[TO CONFIRM]` or `[TO DEFINE]` rather than fabricated.

| Document | When generated | Contents |
|---|---|---|
| **Gap analysis report** | Always | Cover, 15-domain dashboard, domain findings, roadmap, 99-article appendix, PII inventory, processor research |
| **DPA** (Data Processing Agreement) | Always | One per major processor or master DPA with processor-specific schedules |
| **ROPA** (Record of Processing Activities) | Always | One row per processing activity — legal basis, data categories, processors, retention |
| **LIA** (Legitimate Interest Assessment) | If Art. 6(1)(f) basis detected | Three-part test (purpose → necessity → balancing) per activity — analytics, fraud prevention, IT security, B2B marketing, internal admin |
| **DPIA** (Data Protection Impact Assessment) | If high-risk processing detected | Necessity/proportionality assessment + risk table for special category data, systematic monitoring, automated decisions, biometrics, profiling, AI/ML, children's data |
| **Breach response pack** | Always | Response checklist, supervisory authority notification template, data subject notification template, internal breach log — pre-filled with the 72-hour notification endpoint |
| **Access governance pack** | Always | Access register pre-populated with system tiers (DB, cloud console, admin panel, support tools, analytics), quarterly access review record, privileged access log |
| **Training & awareness pack** | Always | Training session record, individual sign-off sheet, organisation-wide policy acknowledgement log (CSV), training completion summary for DPO/board reporting |
| **Sub-processor register + notification** | Always | Approved sub-processor list + customer notification template, pre-filled with every processor detected in the scan |

---

## Coverage

### Regulations
GDPR (all 99 articles) · ePrivacy Directive · EU AI Act (if AI/ML detected) ·
27 EU/UK supervisory authorities · Member-state supplements

### Sector overlays
Healthcare (MDR) · Fintech (PSD2, DORA) · Adtech · Children's services
(Children's Code, DSA)

### 15 standard compliance domains
Lawful basis & consent · Cookie & ePrivacy · Transparency & privacy notice ·
Special category data · Data subject rights · Retention & deletion ·
Processors & ROPA · International transfers · Security & logging ·
Access governance · Privacy by design · DPIA · Breach handling ·
Governance & DPO · Training & awareness

### 3 conditional domains (added when triggered)
Automated decisions & AI · Employment & HR data · Sector-specific compliance

---

## Output Formats

| Format | Best for |
|---|---|
| `.docx` ★ | Legal teams, editing, sharing with counsel, internal sign-off — preserves coloured domain blocks, two-column findings layout, styled tables |
| `.xlsx` | Filtering and sorting findings, tracking remediation in a spreadsheet, ops teams — one sheet per section, conditional formatting on severity |
| `.pdf` | Sending to regulators, external auditors, board members — same content as `.docx`, not editable |

Each format relies on the corresponding output skill (`/mnt/skills/public/docx`,
`/xlsx`, `/pdf`). The skill checks which are installed before starting and
defaults to `.docx` if available.

---

## Example Triggers

The skill fires on any of these — no explicit invocation needed:

```
"Are we GDPR compliant?"
"Run a GDPR audit on this repo"
"I need to check our data privacy"
"Help me get audit-ready"
"Generate a DPA for our processors"
"Build me a ROPA"
"Check our cookie consent compliance"
"Do we need a DPIA?"
```

---

## File Structure

```
gdpr-compliance-checker/
├── SKILL.md                                       ← entry point + workflow
├── sample/
│   └── gdpr-gap-analysis-acme-2025-05-25.docx    ← example output report
└── references/
    ├── pii-patterns.md                            ← codebase PII signatures
    ├── gdpr-articles.md                           ← 99-article gap signals
    ├── member-state-supplements.md                ← national overlays
    ├── eprivacy-checklist.md                      ← cookies, email, tracking
    ├── consent-audit.md                           ← consent record quality
    ├── sector-overlays.md                         ← healthcare, fintech, adtech, children's
    ├── ai-vendor-checklist.md                     ← AI/ML processor checks
    ├── supervisory-authorities.md                 ← 27 EU/UK authority routing
    ├── dpa-template.md
    ├── lia-template.md
    ├── dpia-template.md
    ├── ropa-template.md
    ├── breach-response-pack.md
    ├── access-governance-checklist.md
    ├── training-awareness-template.md
    └── subprocessor-notification-template.md
```

---

## Output Quality Rules

- Never fabricates legal basis or retention — unknowns are marked `[TO CONFIRM]`
  / `[TO DEFINE]`
- Never claims a company is "GDPR compliant" — uses "partially compliant",
  "significant gaps identified", or "no critical gaps found pending legal review"
- Flags special category data (Art. 9) prominently on the cover and at the top
  of relevant findings
- Always notes that legal review by a qualified data protection professional is
  recommended before relying on the outputs

---

*Built by [Sprinto](https://sprinto.com?utm_source=Claude&utm_medium=skill&utm_campaign=gdpr) — compliance automation for fast-growing companies.*
