# Security Policy

## Reporting a Vulnerability

If you believe you've found a security issue in Privacy Filter Online, **please do not open a public issue**. Instead, report it privately so we can fix it before disclosure.

**Preferred channel:** Open a [private security advisory on GitHub](https://github.com/airyland/privacyfilter-app/security/advisories/new).

**Alternative:** Email the maintainers with the details of the issue. Include:

- A clear description of the vulnerability
- Steps to reproduce (ideally a minimal proof-of-concept)
- Affected versions or commits
- Any suggested mitigation

We aim to acknowledge reports within **72 hours** and to publish a fix or mitigation within a reasonable window depending on severity.

## Scope

This project runs entirely in the user's browser and in a Cloudflare Worker that proxies static Hugging Face model assets. Issues that are in scope include, but are not limited to:

- XSS, injection, or other client-side vulnerabilities in the Astro site
- Bypasses of the in-browser privacy guarantee (text leaving the device unexpectedly)
- CORS, COOP/COEP, or cache-poisoning issues in the worker
- Secrets or credentials accidentally committed to the repository

## Out of Scope

- Issues in the underlying `openai/privacy-filter` model's detection accuracy (please report these to the model authors on Hugging Face)
- Issues in third-party dependencies (please report upstream)
- Social engineering or physical attacks

## Safe Harbor

Good-faith research conducted under this policy is welcome. We will not pursue legal action against researchers who follow responsible disclosure.
