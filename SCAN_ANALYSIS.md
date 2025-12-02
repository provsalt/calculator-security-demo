# Security Scan Analysis - 2025/12/2

## DAST Results (ZAP Full)
- Total URLs scanned: 1

### Warning Summary
| Risk Level      | 	Number of Alerts |
|-----------------|-------------------|
| High            | 0                 |
| Medium          | 3                 |
| Low             | 4                 |
| Informational   | 6                 |
| False Positives | 0                 |

| Code  | 	Issue                                       | 	Risk  | 	Affected             | 	Why It Matters                                                                     |
|-------|----------------------------------------------|--------|-----------------------|-------------------------------------------------------------------------------------|
| 10038 | Content Security Policy (CSP) Header Not Set | Medium | http://localhost:5000 | Might allow attackers to trigger XSS on users                                       |
| 10106 | HTTP Only Site                               | Medium | http://localhost:5000 | No encryption might cause users data to be stolen through MITM attacks              |
| 10020 | Missing Anti-clickjacking Header             | Medium | http://localhost:5000 | It might allow the user to be served with a different content when XSS is triggered |

## SCA Results (Dependency Check)
- High severity: X
- Medium severity: Y
- Low severity: Z

### Vulnerable Packages
werkzeug 3.1.3 MEDIUM CVE-2025-66221 Improper Handling of Windows Device Names

## SAST Results (CodeQL)
- Total issues: 1
- By type: High
Flask app is run in debug mode CWE-489

## Recommendations
Disable flask debug mode
Add CSP header
Update dependencies in requirements.txt

# Scanner Comparison
## Only CodeQL (SAST) Found
Flask debug mode turned on

## Only ZAP (DAST) Found

Missing security headers (CSP, X-Frame-Options, HSTS)

Clickjacking vulnerabilities

## Only Dependency-Check (SCA) Found

Known CVEs in third-party libraries\

Outdated package versions

## All Three Tools

Some SQL injection cases (code pattern + runtime exploit + vulnerable ORM library)

Some XSS cases (code flaw + runtime confirmation + vulnerable template engine)
