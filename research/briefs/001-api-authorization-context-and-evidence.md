# Research Brief 001 — Authorization Context and API Security Evidence

**VerityMesh Trust Lab | API Trust Lens | 2026-10-31 | Status: Literature-backed research hypothesis, not an empirical product claim**

## Research question

Why do object-level and workflow-level authorization risks remain hard to recognize and validate, and could a developer-facing *evidence and test-planning assistant* improve early detection without pretending that a static API specification proves runtime vulnerabilities?

## Why investigate this?

- **OWASP API Security Top 10 (2023)** places Broken Object Level Authorization (BOLA, API1), Broken Object Property Level Authorization (API3), Broken Function Level Authorization (API5), and Unrestricted Access to Sensitive Business Flows (API6) among its prominent risk categories. These are risk categories, **not measured prevalence figures** for our target sectors. [OWASP Top 10](https://api-security.owasp.org/editions/2023/en/0x11-t10/); [BOLA details](https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/).
- **NIST SP 800-228** (2025; updated March 2026) calls for risk and control analysis across API design, development, and runtime. The 2026 update adds risk and lifecycle-stage control appendices. [NIST](https://csrc.nist.gov/pubs/sp/800/228/upd1/final).
- A **2026 systematic survey of 69 RESTful API security-testing studies** reports that context-aware and authenticated testing, logic/authorization flaws, reliable security oracles, and multi-step stateful generation remain difficult. This is an evidence synthesis, **not our independent experiment**. [Research paper](https://doi.org/10.1016/j.cosrev.2026.101043).
- A separate **2026 review of 50 Web API studies** connects recurring weaknesses to developer/usability challenges, inconsistent guidance, implementation complexity, and limited empirical tool validation. [Open-access review](https://doi.org/10.3390/software5020025).
- **BolaZ** (ACM, 2026) explores data flow and authorization boundaries for resource IDs. The authors report tests across ten GitHub projects and 35 newly found BOLA vulnerabilities; this finding has **not** been replicated by VerityMesh. [DOI](https://doi.org/10.1145/3833417).

## Problem hypotheses — not yet validated

### H1 — Authorization evidence gap

An OpenAPI contract can describe authentication schemes and request parameters, but typically does not establish per-object authorization outcomes. Developers may mistake authentication declarations for sufficient authorization evidence.

**Test proposal:** Supply a synthetic banking API with accounts belonging to two test users; compare static contract analysis with authorized local execution of an access-control test. See if a report explicitly differentiating *documented*, *unknown*, and *verified* improves developers' interpretations.

### H2 — Workflow-context gap

High-value business rules often depend on multi-step state (e.g., booking changes, payment approval, identity recovery), not just independent endpoint schemas.

**Test proposal:** Create a synthetic airline reservation-change workflow with explicit role, ownership, and state transitions. Compare a standalone schema check with generated test-plan recommendations; later evaluate against existing stateful testing tools in a disposable local environment.

### H3 — Developer guidance/usability gap

A security warning without concrete evidence, scope, or remediation can be hard to act upon.

**Test proposal:** Compare a conventional linter report with an evidence-oriented report containing affected operation, the hypothesized access-control invariant, what cannot be established statically, a test case, and remediation suggestions. Ask developers to rate comprehension and identify the correct next action. No superiority claims until a reproducible study is completed.

## Prior-art baseline — investigate before implementing

| Tool | Documented strengths | What to investigate (not a proven missing capability) |
|---|---|---|
| [Spectral](https://github.com/stoplightio/spectral) | OpenAPI linting, customizable rulesets, local/CI workflows | Can custom policies already provide adequate evidence-oriented authorization guidance? |
| [Schemathesis](https://schemathesis.readthedocs.io/en/stable/) | Property-based tests and stateful API workflow testing | How much authenticated/role-aware authorization testing is supported, and what setup is required? |
| [RESTler](https://github.com/microsoft/restler-fuzzer) | Stateful REST API fuzzing and producer-consumer dependency exploration | Can this complement or replace proposed workflow test planning? |
| [OWASP crAPI](https://github.com/OWASP/crAPI) | Intentionally vulnerable training application | Which controlled scenarios can be adapted into reproducible teaching fixtures? |

Do not assert these tools lack functionality until tested against the same benchmark.

## Proposed initial solution (tentative)

**API Trust Lens** begins as an offline, non-invasive OpenAPI analysis and report tool. For a sensitive endpoint it may:

1. Inventory the operation and resource identifier.
2. Describe the access-control question it raises.
3. Identify what the API contract explicitly declares.
4. Distinguish **design concern**, **missing evidence**, **runtime-verified result** (only when externally supplied, authorized evidence exists).
5. Produce a structured, reproducible test-plan template for a synthetic/local test environment.

**Not in MVP:** autonomous probing of production banks/airlines/defense systems; claims of proven BOLA based on contract-only analysis; paid API dependency; unreviewed AI-generated security verdicts.

## Evaluation plan

- **Fixtures:** two fictional banking APIs and two fictional airline APIs; each contains secure and insecure *synthetic* variants.
- **Ground truth:** independently defined access-control invariants and expected allow/deny outcomes.
- **Metrics:** true/false finding counts against documented labels; proportion correctly marked unknown; remediation actionability; reproducibility; median developer interpretation time (if volunteers available).
- **Baselines:** Spectral custom rules, Schemathesis stateful tests, and RESTler where feasible, with versions/configurations recorded.
- **Safety:** local containers only; no real customers, secrets, or unauthorized live targets.

## Research questions for next session

1. Which categories can a contract-only tool reliably flag without excessive false positives?
2. What minimum role/ownership metadata must be added to represent an authorization invariant?
3. Which existing tools already support role-aware, stateful authorization scenarios?
4. Can reports clearly distinguish missing evidence from verified insecurity?
5. Which findings would developers act upon without security-specialist help?

## Evidence classification

- **Verified from cited publications:** standards' topics; stated methods and limitations of research/tools.
- **Provisional inference:** a simpler evidence-driven report could be valuable.
- **Not yet verified:** superiority to alternatives; actual false-positive rates; customer interest; severity of any specific bank/airline implementation.

## Next deliverables

1. **Research Brief 002:** controlled comparison of Spectral, Schemathesis, RESTler, ZAP and relevant security-spec tools.
2. **Research Brief 003:** synthetic banking/airline authorization invariants and test-oracle design.
3. **Experiment 001:** reproducible fixture + baseline report + measured findings.

## Sources and publication ethics

Link to original publications rather than copying full papers. Reproduce only small, properly attributed excerpts, and report author claims as such. Record tool versions, observation dates, and study limitations with each subsequent update.
