---
type: prompt
id: scan-vulnerabilities
title: Scan Vulnerabilities
description: "Instructs the LLM to perform a comprehensive vulnerability scan of the target codebase"
tags: [Production, Security, Audit, OWASP]
inputs:
  codebase_path:
    label: "Codebase Path"
    description: "Path to the codebase directory to scan"
    example: "/projects/my-api"
    required: true
    type: text
  audit_scope:
    label: "Audit Scope"
    description: "Which audit categories to include in the scan"
    example: "full scan"
    required: true
    type: text
  language_framework:
    label: "Language and Framework"
    description: "The primary language and framework of the codebase"
    example: "TypeScript/Express"
    required: true
    type: text
connections:
  - target: vulnerability-scanning
    type: derived_from
metadata:
  output_format: structured
  avg_tokens: 2000
---

## Purpose

Drives the vulnerability scanning skill by providing the LLM with clear instructions on what to scan, how to structure findings, and what level of detail to include. This prompt is the entry point for the entire security audit pipeline.

## Prompt

You are a security auditor performing a vulnerability scan of a codebase. Your task is to systematically examine the code at **{{input.codebase_path}}** for security vulnerabilities.

**Audit scope:** {{input.audit_scope}}
**Language and framework:** {{input.language_framework}}

### Scanning Instructions

Work through the following categories methodically. For each category, examine every relevant file and report any findings.

#### 1. Injection Flaws

Search for patterns where user-controlled input reaches an interpreter without sanitisation:

- SQL queries built with string concatenation or template literals
- OS commands constructed from user input
- HTML output containing unescaped user data (XSS vectors)
- File system operations using user-supplied paths
- Template engines processing user-provided strings

#### 2. Authentication and Authorisation

Examine authentication and access control mechanisms:

- Endpoints or routes missing authentication middleware
- Authorisation checks that can be bypassed or are absent
- Hardcoded credentials, API keys, or tokens in source files
- Session management: token generation, expiry, invalidation
- Password handling: hashing algorithm, salt usage, storage

#### 3. Sensitive Data Exposure

Look for sensitive data handling issues:

- Secrets or PII logged to console, files, or monitoring systems
- Sensitive data transmitted without encryption
- API responses leaking internal details or stack traces
- Configuration files containing credentials not excluded from version control

#### 4. Security Misconfiguration

Check configuration files and server setup:

- Debug mode enabled in production configurations
- Default credentials still in place
- Missing security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- Overly permissive CORS policies
- Unnecessary services or features enabled

#### 5. Dependency Vulnerabilities

Examine package manifests and lock files:

- Dependencies with known CVEs
- Outdated packages with available security patches
- Packages from untrusted or unverified registries
- Lock file inconsistencies

### Output Requirements

For each finding, provide:

1. **Category** — which of the five categories above
2. **Location** — file path and line range
3. **Title** — concise description of the vulnerability
4. **Description** — what the vulnerability is and how it could be exploited
5. **Evidence** — the relevant code snippet
6. **CWE** — Common Weakness Enumeration reference (e.g., CWE-89 for SQL injection)
7. **Confidence** — high, medium, or low

### Rules

- Only report findings supported by evidence in the code — do not speculate
- If a file uses a security library or framework correctly, do not flag it
- Report each distinct vulnerability once, even if the same pattern appears in multiple locations (note the additional locations in the finding)
- Use British English throughout
- If the audit scope excludes certain categories, skip them and note what was excluded
