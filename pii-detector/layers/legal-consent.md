# Layer: Legal & Consent Infrastructure

## When This Layer Loads

**Repo scan**: always — run as advisory checks, flag as warnings not hard errors.
**Generation mode**: when scaffolding a frontend app, marketing site, signup
  flow, or server configuration.
**Planning mode**: mention cookie consent if discussing analytics setup, forms
  collecting personal data, or public-facing properties. One sentence only.

---

## LC-01: Log Level Set to Debug in Production Config

What to grep:
```
LOG_LEVEL=debug
LOG_LEVEL=verbose
level: 'debug'
level: 'verbose'
logging.basicConfig(level=logging.DEBUG)
log_level :debug
```

Flag when:
- Log level is debug/verbose/trace in any production config file
- Log level not gated on NODE_ENV — always debug regardless of environment
- Production .env has LOG_LEVEL=debug

Why it matters: Debug log level causes most frameworks to log full request
bodies, SQL queries with parameter values, and internal state — all of which
frequently contain PII. One of the highest-frequency PII leak paths in production.

Fix pattern:
```javascript
// Wrong
const logger = winston.createLogger({ level: 'debug' })

// Right
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL ||
    (process.env.NODE_ENV === 'production' ? 'warn' : 'debug')
})
```

```python
# Wrong
logging.basicConfig(level=logging.DEBUG)

# Right
level = logging.DEBUG if os.getenv('NODE_ENV') != 'production' else logging.WARNING
logging.basicConfig(level=level)
```

```bash
# Wrong in .env.production
LOG_LEVEL=debug

# Right
LOG_LEVEL=warn
```

Severity: Hard finding — report in Critical section
Regulation: CCPA, HIPAA, PCI-DSS

---

## LC-02: No Cookie Consent Library Detected

What to grep in package.json / requirements.txt / Gemfile:
```
cookieyes, osano, onetrust, cookiehub, usercentrics,
cookieconsent, react-cookie-consent, vue-cookie-comply,
consent-manager, quantcast
```

Also check for manual consent implementation:
```
cookie-consent, cookieConsent, consentGiven, hasConsent, userConsented
```

Flag when:
- No CMP library in dependencies
- Analytics scripts present (GA, Mixpanel, Amplitude, Meta Pixel) but no
  consent wrapper detectable in frontend code

Fix:
```javascript
// Minimum viable consent gate
function initAnalytics() {
  if (!window.cookieConsentGiven) return
  // load GA, Mixpanel etc. here — never unconditionally
}
document.addEventListener('cookieConsentAccepted', initAnalytics)
```

Severity: Advisory — flag as "no consent library detected, verify manually"
Note: CMP may be injected via tag manager or loaded from a separate domain
Regulation: CCPA, FTC Act

---

## LC-03: No Privacy Policy Route Detected

What to grep in router files:
```
/privacy, /privacy-policy, /legal/privacy, privacy_policy, privacyPolicy
```

Flag when:
- No route matching /privacy* or /legal* in router files
- No privacy policy link in app config or layout

Severity: Advisory — "no privacy policy route detected, verify one exists
and is linked from your app"
Note: Policy may live on a separate marketing domain — don't hard-fail
Regulation: CCPA (required link from data-collecting pages), FTC Act

---

## LC-04: No Terms of Service Route Detected

What to grep in router files:
```
/terms, /tos, /terms-of-service, /legal/terms, termsOfService
```

Also flag if no `terms_accepted_at` field on user model at signup.

Severity: Advisory
Regulation: FTC Act

---

## Repo Scan Reporting

LC-01 (debug log level) → report in Critical section, not advisory.

LC-02, LC-03, LC-04 → group together as one advisory block:

```
### Advisory — Legal & Consent Infrastructure

Cookie consent: No CMP library detected in dependencies. If analytics
scripts are present, verify a consent gate exists (may be via tag manager).

Privacy policy: No /privacy route found in router files. CCPA requires
a privacy policy link on all data-collecting pages.

Terms of service: No /terms route found. Verify ToS is presented at signup.
```
