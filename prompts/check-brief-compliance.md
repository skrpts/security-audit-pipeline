---
type: prompt
id: check-brief-compliance
title: "Check Brief Compliance"
description: "Verifies output meets all requirements from the original brief"
tags: [Production, Quality]
connections:
  - target: brief-compliance-check
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Drives the brief compliance check skill.

## Prompt

You are a quality reviewer. Compare the output below against its original brief and check whether all requirements have been met.

{{step.context.audience_profile}}

If an audience profile is provided above, also check:
- Is the content appropriate for the described audience's expertise level?
- Does the tone match the audience's expectations (formality, directness, jargon tolerance)?
- Are there any audience repellents present in the content?
If no audience profile is provided, check brief compliance only.

### Original Brief

{{step.context.compliance_brief}}

If no brief is provided above, extract the implicit requirements from the output itself — identify what the document claims to deliver (from its title, introduction, and structure) and check whether it delivers on those claims.

### Output to Check

{{steps.previous.output}}

### Compliance Depth: {{step.context.compliance_depth}}

Adjust your review based on the depth level:
- **Overview**: Check the 3-5 most critical requirements only. Quick pass.
- **Standard** (default): Check all stated requirements. Flag missing or violated items.
- **Thorough**: Check all requirements plus implied ones. Assess quality of compliance, not just presence.
- **Exhaustive**: Maximum rigour. Check every requirement, assess quality and completeness, compare against best-in-class examples, and suggest improvements even for met requirements.

### Instructions

For each requirement in the brief, assess:

1. **Met** — the output fully satisfies this requirement
2. **Partially met** — the output addresses this but incompletely
3. **Missing** — the output does not address this at all
4. **Violated** — the output contradicts or breaks this requirement

### Output Format

| Requirement | Status | Notes |
|-------------|--------|-------|
| [requirement from brief] | Met / Partially met / Missing / Violated | [specific explanation] |

After the table, provide:
- **Compliance score** — X of Y requirements fully met
- **Critical gaps** — any missing or violated requirements that must be fixed before the output is acceptable

## Formatting Rules

- Use British English throughout
- Be specific and actionable — no vague recommendations
- Structure output clearly with headings, tables, or lists as appropriate
