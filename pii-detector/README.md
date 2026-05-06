# pii-detector

> Automatic PII detection and privacy compliance checks for US-based companies.
> Powered by [Sprinto](https://sprinto.com).

[![Sprinto](https://sprinto.com/wp-content/uploads/2025/11/sprinto-site-logo-dark.png)](https://sprinto.com)

---

## Install

```bash
claude skills add sprinto/claude-skills/pii-detector
```

---

## What It Does

Fires automatically — no trigger needed — during:

- **Planning & design discussions** — weaves PII considerations into architecture advice
- **Code generation** — checks before writing, generates correct code in one pass
- **Repo scan** — full audit with file + line references and a compliance summary

---

## Coverage

### Regulations
CCPA/CPRA · HIPAA · PCI-DSS · COPPA · GLBA · BIPA · FERPA · FTC Act

### Layers
| Layer | What It Catches |
|-------|----------------|
| Data models & schemas | Plaintext passwords, unencrypted SSN/health fields, missing deletedAt, no consent timestamp |
| Auth & sessions | PII in JWT payload, tokens in localStorage, no expiry, weak reset tokens, OTP storage |
| API layer | Full object responses, IDOR, GraphQL introspection, CORS misconfiguration, rate limiting |
| Frontend | PII in localStorage, cookie flags, consent gates, session replay capturing inputs |
| Data in transit | PII in URLs, hardcoded HTTP, webhook signature missing, HTTPS/HSTS not enforced |
| Testing & seeding | Real PII in seed files, fixtures, factories, snapshot files, prod DB in tests |
| Data lifecycle | No hard-delete, cascade gaps, bad anonymization, no export endpoint, no retention jobs |
| Legal & consent | Debug log level in prod, no cookie consent library, missing privacy policy / ToS routes |

---

## Modes

### Planning / Review
When you ask "how should I build X" or "review this schema" — PII observations
are woven into Claude's natural response. No separate report.

### Generation
When Claude is writing new code — checks run before the first line is generated.
Corrected code is produced in a single pass.

### Repo Scan
Say "scan my repo" or "PII audit" — produces a structured report with:
- 🔴 Critical findings (fix before deploy)
- 🟡 Important findings (fix within 30 days)
- 🟢 Suggestions
- Full compliance summary table across all layers

---

## Example Triggers

The skill fires on any of these — no explicit request needed:

```
"Build a User model with email, password, and date of birth"
"Create a login flow with JWT"
"Review my users table"
"Add a delete account endpoint"
"I want to build a lead capture form"
"Scan my repo for PII issues"
```

---

## File Structure

```
pii-detector/
├── SKILL.md                  ← entry point + mode routing
├── modes/
│   ├── planning.md           ← enrich responses during discussion/review
│   ├── inline.md             ← check-first generation mode
│   └── repo-scan.md          ← full audit orchestration + report format
├── layers/
│   ├── api-layer.md
│   ├── auth-sessions.md
│   ├── data-in-transit.md
│   ├── data-lifecycle.md
│   ├── frontend.md
│   ├── legal-consent.md
│   └── testing-seeding.md
├── patterns/
│   └── fields.md             ← full PII field pattern dictionary
└── rules/
    ├── non-negotiables.md    ← hard rules (passwords, CVV, SSN, health)
    └── leakage-vectors.md    ← logs, analytics, serializers, config
```

---

*Built by [Sprinto](https://sprinto.com) — compliance automation for fast-growing companies.*
