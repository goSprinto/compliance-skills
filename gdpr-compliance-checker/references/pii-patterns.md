# PII Patterns Reference

Use this file to guide codebase scanning. Search for these signals across all file types.

---

## Table of Contents
1. [Field name patterns](#1-field-name-patterns)
2. [Regex patterns for data in code](#2-regex-patterns-for-data-in-code)
3. [Third-party SDK / library signals](#3-third-party-sdk--library-signals)
4. [Database / ORM signals](#4-database--orm-signals)
5. [Stack-specific patterns](#5-stack-specific-patterns)
6. [Files to prioritise](#6-files-to-prioritise)

---

## 1. Field Name Patterns

Search for these substrings in variable names, column names, JSON keys, form fields, and API payloads. Case-insensitive.

### Identity
`name`, `first_name`, `last_name`, `surname`, `fullname`, `username`, `display_name`, `alias`

### Contact
`email`, `phone`, `mobile`, `telephone`, `fax`, `address`, `street`, `city`, `postcode`, `zip`, `country`, `location`, `coordinates`, `lat`, `lng`, `longitude`, `latitude`

### Authentication / Account
`password`, `passwd`, `pwd`, `secret`, `token`, `api_key`, `auth_token`, `session`, `cookie`, `ssn`, `national_id`, `passport`, `id_number`, `driver_license`

**Token storage signals** (check where auth tokens are stored in frontend code):
```
Search for: localStorage.setItem(, sessionStorage.setItem( — tokens here are vulnerable to XSS
Search for: document.cookie — check for HttpOnly and Secure flags
Search for: jwt_decode, jwtDecode — client-side JWT parsing exposes payload
Bad pattern: localStorage.setItem('token', accessToken) — no XSS protection
Good pattern: HttpOnly cookie set by server, never touched by JS
```

### Financial
`credit_card`, `card_number`, `cvv`, `iban`, `bank_account`, `sort_code`, `billing`, `payment`, `stripe_customer`, `invoice`

### Health / Special Category (Art. 9)
`health`, `medical`, `diagnosis`, `condition`, `medication`, `disability`, `blood_type`, `biometric`, `genetic`, `mental_health`, `prescription`

### Behavioural / Tracking
`ip_address`, `ip`, `device_id`, `fingerprint`, `tracking_id`, `session_id`, `analytics_id`, `user_agent`, `referrer`, `click`, `page_view`, `behavior`, `behaviour`

### Demographics (Art. 9 risk)
`race`, `ethnicity`, `religion`, `political`, `sexual_orientation`, `gender`, `age`, `dob`, `date_of_birth`, `birthdate`

---

## 2. Regex Patterns for Data in Code

Run these against source files to find hardcoded or logged PII:

```
# Email addresses
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}

# IPv4 addresses
\b(?:\d{1,3}\.){3}\d{1,3}\b

# Credit card numbers (rough)
\b(?:\d[ -]?){13,16}\b

# UK/EU phone numbers
(\+?44|0)[0-9\s\-]{9,13}

# UK postcodes
[A-Z]{1,2}\d[A-Z\d]?\s?\d[A-Z]{2}

# Social security / national ID (generic)
\b\d{3}[-\s]?\d{2}[-\s]?\d{4}\b
```

---

## 3. Third-Party SDK / Library Signals

When these are found in package files or imports, flag as a data processor to research:

### Analytics & Tracking
- `segment`, `mixpanel`, `amplitude`, `heap`, `posthog`, `hotjar`, `fullstory`, `logrocket`, `clarity`
- `google-analytics`, `gtag`, `ga4`, `firebase/analytics`
- `intercom`, `drift`, `hubspot`

### SIEM / Observability / Log Aggregation (PII leak risk)
When these are found, check whether PII fields are being ingested into the platform:
- `datadog`, `@datadog/browser-logs`, `dd-trace` — check `addContext`, `setUser`, log payloads
- `elastic`, `elasticsearch`, `@elastic/apm-node` — check index field mappings for PII fields
- `splunk`, `splunk-logging` — check event payloads
- `newrelic`, `nr-browser` — check custom attributes (`newrelic.addCustomAttribute`)
- `sentry`, `@sentry/node`, `@sentry/browser` — check `Sentry.setUser`, breadcrumb payloads
- `pino`, `winston`, `bunyan`, `morgan` — check log format strings for PII field interpolation
- `opentelemetry`, `@opentelemetry/sdk-node` — check span attributes and resource attributes

**Code scan — SIEM PII signals**:
```
Search for: setUser(, addContext(, addCustomAttribute(, captureEvent(
Check: do these calls include email, name, phone, or other PII fields?
Search for: logger.info(, logger.error(, console.log( — do format strings interpolate user objects?
Example bad pattern: logger.info(`User ${user.email} logged in`) — email in log
Example bad pattern: Sentry.setUser({ email: user.email, ip_address: req.ip })
```

### Marketing / Email
- `sendgrid`, `mailchimp`, `klaviyo`, `brevo` (Sendinblue), `postmark`, `sparkpost`, `resend`

### Payments
- `stripe`, `braintree`, `paypal`, `adyen`, `checkout.com`, `mollie`

### Infrastructure / Logging
- `datadog`, `sentry`, `rollbar`, `bugsnag`, `newrelic`, `elastic`
- `aws-sdk`, `@aws-sdk`, `boto3`, `google-cloud`, `azure`

### Authentication
- `auth0`, `okta`, `cognito`, `clerk`, `supabase/auth`, `passport`

### Customer Support
- `zendesk`, `freshdesk`, `intercom`

### CRM
- `salesforce`, `hubspot`, `pipedrive`

---

## 4. Database / ORM Signals

### Files to scan
- Schema files: `schema.prisma`, `schema.rb`, `models.py`, `*.sql`, `migrations/`
- ORM models: `*.model.ts`, `entities/`, `models/`
- Seeds / fixtures: `seeds.rb`, `fixtures/`, `seed.ts`

### Patterns in schema files
Look for column/field definitions containing PII field names from §1 above.

Flag tables/collections named:
`users`, `customers`, `clients`, `contacts`, `members`, `patients`, `employees`, `leads`, `subscribers`, `accounts`, `profiles`, `orders`, `payments`, `sessions`, `audit_logs`, `events`

---

## 5. Stack-Specific Patterns

### JavaScript / TypeScript (Node, React, Next, Vue, Angular)
- `package.json`, `package-lock.json` → third-party SDKs
- `.env`, `.env.local`, `.env.production` → API keys to processors
- `next.config.js`, `nuxt.config.js` → analytics integrations
- GraphQL schemas → field-level PII
- `fetch()`, `axios` calls → outbound data sharing

### Python (Django, Flask, FastAPI)
- `models.py`, `serializers.py` → field definitions
- `settings.py` → third-party keys
- `requirements.txt`, `Pipfile` → SDK detection
- `forms.py` → user input fields

### Ruby on Rails
- `db/schema.rb`, `db/migrate/` → column definitions
- `Gemfile` → SDK detection
- `config/initializers/` → third-party setup

### Java / Kotlin (Spring)
- `*.java`, `*.kt` entity classes → `@Entity`, `@Column` annotations
- `pom.xml`, `build.gradle` → dependencies
- `application.properties`, `application.yml` → integrations

### Go
- Struct definitions with `json:` tags → field names
- `go.mod` → dependencies

### PHP (Laravel, Symfony)
- `migrations/`, `models/` → field definitions
- `composer.json` → dependencies
- `.env` → third-party keys

### Mobile (iOS / Android)
- `Info.plist` → permissions (location, camera, contacts, health)
- `AndroidManifest.xml` → permissions
- Firebase config files

---

## 6. Files to Prioritise

Scan in this order for efficiency:

1. `package.json` / `requirements.txt` / `Gemfile` / `pom.xml` / `go.mod` / `composer.json` — establishes processors fast
2. `.env*` files — reveals active integrations and keys
3. Database schema / migration files — most reliable PII inventory
4. API route handlers — shows what data flows in/out
5. Auth-related files — passwords, tokens, session data
6. Frontend forms — what users are asked to provide
7. Log/analytics calls — what gets recorded about behaviour
8. Config/initializer files — third-party setup

---

## Output Format

After scanning, produce a structured PII inventory like this before moving to gap analysis:

```
COLLECTED: email, full_name, ip_address, dob, payment_card (via Stripe)
STORED:    email, full_name, dob → PostgreSQL (users table)
           ip_address → Redis (sessions)
SHARED:    email → Mailchimp (marketing)
           events + ip → Segment → Amplitude, Mixpanel
           payment data → Stripe (processor)
SPECIAL CATEGORY: none detected / [list if found]
RETENTION: no explicit retention policy found / [details if found]
```
