---
name: pre-merge-bug-review
description: Review a non-main git branch before merge to main and find defects, regressions, risky migrations, and release blockers. Use when asked to do pre-merge QA, branch audits, bug hunts, migration safety checks, or go/no-go merge reviews. Require verification of findings whenever possible and report issues sorted by risk level.
---

# Pre-Merge Bug Review

## Goal

Find and verify merge-blocking issues before code from a non-main branch is merged into `main`.

Prioritize:
- Behavioral bugs and regressions
- Data and schema migration risk
- Runtime and deploy failures
- Security/privacy leaks

## Inputs To Collect

1. Confirm comparison target (`main` by default).
2. Identify branch under review (`HEAD` by default).
3. Capture diff scope:
   - `git diff --name-status <base>...<head>`
   - `git diff --stat <base>...<head>`
4. Detect migration-related files quickly:
   - `rg --files | rg -i "migrat|prisma|schema|sql"`

If project-specific test commands exist, run the smallest reliable set first, then expand for touched areas.

## Review Workflow

1. Build a risk map from changed files.
   - Mark auth, payments, migrations, data writes, background jobs, and public API paths as high attention.
2. Perform targeted code review on changed files.
   - Focus on correctness, null/edge handling, race conditions, error propagation, and backward compatibility.
3. Run verification.
   - Execute tests for touched modules.
   - Reproduce suspected bugs via commands/tests when possible.
   - For each finding, record concrete evidence (failing test, stack trace, bad query plan, or deterministic reasoning from code path).
4. Run migration safety checks (see `references/migration-checklist.md`).
5. Score every finding using `references/risk-levels.md`.
6. Output only issues, sorted highest risk to lowest risk.

## Verification Standard

Always verify findings you can verify in the current environment.

For each finding, set `verification_status` to one of:
- `Verified`: Reproduced with command/test/evidence.
- `Partially Verified`: Evidence is strong but full repro blocked.
- `Not Verified`: Could not run due to environment gap.

Never present a speculative issue as verified.
When verification is blocked, state exactly what is missing.

## Output Format

Return findings in this order only: `Critical`, `High`, `Medium`, `Low`.
Sort within each level by likely user impact and blast radius.

Use this structure for each finding:

- `id`: short stable identifier
- `risk_level`: Critical | High | Medium | Low
- `title`: one-line issue summary
- `files`: changed file paths with line references when available
- `impact`: user/business/operational effect
- `evidence`: repro command, failing test, error output, or code-path proof
- `verification_status`: Verified | Partially Verified | Not Verified
- `fix_recommendation`: minimal safe fix
- `merge_recommendation`: Blocker | Fix Before Merge | Can Follow Up

If no issues are found, explicitly output `No verified pre-merge blockers found` and list residual risks (for example: unrun integration tests).

## Migration Focus

When migration files change, always include a dedicated `Migration Risk` section in the report, even if no defects are found.

Minimum checks:
- Backward compatibility during rolling deploys
- Data loss/destructive operations
- Nullability/default/backfill strategy
- Index and lock impact on large tables
- Down migration realism (or explicit non-reversible acceptance)
- App code and schema rollout order

Load details from `references/migration-checklist.md` and `references/risk-levels.md` when needed.
