---
name: draft-pr-summary
description: Draft concise bullet-style PR summaries of current branch changes. Use when asked to write or refresh PR descriptions, summarize HEAD/working-tree diffs against a base branch, or highlight key changes, risks, and tests for a pull/merge request.
---

# Draft PR Summary

## Overview

Produce crisp bullet summaries for pull/merge requests that reflect the current branch state. Highlight what changed, who or what is affected, risks or breaking items, and tests performed without rewriting the entire diff.

## Quick Start
- Set the base branch (default to `main` if unknown) and gather context: `git status -sb`, `git diff --stat <base>...HEAD`, `git diff --name-only <base>...HEAD`.
- Skim the diff or staged changes to spot user-facing updates, backend/API shifts, migrations/config changes, and tests added or updated.
- Group related changes, then draft 3-7 bullets in present tense that emphasize impact and scope.
- Run the relevant test commands and add a short `Tests:` section; call out "not run" only if true.
- Run a pre-merge bug review after tests and present its outcome in a separate paragraph.
- Re-read to ensure bullets match the diff, avoid speculation, and stay concise.

## Workflow
1) **Collect inputs**
   - Confirm base branch; use `main` if not specified.
   - Capture status/diff: `git status -sb`; `git diff --stat <base>...HEAD`; inspect key hunks with `git diff <base>...HEAD path`.
   - Note linked ticket or context if provided; note test results or ask.

2) **Extract themes**
   - Bucket changes by area (feature, API, UI, infra, data).
   - Flag risky items: migrations, auth or permissions, perf-sensitive code, config or flag changes, deletions or removals.
   - Note test coverage updates or new checks.

3) **Draft the summary**
   - Aim for 3-7 bullets; start with the most user-facing or impactful.
   - Use present tense, concise phrasing; include scope (feature or area) and effect.
   - Explicitly call out breaking changes, migrations, or follow-up work.
   - Avoid file lists; focus on behavior and intent.

4) **Run tests and report**
   - Execute relevant checks first (for example, unit, integration, e2e, manual).
   - List commands and outcomes under `Tests:`.
   - If none run, state that plainly (`Tests: not run`).

5) **Run pre-merge review**
   - After tests complete, run the `pre-merge-bug-review` skill workflow on the same diff scope.
   - Summarize its result in a separate paragraph labeled `Pre-merge review:`.
   - If no verified issues are found, state that explicitly and include residual risks from unrun checks.

6) **Validate**
   - Ensure bullets map to the diff; remove conjecture.
   - Keep lines tight (under 110 chars), no filler.
   - If base branch is uncertain or the diff is huge, say what was summarized and any gaps.

## Output Template
```
Summary:
- <bullet 1>
- <bullet 2>
- <bullet 3>

Tests:
- <test/command or "not run">

Pre-merge review:
<separate paragraph summarizing verified findings, or "No verified pre-merge blockers found" with residual risks>
```
