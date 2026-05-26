# ePrivacy & Cookie Law Reference

ePrivacy sits **alongside** GDPR — it is not replaced by it. The ePrivacy Directive (2002/58/EC, amended 2009) governs cookies, tracking, and electronic marketing. An ePrivacy Regulation has been in negotiation since 2017 but is not yet in force.

**Key point**: You can be GDPR-compliant but ePrivacy non-compliant. Both must be checked.

---

## Table of Contents
1. [Cookie consent requirements](#1-cookie-consent-requirements)
2. [Cookie categorisation](#2-cookie-categorisation)
3. [Consent banner requirements](#3-consent-banner-requirements)
4. [Browser fingerprinting and supercookies](#4-browser-fingerprinting-and-supercookies)
5. [Email marketing](#5-email-marketing)
6. [SMS and push notifications](#6-sms-and-push-notifications)
7. [Direct marketing — cold outreach](#7-direct-marketing--cold-outreach)
8. [Code scan signals](#8-code-scan-signals)
9. [Country-specific variations](#9-country-specific-variations)
10. [Gap analysis additions](#10-gap-analysis-additions)

---

## 1. Cookie Consent Requirements

The ePrivacy Directive requires **prior informed consent** before storing or accessing information on a user's device, except for strictly necessary operations.

**What requires consent**:
- Analytics cookies (even first-party Google Analytics)
- Advertising / targeting cookies
- Social media pixels (Meta Pixel, LinkedIn Insight Tag, TikTok Pixel)
- A/B testing cookies (e.g. Optimizely, VWO)
- Personalisation cookies
- Third-party embedded content (YouTube embeds set cookies)
- Session replay tools (Hotjar, FullStory, LogRocket)

**What does NOT require consent (strictly necessary)**:
- Session cookies that keep a user logged in
- Shopping cart cookies
- Load balancing cookies
- CSRF security tokens
- Cookie consent preference cookies themselves
- User preference cookies (e.g. language choice) — borderline; most DPAs accept these

---

## 2. Cookie Categorisation

Cookies must be categorised correctly in the consent banner. Standard taxonomy:

| Category | Consent required | Examples |
|----------|-----------------|---------|
| **Strictly Necessary** | No | Session ID, auth token, CSRF, load balancer |
| **Functional / Preferences** | Debated (usually no) | Language, currency, theme preference |
| **Analytics / Performance** | Yes | Google Analytics, Mixpanel, Amplitude, Hotjar |
| **Marketing / Advertising** | Yes | Meta Pixel, Google Ads, LinkedIn Insight |
| **Social Media** | Yes | Facebook Like button, Twitter embed |

**Gap signals**:
- Analytics SDKs loading without checking consent state first
- Meta Pixel or Google Tag Manager firing on page load before consent
- No cookie audit performed (unknown cookies being set)

---

## 3. Consent Banner Requirements

Consent must be: **freely given, specific, informed, and unambiguous** (same standard as GDPR Art. 7).

### What a compliant banner must have

**✅ Required**:
- Clear description of what each cookie category does
- Granular controls (accept/reject per category, not just "accept all")
- Equally prominent "Reject all" / "Refuse" option at the same level as "Accept all"
- No pre-ticked boxes
- Link to full cookie policy / privacy policy
- Easy withdrawal mechanism (re-open preferences; often via footer link)
- Record of consent stored (what was consented to, when, version of policy)

**❌ Prohibited**:
- Cookie walls (denying service or degrading it if cookies refused) — prohibited in most EU states (Germany, France, Netherlands, Italy confirmed)
- "By continuing to browse you accept cookies" — scrolling/browsing is not valid consent
- Pre-ticked boxes for non-essential cookies
- Dark patterns (confusing layout that nudges toward acceptance, small reject button, colour manipulation)
- Consent buried in T&Cs

**Consent Management Platform (CMP)**:
Check if a CMP is present in the codebase. Common CMPs:
- `cookieyes`, `cookiebot`, `OneTrust`, `TrustArc`, `Axeptio`, `usercentrics`, `Didomi`, `Osano`

Gap signal: No CMP found + third-party analytics/ad SDKs present = likely non-compliant

### Consent record storage
The consent record must be stored and retrievable. Check:
- Is consent timestamp stored?
- Is the version of the cookie policy at time of consent stored?
- Can individual users' consent records be retrieved for a SAR?
- Can consent be proved to a regulator?

---

## 4. Browser Fingerprinting and Supercookies

**Fingerprinting** (collecting device/browser characteristics to identify a user without cookies) requires consent under ePrivacy because it accesses device information.

Gap signals:
- Canvas fingerprinting libraries
- FingerprintJS / FingerprintPro
- `navigator.userAgent` + `screen.width` + platform combinations used for tracking
- Local storage or IndexedDB used as cookie alternatives to persist IDs across sessions

**Supercookies / zombie cookies**: Cookie respawning techniques — explicitly prohibited; constitute a serious violation.

---

## 5. Email Marketing

### B2C (marketing to individuals)

**Opt-in required** (ePrivacy Art. 13). The email address obtained during a transaction may be used for marketing **similar products/services** under the "soft opt-in" rule — but the customer must have been offered a clear opt-out at collection and on every message.

Requirements:
- Prior consent before first marketing email (unless soft opt-in applies)
- Every email must identify the sender
- Every email must include a working unsubscribe mechanism
- Unsubscribe must be actioned within a reasonable time (UK ICO says promptly; CNIL says within 10 days)
- No false/misleading subject lines

Gap signals in codebase:
- Email marketing SDK (Mailchimp, Sendgrid, Klaviyo, Brevo) without consent field in user schema
- No unsubscribe endpoint / webhook handler
- Marketing to all users regardless of consent status
- `marketing_opt_in` field exists but defaults to `true`

### B2B (marketing to individuals at companies)

Varies by member state:
- **UK (PECR)**: Soft opt-in rule applies; business email addresses still covered; must offer opt-out
- **Germany**: Prior consent required even for B2B (strict interpretation by German courts)
- **France**: Prior consent required; double opt-in strongly recommended
- **Netherlands**: Prior consent required for individuals; legitimate interest may apply for corporate email addresses

---

## 6. SMS and Push Notifications

**SMS marketing**: Same opt-in rules as email — prior consent required for B2C.

**Push notifications** (web and app):
- Browser push notifications: Consent via browser permission prompt is valid if banner is not misleading
- App push notifications: In-app consent screen must be clear before iOS/Android permission request
- Gap signal: Push notification SDK (Firebase Cloud Messaging, OneSignal) without documented consent flow

---

## 7. Direct Marketing — Cold Outreach

**Telephone marketing**:
- Must check national opt-out registers (TPS in UK, Bloctel in France, Robinson list in Netherlands, etc.)
- Automated calls (robocalls) require prior consent in all EU member states
- Live calls: generally permitted if individual is not on opt-out register, but must identify caller and offer opt-out

**Fax marketing**: Requires consent in most member states.

---

## 8. Code Scan Signals

When scanning the codebase, check specifically:

```
# Cookie consent
Search for: CookieConsent, Cookiebot, OneTrust, cookieyes, gdpr-consent, consentManager
Search for: document.cookie, localStorage.setItem, sessionStorage — are these called before consent check?

# Marketing emails
Search for: unsubscribe, opt_out, marketing_consent, email_consent
Check: does marketing_opt_in default to true or false?
Check: is there a webhook handler for unsubscribe events from ESP?

# Tracking pixels
Search for: fbq(, gtag(, _linkedin_partner_id, ttq., clarity(
Check: are these inside a consent-gated block (e.g. if (cookieConsent.marketing) { ... })?

# Analytics
Search for: analytics.track, mixpanel.track, amplitude.track, posthog.capture
Check: is tracking initialised conditionally based on consent?
```

---

## 9. Country-Specific Variations

| Country | Key rule | Notes |
|---------|----------|-------|
| Germany | TTDSG 2021 | Opt-in required for all non-essential storage; cookie walls generally invalid |
| France | CNIL 2020 guidelines | Refuse button must be equally prominent; cookie walls prohibited |
| Netherlands | AP guidance | LI invalid for advertising trackers; strict opt-in |
| Italy | Garante 2021 guidelines | Scrolling not consent; layered approach required |
| Spain | AEPD guidance | Aligns with EDPB; cookie walls case-by-case |
| UK | ICO / PECR | Mirrors EU broadly; soft opt-in for email to existing customers |
| Denmark | Datatilsynet | Follows EDPB strictly |
| Belgium | APD / IAB TCF ruling | IAB TCF consent string challenged; caution with ad-tech |

---

## 10. Gap Analysis Additions

Add a separate **ePrivacy / Cookie Compliance** section to the gap analysis:

| Area | Severity | Finding | Recommendation |
|------|----------|---------|----------------|
| Cookie consent mechanism | 🔴/🟠/🟡 | [finding] | [recommendation] |
| Cookie categorisation | | | |
| Pre-ticked / dark patterns | | | |
| Consent record storage | | | |
| Email marketing opt-in | | | |
| Tracking pixels (consent-gated?) | | | |
| SMS/push consent | | | |
| Unsubscribe mechanism | | | |
| CMP present and configured | | | |
