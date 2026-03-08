# Agent Policy

This document defines organization-wide guidance for AI agents working in Karida
repositories. Repository-specific rules may extend this policy but should not
conflict with it.

Related org docs:

- `docs/context-maintenance.md`
- `docs/roadmap-workflow.md`
- `docs/tooling.md`

## Repository AGENTS.md files

- Keep repo `AGENTS.md` focused on repo-specific guidance (commands, architecture, conventions, tests).
- Do not copy org-wide policy into repo files; link to this document instead to avoid drift.
- Repo rules may extend this policy but should be minimal and must not conflict.

## Deployment workflow

All Karida repositories use a `next` preview branch as the mandatory gate before `main` (production).

```
feature work  ->  next branch  ->  PR to main  ->  production
                  (preview)        (reviewed)       (auto-deploy)
```

**Non-negotiable rules for agents:**

- NEVER push directly to `main` in any Karida repo.
- NEVER use CLI tools (e.g. `supabase db push`, `supabase migration repair`) to manipulate a production database directly, unless the user explicitly instructs it to fix historical drift. If in doubt, stop and ask.
- NEVER delete or alter the `next` GitHub branch or its associated Supabase preview branch.
- All work is developed and tested on `next` first, then merged to `main` via a PR.

Direct production manipulation causes migration history drift and can break the Supabase branch merge pipeline.

## Issue-first workflow

- Required for feature/bugfix work that affects user-facing behavior or touches multiple files.
- Optional for docs, typos, and small single-file refactors.
- Use the repo issue template if present.
- Branch naming: `issue-<n>-<slug>` from `next` (not `main`).
- Manual merge after checks pass.

Recommended command flow:

```bash
gh issue create --template issue-first.md --label "<label>"
git checkout next && git checkout -b issue-<n>-<slug>
pnpm lint && pnpm test
gh pr create --title "<title>" --body "Fixes #<n>" --base next --label "<label>"
```

## Labels

- Apply labels on issues and PRs (use existing defaults like `bug`, `enhancement`, `documentation`).

## Safety and hygiene

- Never commit secrets or private keys.
- Avoid destructive git commands unless explicitly requested.
- Keep changes minimal and aligned with existing patterns.

## Code reviews

- Check for review comments after opening PRs (including Codex and human reviewers).
- Address feedback promptly and resolve review threads when the change is applied.

