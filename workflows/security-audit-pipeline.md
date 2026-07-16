---
type: workflow
id: security-audit-pipeline
title: Security Audit Pipeline
description: "Orchestrates a full security audit: scan for vulnerabilities, categorize by OWASP type, assess severity, plan remediation, and produce an executive report"
tags: [Production, Security, Audit, OWASP]
connections:
  - target: vulnerability-scanning
    type: uses
  - target: finding-categorisation
    type: uses
  - target: severity-assessment
    type: uses
  - target: remediation-planning
    type: uses
  - target: executive-reporting
    type: uses
  - target: language-polish
    type: uses
  - target: consistency-check
    type: uses
  - target: brief-compliance-check
    type: uses
  - target: llm-service
    type: runs_on
  - target: owasp-top-10-reference
    type: references
  - target: security-finding-template
    type: references
metadata:
  estimated_duration: "60-180 seconds"
  avg_tokens: 10000
  trigger: manual
output_step: "language-polish"
composite_steps:
  - "vulnerability-scanning"
  - "finding-categorisation"
  - "severity-assessment"
  - "remediation-planning"
  - "executive-reporting"
  - "language-polish"
  - "consistency-check"
  - "brief-compliance-check"
execution:
  - skill: "vulnerability-scanning"
    prompt: "scan-vulnerabilities"
    step_type: "synthesis"
    output: { name: "vulnerabilities", type: "list" }
  - skill: "finding-categorisation"
    prompt: "categorise-findings"
    step_type: "synthesis"
    output: { name: "categorised_findings", type: "text" }
  - skill: "severity-assessment"
    prompt: "assess-severity"
    step_type: "synthesis"
    output: { name: "severity_assessment", type: "text" }
  - skill: "remediation-planning"
    prompt: "plan-remediation"
    step_type: "synthesis"
    output: { name: "remediation_plan", type: "text" }
  - skill: "executive-reporting"
    prompt: "write-executive-report"
    step_type: "synthesis"
    output: { name: "executive_report", type: "text" }
  - skill: "language-polish"
    prompt: "polish-language"
    step_type: "content"
    output: { name: "polished_report", type: "text" }
    context:
      voice_profile: "Neutral professional tone"
      grammar_strictness: "Professional"
  - parallel:
    - skill: "consistency-check"
      prompt: "check-consistency"
      step_type: "review"
      output: { name: "consistency_verdict", type: "decision" }
      context:
        voice_profile: "Neutral professional tone"
        consistency_strictness: "Standard"
    - skill: "brief-compliance-check"
      prompt: "check-brief-compliance"
      step_type: "review"
      output: { name: "compliance_verdict", type: "decision" }
      context:
        audience_profile: "General professional audience"
        compliance_brief: "No specific compliance requirements"
        compliance_depth: "Standard"
---

## Overview

This workflow runs a complete security audit on a codebase. It scans for vulnerabilities, categorizes findings by OWASP type, rates severity, produces a remediation plan, and generates a two-part report for executives and engineers. The pipeline is sequential — each stage builds on the output of the previous one.

## Pipeline Stages

### Stage 1: Vulnerability Scanning

**Skill:** vulnerability-scanning

**Input:** {{input.codebase_path}}, {{input.audit_scope}}, {{input.language_framework}}

Scan the target codebase for security vulnerabilities across five categories: injection flaws, authentication and authorisation issues, exposed secrets, insecure configurations, and dependency vulnerabilities. The scan references the OWASP Top 10 reference card for classification context.

**Output:** Structured list of raw findings with location, description, evidence, and CWE references.

### Stage 2: Finding Categorization

**Skill:** finding-categorisation

**Input:** {{steps.Vulnerability Scanning.output}}

Take the raw scan findings and organize them into OWASP Top 10 (2021) categories. Each finding is assigned to a single primary category based on its root cause. The output includes a per-category summary and a distribution table.

**Output:** Findings grouped by OWASP category with category-level summaries.

### Stage 3: Severity Assessment

**Skill:** severity-assessment

**Input:** {{steps.Finding Categorization.output}}

Evaluate each categorized finding using structured criteria: exploitability (attack vector, authentication, complexity), impact (confidentiality, integrity, availability), and scope (user base, system reach, recovery difficulty). Assign a severity rating of critical, high, medium, or low.

**Output:** Severity-rated findings with justifications and a priority ranking.

### Stage 4: Remediation Planning

**Skill:** remediation-planning

**Input:** {{steps.Severity Assessment.output}}

Generate a detailed remediation plan. Each finding receives a specific fix recommendation with code examples (before and after), an effort estimate, and dependency mapping. The plan identifies quick wins and suggests a phased work schedule.

**Output:** Complete remediation plan with ordered fix list, dependency map, and suggested work batches.

### Stage 5: Executive Reporting

**Skill:** executive-reporting

**Input:** {{steps.Remediation Planning.output}}, {{input.stakeholder_audience}}

Produce the final two-part report. The executive summary covers risk posture, key metrics, top risks, remediation roadmap, and compliance implications — written for non-technical stakeholders. The detailed technical report provides full finding details, code examples, and fix instructions for the engineering team.

**Output:** Two-part audit report (executive summary + technical findings).

## Error Handling

- If the vulnerability scan finds no issues, the pipeline completes with a clean-audit report stating the scope of what was checked
- If categorization encounters a finding that does not map to any OWASP category, it is placed in an "Other" group and flagged for manual review
- If the codebase path is inaccessible, the pipeline aborts at Stage 1 with a clear error message
- Each stage validates that it received non-empty input from the previous stage before proceeding

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.codebase_path}}` | Yes | Path to the codebase to audit | `/projects/my-api` |
| `{{input.audit_scope}}` | Yes | Audit categories to include | `full scan` |
| `{{input.language_framework}}` | Yes | Primary language and framework | `TypeScript/Express` |
| `{{input.stakeholder_audience}}` | Yes | Who will read the executive summary | `Board of directors and VP of Engineering` |

## Outputs

| Name | Description |
|------|-------------|
| Executive summary | Risk posture overview for non-technical stakeholders |
| Technical report | Detailed findings with fix instructions for engineers |
| Remediation plan | Prioritized fix list with effort estimates and dependencies |
| Severity summary | Finding counts by severity level |
| OWASP distribution | Finding counts by OWASP Top 10 category |

## Setup

Before running this workflow:

1. **Codebase access** — ensure the skrptiq app has filesystem read access to the target codebase directory
2. **LLM provider** — configure a language model provider in your skrptiq settings. A model with strong reasoning capabilities is recommended for the severity assessment and remediation planning stages.

No external APIs, MCP servers, or additional credentials are required beyond your configured LLM provider.

## Provider Notes

- **Stages 1–3** (scanning, categorization, severity) benefit from a model with strong analytical and classification capabilities
- **Stage 4** (remediation) benefits from a model with code generation strength for producing fix examples
- **Stage 5** (reporting) benefits from a model with strong writing capabilities for producing clear, audience-appropriate prose
- The full pipeline is token-intensive — expect 8,000–12,000 tokens across all stages for a moderately sized codebase
- Each stage receives the full output of the previous stage, so context window size matters

## Example Input

To test this workflow immediately after import:

```
Codebase path: /projects/sample-express-api
Audit scope: full scan
Language and framework: TypeScript/Express
Stakeholder audience: CTO and engineering leads
```

Point it at any codebase you have read access to. The pipeline adapts to any language and framework — specify yours in the input to get language-specific vulnerability patterns and fix examples.
