---
type: asset
id: security-finding-template
title: Security Finding Template
description: "Structured template for documenting individual security findings consistently across the audit"
tags: [Production, Security, Audit, Template]
connections: []
metadata:
  asset_type: template
  format: markdown
---

## Security Finding Template

Use this template to document each security finding consistently. Copy one instance per finding and fill in all fields. Remove guidance text in square brackets before finalising the report.

---

### Finding: [FINDING-ID]

## [Finding Title]

| Field | Value |
|-------|-------|
| **ID** | [FINDING-NNN] |
| **Severity** | [Critical / High / Medium / Low] |
| **OWASP Category** | [A01–A10: Category Name] |
| **CWE** | [CWE-NNN: Title] |
| **Location** | [`path/to/file.ts:42-58`] |
| **Confidence** | [High / Medium / Low] |
| **Status** | [Open / In Progress / Resolved / Accepted Risk] |

### Description

[One to two paragraphs explaining the vulnerability in plain language. Describe what the issue is, where it occurs, and under what conditions it can be exploited. A non-specialist should be able to understand the risk after reading this section.]

### Evidence

```
[Paste the relevant code snippet, configuration entry, or log output that demonstrates the vulnerability. Include enough context for a developer to locate the issue.]
```

### Exploitability Assessment

| Factor | Rating | Notes |
|--------|--------|-------|
| Attack vector | [Remote / Local / Physical] | [Brief justification] |
| Authentication required | [None / Low / High] | [Brief justification] |
| Complexity | [Low / Medium / High] | [Brief justification] |
| User interaction | [None / Required] | [Brief justification] |

### Impact Assessment

| Factor | Rating | Notes |
|--------|--------|-------|
| Confidentiality | [None / Low / High] | [What data could be exposed] |
| Integrity | [None / Low / High] | [What could be modified] |
| Availability | [None / Low / High] | [Could this cause downtime or data loss] |
| Compliance | [None / Relevant] | [Which frameworks are affected, if any] |

### Remediation

**Recommended fix:**

[Step-by-step instructions for fixing the vulnerability.]

**Before (vulnerable):**

```
[Code showing the current vulnerable pattern]
```

**After (secure):**

```
[Code showing the fixed pattern]
```

**Effort estimate:** [Trivial / Small / Medium / Large / Major]

**Dependencies:** [List any other findings that must be fixed first, or state "None"]

### References

- [Link to relevant OWASP page]
- [Link to CWE entry]
- [Link to language or framework security documentation]
- [Link to any relevant CVE if applicable]

### Notes

[Any additional context, caveats, or considerations. For example: "This finding is mitigated in production by the WAF rule set, but the underlying code should still be fixed." Or: "This pattern appears in 12 additional locations — see the full inventory in the appendix."]

---

**Template usage notes:**

- Assign finding IDs sequentially: FINDING-001, FINDING-002, etc.
- Use consistent severity ratings aligned with the severity assessment criteria in this pipeline
- All text should use British English
- Remove this usage notes section from the final report
