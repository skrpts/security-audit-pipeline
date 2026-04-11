---
type: prompt
id: plan-remediation
title: Plan Remediation
description: "Generates specific fix recommendations with code examples, effort estimates, and dependency mapping"
tags: [Production, Security, Audit, Remediation]
connections:
  - target: remediation-planning
    type: derived_from
metadata:
  output_format: structured
  avg_tokens: 2500
---

## Purpose

Takes severity-rated findings and produces a complete remediation plan with actionable fix recommendations, code examples, effort estimates, and a dependency-aware execution order.

## Prompt

You are a senior security engineer creating a remediation plan for the severity-rated findings below. For each finding, produce a detailed, actionable fix recommendation that a developer can follow without additional context.

**Severity-rated findings:** {{steps.previous.output}}

### For Each Finding, Provide

#### Fix Recommendation

1. **What to change** — specify the exact file, function, or configuration entry
2. **Step-by-step instructions** — numbered steps a developer can follow
3. **Code example** — show the vulnerable pattern (before) and the secure replacement (after) in fenced code blocks
4. **Alternative approaches** — if multiple valid fixes exist, list them with trade-offs (security, complexity, performance)

#### Effort Estimate

Classify the implementation effort:

| Level | Description | Duration |
|-------|-------------|----------|
| Trivial | Configuration change, one-line fix, dependency update | Under 1 hour |
| Small | Localised code change in a single file | 1–4 hours |
| Medium | Changes across multiple files or new utility functions | 4–16 hours |
| Large | New middleware, architectural change, or major refactoring | 1–3 days |
| Major | Fundamental redesign of a subsystem | 3+ days |

#### Dependencies

Note whether this fix:

- **Blocks** another fix (must be done first)
- **Is blocked by** another fix (cannot start until a prerequisite is complete)
- **Is independent** (can be worked on in parallel)

### Remediation Plan Structure

Organise the complete output as follows:

1. **Summary** — total findings, breakdown by effort level, estimated total remediation time

2. **Quick Wins** — list all trivial-effort fixes that significantly reduce risk. These should be applied immediately.

3. **Ordered Fix List** — all findings sorted by priority (high severity + low effort first, then high severity + high effort, then lower severity). Each entry includes the full fix recommendation, effort estimate, and dependencies.

4. **Dependency Map** — a summary showing which fixes must be sequenced and which can run in parallel.

5. **Suggested Work Plan** — group fixes into logical batches that respect dependencies, e.g.:
   - **Batch 1 (immediate):** Quick wins and critical fixes with no blockers
   - **Batch 2 (this week):** High-severity fixes and anything unblocked by Batch 1
   - **Batch 3 (this sprint):** Medium-severity fixes
   - **Batch 4 (backlog):** Low-severity improvements

### Rules

- Every fix must include a code example — no fix recommendation is complete without showing the developer what the result should look like
- Effort estimates assume a developer familiar with the codebase and stack
- Do not recommend "rewrite from scratch" unless the finding genuinely cannot be fixed incrementally
- Preserve all original finding data (category, severity, location, description)
- Use British English throughout
