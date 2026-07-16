---
type: skill
id: executive-reporting
title: Executive Reporting
description: "Produces an executive summary and detailed technical findings report for separate audiences"
tags: [Production, Security, Audit, Reporting]
connections:
  - target: llm-service
    type: runs_on
  - target: security-finding-template
    type: references
metadata:
  complexity: medium
  avg_tokens: 2000
  output_audiences: [executive, engineering]
---

## Capability

Generates a two-part security audit report from the completed remediation plan. The executive summary provides a high-level risk posture overview for non-technical stakeholders. The detailed technical report gives engineers the specifics they need to understand, prioritize, and fix each issue.

## Executive Summary

The executive summary is written for board members, product managers, and other non-technical stakeholders. It avoids jargon and focuses on business impact.

### Contents

1. **Risk Posture Statement** — one-paragraph assessment of the overall security health of the codebase. States whether the risk level is acceptable, elevated, or critical.

2. **Key Metrics Dashboard**

   | Metric | Value |
   |--------|-------|
   | Total findings | {count} |
   | Critical | {count} |
   | High | {count} |
   | Medium | {count} |
   | Low | {count} |
   | Estimated remediation effort | {total hours/days} |
   | Quick wins available | {count} |

3. **Top Risks** — the three to five most significant findings, each described in plain language with the potential business impact (data breach, service outage, regulatory penalty, reputational damage).

4. **Remediation Roadmap** — a high-level timeline showing when critical and high-severity issues will be addressed, grouped into immediate (this week), short-term (this month), and medium-term (this quarter).

5. **Compliance Implications** — any findings that may affect compliance with relevant frameworks (SOC 2, GDPR, PCI-DSS, HIPAA, ISO 27001). States which controls are affected and whether remediation is required for continued compliance.

6. **Recommendation** — a clear, actionable recommendation: proceed with caution, pause deployment until critical issues are resolved, or proceed with confidence.

## Detailed Technical Report

The technical report is written for security engineers, developers, and DevOps teams. It uses precise technical language and includes code-level detail.

### Contents

1. **Audit Scope** — what was scanned (repositories, branches, languages, frameworks), what was excluded, and why.

2. **Methodology** — the scanning approach, categorization framework (OWASP Top 10), and severity rating criteria used.

3. **Findings by Category** — all findings grouped by OWASP category, each containing:
   - Finding title and unique identifier
   - Severity rating with justification
   - Affected file(s) and line range(s)
   - Vulnerability description with CWE reference
   - Evidence (code snippet showing the vulnerable pattern)
   - Remediation steps with code example
   - Effort estimate and dependencies

4. **Dependency Audit Results** — findings related to third-party packages, including CVE references, affected versions, and upgrade paths.

5. **Remediation Priority Matrix** — a table combining severity and effort to produce a prioritized action list.

6. **Appendices**
   - Full finding inventory (for reference and tracking)
   - OWASP category distribution chart data
   - Glossary of security terms used in the report

## Tone and Style

- **Executive summary:** conversational but authoritative. No acronyms without expansion. Focus on "so what" — what does this mean for the business?
- **Technical report:** precise and factual. Use standard security terminology. Include enough detail for an engineer to reproduce and fix each issue without additional context.
- **Both sections:** use British English throughout. Maintain a professional, objective tone. Do not exaggerate risks or downplay findings.

## Limitations

The report reflects the findings of the automated scan and analysis pipeline. It does not include findings from manual penetration testing, runtime analysis, or social engineering assessments. The executive summary's business impact assessments are based on common risk scenarios and may not account for organisation-specific factors such as insurance coverage, incident response capabilities, or contractual obligations.
