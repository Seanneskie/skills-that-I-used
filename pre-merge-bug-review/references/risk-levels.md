# Risk Levels

## Critical

Use for issues that can cause severe production harm or immediate merge stop:
- Data corruption or irreversible data loss
- Authentication/authorization bypass
- Severe security exposure of sensitive data
- System-wide outage or startup failure

Default merge recommendation: `Blocker`.

## High

Use for issues likely to impact core user flows or integrity:
- Broken payment, messaging, account, or admin-critical flows
- Migration that can lock hot tables or fail on real data
- Backward-incompatible API or schema rollout mismatch

Default merge recommendation: `Fix Before Merge`.

## Medium

Use for important but non-catastrophic issues:
- Edge-case failures with limited blast radius
- Performance regressions without immediate outage risk
- Incomplete validation or error handling in non-core paths

Default merge recommendation: `Fix Before Merge` unless explicitly accepted.

## Low

Use for minor defects or maintainability concerns:
- Cosmetic bugs
- Non-blocking logging/telemetry issues
- Small correctness concerns with low frequency and low impact

Default merge recommendation: `Can Follow Up`.

## Risk Scoring Heuristic

Score each finding on:
- Impact (1-4)
- Likelihood (1-4)
- Blast radius (1-4)
- Recoverability penalty (0-2)

Suggested total score to level:
- 11-14: Critical
- 8-10: High
- 5-7: Medium
- 1-4: Low

Use judgment to override when security/data-integrity concerns are present.
