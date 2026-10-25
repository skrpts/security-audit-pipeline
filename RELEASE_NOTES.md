# Release Notes

## v1.0.27
GH#1100 consumer wave (batch 3) — declare the data flow this workflow was resolving positionally.

Each stage below now receives its input through a declared `bindings:` entry instead of `default_from_previous`, which K-073 revoked. **The value is unchanged**: every binding names the step that immediately precedes it, which is exactly what the positional default resolved to. What changes is that the dependency is stated rather than inferred from position, so a reorder can no longer silently feed a stage the wrong artefact.

Bindings added:
  `polish-language` ← `from_step: "Executive Reporting"` (step 6)
  `check-consistency` ← `from_step: "Language Polish"` (step 7)
  `check-brief-compliance` ← `from_step: "Consistency Check"` (step 8)

Dependency pins moved to the published versions that declare these slots:
  `brief-compliance-check` 1.0.1 → 1.0.2
  `check-brief-compliance` 1.0.1 → 1.0.4
  `check-consistency` 1.0.5 → 1.0.6
  `polish-language` 1.0.1 → 1.0.7

## v1.0.26
GH#845 — republish with American English (en-US) content, completing the source-only GH#805 flip that never reached the Hub. Copy only — no functional or behaviour change.

## v1.0.25
GH#745 — declare per-step `output: {name, type}` on every execution step (vulnerabilities/list, categorised_findings/text, severity_assessment/text, remediation_plan/text, executive_report/text, polished_report/text, consistency_verdict/decision, compliance_verdict/decision). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.0.24
Fix-forward after Row 3b v1.0.23 publish failure. The v1.0.23 per-skrpt CI's "Register version with Hub API" step failed because the consumer's source `manifest.id` (c9d5e32b…) did not match the D1 catalog row's id (a80ac015…) — a legacy drift from before Action 6 (`0bcc5ae0`) made publish-skrpt.mjs Step 2 INSERT use `manifest.id` for the D1 id column. v1.0.24 reconciles the source `manifest.id` to the catalog authoritative value (Row-5-equivalent for consumers) and republishes. Per Adj-1: no re-tag of v1.0.23; the orphaned GitHub release artefact stays inert (no D1 versions row, no consumer pinned it).

## v1.0.23
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 7 inline shared-content files and declare 7 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.22
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.21
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.20
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.19
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.18
Initial catalog release with full structural and content-quality validation. All scanner checks pass.
