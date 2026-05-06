# Non-Negotiable Rules

These apply always, regardless of stack, language, or context.
No exceptions. No "but we'll fix it later."

---

## Passwords

- **Never** store plaintext — bcrypt (cost ≥ 12), argon2id, or scrypt only
- Field name must be `password_hash` or `hashed_password` — never `password`
- **Never** log passwords, even temporarily or "just for debugging"
- **Never** return password hash in any API response or serializer
- **Never** store plaintext alongside the hash (`password` + `password_hash` = both wrong)

Fix pattern:
```
# Wrong
password: String

# Right
password_hash: String  # bcrypt/argon2id — never store or return raw value
```

---

## CVV / Card Security Codes

- **Never** store — not encrypted, not hashed, not "just temporarily"
- PCI-DSS Requirement 3.2: absolute prohibition, no exceptions
- If a `cvv` field exists anywhere: remove it entirely

---

## Card Numbers (PAN)

- Don't store raw — tokenize via Stripe, Braintree, or Adyen
- If unavoidable: AES-256 encryption, display last 4 digits only
- **Never** log payment request or response bodies (Datadog, Sentry, CloudWatch)

---

## SSN / Government IDs

- AES-256 encryption at rest, stored as BYTEA or BLOB
- **Never** in logs, **never** in API responses
- Display mask always: `***-**-6789`
- Audit log on every read access, not just writes
- If only last 4 digits are needed — only store last 4

Fix pattern:
```
# Wrong
ssn: String  # VARCHAR(11)

# Right
ssn_encrypted: Bytes     # AES-256, never returned in API
ssn_last_four: String    # "6789" — safe to display
```

---

## Health / Medical Data (HIPAA)

- Encryption at rest AND in transit — no exceptions
- Audit log on every **read** (not just mutations)
- Minimum necessary — return only the fields the caller explicitly needs
- **Never** send to: Sentry, Datadog, Mixpanel, HubSpot, Intercom, or any
  tool without a signed Business Associate Agreement (BAA)

---

## Biometric Data (BIPA)

- Flag for written consent requirement — must exist before any storage
- Add `biometric_expires_at TIMESTAMP` — retention must be time-limited
- Use irreversible hash for comparison — never store raw biometric
- Applies across all US states, not just Illinois

---

## IP Addresses (CCPA)

- Treated as PII — don't log permanently without a defined purpose and TTL
- For analytics: mask last octet → `192.168.1.0`
- Don't index IP columns without a documented reason

---

## Generic String Fields on User Models

`notes TEXT`, `metadata JSONB`, `data TEXT`, `extra TEXT` on user-facing models
are potential unstructured PII containers. Always flag them.

Recommended response: document what goes in the field, add input validation,
encrypt if sensitive data is confirmed, add to deletion scope for CCPA erasure.

---

## Deletion (CCPA Right to Erasure)

Every model storing PII needs:
1. `deleted_at TIMESTAMP NULL` for soft delete
2. A hard-delete function that actually purges the row
3. Deletion to cascade to downstream vendors (analytics, CRM, email tools)

Soft-delete-only is not sufficient for CCPA compliance.

---

## Consent Tracking

Any model that stores PII collected directly from a user should have:
```
data_consent_at: Timestamp   # when user consented
consent_version: String      # which version of privacy policy they accepted
```

---

## Audit Trail

For SSN, health data, financial account data, and biometrics:
- Every read access must be logged (not just writes)
- Log: who accessed, what record, when, from what IP
- Either a DB trigger or application-level audit log — one of the two, always
