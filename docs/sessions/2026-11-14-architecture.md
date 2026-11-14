# Session log: 2026-11-14 (Sprint 1.0 finished + architecture docs)

## Where things stand

- **Paused** until my Microsoft Student Ambassador account (Azure access) is restored. It was restricted for security reasons for about a week. Resume after it is back.
- **`main`** is protected, with two **required** CI checks. Sprint 1.0 is complete.
- **PR #5** `docs: add architecture diagrams, document and adrs` (branch `docs/architecture`) is **open with CI green, not merged yet**. Review the diagrams, then squash-merge.
- **Next action after resuming:** merge PR #5 → start **Part C: walking skeleton** on a new branch `feat/walking-skeleton`.
- A full copy of the repo was also made at `C:\Users\sdhaksin\Documents\shopsphere` (a snapshot; the working repo is `C:\Users\sdhaksin\shopsphere`).

> The Azure account restriction does **not** block the next steps. Azure is only needed from Sprint 1.7. Part C and Sprints 1.1–1.6 run fully locally (Docker + Node), so work can continue as soon as I'm ready, even before the account is restored.

## What happened this session

### Part A: finishing Sprint 1.0 (step 11)

1. Opened PR #3 from `chore/sprint-1.0-setup`. CI did not start, because GitHub showed the PR as **conflicting**.
2. **Cause:** PR #1 (steps 2–8) had already been **squash-merged** on 2026-11-13 as `Chore/sprint 1.0 setup` (not a conventional title; the title check didn't exist yet). Steps 9–10 were committed on top of the _original_ commits. A squash creates one **new** commit on `main` with a different ID, so Git saw the old branch as different history.
3. A rebase + force-push was blocked by the permission check. **Safer fix used:** a new branch `chore/sprint-1.0-ci` from `main`, then `git cherry-pick` of the 3 new commits (content verified identical), then **PR #4** `ci: add pr template and quality checks`. CI passed and it was squash-merged as `71cb949`.
4. Added both CI jobs as **required status checks** on `main` (`app_id 15368` = GitHub Actions, `strict: true`), keeping every other protection setting.
5. Later I merged PR #2 (main → old branch) and PR #3 into `main` myself. That produced `0addc2d`, which changes no files.

**Lesson:** after a squash merge, **never keep committing on the merged branch**. Start a new branch from the updated `main`.

### Part B: architecture before code (PR #5)

Decisions made this session:

| Decision        | Choice                                                                             |
| --------------- | ---------------------------------------------------------------------------------- |
| POC type        | **Walking skeleton**, which becomes the base for Sprints 1.1–1.4 (not thrown away) |
| Where docs live | **In the repo**: `docs/ARCHITECTURE.md`, `docs/adr/`, `docs/architecture/`         |
| IaC tool        | **Terraform** ([ADR-0013](../adr/0013-terraform-for-iac.md))                       |
| Order           | Finish Sprint 1.0 first, then architecture, then the POC                           |

Files added:

- `docs/architecture/shopsphere.drawio`: 6 pages (context, Phases 1–2, 3–4, 5, 6–7, 8) plus an SVG export of each page.
- `docs/ARCHITECTURE.md`: goals, constraints, quality attributes, evolution by phase, key flows, cross-cutting concerns, technology tables (what, why, alternatives, sprint).
- `docs/adr/0001–0015`: one decision record per major technology choice.
- `docs/ROADMAP.md`: Sprint 1.0 ticked, Terraform chosen, architecture linked. README updated.

To edit the diagrams: open the `.drawio` in draw.io Desktop or the VS Code "Draw.io Integration" extension, then re-export the changed page (command in `ARCHITECTURE.md` §11).

## The CI pipeline (summary of the explanation)

- Triggers: PR `opened / edited / synchronize / reopened`, and push to `main`. `edited` re-checks a renamed PR title.
- `permissions: contents: read` gives a least-privilege token. `concurrency` cancels outdated runs.
- Job 1 **Lint, format, typecheck**: checkout → Node from `.nvmrc` + npm cache → `npm ci` → lint → format:check → `typecheck --if-present`.
- Job 2 **PR title is a conventional commit** (PRs only): the squash title becomes the commit on `main`, so it's checked with commitlint. The title is passed via `env:` to prevent script injection.
- Branch protection: both checks required, branch must be up to date (`strict`), PR required (admins too), squash only.

## Part C plan: walking skeleton (next)

| #   | What                                                                                   | Proves                     |
| --- | -------------------------------------------------------------------------------------- | -------------------------- |
| C1  | `docker-compose.yml` with SQL Server 2022                                              | Local DB matches Azure SQL |
| C2  | `apps/api`: NestJS `/health` + `GET /api/v1/products`, Prisma, `Product` model, seed 5 | API → ORM → DB             |
| C3  | `apps/web`: Vite + React page listing products                                         | Browser → API (CORS, env)  |
| C4  | `packages/shared`: shared `Product` type                                               | Workspace packages work    |
| C5  | Root `typecheck` script; CI builds both apps                                           | CI covers real code        |
| C6  | Session log + README "Run it locally"                                                  | Anyone can start it        |

## Open self-check questions

- Why can't you just commit a script in `.git/hooks/`?
- What does `core.hooksPath` do, and which npm feature sets it?
- Why does a squash merge make the old branch "conflict" with `main`?
- Why must the PR title be checked in CI and not only by the local commit-msg hook?
- What does `strict: true` on required checks protect against?
- What is an ADR, and how is it different from a design note?

## Resuming

```bash
cd C:/Users/sdhaksin/shopsphere
git switch main && git pull
gh pr view 5            # review, then: gh pr merge 5 --squash --delete-branch
git pull
git switch -c feat/walking-skeleton
```

Or in Claude Code: `claude --continue` (or `claude --resume`), then say "resume shopsphere".
