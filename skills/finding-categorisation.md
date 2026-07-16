---
type: skill
id: finding-categorisation
title: Finding Categorization
description: "Groups scan findings by OWASP Top 10 category for structured analysis and reporting"
tags: [Production, Security, Audit, OWASP]
connections:
  - target: llm-service
    type: runs_on
  - target: owasp-top-10-reference
    type: references
metadata:
  complexity: medium
  avg_tokens: 1500
  owasp_version: "2021"
---

## Capability

Takes raw vulnerability scan findings and organizes them into the OWASP Top 10 (2021) categories. This categorization provides a standardized framework for understanding the security posture of a codebase and ensures findings are reported in a format that security professionals and auditors recognize.

## OWASP Top 10 Categories

### A01: Broken Access Control

Findings where access restrictions are not properly enforced — users can act outside their intended permissions. Includes missing authorisation checks, insecure direct object references, CORS misconfigurations, and privilege escalation paths.

### A02: Cryptographic Failures

Previously "Sensitive Data Exposure." Findings related to weak or missing cryptography — use of deprecated algorithms (MD5, SHA-1, DES), hardcoded encryption keys, missing TLS, sensitive data transmitted or stored in cleartext, and weak random number generation.

### A03: Injection

Findings where untrusted data is sent to an interpreter as part of a command or query. Covers SQL injection, NoSQL injection, command injection, LDAP injection, XSS, and template injection.

### A04: Insecure Design

Architectural and design-level weaknesses rather than implementation bugs. Includes missing threat modeling, insecure business logic flows, lack of rate limiting on sensitive operations, and absence of defence-in-depth patterns.

### A05: Security Misconfiguration

Findings related to insecure default configurations, incomplete setups, open cloud storage, misconfigured HTTP headers, verbose error messages, and unnecessary features or services enabled.

### A06: Vulnerable and Outdated Components

Dependencies with known vulnerabilities, unmaintained packages, outdated frameworks, and components pulled from untrusted sources.

### A07: Identification and Authentication Failures

Findings related to authentication mechanisms — weak password policies, credential stuffing vulnerabilities, missing multi-factor authentication, exposed session identifiers, and improper session management.

### A08: Software and Data Integrity Failures

Findings where code or data integrity is not verified — unsigned deployments, insecure deserialisation, CI/CD pipeline vulnerabilities, and auto-update mechanisms without integrity verification.

### A09: Security Logging and Monitoring Failures

Missing or inadequate logging of security-relevant events — failed login attempts, authorisation failures, input validation rejections, and suspicious activity patterns not being captured or alerted upon.

### A10: Server-Side Request Forgery (SSRF)

Findings where the application fetches remote resources based on user-supplied URLs without proper validation or restriction. Includes internal network scanning, cloud metadata access, and protocol smuggling.

## Categorization Process

1. **Map each finding** to one or more OWASP categories based on the vulnerability type and CWE reference
2. **Resolve ambiguity** — when a finding could fit multiple categories, assign the primary category based on the root cause (e.g., an XSS vulnerability caused by missing input validation maps to A03 Injection, not A05 Misconfiguration)
3. **Flag uncategorised findings** — any vulnerability that does not clearly map to an OWASP category is placed in an "Other" group with a note explaining why
4. **Count and summarize** — produce a category-level summary showing the number of findings per OWASP category

## Output Format

Returns findings grouped by OWASP category, with each group containing:

1. **Category code and name** (e.g., A03: Injection)
2. **Finding count** for this category
3. **List of findings** — each retaining all fields from the scan output plus the assigned category
4. **Category summary** — one-paragraph assessment of the overall risk in this category

A final summary table shows the distribution of findings across all ten categories.

## Limitations

Categorization relies on the quality and completeness of the upstream scan findings. If the scanner missed a vulnerability, it will not appear in any category. Design-level weaknesses (A04) and logging gaps (A09) are particularly difficult to detect from code alone and may require manual review to identify.
