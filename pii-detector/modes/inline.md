# Inline Mode

Loaded when Claude is about to generate any code — regardless of what the user said.
The trigger is what Claude is PRODUCING, not just what was asked.

## Language Rule

Always suggest fixes in the language of the code being reviewed.
Never translate to a different language. Python code gets Python fixes.
Ruby gets Ruby. Go gets Go. Infer from context — never ask.

---

## Process (run in this order every time)

### 1. Detect What Claude Is About to Generate

Before writing any code, identify the code type. This determines which layers to load.
Do this even if the user's request had no PII keywords.

| Claude is about to generate... | Load these layers |
|---|---|
| A model / schema / migration / ORM entity | `patterns/fields.md` + `rules/non-negotiables.md` (current file) |
| A login / signup / auth / registration flow | `layers/auth-sessions.md` |
| A frontend component / form / page | `layers/frontend.md` |
| An API route / controller / serializer | `layers/api-layer.md` |
| Middleware / request handler / HTTP client | `layers/data-in-transit.md` |
| A JWT / token / session / cookie handler | `layers/auth-sessions.md` |
| A seed file / fixture / factory / test data | `layers/testing-seeding.md` |
| A delete / purge / export / cleanup function | `layers/data-lifecycle.md` |
| A cron job / background worker | `layers/data-lifecycle.md` |
| A webhook handler | `layers/data-in-transit.md` + `layers/api-layer.md` |
| An analytics / monitoring integration | `rules/leakage-vectors.md` |

Multiple layers can apply at once — e.g. building an authenticated API endpoint
that creates a user loads: `patterns/fields.md` + `rules/non-negotiables.md` +
`layers/auth-sessions.md` + `layers/api-layer.md`.

If uncertain what layer applies → load all plausible ones. Over-checking is fine.

### 2. Scan Fields (load `patterns/fields.md`)

Check every field name and type against the pattern list.
Include indirect and abbreviated names based on model context.
When ambiguous: flag it, note the uncertainty, apply safest fix.

### 3. Apply Checks

For each flagged field, run through this table:

| Issue | Detected When | Fix |
|-------|--------------|-----|
| Plaintext password | `password String/VARCHAR` | Rename `password_hash`, bcrypt cost ≥ 12 or argon2id |
| CVV stored | Any `cvv` field | Remove entirely — no exceptions |
| Plaintext card number | `card_number VARCHAR` | Remove — tokenize via Stripe/Braintree |
| Unencrypted SSN | `ssn String/VARCHAR` | AES-256, rename `ssn_encrypted`, store BYTEA |
| Unencrypted health field | `diagnosis TEXT` plaintext | Encrypt at rest, add access log |
| Hash returned in API | Serializer includes `password_hash` | Exclude from all serializers |
| No deletion field | No `deleted_at` or hard-delete | Add `deleted_at TIMESTAMP NULL` + hard-delete path |
| No consent record | No consent timestamp | Add `data_consent_at TIMESTAMP` |
| Unnecessary precision | `dob DATE` when range is enough | `age_range ENUM('18-24','25-34',...)` |
| PII indexed | `INDEX ON (ssn)` | Remove — don't index PII fields |
| No audit trail | Sensitive field, no access log | DB trigger or application-level audit log |
| Biometric, no retention | `fingerprint BLOB`, no expiry | Add `biometric_expires_at TIMESTAMP` |
| Generic field on user model | `metadata JSONB`, `notes TEXT` | Flag as potential PII container, recommend encryption |

Also load `rules/non-negotiables.md` for password, CVV, SSN, and health data rules.

### 4. Output

Always use this format — tight, scannable, field-specific:

```
⚡ PII Check — [ModelName]

🔴 Fix now:
  [field_name] ([type]): [exact issue] → [exact fix]

🟡 Fix soon:
  [field_name]: [issue] → [fix]

🟢 Consider:
  [field_name]: [suggestion]

Regulations: [only the ones actually triggered]
```

Skip sections with no findings. Don't pad with empty categories.

### 5. Generate Corrected Code

Immediately after the check — generate the model with all fixes already applied.

**Never generate the broken version first.**
The output should be: check → corrected code. One pass, no back-and-forth.

---

## Examples

### "Build a User model with name, email, password (String), and dob"

```
⚡ PII Check — User

🔴 Fix now:
  password (String): plaintext storage → rename to password_hash,
  hash with bcrypt (cost ≥ 12) or argon2id, never store or return raw value

🟡 Fix soon:
  No deleted_at column → add for CCPA right-to-erasure
  No data_consent_at → add to record when user consented to data collection

🟢 Consider:
  dob (Date): if exact birth date isn't needed, use age_range enum instead

Regulations: FTC Act, CCPA
```

Then generate the corrected model immediately.

---

### "Add a field called p of type string to the auth model"

```
⚡ PII Check — Auth model

🔴 Fix now:
  p (String): abbreviated field on auth model likely stores a password or key.
  If password → rename password_hash, use bcrypt/argon2id, never store plaintext.
  If API key → store hash only, never the raw key. Confirm what this field stores.

Regulations: FTC Act
```

Ask what `p` stores if genuinely unclear, then generate corrected code.

---

### "Create a login system"

Treat as: User model with email + password_hash fields, plus session handling.
Run PII check on the implied schema before generating any code.

```
⚡ PII Check — Login system (implied schema)

🔴 Fix now:
  password: must be stored as password_hash using bcrypt (cost ≥ 12) or argon2id
  session_token: use short-lived opaque tokens — never store raw; store hash only

🟡 Fix soon:
  Add deleted_at to User model for CCPA erasure compliance
  Add data_consent_at to record consent at registration

Regulations: FTC Act, CCPA
```

Then generate the full login system with these applied.
