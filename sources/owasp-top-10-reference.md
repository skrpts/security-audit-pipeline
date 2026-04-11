---
type: source
id: owasp-top-10-reference
title: OWASP Top 10 Reference
description: "Reference card listing the OWASP Top 10 2021 categories with descriptions and common examples"
tags: [Production, Security, OWASP, Reference]
connections: []
metadata:
  last_updated: "2021-09-24"
  source_url: "owasp.org/Top10/"
  version: "2021"
---

## Purpose

This reference card provides a concise summary of the OWASP Top 10 (2021 edition) — the most widely recognised standard for web application security risks. The finding categorisation skill uses this as its classification framework.

## OWASP Top 10 — 2021 Edition

### A01:2021 — Broken Access Control

Moves up from fifth position. 94% of applications were tested for some form of broken access control. Access control enforces policy so that users cannot act outside their intended permissions. Failures typically lead to unauthorised information disclosure, modification, or destruction of data, or performing a business function outside the user's limits.

**Common Examples:**
- Bypassing access control checks by modifying the URL, application state, or HTML page
- Allowing the primary key to be changed to another user's record (insecure direct object references)
- Elevation of privilege — acting as a user without being logged in, or acting as an admin when logged in as a user
- CORS misconfiguration allowing access from unauthorised origins

### A02:2021 — Cryptographic Failures

Shifts up one position, previously known as "Sensitive Data Exposure." The renewed focus is on failures related to cryptography, which often lead to sensitive data exposure or system compromise.

**Common Examples:**
- Data transmitted in cleartext (HTTP, SMTP, FTP)
- Old or weak cryptographic algorithms in use (MD5, SHA-1, DES)
- Default or weak encryption keys, or no key rotation
- Missing or improper certificate validation
- Passwords stored using reversible encryption rather than adaptive hashing (Argon2, bcrypt)

### A03:2021 — Injection

Drops to third position. 94% of applications were tested for some form of injection. Cross-site scripting is now included in this category.

**Common Examples:**
- SQL injection via string concatenation in queries
- OS command injection through unsanitised input
- Cross-site scripting (XSS) — stored, reflected, and DOM-based
- LDAP injection, XPath injection, NoSQL injection
- Server-side template injection

### A04:2021 — Insecure Design

A new category for 2021, focusing on risks related to design and architectural flaws. Calls for greater use of threat modelling, secure design patterns, and reference architectures.

**Common Examples:**
- Missing or ineffective threat modelling during design
- Business logic flaws allowing abuse of legitimate features
- Insufficient rate limiting on sensitive operations
- Missing defence-in-depth (relying on a single control layer)
- Trust boundary violations

### A05:2021 — Security Misconfiguration

Moves up from sixth position. With the shift towards highly configurable software, it is no surprise that this category moves up. Includes the former XML External Entities (XXE) category.

**Common Examples:**
- Missing or permissive security headers
- Unnecessary features enabled (ports, services, pages, accounts, privileges)
- Default accounts and passwords unchanged
- Error handling revealing stack traces or internal information
- XML external entity (XXE) processing enabled
- Missing security hardening across the application stack

### A06:2021 — Vulnerable and Outdated Components

Previously titled "Using Components with Known Vulnerabilities." This is the only category without any CVEs mapped to it, so a default exploits/impact weight of 5.0 is used.

**Common Examples:**
- Running software with known vulnerabilities (including the OS, web/application server, DBMS, applications, APIs, and all components)
- Not scanning for vulnerabilities regularly
- Not fixing or upgrading underlying platforms, frameworks, and dependencies in a timely fashion
- Using components from untrusted sources

### A07:2021 — Identification and Authentication Failures

Previously "Broken Authentication," this category has slid down from second position. It now includes CWEs related to identification failures.

**Common Examples:**
- Permitting brute-force or credential-stuffing attacks
- Permitting weak or well-known passwords
- Using plain text, encrypted, or weakly hashed passwords
- Missing or ineffective multi-factor authentication
- Exposing session identifiers in the URL
- Reusing session identifiers after successful login

### A08:2021 — Software and Data Integrity Failures

A new category for 2021, focusing on making assumptions related to software updates, critical data, and CI/CD pipelines without verifying integrity. Includes the former "Insecure Deserialisation" category.

**Common Examples:**
- Applications relying on plugins, libraries, or modules from untrusted sources
- Insecure CI/CD pipeline introducing the opportunity for unauthorised access or malicious code
- Auto-update functionality downloading updates without integrity verification
- Insecure deserialisation of untrusted data

### A09:2021 — Security Logging and Monitoring Failures

Previously "Insufficient Logging and Monitoring." This category helps detect, escalate, and respond to active breaches. Without logging and monitoring, breaches cannot be detected.

**Common Examples:**
- Auditable events (logins, failed logins, high-value transactions) not being logged
- Warnings and errors generating no, inadequate, or unclear log messages
- Logs not being monitored for suspicious activity
- Logs only stored locally with no centralised aggregation
- Alerting thresholds and response escalation processes not in place or effective

### A10:2021 — Server-Side Request Forgery (SSRF)

A new category added from the community survey. SSRF flaws occur whenever a web application fetches a remote resource without validating the user-supplied URL.

**Common Examples:**
- Fetching internal resources by manipulating the URL parameter
- Accessing cloud service metadata endpoints (e.g., `http://169.254.169.254/`)
- Scanning internal networks or ports through the application
- Protocol smuggling to bypass firewalls or access controls
