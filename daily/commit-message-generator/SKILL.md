---
name: commit-message-generator
description: Craft concise git commit messages for the current branch changes. Use when asked to write/suggest a commit message, summarize staged or working-tree diffs, or align wording with branch/ticket context.
---

# Commit Message Generator

## Overview

Produce clear, concise commit messages that summarize current changes, align with repo conventions, and respect branch/ticket context. Default to imperative, ≤72-char subject; include body bullets only when they add value.

## Quick start
- Gather context: staged vs unstaged diff, branch name, ticket/issue ID, notable risks/breaking changes.
- Identify intent: feature, fix, refactor, chore, docs, test, perf, revert, or mixed.
- Pick format:
  - If repo has a stated convention (Conventional Commits, team style, ticket prefix), follow it.
  - Otherwise use `Subject line` (+ optional body bullets).
- Draft subject: imperative mood, concise, mention scope if helpful (area/component). Keep ≤72 chars.
- Draft body (only if useful): bullets with past-tense detail, highlight breaking changes or follow-ups.
- Sanity check: covers main change, no redundancy, matches diff, respects style.

## Workflow
1) **Collect inputs (ask if missing):**
   - Summary of changes (git status/diff), scope/area, branch name and any ticket key.
   - Whether Conventional Commits or other format is required.
   - Breaking changes or user-facing impacts.

2) **Choose style:**
   - If Conventional: `type(scope?): subject` where type ∈ `feat|fix|refactor|chore|docs|test|perf|revert`.
   - If ticket prefix: add `ABC-123: Subject`.
   - Otherwise: plain subject.

3) **Compose subject (imperative, present-tense):**
   - Lead with outcome, not action steps.
   - Mention scope concisely (e.g., `profile`, `search`, `api`).
   - Avoid punctuation at end; avoid past tense/future tense.

4) **Optional body (only if it adds clarity):**
   - Bullets or short paragraphs describing *why* or notable *what* (e.g., migrations, breaking routes).
   - Call out breaking changes explicitly: `BREAKING CHANGE: ...`.
   - Note follow-ups or known gaps if important.

5) **Validate:**
   - Subject ≤72 chars; no trailing period.
   - Aligns with diff; no unrelated items.
   - Mirrors requested style and includes ticket/scope when applicable.

## Templates
- Conventional (with scope): `feat(profile): normalize usernames for routing`
- Ticket prefix: `ABC-123: tighten auth for admin routes`
- Plain: `Normalize profile usernames for URL routing`
- Body example:
  - `- derive slug server-side and redirect legacy URLs`
  - `- update messaging/profile links to slug helper`
  - `- note: legacy paths redirect for compatibility`

## Tips
- Prefer single-line commits unless extra context is truly helpful.
- If changes are mixed, pick dominant intent or split into multiple commits if possible.
- Avoid verbose list of files; focus on user-visible or behavioral changes.
