# AGENTS.md
# Machine-readable policy for all coding agents in this repository.
# Read automatically by Codex, Claude Code, and any ACP agent via Goose.

## Project Overview
Digital-card static site generated from the Obsidian vault source of truth for Aksharmurti Swami's digital contact card. Generated files should not be manually edited unless explicitly requested.

## Stack
- Runtime: static HTML/CSS/JS
- Framework: GitHub Pages static site generated from Obsidian tooling
- Test runner: static inspection / browser smoke check
- Linter: N/A unless introduced

---

## Required Workflow Sequence
Follow this for EVERY task. Do not skip or reorder phases.

### Phase 1 — Understand before acting
- Read the Linear/Plane issue spec fully before writing any code.
- Run /spec in Claude Code to produce an executable implementation plan.
- If task is large (>1 day), run /autoplan to chain CEO → design → eng → DX reviews.
- If task touches UI, also run /plan-design-review.
- REQUIRED FOR ALL NON-TRIVIAL TASKS: run /plan-eng-review to produce architecture diagrams, sequence diagrams, failure modes, edge cases, and a test plan artifact.
- GATE: Do not proceed to implementation until /plan-eng-review artifact exists. Hermes verifies this independently.

### Phase 2 — Implement
- Codex (CODEX_HOME=/Users/am0401/.codex-azure) handles mechanical implementation, test-fix loops, refactors.
- Claude Code handles judgment: design critique, architecture, ambiguity.
- If debugging is needed: run /investigate BEFORE any patches. No fixes without root cause. If three attempts fail, escalate.
- Do NOT make speculative changes. Implement only what the spec says.

### Phase 3 — Review Gate (ALL required before opening any PR)
Run in this exact order. Hermes verifies all ran before /ship is permitted:
1. /review (Claude Code) — staff engineer structural audit, auto-fix pass
2. /codex (gstack cross-model mode) — independent review, cross-model analysis vs /review output
3. /cso — REQUIRED on any PR touching auth, sessions, API surfaces, env vars, file I/O (OWASP Top 10 + STRIDE)
4. /health — code quality score 0–10, committed to PR as badge comment
5. /qa — diff-aware browser QA, tests affected pages by default
6. /benchmark — Core Web Vitals before/after delta, required on any rendering/asset/data PR

### Phase 4 — Ship
- /ship → sync main, coverage audit, open PR with coverage delta in body
- After merge: /land-and-deploy → confirm deploy and production health
- /canary → 10-minute post-deploy monitoring loop
- /document-release → update any stale README or docs

---

## Test Commands
Agents MUST run these and fix all failures before opening a PR.
- Install: `No install required for generated static site.`
- Test: `Open or serve the page locally and verify the changed card renders; if no code changed, inspect generated diff only.`
- Lint: `N/A`
- Type check: `N/A`

---

## Off-Limits — Never Touch Without Explicit Human Approval
- /infra/ or /terraform/ directories
- Files containing SECRET, KEY, TOKEN, PASSWORD, CREDENTIAL in the filename
- .env, .env.local, .env.production files
- .github/workflows/ files
- Database migration files (unless task explicitly scopes them)
- package-lock.json or poetry.lock (only if explicitly changing dependencies)

---

## Secrets and Credentials
- Never hardcode secrets in source files.
- All secrets are in Infisical (self-hosted, http://localhost:80).
- In Daytona sandboxes, use: infisical run -- <command>
- INFISICAL_TOKEN was injected at container startup via machine identity.
- Never commit .env files.

---
## Git and PR Conventions
- Branch: [type]/[linear-issue-id]-[short-description]
  Types: feat, fix, refactor, test, docs, chore
  Example: feat/LIN-142-add-user-auth
- Commits: Conventional Commits with Linear issue ID
  Example: feat(auth): add OAuth flow [LIN-142]
- One logical change per commit.
- PR title must match the Linear issue title.
- PR body must include: what changed, why, test plan, screenshot if UI changed, coverage delta from /health.

---

## Observability
- All LLM calls must emit OpenTelemetry traces to Langfuse at http://localhost:3000
- Project: hermes-stack
- Use LANGFUSE_PUBLIC_KEY and LANGFUSE_SECRET_KEY from Infisical (injected at runtime).
- Trace names must include the Linear issue ID: e.g. LIN-142-implement-oauth

---

## gstack Skills Reference
| Skill | When to Use |
|---|---|
| /office-hours | Large or ambiguous tasks — reframe before any code |
| /spec | Convert any Linear issue into an executable spec |
| /autoplan | Non-trivial features — chains CEO+design+eng+DX automatically |
| /plan-ceo-review | Find the 10-star version of the request |
| /plan-eng-review | REQUIRED: architecture, diagrams, failure modes, test plan |
| /plan-design-review | Any UI task: pre-implementation design audit |
| /plan-devex-review | API/SDK tasks: TTHW audit, friction points |
| /design-consultation | New projects: build design system, write DESIGN.md |
| /design-shotgun | 3+ visual variants in parallel |
| /design-html | Convert mockups to production components |
| /design-review | Post-implementation 80-item visual audit |
| /review | REQUIRED: staff engineer audit, auto-fix pass |
| /investigate | REQUIRED for debugging: root-cause first |
| /health | Code quality score and trend |
| /cso | REQUIRED on security-sensitive PRs |
| /qa | REQUIRED: diff-aware browser QA |
| /qa-only | Read-only bug report |
| /browse | Headless Chromium for browser verification |
| /benchmark | REQUIRED: Core Web Vitals before/after delta |
| /codex | REQUIRED: cross-model review vs /review |
| /pair-agent | Bridge Hermes into Claude Code session |
| /ship | Open PR with coverage audit |
| /land-and-deploy | Post-merge deploy verification |
| /canary | Post-deploy 10-minute monitoring |
| /document-release | Update stale docs on merge |
| /context-save | Save context before handoff |
| /context-restore | Restore context at session start |
| /retro | Weekly retrospective |
