---
type: skill
id: remediation-planning
title: Remediation Planning
description: "Generates specific fix recommendations per finding with code examples, effort estimates, and dependency mapping"
tags: [Production, Security, Audit, Remediation]
connections:
  - target: llm-service
    type: runs_on
  - target: security-finding-template
    type: references
metadata:
  complexity: high
  avg_tokens: 2500
  output_format: structured
---

## Capability

Takes severity-rated security findings and produces a detailed remediation plan for each one. Each plan includes a specific fix recommendation, code examples showing the before and after, effort estimates, and identification of dependencies between fixes. The output is structured to help engineering teams prioritize and schedule remediation work.

## Remediation Components

### Fix Recommendations

For each finding, the plan specifies:

- **What to change** — the exact file, function, or configuration entry that needs modification
- **How to fix it** — step-by-step instructions a developer can follow
- **Code example** — a before-and-after comparison showing the vulnerable pattern and the secure replacement
- **Alternative approaches** — where multiple valid fixes exist, list them with trade-offs

### Effort Estimates

Each fix is classified by implementation effort:

| Level | Description | Typical Duration |
|-------|-------------|------------------|
| **Trivial** | Configuration change, one-line fix, or dependency update | Under 1 hour |
| **Small** | Localized code change in a single file or function | 1–4 hours |
| **Medium** | Changes spanning multiple files or requiring new utility functions | 4–16 hours |
| **Large** | Architectural change, new middleware, or significant refactoring | 1–3 days |
| **Major** | Fundamental redesign of a subsystem or security model | 3+ days |

### Dependency Mapping

Fixes are analyzed for dependencies on one another:

- **Blocking dependencies** — Fix A must be completed before Fix B can be applied (e.g., introducing a sanitisation utility before updating all call sites to use it)
- **Recommended ordering** — Fix A is not strictly required before Fix B, but doing A first makes B simpler
- **Independent fixes** — can be worked on in parallel without coordination

### Quick Wins

The plan highlights "quick wins" — fixes that are trivial to implement but significantly reduce risk. These are flagged for immediate action regardless of the broader remediation schedule.

## Remediation Strategies by Category

### Injection Fixes

- Replace string concatenation with parameterised queries or prepared statements
- Introduce input validation and sanitisation at the boundary layer
- Apply context-aware output encoding for XSS prevention
- Use allowlists rather than denylists for permitted input patterns

### Authentication and Access Control Fixes

- Add middleware or decorators that enforce authentication on all protected routes
- Implement role-based access control with explicit permission checks
- Migrate to secure session management (cryptographically random tokens, server-side storage, appropriate expiry)
- Remove hardcoded credentials and migrate to secrets management

### Configuration Fixes

- Disable debug mode in production environments
- Add missing security headers via middleware or server configuration
- Tighten CORS policies to specific allowed origins
- Update default credentials and enforce credential rotation

### Dependency Fixes

- Update vulnerable packages to patched versions
- Replace unmaintained dependencies with actively supported alternatives
- Pin dependency versions and verify lock file integrity
- Add automated dependency scanning to the CI/CD pipeline

## Output Format

Returns a structured remediation plan containing:

1. **Remediation summary** — total findings, breakdown by effort level, estimated total remediation time
2. **Quick wins** — list of trivial fixes that should be applied immediately
3. **Ordered fix list** — all findings with their fix recommendations, sorted by a combination of severity and effort (high severity + low effort first)
4. **Dependency graph** — which fixes depend on or relate to other fixes
5. **Suggested sprint plan** — a recommended grouping of fixes into work batches based on dependencies and effort

## Limitations

Code examples are illustrative and may need adaptation to the specific codebase's patterns, frameworks, and coding standards. Effort estimates are approximate and assume a developer familiar with the codebase and technology stack. Actual effort may vary based on test coverage, code complexity, and organisational processes (code review, QA, deployment).
