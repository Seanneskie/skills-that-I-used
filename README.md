# Skills Collection

A personal repository of reusable Codex skills organized by purpose.

## Repository Structure

```text
├── daily/                          # Skills for everyday engineering workflows
│   ├── commit-message-generator/   # Generates concise, convention-aligned commit messages
│   ├── create-migrations-branch/   # Splits migration files into a dedicated branch
│   ├── draft-pr-summary/           # Produces bullet-style PR summaries with test results
│   └── pre-merge-bug-review/       # Pre-merge defect/risk review with verification
│
└── agents/                         # Role-specific agent skills
    ├── pr-reviewer/                # Thorough code review agent for pull requests
    ├── marketing-agent/            # Marketing copy, campaigns, and positioning
    └── blog-specialist/            # Blog posts, outlines, editing, and SEO optimization
```

## Daily Skills

Engineering workflow skills used during development:

1. **commit-message-generator** — Craft concise commit messages from current diffs.
2. **draft-pr-summary** — Draft PR descriptions with summary bullets, test results, and pre-merge review.
3. **pre-merge-bug-review** — Find and verify merge-blocking issues before merging to main.
4. **create-migrations-branch** — Split migration files out of a feature branch into a separate branch.

### Daily Workflow

1. Prepare changes and inspect diff.
2. Use `commit-message-generator` to create the commit message.
3. Use `draft-pr-summary` to produce PR summary bullets.
4. Run tests and include outcomes in the `Tests:` section.
5. Run `pre-merge-bug-review` and include its result as a separate `Pre-merge review:` paragraph.

## Agent Skills

Role-specific agents for specialized tasks:

1. **pr-reviewer** — Acts as a senior code reviewer. Reviews PRs for correctness, security, performance, maintainability, and testing. Outputs structured feedback with blocker/should-fix/suggestion categories.
2. **marketing-agent** — Acts as a marketing specialist. Writes landing page copy, email sequences, social media posts, ad copy, and campaign plans. Focuses on conversion and audience-appropriate messaging.
3. **blog-specialist** — Acts as a blog content specialist. Writes, outlines, edits, and optimizes blog posts. Handles SEO, content structure, and engagement optimization.

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
- Daily skills go in `daily/`, role-specific agents go in `agents/`.
- Update this README when adding, removing, or changing skills.
- Prefer explicit verification steps over assumptions.
