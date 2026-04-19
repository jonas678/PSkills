# PSkills — Claude Code Plugin

A Claude Code plugin with skills for spec-first development, multi-agent coordination, and browser automation.

## Installation

```bash
claude plugin install jonas678/PSkills
```

Or manually:

```bash
git clone https://github.com/jonas678/PSkills /tmp/PSkills
cp -r /tmp/PSkills/skills/* ~/.claude/skills/
```

---

## Skills included

### `/prd-planner`

**The PM-first Product Requirement Planner**

Turn vague product requests into structured planning outputs that can progress from discovery to PRD, supporting documents, and team handoff.
- **Requirement clarification**: Ask high-impact questions first to shape new systems or existing feature enhancements.
- **Handoff & Execution**: Generate role-specific briefs (Designer, Engineering, QA) with direct-copy prompts.
- **Automated Swarm Teams**: Automatically sets up an Agent Team (using `TeamCreate` and `run_in_background`) so the PM session orchestrates downstream Designer, QA, and Engineering agents without context pollution.
- **Seamlessly integrates with `/sdd-tdd`**: Passes the generated `PRD.md` and `Engineering-Brief.md` directly to the development workflow.

### `/qa-engineer`

**The Dedicated Quality Assurance Agent**

A specialized agent skill designed to enforce Test-Driven Development (TDD) and objective verification. Use this standalone or spawn it as an agent during a team Swarm.
- **Mode 1 (Strategy)**: Reads PRDs/Briefs to output structured test dimensions, risk models, and edge cases.
- **Mode 2 (TDD Red)**: Reads SDDs/Specs to write failing test cases (unit/e2e/integration). Strictly prohibited from writing feature implementation.
- **Mode 3 (TDD Green / Verify)**: Independently runs test suites and reports PASS/FAIL logs back to the orchestrator. Strictly prohibited from fixing bugs itself to maintain verification integrity.

### `/ui-designer`

**The Dedicated UI/UX Prototyping Agent**

A specialized design agent that creates high-fidelity HTML/CSS/JS prototypes (using Tailwind CDN) directly from a PRD or Designer Brief.
- **Zero-Build Prototypes**: Creates standalone HTML files in `docs/prototypes/<feature-slug>/index.html` that the PM can double-click and review instantly.
- **State Completeness**: Automatically mocks empty, loading, error, and success states for visual review before any backend engineering begins.
- **Multi-Round Feedback**: Can be spawned in a Swarm. Goes idle when the prototype is ready, allowing the PM to review in their browser and send iterative feedback via `SendMessage`.

### `/frontend-design`

**The Dedicated Frontend Engineer**

A standardized frontend design and component generation skill. It bridges the gap between raw UI prototypes (from `/ui-designer`) and production-ready React/Vue components.
- **Component-Driven**: Enforces reusable, single-responsibility components with strict state management (Loading, Error, Success, Empty).
- **Production-Ready**: Automatically manages a11y (accessibility), responsive design (mobile-first Tailwind/CSS), and semantic HTML.
- **Swarm Ready**: Perfectly complements the `/ui-designer` and `/sdd-tdd` workflows. The Frontend Developer agent can stub out API calls with `setTimeout` mocks based on the SDD until the Backend developer finishes their part.

### `/sdd-tdd`

Three-phase workflow: **Spec → Test → Implement**

- **Phase 1 (SPEC)**: Enters plan mode and produces a full design document — Refined Requirements, As-Is, Technical Design, To-Be, and Team Composition. The approved plan becomes the spec. Works for new features (Mode A) and enhancements to existing systems (Mode B).
- **Phase 2 (TEST)**: A QA agent drafts test cases from the spec. Tests are approved via plan mode, then written. Tests must be **RED** (failing) before implementation starts.
- **Phase 3 (IMPLEMENT)**: Implementation agents (frontend, backend, database) run in parallel based on the team decided in Phase 1. A QA agent acts as independent verifier after implementation — it runs the full suite and reports pass/fail without fixing anything. Tests must be **GREEN**.

Features:
- All flow artifacts per feature in `docs/specs/<feature-slug>/` (spec, test cases, status)
- Phase state tracked in `docs/specs/<feature-slug>/status.json` — resume mid-flow across sessions
- Team composition and coordination pattern decided in Phase 1, read by Phases 2 and 3
- Project-scoped hook watches `docs/specs/*/status.json` and reminds you of incomplete phases
- Status file updated with Read → Write tools only (no bash/jq)

### `coordination-patterns`

Decision aid for shaping non-trivial work. Helps choose between doing work solo, spawning subagents, write-then-verify loops, or plan-first approaches. Based on 5 patterns: Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, Shared State.

### `playwright-cli`

Automates browser interactions and Playwright test workflows within Claude Code sessions.

---

## Example walkthrough

Here's what happens when you run `/sdd-tdd user login with JWT`.

---

### Setup (automatic)

Claude creates the feature folder and two project-level files:

**`docs/specs/user-login-jwt/status.json`** — tracks progress across sessions:
```json
{
  "feature": "user-login-jwt",
  "description": "user login with JWT",
  "mode": null,
  "started_at": "2026-04-18T10:00:00Z",
  "spec_file": null,
  "test_cases_file": null,
  "test_files": [],
  "test_commands": {},
  "team": {},
  "coordination_pattern": null,
  "phases": {
    "spec":      { "status": "pending", "completed_at": null },
    "test":      { "status": "pending", "completed_at": null, "red_verified": false },
    "implement": { "status": "pending", "completed_at": null, "green_verified": false }
  }
}
```

**`.claude/hooks/sdd-tdd-check.sh`** — registered in `.claude/settings.json`. Scans all `docs/specs/*/status.json` files and at the end of every Claude turn prints for any incomplete flow:
```
[sdd-tdd] Active flow: user-login-jwt — user login with JWT
[sdd-tdd] Pending: Phase 1 (SPEC), Phase 2 (TEST), Phase 3 (IMPLEMENT)
```
It goes silent once all phases are complete. Each new feature adds a new subfolder — no archiving or renaming needed.

---

### Phase 1 — SPEC

Claude enters **plan mode** and presents the full design document:

```
## Refined Requirements
User story: As a registered user, I can log in with email + password and receive a JWT
  so I can access protected routes.
Acceptance criteria:
  - Valid credentials → 200 + { token, user: { id, email, role } }
  - Invalid credentials → 401 + { error: "invalid credentials" }
  - Missing fields → 400 + field-level errors
Out of scope: registration, password reset, OAuth

## As-Is
No auth system exists. Routes are currently unprotected.

## Technical Design
POST /auth/login → AuthController → UserRepository (findByEmail) → JWTService (sign)
Data models:
  User { id, email, passwordHash, role: "admin"|"user" }
  LoginResponse { token: string, user: { id, email, role } }
Key decision: JWT expiry 15min + refresh token (short-lived reduces leak exposure)

## To-Be
New: AuthController, JWTService, UserRepository, /auth/login endpoint
All protected routes gain JWT middleware

## Team Composition
- QA: Phase 2 test author + Phase 3 verifier
- Backend agent: AuthController, JWTService, UserRepository, JWT middleware
(No frontend agent — API only per scope)

Approve?
```

You approve (or correct it). Claude writes the spec from the plan content in context:

**`docs/specs/user-login-jwt/spec.md`** — mirrors the approved plan structure exactly.

Claude then records the team in status.json and confirms test commands:
```json
{
  "mode": "new-feature",
  "spec_file": "docs/specs/user-login-jwt/spec.md",
  "test_commands": { "backend": "pytest", "e2e": "PLAYWRIGHT_HTML_OPEN=never npx playwright test" },
  "team": {
    "qa": "Phase 2 test author + Phase 3 verifier",
    "backend": "implements AuthController, JWTService, UserRepository, JWT middleware"
  },
  "coordination_pattern": "orchestrator-subagent"
}
```

---

### Phase 2 — TEST

Claude spawns a **QA agent** that reads the spec and drafts test cases. Claude enters **plan mode**:

```
Proposed test cases:

BACKEND API (pytest + httpx):
1. POST /auth/login — valid credentials → 200 + { token, user.email }
2. POST /auth/login — wrong password → 401 + { error: "invalid credentials" }
3. POST /auth/login — missing email → 400 + { error: "email required" }
4. POST /auth/login — unknown email → 401 (no user enumeration)
5. GET /profile — no Authorization header → 401

E2E (Playwright):
6. Login flow — fill form, submit, assert redirected to dashboard
7. Login failure — wrong password, assert error message visible

Approve, or add/remove tests?
```

Claude writes the test case list to `docs/specs/user-login-jwt/test-cases.md` (descriptions only, no code), then enters plan mode. After approval, test code is written:
```
docs/specs/user-login-jwt/
└── test-cases.md      ← approved test case descriptions
tests/
├── test_auth_api.py   ← backend HTTP contract tests
└── e2e/
    └── login.spec.ts  ← Playwright E2E tests
```

Tests run — must all **FAIL (RED)**:
```
FAILED tests/test_auth_api.py::test_login_valid - ConnectionRefusedError
FAILED tests/test_auth_api.py::test_login_wrong_password - ConnectionRefusedError
...
✓ All 7 tests RED. Proceeding to Phase 3.
```

---

### Phase 3 — IMPLEMENT

Claude enters **plan mode** to confirm the implementation plan from the Phase 1 team:

```
Implementation plan (from Phase 1 team):
- Backend agent: AuthController, JWTService, UserRepository, JWT middleware
  Owns: tests/test_auth_api.py, e2e/login.spec.ts
- Shared contract to write first: src/types/user.ts

Approve?
```

After approval:
1. Claude writes `src/types/user.ts` (shared contract)
2. Backend agent runs — reads spec, implements, runs its tests
3. **QA verifier agent** runs the full suite independently and reports:

```
PASSED tests/test_auth_api.py::test_login_valid
PASSED tests/test_auth_api.py::test_login_wrong_password
...
✓ All 7 tests GREEN.
```

All phases complete. The hook goes silent (all phases in status.json show `"completed"`).

---

### Files generated by the end

```
your-project/
├── .claude/
│   ├── settings.json                     ← hook + AGENT_TEAMS flag
│   └── hooks/sdd-tdd-check.sh            ← phase reminder hook
├── docs/specs/
│   └── user-login-jwt/
│       ├── spec.md                       ← the spec (from approved plan)
│       ├── test-cases.md                 ← approved QA test case descriptions
│       └── status.json                   ← flow tracking (all phases completed)
├── tests/
│   ├── test_auth_api.py                  ← backend API tests
│   └── e2e/login.spec.ts                 ← Playwright E2E tests
└── src/
    ├── types/user.ts                     ← shared type (pre-agent)
    ├── controllers/auth.py
    ├── services/jwt.py
    └── repositories/user.py
```

---

## Project structure

```
skills/
├── sdd-tdd/
│   ├── SKILL.md                  ← orchestration rules (loaded always)
│   └── references/
│       ├── setup.md              ← status file + hook setup
│       ├── phase-1-spec.md       ← loaded during Phase 1
│       ├── phase-2-test.md       ← loaded during Phase 2
│       ├── phase-3-implement.md  ← loaded during Phase 3
│       └── agent-roles.md        ← QA (Phase 2 + Phase 3 verifier), frontend, backend, DB briefs
├── coordination-patterns/
│   └── SKILL.md
└── playwright-cli/
    └── SKILL.md
```
