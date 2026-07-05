# Phabricator Bug Bounty Recon - 2026-07-05

## Summary
- **Target**: phabricator.com (+ phacility.com)
- **Subdomains Enumerated**: 3 (phabricator.com) + 23 (phacility.com) = 26 total
- **Date**: 2026-07-05
- **Primary Domain**: phabricator.com, phacility.com (now defunct)

## Live Hosts
| Host | Status | Notes |
|------|--------|-------|
| www.phabricator.com | 301 -> secure.phabricator.com |
| secure.phabricator.com | 200 OK | Phabricator instance |
| meta.phacility.com | 302 -> admin.phacility.com/oauth/ | Login redirect |

## CORS Testing
- No `access-control-allow-origin` headers detected on any host
- **Status**: SECURE

## Exposed Files (www.phabricator.com)
All paths return HTTP 200 with identical content (24754 bytes) - Phabricator catch-all routing.
- `/.env` - 200 (catch-all)
- `/.git/config` - 200 (catch-all)
- `/robots.txt` - 200 (catch-all, same size)
- `/admin` - 200 (catch-all)

## Exposed Files (meta.phacility.com)
All paths return HTTP 200 with identical content (10651 bytes).
- All paths show same catch-all page

## Directory Fuzzing (ffuf)
- All paths return HTTP 301 redirect (239 bytes) - redirects to secure.phabricator.com
- Consistent behavior across all fuzzed paths

## Security Findings
- **Severity**: Low
- Phabricator instance is no longer maintained (since June 2021)
- CSP properly restricts resources to p.phcdn.net
- HSTS with preload on secure.phabricator.com
- X-Frame-Options: Deny on all pages
- CSRF tokens present in forms
- No CORS misconfiguration detected
- Catch-all routing prevents directory enumeration
