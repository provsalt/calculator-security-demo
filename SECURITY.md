# Security Scanning

## Overview
This repository uses three types of security scanning to identify vulnerabilities at different stages:

### SAST (Static Analysis with CodeQL)
- **Tool:** GitHub CodeQL
- **Workflow:** [`.github/workflows/1-sast-only.yml`](.github/workflows/1-sast-only.yml)
- **Runs:** On push to main/master and pull requests
- **Checks for:** SQL injection, XSS, command injection, path traversal, hardcoded credentials, insecure cryptography
- **Results:** GitHub Security tab → Code scanning alerts

### SCA (Dependency Check with OWASP)
- **Tool:** OWASP Dependency-Check
- **Workflow:** [`.github/workflows/2-sca-only.yml`](.github/workflows/2-sca-only.yml)
- **Runs:** On push to main/master and pull requests
- **Checks for:** Known CVEs in Python packages, vulnerable dependencies (CVSS ≥ 7), retired packages
- **Artifacts:** `sca-reports` (HTML, JSON, CSV, XML formats)
- **Results:** Actions tab → Download artifacts

### DAST (Live Application Scan with ZAP)
- **Tool:** OWASP ZAP (Baseline + Full Scan)
- **Workflow:** [`.github/workflows/3-dast-only.yml`](.github/workflows/3-dast-only.yml)
- **Runs:** On push to main/master and pull requests (after successful build)
- **Checks for:** XSS, SQL injection, CSRF, security headers, cookie issues, directory traversal, information disclosure
- **Artifacts:** `zap-baseline-reports`, `zap-fullscan-reports`
- **Results:** Actions tab → Download artifacts → Open report_html.html

### Complete Pipeline
- **Workflow:** [`.github/workflows/4-complete-security.yml`](.github/workflows/4-complete-security.yml)
- **Runs all three scans** in sequence: SAST → SCA → Build → DAST → Summary
- **Fails fast** if critical issues are detected early

## Understanding Results

### Where to Find Results
1. **CodeQL (SAST):** Security tab → Code scanning alerts
2. **Dependency-Check (SCA):** Actions tab → Artifacts → `sca-reports` → Open `dependency-check-report.html`
3. **ZAP (DAST):** Actions tab → Artifacts → `zap-baseline-reports` or `zap-fullscan-reports` → Open `report_html.html`

### Severity Levels
- **Critical** (CVSS 9.0-10.0): Fix immediately
- **High** (CVSS 7.0-8.9): Fix promptly
- **Medium** (CVSS 4.0-6.9): Plan remediation
- **Low** (CVSS 0.1-3.9): Consider fixing
- **Informational:** Best practices

### How to Interpret
- Review findings by severity (Critical/High first)
- Check affected files and line numbers
- Read remediation guidance provided
- Verify findings aren't false positives
- Fix, test, and verify the scan passes

## Reporting Vulnerabilities
Please email security@example.com