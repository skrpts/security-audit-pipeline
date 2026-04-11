---
type: skill
id: severity-assessment
title: Severity Assessment
description: "Rates each finding by severity using CVSS-like criteria: exploitability, impact, and affected scope"
tags: [Production, Security, Audit, Risk]
connections:
  - target: llm-service
    type: runs_on
metadata:
  complexity: high
  avg_tokens: 1800
  rating_scale: [critical, high, medium, low]
---

## Capability

Evaluates each categorised security finding and assigns a severity rating (critical, high, medium, or low) based on a structured assessment of exploitability, impact, and scope. The methodology draws from the Common Vulnerability Scoring System (CVSS) to produce consistent, defensible ratings.

## Severity Levels

### Critical

Findings that are directly exploitable with minimal effort and result in severe consequences. Characteristics:

- **Exploitability:** Can be triggered remotely, without authentication, using widely available tools or techniques
- **Impact:** Full system compromise, mass data breach, complete loss of confidentiality, integrity, or availability
- **Scope:** Affects all users or the entire system; may propagate to connected systems

Examples: unauthenticated remote code execution, SQL injection on a login endpoint, exposed admin credentials with no MFA.

### High

Findings that are exploitable with moderate effort and result in significant damage. Characteristics:

- **Exploitability:** Requires some preconditions (e.g., authenticated access, specific user interaction) but is reliably reproducible
- **Impact:** Significant data exposure, privilege escalation, or service disruption for a subset of users
- **Scope:** Affects a significant portion of the system or user base

Examples: stored XSS in a shared dashboard, broken access control allowing horizontal privilege escalation, use of a deprecated cryptographic algorithm for password hashing.

### Medium

Findings that require specific conditions to exploit and result in limited damage. Characteristics:

- **Exploitability:** Requires a chain of conditions, insider access, or social engineering to trigger
- **Impact:** Limited data exposure, localised functionality disruption, or information leakage that aids further attacks
- **Scope:** Affects a small number of users or a non-critical component

Examples: reflected XSS requiring a crafted URL, verbose error messages leaking internal paths, missing rate limiting on a non-critical endpoint.

### Low

Findings that represent best-practice deviations or defence-in-depth gaps with minimal direct risk. Characteristics:

- **Exploitability:** Theoretical or requires an already-compromised system to exploit
- **Impact:** Minimal direct harm; primarily increases attack surface or reduces defence layers
- **Scope:** Localised to a single component with no cascading effect

Examples: missing security headers on a non-sensitive page, outdated dependency with no known exploit in the current usage context, overly broad CORS policy on an internal-only endpoint.

## Assessment Criteria

Each finding is evaluated across three dimensions:

### 1. Exploitability

| Factor | Question |
|--------|----------|
| Attack vector | Can this be exploited remotely, or only locally? |
| Authentication | Does exploitation require authenticated access? |
| Complexity | How many preconditions must be met? |
| User interaction | Does the victim need to take an action? |
| Tooling | Are exploit tools or techniques publicly available? |

### 2. Impact

| Factor | Question |
|--------|----------|
| Confidentiality | What data could be exposed? How sensitive is it? |
| Integrity | Could an attacker modify data or system behaviour? |
| Availability | Could this cause a denial of service or data loss? |
| Compliance | Does this finding trigger regulatory obligations? |

### 3. Scope

| Factor | Question |
|--------|----------|
| User base | How many users are affected? |
| System reach | Does exploitation affect only the vulnerable component, or can it propagate? |
| Data volume | How much data is at risk? |
| Recovery | How difficult is it to recover from successful exploitation? |

## Output Format

Returns the original categorised findings with added severity fields:

1. **Severity rating** — critical, high, medium, or low
2. **Exploitability score** — brief justification of the exploitability assessment
3. **Impact score** — brief justification of the impact assessment
4. **Scope assessment** — brief justification of the scope evaluation
5. **Rationale** — one-paragraph explanation of why this rating was assigned
6. **Priority rank** — numerical rank within the full findings list, ordered by severity

A summary table shows the count of findings at each severity level.

## Limitations

Severity assessment is contextual — the same vulnerability may be critical in one application and low in another depending on the data it handles, the user base, and the deployment environment. This skill assesses severity based on the information available in the code and configuration. Deployment-specific factors (network segmentation, WAF rules, monitoring coverage) are not accounted for unless explicitly provided.
