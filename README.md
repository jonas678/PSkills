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

### `/sdd-tdd`

Three-phase workflow: **Spec → Test → Implement**

- **Phase 1 (SPEC)**: Enters plan mode and produces a full design document — Refined Requirements, As-Is, Technical Design, To-Be, and Team Composition. The approved plan becomes the spec. Works for new features (Mode A) and enhancements to existing systems (Mode B).
- **Phase 2 (TEST)**: A QA agent drafts test cases from the spec. Tests are approved via plan mode, then written. Tests must be **RED** (failing) before implementation starts.
- **Phase 3 (IMPLEMENT)**: Implementation agents (frontend, backend, database) run in parallel based on the team decided in Phase 1. A QA agent acts as independent verifier after implementation — it runs the full suite and reports pass/fail without fixing anything. Tests must be **GREEN**.

Features:
- Phase state tracked in `.claude/sdd-tdd-status.json` — resume mid-flow across sessions
- Team composition decided and approved in Phase 1, read by Phases 2 and 3 — no re-derivation
- Project-scoped hook reminds you of incomplete phases at end of each turn
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

Claude creates two files in your project:

**`.claude/sdd-tdd-status.json`** — tracks progress across sessions:
```json
{
  "feature": "user-login-jwt",
  "description": "user login with JWT",
  "mode": null,
  "started_at": "2026-04-18T10:00:00Z",
  "spec_file": null,
  "test_files": [],
  "test_commands": {},
  "team": {},
  "phases": {
    "spec":      { "status": "pending", "completed_at": null },
    "test":      { "status": "pending", "completed_at": null, "red_verified": false },
    "implement": { "status": "pending", "completed_at": null, "green_verified": false }
  }
}
```

**`.claude/hooks/sdd-tdd-check.sh`** — registered in `.claude/settings.json`. At the end of every Claude turn it prints:
```
[sdd-tdd] Active flow: user-login-jwt — user login with JWT
[sdd-tdd] Pending: Phase 1 (SPEC), Phase 2 (TEST), Phase 3 (IMPLEMENT)
```
It goes silent once all phases are complete.

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

**`docs/specs/user-login-jwt.md`** — mirrors the approved plan structure exactly.

Claude then records the team in status.json and confirms test commands:
```json
{
  "mode": "new-feature",
  "spec_file": "docs/specs/user-login-jwt.md",
  "test_commands": { "backend": "pytest", "e2e": "PLAYWRIGHT_HTML_OPEN=never npx playwright test" },
  "team": {
    "qa": "Phase 2 test author + Phase 3 verifier",
    "backend": "implements AuthController, JWTService, UserRepository, JWT middleware"
  }
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

After approval, tests are written:
```
tests/
├── test_auth_api.py       ← backend HTTP contract tests
└── e2e/
    └── login.spec.ts      ← Playwright E2E tests
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

Status file is archived to `.claude/sdd-tdd-status.user-login-jwt.json`. The hook goes silent.

---

### Files generated by the end

```
your-project/
├── .claude/
│   ├── settings.json                          ← hook registered here
│   ├── hooks/sdd-tdd-check.sh                 ← phase reminder hook
│   └── sdd-tdd-status.user-login-jwt.json     ← archived (flow complete)
├── docs/specs/
│   └── user-login-jwt.md                      ← the spec (from approved plan)
├── tests/
│   ├── test_auth_api.py                       ← backend API tests
│   └── e2e/login.spec.ts                      ← Playwright E2E tests
└── src/
    ├── types/user.ts                          ← shared type (pre-agent)
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
