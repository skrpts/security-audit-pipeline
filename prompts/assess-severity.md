---
type: prompt
id: assess-severity
title: Assess Severity
description: "Rates each categorized finding by severity using structured exploitability, impact, and scope criteria"
tags: [Production, Security, Audit, Risk]
connections:
  - target: severity-assessment
    type: derived_from
metadata:
  output_format: structured
  avg_tokens: 1800
---

## Purpose

Evaluates each categorized finding and assigns a severity rating based on exploitability, impact, and scope. Produces consistent, defensible ratings that drive remediation priority.

## Prompt

You are a security engineer assessing the severity of vulnerability findings. For each finding in the categorized results below, assign a severity rating and provide a structured justification.

**Categorized findings:** {{steps.previous.output}}

### Severity Scale

Rate each finding as one of:

- **Critical** — directly exploitable with minimal effort; severe consequences (full compromise, mass data breach, complete loss of CIA)
- **High** — exploitable with moderate effort or preconditions; significant damage (major data exposure, privilege escalation, service disruption)
- **Medium** — requires specific conditions to exploit; limited damage (localized exposure, information leakage aiding further attacks)
- **Low** — theoretical risk or defence-in-depth gap; minimal direct harm (increased attack surface, best-practice deviation)

### Assessment Criteria

For each finding, evaluate three dimensions:

#### Exploitability

- Can it be triggered remotely or only locally?
- Does it require authentication?
- How many preconditions must be met?
- Does the victim need to take an action (e.g., click a link)?
- Are exploit tools or techniques publicly available?

#### Impact

- What data could be exposed, and how sensitive is it?
- Could an attacker modify data or system behavior?
- Could this cause denial of service or data loss?
- Does this trigger regulatory or compliance obligations?

#### Scope

- How many users or systems are affected?
- Can exploitation propagate beyond the vulnerable component?
- How much data is at risk?
- How difficult is recovery from successful exploitation?

### Output Format

For each finding, output:

1. All original fields (category, location, title, description, evidence, CWE, confidence)
2. **Severity** — critical, high, medium, or low
3. **Exploitability assessment** — two to three sentences justifying the exploitability evaluation
4. **Impact assessment** — two to three sentences justifying the impact evaluation
5. **Scope assessment** — two to three sentences justifying the scope evaluation
6. **Rationale** — one paragraph summarizing why this severity was assigned
7. **Priority rank** — numerical rank across all findings (1 = highest priority)

End with a severity summary:

| Severity | Count |
|----------|-------|
| Critical | {n} |
| High | {n} |
| Medium | {n} |
| Low | {n} |

### Rules

- Be consistent — similar vulnerabilities in similar contexts should receive similar ratings
- Do not inflate severity to appear thorough; do not downplay severity to minimize the report
- If two findings have equal severity, rank the one with lower remediation effort higher (it should be fixed first)
- Preserve all original finding data — do not modify the categorization or descriptions
- Use British English throughout
