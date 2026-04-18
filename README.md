# SDD-TDD Workflow Plugin

A Claude Code plugin that enforces a spec-first, test-driven development workflow with plan mode human gates and multi-agent coordination.

## Skills included

### `/sdd-tdd`
Three-phase workflow: **Spec → Test → Implement**

- **Phase 1 (SPEC)**: Writes a specification document (SDD = Specification-Driven Development) with plan mode approval before any code is written. Supports both new features (Mode A) and enhancements to existing systems (Mode B).
- **Phase 2 (TEST)**: A QA agent drafts test cases from the spec. Tests are approved via plan mode, then written. Tests must be **RED** (failing) before implementation starts.
- **Phase 3 (IMPLEMENT)**: Specialized agents (frontend, backend, database) implement in parallel. Tests must be **GREEN** before the phase completes.

Features:
- Phase state tracked in `.claude/sdd-tdd-status.json` — resume mid-flow across sessions
- Project-scoped hook reminds you of incomplete phases at end of each turn
- Hook has zero effect on other projects

### `coordination-patterns`
Decision aid for shaping non-trivial work. Helps you choose between doing work solo, spawning subagents, write-then-verify loops, or plan-first approaches. Based on 5 patterns: Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, Shared State.

## Installation

```bash
claude plugin install https://github.com/jonas678/PSkills
```

## Usage

```
/sdd-tdd <feature description>
```

The skill will:
1. Check for an existing in-progress flow (resume if found)
2. Set up `.claude/sdd-tdd-status.json` and install a project-scoped hook
3. Walk you through each phase with plan mode gates

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
  "phases": {
    "spec":      { "status": "pending", "completed_at": null },
    "test":      { "status": "pending", "completed_at": null, "red_verified": false },
    "implement": { "status": "pending", "completed_at": null, "green_verified": false }
  }
}
```

**`.claude/hooks/sdd-tdd-check.sh`** — a project-scoped hook registered in `.claude/settings.json`. At the end of every Claude turn it prints:
```
[sdd-tdd] Active flow: user-login-jwt — user login with JWT
[sdd-tdd] Pending: Phase 1 (SPEC), Phase 2 (TEST), Phase 3 (IMPLEMENT)
```
It goes silent once all phases are complete.

---

### Phase 1 — SPEC

Claude enters **plan mode** and proposes the spec outline:

```
I'll write a spec covering:
- Mode A (new feature)
- Components: AuthController (POST /auth/login), JWTService, UserRepository
- Interface: POST /auth/login → { token: string, user: { id, email, role } }
- Data model: User { id, email, passwordHash, role }
- Decisions: JWT expiry 15min + refresh token, bcrypt for password hashing
- NFRs: 401 on invalid credentials, 400 on missing fields

Shall I proceed?
```

You approve (or correct it). Claude then writes:

**`docs/specs/user-login-jwt.md`**
```markdown
# Spec: User Login with JWT

**Date:** 2026-04-18
**Mode:** New Feature
**Status:** Approved

## 1. Goal & Scope
Allow users to authenticate via email/password and receive a JWT...

## 2. Components
- **AuthController** — handles POST /auth/login
  Interface: receives { email, password }, returns { token, user } or 4xx
- **JWTService** — signs and verifies tokens
  Interface: sign(payload) → string, verify(token) → payload | null
- **UserRepository** — queries user by email
  Interface: findByEmail(email: string) → User | null

## 3. Data Models
type User = { id: string, email: string, passwordHash: string, role: "admin"|"user" }
type LoginResponse = { token: string, user: { id: string, email: string, role: string } }

## 4. Key Decisions
Decision: JWT expiry 15 minutes + refresh token
Why: short-lived access tokens reduce exposure if leaked
...
```

Claude also asks you to confirm the test commands:
```
Backend test command: pytest  (or npm test, go test ./...)
E2E test command: PLAYWRIGHT_HTML_OPEN=never npx playwright test
```

---

### Phase 2 — TEST

Claude spawns a **QA agent** that reads the spec and drafts test cases. Claude enters **plan mode** to show you the list:

```
Proposed test cases:

BACKEND API (pytest + httpx):
1. POST /auth/login — valid credentials → 200 + { token, user.email }
2. POST /auth/login — wrong password → 401 + { error: "invalid credentials" }
3. POST /auth/login — missing email field → 400 + { error: "email required" }
4. POST /auth/login — unknown email → 401 (same error, no user enumeration)
5. GET /profile — no Authorization header → 401

E2E (Playwright):
6. Login flow — fill form, submit, assert redirected to dashboard
7. Login failure — wrong password, assert error message visible

Approve, or add/remove tests?
```

After approval, tests are written to:
```
tests/
├── test_auth_api.py       ← backend HTTP contract tests (httpx)
└── e2e/
    └── login.spec.ts      ← Playwright E2E tests
```

Claude runs the tests — they must all **FAIL (RED)**:
```
FAILED tests/test_auth_api.py::test_login_valid - ConnectionRefusedError
FAILED tests/test_auth_api.py::test_login_wrong_password - ConnectionRefusedError
...
✓ All 7 tests are RED. Proceeding to Phase 3.
```

---

### Phase 3 — IMPLEMENT

Claude enters **plan mode** to propose the agent split:

```
Implementation plan:
- Backend agent: AuthController, JWTService, UserRepository, DB migration
- No frontend agent needed (API only per spec)

Backend agent will own: tests/test_auth_api.py
E2E agent will handle login.spec.ts after backend is done

Dependencies to resolve first: shared User type → src/types/user.ts

Approve?
```

After approval, Claude writes `src/types/user.ts`, then spawns the **backend agent**. The agent reads the spec, implements the endpoints, and runs the tests. Claude verifies:

```
PASSED tests/test_auth_api.py::test_login_valid
PASSED tests/test_auth_api.py::test_login_wrong_password
...
✓ All 7 tests GREEN. Implementation complete.
```

Status file is archived to `.claude/sdd-tdd-status.user-login-jwt.json`. The hook goes silent.

---

### Files generated by the end

```
your-project/
├── .claude/
│   ├── settings.json                          ← hook registered here
│   ├── hooks/sdd-tdd-check.sh                 ← phase reminder hook
│   ├── sdd-tdd-status.user-login-jwt.json     ← archived (flow complete)
├── docs/specs/
│   └── user-login-jwt.md                      ← the spec
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
│   ├── SKILL.md                  ← orchestration (loaded always)
│   └── references/
│       ├── setup.md              ← hook + status file setup
│       ├── phase-1-spec.md       ← loaded only during Phase 1
│       ├── phase-2-test.md       ← loaded only during Phase 2
│       ├── phase-3-implement.md  ← loaded only during Phase 3
│       ├── spec-template.md      ← Mode A/B spec templates
│       └── agent-roles.md        ← QA, frontend, backend, DB agent briefs
└── coordination-patterns/
    └── SKILL.md
```
