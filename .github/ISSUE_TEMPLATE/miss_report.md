---
name: Miss / Bypass Report
about: Report a secret, credential, PII, or trade secret that SAIF failed to intercept
title: "[MISS]: "
labels: miss-report, security
assignees: ''
---

**Prompt Snippet (Sanitized - DO NOT include genuine production secrets)**:
```text
<paste sanitized prompt here>
```

**Target Model Profile**:
- [ ] SAIF Light (Deterministic Fast-Path)
- [ ] SAIF Neural (512 Tokens)
- [ ] SAIF Deep Neural (8,192 Tokens)

**Expected Detection Entity / Rule**:
(e.g., AWS Secret Key, GitHub PAT, Credit Card, Project Phoenix Trade Secret)

**Actual Result Observed**:
- Verdict: ALLOW / PASS (Should have been BLOCK)

**Context / Evasion Technique Used**:
(e.g., Base64, Leetspeak, Roleplay prompt injection framing, Markdown table spacing)
