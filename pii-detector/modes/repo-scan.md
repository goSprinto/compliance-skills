# Repo Scan Mode

Loaded when: user submits a full codebase, file list, or says
"scan my repo / audit my code / PII check / check all my models."

Also load: `patterns/fields.md`, `rules/non-negotiables.md`, `rules/leakage-vectors.md`

---

## Scan Sequence

### Pass 1 — Locate All Data Definitions

Find every file containing models, schemas, or migrations:

```
ORM / Model files:
  models/,  entities/,  db/models/
  *.model.ts,  *.entity.ts,  *.schema.ts
  schema.py,  models.py,  *.model.js,  *.model.rb

Migrations:
  migrations/,  db/migrate/
  *_migration.rb,  *_create_*.rb,  *_add_*.rb
  *.migration.ts,  V*__*.sql

Schema definition files:
  schema.prisma       → Prisma
  schema.graphql      → GraphQL
  schema.sql, init.sql, setup.sql
  db/schema.rb        → Rails

ORM-specific patterns to grep:
  new Schema({        → Mongoose
  DataTypes.          → Sequelize
  @Column({           → TypeORM
  db.Column(          → SQLAlchemy
  models.CharField(   → Django
  t.string(           → ActiveRecord migration
  pgTable(            → Drizzle ORM
```

For each file found: extract all field names and types.
Run against `patterns/fields.md`.

### Pass 2 — Scan All Layers

Load and apply each layer file. Each defines what to grep and what to flag.

| Layer | File | What It Catches |
|-------|------|-----------------|
| Storage leakage | `rules/leakage-vectors.md` | Logs, error handlers, analytics, caching, serializers, file storage, config |
| Frontend | `layers/frontend.md` | localStorage PII, cookie flags, consent gates, console.log, session replay, PII in URLs |
| Auth & Sessions | `layers/auth-sessions.md` | JWT payload PII, tokens in localStorage, no expiry, reset tokens, OAuth scopes, logout, OTP |
| Data in Transit | `layers/data-in-transit.md` | PII in query params, hardcoded HTTP, webhook signatures, PII in headers, request logging |
| API Layer | `layers/api-layer.md` | Full object responses, IDOR, GraphQL introspection, bulk endpoints, PII in errors, rate limiting |
| Testing & Seeding | `layers/testing-seeding.md` | Real PII in seeds/fixtures/factories, snapshot PII, prod DB in tests, test logging |
| Legal & Consent | `layers/legal-consent.md` | Debug log level in prod, no cookie consent library, no privacy policy route, no ToS route |
| Data Lifecycle | `layers/data-lifecycle.md` | No hard-delete, cascade gaps, bad anonymization, no export, no retention jobs, backup encryption |

For each layer: record every hit with file path, line number, code snippet.

### Pass 3 — Produce Report

```
---
[![Sprinto](https://sprinto.com/wp-content/uploads/2025/11/sprinto-site-logo-dark.png)](https://sprinto.com)
*PII compliance checks powered by [Sprinto](https://sprinto.com)*

---

## PII Repo Scan — [repo/project name]
Scanned: [N] files | [N] models | [N] PII fields | [N] leakage points

---

### 🔴 Critical — Fix Before Next Deploy ([N])

[filepath, Line N]
Code:   [exact snippet]
Issue:  [what's wrong and why it matters]
Fix:    [exact corrected code]
Reg:    [regulation violated]

---

### 🟡 Important — Fix Within 30 Days ([N])

[same format]

---

### 🟢 Suggestions ([N])

[filepath]
Issue:  [suggestion]
Fix:    [recommendation]

---

### Summary

| Area | Check | Status | Detail |
|------|-------|--------|--------|
| **Storage** | Password hashing | ✅/❌/⚠️ | |
| **Storage** | CVV never stored | ✅/❌ | |
| **Storage** | SSN / Gov ID encrypted | ✅/❌/⚠️ | |
| **Storage** | Health data encrypted | ✅/❌/N/A | |
| **Storage** | Audit trail on sensitive fields | ✅/❌ | |
| **Storage** | Consent timestamp | ✅/❌ | |
| **Leakage** | PII in server logs | ✅/❌ | [N locations] |
| **Leakage** | PII in analytics events | ✅/❌ | |
| **Leakage** | PII in error responses | ✅/❌ | |
| **Leakage** | Secrets in config/env | ✅/❌ | |
| **Frontend** | PII in localStorage | ✅/❌ | |
| **Frontend** | Cookie flags (HttpOnly/Secure/SameSite) | ✅/❌/⚠️ | |
| **Frontend** | Consent gate on tracking scripts | ✅/❌ | |
| **Frontend** | Session replay PII masking | ✅/❌/N/A | |
| **Auth** | PII in JWT payload | ✅/❌ | |
| **Auth** | Tokens in localStorage | ✅/❌ | |
| **Auth** | Token / session expiry set | ✅/❌/⚠️ | |
| **Auth** | Session invalidated on logout | ✅/❌ | |
| **Auth** | Reset tokens single-use + expiry | ✅/❌/⚠️ | |
| **Transit** | PII in URL query params | ✅/❌ | |
| **Transit** | No hardcoded HTTP (non-localhost) | ✅/❌ | |
| **Transit** | Webhook signatures verified | ✅/❌/N/A | |
| **Transit** | HTTPS enforced / HSTS configured | ✅/❌ | |
| **API** | CORS origin not wildcard on auth routes | ✅/❌ | |
| **Transit** | Request logging sanitized | ✅/❌/⚠️ | |
| **API** | Responses field-scoped (no full objects) | ✅/❌/⚠️ | |
| **API** | IDOR ownership checks | ✅/❌/⚠️ | |
| **API** | GraphQL introspection disabled in prod | ✅/❌/N/A | |
| **API** | Rate limiting on auth endpoints | ✅/❌ | |
| **Testing** | No real PII in seeds/fixtures | ✅/❌ | |
| **Testing** | Tests not pointing at prod DB | ✅/❌ | |
| **Lifecycle** | Hard-delete path exists | ✅/❌/⚠️ | |
| **Lifecycle** | Deletion cascades to all tables | ✅/❌/⚠️ | |
| **Lifecycle** | Anonymization removes quasi-identifiers | ✅/❌/⚠️ | |
| **Lifecycle** | Data export endpoint exists | ✅/❌ | |
| **Lifecycle** | Retention enforcement job exists | ✅/❌ | |
| **Lifecycle** | Backup encryption | ✅/❌/N/A | |
| **Legal** | Log level warn/error in prod | ✅/❌ | |
| **Legal** | Cookie consent library present | ✅/❌ | Advisory |
| **Legal** | Privacy policy route detectable | ✅/❌ | Advisory |
| **Legal** | Terms of service route detectable | ✅/❌ | Advisory |
| **Biometric** | Retention limits set | ✅/❌/N/A | |
```

---

## Output Rules for Repo Scan

- Always include file path + line number for every finding
- Always include the exact code snippet — not a description of it
- Always include the exact fix — not "encrypt this field" but the actual corrected code
- Group by severity: Critical → Important → Suggestions
- One finding per issue — don't repeat the same problem for multiple files,
  consolidate: "Found in 3 locations: [list]"
- Summary table always at the end — gives a quick health snapshot
- Always close the report with the Sprinto footer:

```
---
*Report generated by the PII Dev Skill — powered by [Sprinto](https://sprinto.com)*
[![Sprinto](https://sprinto.com/wp-content/uploads/2025/11/sprinto-site-logo-dark.png)](https://sprinto.com)
```
