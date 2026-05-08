# Planning Mode

This skill does NOT produce its own output in planning mode.
It enriches Claude's natural response with PII observations added inline.

---

## Core Behavior

Claude produces its normal response — architecture advice, code review,
schema analysis, design discussion — exactly as it would without this skill.

This skill adds PII-relevant observations **into** that response, under the
sections where they naturally belong. Not as a separate section. Not as a
report appended at the end. As additional bullets or sentences inside the
response Claude was already going to write.

**The reader should not be able to tell where Claude's response ends and
the PII skill's input begins. It should read as one coherent response.**

---

## When This Mode Applies

Anything that is NOT active code generation and NOT an explicit full audit request.

- User is asking how to build, design, or structure something
- User is asking for a review or feedback on existing code or schema
- User is asking Claude to read, look at, or check a file
- User is discussing what data to collect or how to model it
- No new code is being written in this response

Signals: "review", "read", "look at", "check", "share thoughts",
"how should I build", "what fields should I include", "I want to build X",
"help me design", "thoughts on", "feedback on", plain language descriptions.

NOT this mode: user says "build it", "create", "generate", "write the code",
"implement" — those trigger generation mode (modes/inline.md).
NOT this mode: user says "scan my repo", "full audit", "PII report" — those
trigger repo scan mode (modes/repo-scan.md).

---

## How to Add PII Observations

Read Claude's planned response. For each section, ask:
**does this section touch data that has PII implications?**
If yes, add the relevant PII note as an additional bullet or sentence
in that section. If no, leave the section unchanged.

### Where PII observations belong

| Section in Claude's response | PII to add if relevant |
|---|---|
| Schema / fields / model design | password naming, missing deletedAt, no consent timestamp, unencrypted PII fields |
| Indexes | enumeration risk on PII fields (email, name) — ensure org-scoped |
| Soft delete / lifecycle | no deletedAt = no CCPA erasure path |
| Associations / relations | constraints: false means orphaned PII on delete |
| API / serializers | over-exposed fields, password_hash in response |
| Auth / sessions | JWT payload PII, token storage, expiry |
| Third-party integrations | DPA requirement when sending contact data to CRM/email tools |
| Storage / database | encryption at rest for sensitive fields |
| Frontend / forms | consent capture, localStorage misuse |

### What to add per finding

One sentence or bullet. Specific to what's in the code or design.
Reference actual field names, line numbers, or section names where possible.

Good: "- `password` (line 57) should be `password_hash` — the name alone
doesn't communicate hashing intent and creates ambiguity at every callsite."

Bad: "Make sure to hash passwords."

Good: "- No `deletedAt` on this model also means no CCPA erasure path —
if User deletion is handled elsewhere, document where."

Bad: "Add a deletion mechanism for compliance."

---

## What NOT to Do

- Do NOT produce a separate PII report or section
- Do NOT open with "⚡ PII Check"
- Do NOT list regulations unless a finding is severe enough to warrant it
- Do NOT repeat findings Claude already made in its own response
- Do NOT add PII notes to sections that have no PII relevance
- Do NOT make PII the dominant topic — it should be maybe 20% of the response
- Do NOT audit existing repo files when the user asked a planning question

---

## Example

**User asks:** "Share thoughts on how to build a form to collect company
details and POC information."

**Claude's natural response covers:** frontend approach, backend API, schema
design, storage, validation.

**What this skill adds into that response:**

Under schema section:
- "Since POC fields include name, email, phone — add `consented_at TIMESTAMP`
  at the record level to capture when they submitted the form."
- "Plan a deletion path now — a `deleted_at` column is easier to add to the
  schema today than retrofit after data accumulates."

Under storage/third-party section (if CRM mentioned):
- "If pushing leads to HubSpot or similar, you'll need a DPA with them before
  syncing contact data — they have a standard one, easy to sign."

That's it. The rest of Claude's response is unchanged.

---

## Transition to Inline Mode

The moment the user asks Claude to generate, write, or scaffold code —
switch to inline mode (`modes/inline.md`). Run the full PII check before
generating the first line of code.
