# Skills I Use Daily

A personal repository of reusable Codex skills for everyday engineering workflows.

## Purpose

This repo is the source of truth for the skills I use most often:
- Write better commit messages
- Draft concise PR summaries
- Run pre-merge bug/risk reviews before merging

## Repository Structure

- `commit-message-generator/` - Generates concise, convention-aligned commit messages from current diffs.
- `draft-pr-summary/` - Produces PR summaries with required test results and a separate pre-merge review paragraph.
- `pre-merge-bug-review/` - Performs structured pre-merge defect/risk review with verification status.

## Daily Workflow

1. Prepare changes and inspect diff.
2. Use `commit-message-generator` to create the commit message.
3. Use `draft-pr-summary` to produce PR summary bullets.
4. Run tests and include outcomes in the `Tests:` section.
5. Run `pre-merge-bug-review` and include its result as a separate `Pre-merge review:` paragraph.

## Output Standard For PRs

Use this structure for PR descriptions:

```text
Summary:
- ...

Tests:
- <command + outcome>

Pre-merge review:
<separate paragraph with verified findings or "No verified pre-merge blockers found" plus residual risks>
```

## Maintenance

- Keep each skill's `SKILL.md` focused on one job.
- Update this README when adding, removing, or changing skill workflows.
- Prefer explicit verification steps over assumptions.
