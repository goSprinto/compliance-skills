# Sprinto Claude Skills

A collection of Claude skills for compliance, security, and privacy-aware development.
Built and maintained by [Sprinto](https://sprinto.com).

---

## Available Skills

### [`pii-detector`](./pii-detector)

Automatic PII detection and privacy compliance checks for US-based companies.
Fires during planning, code generation, and full repo audits — without being asked.

```bash
claude skills add gosprinto/claude-skills/pii-detector
```

**Covers**: CCPA, HIPAA, PCI-DSS, COPPA, GLBA, BIPA, FERPA, FTC Act  
**Layers**: data models, auth, API, frontend, transit, lifecycle, testing, legal & consent

---

## Installation

Each skill installs independently:

```bash
# Using npx (Node.js)
npx skills add gosprinto/claude-skills/pii-detector

# Latest version
claude skills add gosprinto/claude-skills/pii-detector

# Pin to a specific version
claude skills add gosprinto/claude-skills/pii-detector@v1.0.0

# Install globally across all projects
claude skills add --global gosprinto/claude-skills/pii-detector
```

---

[![Sprinto](https://sprinto.com/wp-content/uploads/2025/11/sprinto-site-logo-dark.png)](https://sprinto.com)
