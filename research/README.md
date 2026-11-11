# VerityMesh Trust Lab — Research Library

**Open Research · Verifiable Security · Digital Trust**

This index helps engineers, researchers, and practitioners find our published research, primary references, hypotheses, and planned experiments.

> **Research integrity:** A published research brief is not necessarily peer reviewed or experimentally validated. We mark author-reported results, our hypotheses, and our independently reproduced findings separately.

## Published research

| ID | Research brief | Status | Focus |
|---|---|---|---|
| 001 | [Authorization Context and API Security Evidence](briefs/001-api-authorization-context-and-evidence.md) | Literature-backed brief; experimental validation pending | OWASP BOLA, API authorization context, business logic, evidence gaps, and a testable approach |

### Research Brief 001: Starting question

**Can developers distinguish documented authentication, missing object-level authorization evidence, and genuinely verified authorization behavior without mistakenly claiming that an OpenAPI specification proves a vulnerability?**

[Read Brief 001](briefs/001-api-authorization-context-and-evidence.md)

## Weekly documentation

- [October 27–November 2, 2026: Weekly research update](weekly-updates/2026-11-02.md) — verified repository status, proposed research refinement, and next actions.
- [November 2, 2026: Documentation check-in](check-ins/2026-11-02.md) — structured capture of sources, problems, competitors, decisions, experiments, blockers, and next actions; unconfirmed inputs remain pending.

## Research examples

- [Example 001 — Banking permission-to-operation authorization](examples/001-banking-permission-to-operation.md) — synthetic brokerage entitlement and reward-redemption scenarios; proposed policies and tests, no runtime results.

## Research planned — not yet published

1. **Brief 002 — Existing API security tools and reproducible comparison.** Evaluate supported capabilities and limitations using comparable synthetic fixtures. Candidate baselines: Spectral, Schemathesis, RESTler, and other relevant tools.
2. **Brief 003 — Explainable authorization evidence for fictional banking and airline workflows.** Define roles, object ownership, state transitions, access-control invariants, expected results and an evidence model.
3. **Experiment 001 — Repeatable authorization tests.** Record software versions, synthetic fixtures, methodology, observed results, false positives and unknowns.

These entries are proposed work, **not completed investigations or claims of product superiority**.

## Primary starting sources

- [OWASP API Security Top 10 (2023)](https://api-security.owasp.org/editions/2023/en/0x11-t10/)
- [NIST SP 800-228: Guidelines for API Protection for Cloud-Native Systems (updated 2026)](https://csrc.nist.gov/pubs/sp/800/228/upd1/final)
- [Security Testing of RESTful APIs: Approaches and Research Challenges (2026)](https://doi.org/10.1016/j.cosrev.2026.101043)
- [Bridging the Gap in Web API Security (2026)](https://doi.org/10.3390/software5020025)
- [Rethinking Broken Object Level Authorization Attacks Under Zero Trust Principle (2026)](https://doi.org/10.1145/3833417)

Please consult original sources and the linked brief for framing, methodological limits, and attribution.

## Product research areas

- **API Trust Lens:** flagship direction. Explain API security evidence and missing authorization assumptions; no vulnerability claims from schema alone.
- **TrueTrace:** future research into media provenance and authenticity evidence, with uncertainty communicated clearly.
- **AgentScope:** future research into AI-agent tool access, permissions, and least-privilege boundaries.

## How to use or review this work

Read a brief, follow the linked primary research, and challenge specific hypotheses or evidence interpretations. Reproducible experiments will be added as they are conducted. We welcome useful technical corrections and comparable controlled test cases.

**Security boundary:** Do not upload sensitive customer data, credentials, live system specifications, or details of unpatched vulnerabilities. Use synthetic examples and systems you own or are explicitly authorized to test.

[Back to the project overview](../../README.md)
