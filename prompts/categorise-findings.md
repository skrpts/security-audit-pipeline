---
type: prompt
id: categorise-findings
title: Categorise Findings
description: "Groups raw scan findings into OWASP Top 10 categories for structured analysis"
tags: [Production, Security, Audit, OWASP]
connections:
  - target: finding-categorisation
    type: derived_from
metadata:
  output_format: structured
  avg_tokens: 1500
---

## Purpose

Takes the raw vulnerability scan output and organises every finding into the appropriate OWASP Top 10 (2021) category. This standardised grouping makes the findings immediately useful for security professionals and auditors.

## Prompt

You are a security analyst categorising vulnerability findings against the OWASP Top 10 (2021) framework. Review the scan results below and assign each finding to the most appropriate OWASP category.

**Scan results:** {{steps.previous.output}}

### OWASP Top 10 Categories

Map each finding to one of these categories based on its root cause:

| Code | Category | Examples |
|------|----------|----------|
| A01 | Broken Access Control | Missing auth checks, IDOR, CORS misconfiguration |
| A02 | Cryptographic Failures | Weak algorithms, cleartext transmission, hardcoded keys |
| A03 | Injection | SQL, command, XSS, template injection |
| A04 | Insecure Design | Missing threat modelling, insecure business logic |
| A05 | Security Misconfiguration | Debug mode, default credentials, missing headers |
| A06 | Vulnerable and Outdated Components | Known CVEs, unmaintained packages |
| A07 | Identification and Authentication Failures | Weak passwords, session issues, missing MFA |
| A08 | Software and Data Integrity Failures | Insecure deserialisation, unsigned deployments |
| A09 | Security Logging and Monitoring Failures | Missing audit logs, unmonitored security events |
| A10 | Server-Side Request Forgery | Unvalidated URL fetching, internal network access |

### Categorisation Rules

1. **Primary category only** — assign each finding to the single most relevant category based on its root cause. If a finding could fit multiple categories, choose the one that best describes why the vulnerability exists, not just what it does.

2. **Ambiguity resolution:**
   - An XSS vulnerability caused by missing input validation → A03 (Injection), not A05 (Misconfiguration)
   - Hardcoded credentials → A07 (Authentication Failures), not A02 (Cryptographic Failures)
   - Missing security headers → A05 (Misconfiguration), not A01 (Broken Access Control)

3. **Uncategorised findings** — if a finding genuinely does not fit any OWASP category, place it in an "Other" group and explain why it does not map.

### Output Format

For each OWASP category that has findings:

**[Category Code]: [Category Name]** — {finding count} finding(s)

[One-paragraph summary of the risk in this category]

Then list each finding with all its original fields (location, title, description, evidence, CWE, confidence) plus the assigned category.

End with a summary table:

| Category | Finding Count |
|----------|--------------|
| A01 | {n} |
| ... | ... |
| Total | {total} |

### Rules

- Preserve all original finding data — do not modify or summarise the findings themselves
- Order categories by finding count (most findings first)
- If a category has zero findings, omit it from the detailed output but include it in the summary table with a count of 0
- Use British English throughout
