---
type: prompt
id: write-executive-report
title: Write Executive Report
description: "Produces a two-part audit report: executive summary for leadership and detailed technical findings for engineers"
tags: [Production, Security, Audit, Reporting]
inputs:
  stakeholder_audience:
    label: "Stakeholder Audience"
    description: "Who will read the executive summary — determines tone and detail level"
    example: "Board of directors and VP of Engineering"
    required: true
    type: text
connections:
  - target: executive-reporting
    type: derived_from
metadata:
  output_format: markdown
  avg_tokens: 2000
---

## Purpose

Generates the final deliverable of the security audit pipeline: a two-part report with an executive summary for non-technical stakeholders and a detailed technical report for the engineering team.

## Prompt

You are a security consultant writing the final report for a security audit. Using the remediation plan below, produce two distinct report sections tailored to different audiences.

**Remediation plan:** {{steps.previous.output}}
**Stakeholder audience:** {{input.stakeholder_audience}}

---

### Part 1: Executive Summary

Write this section for **{{input.stakeholder_audience}}**. Assume they understand business risk but not technical security terminology.

#### 1.1 Risk Posture Statement

One paragraph summarizing the overall security health. Use language like "acceptable risk," "elevated risk," or "critical risk requiring immediate action." State the headline numbers: total findings, critical count, estimated remediation effort.

#### 1.2 Key Metrics

Present a metrics table:

| Metric | Value |
|--------|-------|
| Total findings | |
| Critical | |
| High | |
| Medium | |
| Low | |
| Estimated remediation effort | |
| Quick wins available | |

#### 1.3 Top Risks

Describe the three to five most significant findings in plain language. For each:

- What is the risk, in one sentence a non-technical person can understand?
- What is the potential business impact? (data breach, service outage, regulatory fine, reputational damage)
- What is the recommended action?

#### 1.4 Remediation Roadmap

Present a high-level timeline:

- **Immediate (this week):** critical fixes and quick wins
- **Short-term (this month):** high-severity fixes
- **Medium-term (this quarter):** medium and low-severity improvements

#### 1.5 Compliance Implications

Note any findings relevant to compliance frameworks (SOC 2, GDPR, PCI-DSS, HIPAA, ISO 27001). State which controls are affected. If no compliance implications exist, state that explicitly.

#### 1.6 Recommendation

One clear, direct recommendation: proceed, proceed with conditions, or halt until critical issues are resolved.

---

### Part 2: Detailed Technical Report

Write this section for security engineers and developers. Use precise technical language.

#### 2.1 Audit Scope

What was scanned, what was excluded, and the scanning methodology used.

#### 2.2 Findings by OWASP Category

For each category with findings, present every finding using this structure:

**[ID] [Finding Title]** — [Severity]

| Field | Detail |
|-------|--------|
| Category | [OWASP category] |
| CWE | [CWE reference] |
| Location | [file:lines] |
| Exploitability | [assessment] |
| Impact | [assessment] |

**Description:** [What the vulnerability is]

**Evidence:** [Code snippet]

**Remediation:** [Fix steps with code example]

**Effort:** [Estimate]

#### 2.3 Dependency Audit

Findings related to third-party packages with CVE references and upgrade paths.

#### 2.4 Priority Matrix

A combined view sorting all findings by severity and effort.

#### 2.5 Appendices

- Full finding inventory table
- Glossary of security terms used

---

### Formatting Rules

- Use British English throughout both sections
- The executive summary must stand alone — a reader should not need the technical report to understand the risk posture
- The technical report must be detailed enough for an engineer to fix every issue without asking follow-up questions
- Do not duplicate content between sections — the executive summary describes risk, the technical report describes fixes
- Keep the tone professional and objective throughout
