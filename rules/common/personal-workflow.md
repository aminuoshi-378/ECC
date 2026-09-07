# Personal Workflow & Communication Conventions

> User-owned cross-project conventions. Merged into the ECC distribution so every
> project inherits the operator's working style. Language-agnostic.

## 1. Task Execution

- Read relevant docs (README/AGENTS) before touching a new project.
- Read a file before editing it; reuse similar existing implementations.
- Make the smallest change that satisfies the task; do not refactor/rename/fix formatting on the side.
- Probe improvements or hidden risks as suggestions only; never change them without consent.
- Changes must be tested; report what was run and the result. Unverified means incomplete.
- If unsure, say so. Never assert something unverified; verify paths, line numbers, and references before citing them.

## 2. Code Style & Commits

- Follow existing repo conventions; defer to configured linters/formatters; place new files with peers.
- Self-explanatory names; booleans as `is/has/can`, function names as verbs, constants in SCREAMING_SNAKE_CASE. Name by business meaning, not implementation detail.
- Comments only when they explain "why"; do not restate the code.
- Avoid magic numbers/strings/hardcoded thresholds; extract to config and read through one source.
- Commit message: concise Chinese, first line <= 50 chars, format `type: summary`; one commit per logical change.
- Before push: pull/sync remote; have another agent review for secrets/dangerous instructions and show the user the prompt for approval; branch name `type/scope`.
- Any git operation: run `git status` first; prefer reversible operations over destructive ones; check upstream sync before committing in a fork.

## 3. Communication

- All user-facing output in Simplified Chinese; keep code/commands/paths/technical terms in original.
- Be direct: no filler, no trailing pleasantries, no repeating the user's words.
- No flattery ("good question"); raise disagreements directly with reasons.
- Report changes starting with affected **documents**, then **code**. When adding doc content, check for duplication/conflict first: report conflicts to the user to decide, do not overwrite.

## 4. Security & Boundaries

- No hardcoded secrets/tokens/passwords/internal URLs in code, comments, or commits; use env vars or a vault; flag existing secrets to the user.
- Do not touch production (deploy/release/drop DB/change config) without stating impact and consent; prefer local/test validation.
- Confirm disruptive commands (`rm -rf`, `git push --force`, `reset --hard`, `DROP TABLE`, bulk delete/overwrite) before running; try in a temp dir/branch when unsure of impact.
- Do not access files/services/systems outside the task scope; stop and ask when crossing a boundary.
- Installing any tool/package/dependency requires explaining what, where, and why, and getting consent first — never skip for dev-only or isolated-environment reasons.

## 5. Documentation

- Root: `AGENTS.md` (rules), `README.md` (zh) + `README-en.md` (en, same content), `CHANGELOG.md`.
- `docs/changelog/`: per-session granular logs, `YYYYMMDD-HHmm-NNN.md`, append-only.
- `docs/todo/`: per-session TODO with pending items, owner, and done criteria.
- `docs/requirements.md`: one per project, maintained continuously.
- `docs/design/`: system/technical design docs (ADRs go to root CHANGELOG, no duplication).
- Deletion defaults to no; mark obsolete docs as deprecated with replacement pointers; only delete when fully wrong/valueless or fully superseded and unreferenced (grep first).
- Size caps: `AGENTS.md` <= 5000 chars; `README*.md` <= 3000; `docs/design/*` <= 5000 (split with an index at the root); changelog/requirements/todo un-capped but changelog must avoid log-spam.

## 6. Token Cost Control (Hard Constraint)

- Keep each change local; plan skeleton-level, refine during execution.
- Prefer modular single files (<= ~300 lines) with clear interfaces.
- Feed precise context: locate with grep/glob then read fragments; cite with path + line numbers.
- Compress memory into docs (architecture docs + CHANGELOG/ADR) instead of large code blocks in context.
- Open new sessions/agents for subtasks; test as you go, avoid accumulating large diffs.
- Never loosen these constraints; if in conflict with the user, align first; note any deviation with cause.

## 7. Windows Notes

- `.bat` files must be ASCII-only (GBK on Chinese Windows corrupts UTF-8).
- Do not run server processes (hangs the agent); hand over to the user.
- Instructions written to docs/users must be actually run and verified first.