# API Trust Lens — Research Example 001
## Permission-to-Operation Authorization in a Fictional Banking API

**Status:** Proposed synthetic research scenario — not implemented or tested  
**Primary research area:** Authorization context, business entitlements, and evidence gaps  
**Related research:** [Research Brief 001](../briefs/001-api-authorization-context-and-evidence.md)

## 1. Research objective
Investigate whether API Trust Lens can distinguish authentication/token validity, OAuth scopes, resource ownership, product entitlements, business/workflow restrictions, and runtime authorization evidence.

**Hypothesis:** A valid token with a broad read/write scope may be necessary but insufficient for sensitive business operations. Missing specification detail is not proof of a vulnerability.

## 2. Fictional banking environment
Northstar Bank is fictional. Alice (`CUS-1001`) owns active checking account `ACC-1001`, has completed identity verification, and has the illustrative `Account.ReadWrite` OAuth scope. Her brokerage onboarding is incomplete, brokerage entitlement is not granted, and she has 60,000 reward points. All identifiers, scopes and policies are synthetic.

## 3. Scenario A — Conditional account and brokerage creation
Alice can read her account and statements, update permitted profile fields, and manage eligible existing-account settings. These permissions do not automatically authorize opening or activating a brokerage account.

**Operation:** `POST /customers/me/accounts`

```json
{
  "productType": "BROKERAGE",
  "fundingAccount": "ACC-1001",
  "initialDeposit": 5000
}
```

Illustrative OpenAPI contract:

```yaml
paths:
  /customers/me/accounts:
    post:
      operationId: createCustomerAccount
      security:
        - OAuth2:
            - Account.ReadWrite
      responses:
        "201": { description: Account created }
        "202": { description: Application pending }
        "403": { description: Operation not permitted }
        "409": { description: Business prerequisite not satisfied }
```

The example assumes valid authentication, matching scope, and ownership of the funding account. **Product entitlement, onboarding and applicable disclosures are not satisfied.** The expected outcome is rejection or controlled pending onboarding—not an activated brokerage account. Response codes and scope naming are fixture choices, not universal standards.

**Proposed invariant:** A customer must not receive an active brokerage account solely because their token contains `Account.ReadWrite`; required entitlements and onboarding prerequisites must be met.

| Test | Conditions | Expected behavior |
| --- | --- | --- |
| A1 | Valid token, existing-account read | Allowed |
| A2 | Valid token, permitted profile update | Allowed |
| A3 | Valid token, no brokerage entitlement | Creation denied |
| A4 | Entitlement granted, onboarding incomplete | Denied or pending, not activated |
| A5 | All prerequisites satisfied | Eligible, subject to other controls |
| A6 | Funding account not controlled by Alice | Denied |
| A7 | Client submits `brokerageEligible: true` | Client assertion cannot override trusted eligibility state |

**Observed results:** None; no API or runtime testing performed.

## 4. Scenario B — Reward-point redemption
**Operation:** `POST /rewards/redemptions`

```json
{
  "points": 50000,
  "rewardType": "GIFT_CARD",
  "destination": "alice@example.com"
}
```

A reward-specific permission or entitlement may be required in addition to or instead of `Account.ReadWrite`; this remains a fixture design decision.

**Proposed invariants:** Only eligible and available points can be redeemed; reward eligibility and any step-up verification must be satisfied; retries must not duplicate fulfillment; concurrent requests cannot spend the same points twice; deductions and issuance must maintain consistent state.

If Alice starts with 60,000 points and submits two concurrent 50,000-point requests, **at most one should succeed**. This is a business-state integrity and concurrency-control scenario, not automatically an OAuth authorization vulnerability.

| Test | Conditions | Expected behavior |
| --- | --- | --- |
| B1 | 50,000 requested, 60,000 available | Allowed if all other rules pass |
| B2 | 70,000 requested, 60,000 available | Denied |
| B3 | Two concurrent 50,000-point requests | At most one succeeds |
| B4 | Identical retry with same idempotency key | No duplicate redemption |
| B5 | Customer lacks rewards entitlement | Denied |
| B6 | Required step-up absent | Denied or challenged |

**Observed results:** None; no replay or concurrency tests performed.

## 5. Proposed evidence classification
- **Documented:** Explicitly supported by inspected specifications or policy documents.
- **Hypothesized:** Testable expectation, not validated.
- **Unknown:** Insufficient evidence.
- **Runtime verified:** Reproduced in an authorized environment with retained evidence.

Labels apply to individual claims, not indiscriminately to whole APIs.

### Illustrative finding ATL-001
**Title:** Brokerage creation requires authorization evidence beyond declared OAuth scope.  
**Operation:** `POST /customers/me/accounts`  
**Documented in proposed synthetic contract:** `Account.ReadWrite` scope.  
**Proposed policy:** Brokerage entitlement and completed onboarding required.  
**Unknown:** Whether a future implementation enforces those prerequisites.  
**Validation:** Test with valid token and funding-account ownership but no brokerage entitlement.  
**Conclusion:** Evidence gap in illustrative specification; **no vulnerability established**.

### Illustrative finding ATL-002
**Title:** Reward redemption requires stateful business-invariant validation.  
**Operation:** `POST /rewards/redemptions`  
**Proposed policy:** Points cannot be spent more than once.  
**Unknown:** Atomic balance enforcement and idempotency behavior.  
**Validation:** Controlled concurrent and repeated requests against an owned synthetic fixture.  
**Conclusion:** Testable business-integrity hypothesis; **no observed defect**.

## 6. Evidence sources and limits
| Source | What it can support | What it cannot establish alone |
| --- | --- | --- |
| OpenAPI | Declared operations, scopes, schemas, responses | Runtime enforcement |
| Policy metadata | Intended entitlements, prerequisites, invariants | Whether implementation complies |
| Synthetic fixture | Controlled expected behavior | Production-bank behavior |
| Authorized runtime tests | Observed results for tested cases | Absence of all vulnerabilities |
| Source review | Implementation logic | All deployed configurations and paths |

## 7. Open research questions
1. How should authorization requirements beyond OAuth scopes be represented?
2. Can business-policy metadata be portable and inspectable?
3. What evidence gaps are identifiable without execution?
4. How can the tool distinguish missing documentation from missing enforcement?
5. How should it generate reproducible tests without premature vulnerability claims?
6. What comparable capabilities already exist in Spectral, Schemathesis, RESTler, OWASP ZAP, and other tools?

## 8. Proposed experiment (not started)
Build an owned local synthetic banking fixture with both correctly enforced and deliberately misconfigured brokerage entitlements, and correct versus intentionally unsafe concurrent redemption behavior. Compare tools using recorded versions, configurations, request/response evidence, expected and observed outcomes, false positives, and reproduction instructions.

## 9. Research integrity
No real bank, customer, credentials, financial records, or production service is involved. Scope naming is fictional. No tests have been executed or vulnerabilities found. A missing policy description does not prove missing enforcement. Competitor differentiation must be independently researched.

## 10. Next actions
1. Add this example to the research index.
2. Use it as a comparison case in Research Brief 002.
3. Define machine-readable policy metadata and expected outcomes before Experiment 001.
