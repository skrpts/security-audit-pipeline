---
type: skill
id: brief-compliance-check
title: Brief Compliance Check
description: "Checks output against its stated brief — required sections, constraints, and missing deliverables"
tags: [Production, Quality]
connections:
  - target: llm-service
    type: runs_on
context_params:
  compliance_brief:
    label: "Compliance Brief"
    description: "The original brief, specification, or requirements to check against"
    required: false
  audience_profile:
    label: "Audience Profile"
    description: "Target audience for checking content relevance and tone appropriateness"
    required: false
  compliance_depth:
    label: "Compliance Depth"
    description: "How thoroughly to check compliance — Overview, Standard, Thorough, or Exhaustive"
    default: "Standard"
    required: false
---

## Capability

Compares a completed output against the brief or specification it was supposed to fulfil. Identifies missing sections, unmet requirements, violated constraints, and incomplete deliverables.

Works with any structured brief — content briefs, feature specs, essay outlines, documentation checklists, research proposals, or release plans. The brief defines what "done" looks like; this skill checks whether the output matches.

## When to Use

- After a drafting or assembly stage, before final polish
- When a workflow produces output against a defined specification
- To validate that multi-stage pipelines haven't dropped requirements between stages

## What It Checks

1. **Required sections** — are all sections specified in the brief present in the output?
2. **Constraints** — does the output respect word counts, format requirements, or scope boundaries?
3. **Deliverables** — are all expected outputs present (e.g. summary + recommendations + appendix)?
4. **Omissions** — are there requirements in the brief that the output doesn't address at all?

## What It Does NOT Do

- Judge quality or correctness of content (use `evidence-claim-check` for that)
- Check grammar or spelling (use `language-polish`)
- Enforce style consistency (use `consistency-check`)

## Inputs

- The output to check
- The brief, specification, or checklist to check against

## Outputs

A compliance report listing: sections present, sections missing, constraints met/violated, and an overall pass/fail assessment with specific remediation suggestions.
