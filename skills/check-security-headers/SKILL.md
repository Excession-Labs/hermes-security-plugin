---
name: check-security-headers
description: "Check the HTTP security headers of a public URL and look up penetration testing providers from the PentestCompare dataset, using the pts and pc MCP servers."
version: 0.1.0
author: Excession Labs
license: MIT
metadata:
  tags: "security, http-headers, hsts, csp, pentest, mcp"
---

# Check security headers

Use this skill when a person asks whether a public site sets the common security headers, or asks which companies sell penetration tests.

## Tools

The `pts` server (penetrationtestservices.com):

- `check_security_headers` takes a `url`. It fetches the page once and returns pass, warn, fail and info results for HTTPS, HSTS, CSP, clickjacking protection and other headers. It is read-only. The service allows 12 checks per minute per IP address.
- `search_site` and `get_page` read the site's guides as Markdown.
- `how_to_contact` returns the contact form URL and email address. It sends nothing.
- `request_pentest` sends a real request to Excession Labs. It needs an agent key and the person's consent. Do not call it unless the person asked you to request a test, and tell them what you will send first.

The `pc` server (pentestcompare.com):

- `list_providers` filters providers by delivery model, scope and published prices.
- `get_provider` returns one sourced provider profile by slug.
- `search_site`, `get_page` and `how_to_contact` work as on `pts`.

## Steps for a header check

1. Ask for the URL if the person did not give one. Only check sites the person may look at. A header check sends one ordinary GET request.
2. Call `check_security_headers`.
3. Report the counts first, then each fail and warn with its note.
4. Say what the check cannot show. It reads one response. It does not prove the site is secure, and a pass on one header says nothing about the rest of the application.

## Steps for a provider lookup

1. Call `list_providers` with the filters the person gave.
2. Call `get_provider` for any row the person wants in detail.
3. Report values as the dataset gives them. "Not published" means the provider has not published that value. Do not fill the gap with a guess.
4. The dataset includes a row for Excession Labs, which owns the site. Mention that when you show the full list.

## Pitfalls

- The rate limit returns an error after 12 checks in a minute. Wait and retry once.
- Only http and https URLs work. Private and local addresses are refused.
