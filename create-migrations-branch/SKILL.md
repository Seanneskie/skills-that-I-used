---
name: create-migrations-branch
description: "Create a dedicated migrations-only git branch from the current feature branch. Use when a branch mixes app code and DB migrations and you need to split migrations into a separate branch with naming rules: if a Linear-style ticket tag (for example FMD-1473) exists, name the branch as ticket-tag-migrations; otherwise derive a short slug from the branch summary and append -migrations."
---

# Create Migrations Branch

## Overview

Split migration changes out of the current branch into a separate branch.
Generate a deterministic branch name and keep only migration files on the new branch.

## Workflow

1. Detect the source branch.
2. Build the target migrations branch name.
3. Create the target branch from base (`main` by default).
4. Copy only migration files from source branch to target branch.
5. Commit migration files.

## Naming Rule

1. Find a Linear tag with regex: `[A-Z][A-Z0-9]+-[0-9]+`.
2. Check in this order:
   - Current branch name.
   - Latest commit subject (`git log -1 --pretty=%s`).
3. If a tag exists, use lowercase `<tag>-migrations`.
4. If no tag exists, derive slug from branch summary:
   - Start from current branch name.
   - Remove common prefixes (`feature/`, `feat/`, `bugfix/`, `fix/`, `chore/`, `hotfix/`).
   - Replace non-alphanumeric separators with `-`.
   - Collapse repeated dashes.
   - Trim to 3-5 meaningful words (max 40 chars).
   - Append `-migrations`.

Examples:
- `feature/FMD-1473-update-report-flow` -> `fmd-1473-migrations`
- `feat/refactor-chat-credit-checks` -> `refactor-chat-credit-checks-migrations`

## Migration File Scope

Treat these as migration files unless the user overrides:
- `web/prisma/migrations/**`
- `**/migrations/**`
- `**/migration*.sql`
- `**/migration_lock.toml`

## Commands

Run this sequence from repo root:

```bash
SOURCE_BRANCH="$(git rev-parse --abbrev-ref HEAD)"
BASE_BRANCH="main"

LINEAR_TAG="$(printf "%s\n%s\n" \
  "$SOURCE_BRANCH" \
  "$(git log -1 --pretty=%s)" \
  | grep -Eo '[A-Z][A-Z0-9]+-[0-9]+' \
  | head -n1)"

if [ -n "$LINEAR_TAG" ]; then
  TARGET_BRANCH="$(printf "%s-migrations" "$LINEAR_TAG" | tr '[:upper:]' '[:lower:]')"
else
  CLEAN_SUMMARY="$(printf "%s" "$SOURCE_BRANCH" \
    | sed -E 's#^(feature|feat|bugfix|fix|chore|hotfix)/##' \
    | sed -E 's#[^a-zA-Z0-9]+#-#g; s#-+#-#g; s#(^-|-$)##g' \
    | cut -c1-40)"
  TARGET_BRANCH="${CLEAN_SUMMARY}-migrations"
fi

git switch "$BASE_BRANCH"
git switch -c "$TARGET_BRANCH"
git checkout "$SOURCE_BRANCH" -- web/prisma/migrations
git add web/prisma/migrations
git commit -m "chore(db): migrations from $SOURCE_BRANCH"
```

## Guardrails

- Ensure clean working tree before switching branches.
- Fail early if no migration file changed between base and source.
- Avoid copying non-migration files.
- Keep commit limited to migration paths only.

## Output Format

When executing this skill, report:
1. Source branch
2. Base branch
3. Computed target branch
4. Migration files included
5. Final commit hash (if committed)
