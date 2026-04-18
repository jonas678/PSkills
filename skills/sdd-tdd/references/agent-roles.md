# Agent Roles & Briefing Guide

Use this when writing prompts for subagents in Phase 2 (QA) and Phase 3 (implementation). Each agent starts cold — it has zero context from your conversation. The brief must be fully self-contained.

---

## QA Agent (Phase 2)

**When to spawn:** Always, once the spec is approved.

**What it returns:** A structured list of proposed test cases (descriptions only, not code). You'll present these to the user in plan mode before writing any code.

**Brief template:**
```
You are a QA engineer reviewing a software design spec to draft test cases.

Read the spec at: <path to docs/specs/<feature>.md>

Focus on the Components/Interface section — every API endpoint or interface defined there
needs a corresponding test that calls it at the HTTP/transport level and asserts the exact
response defined in the spec. Do not test internal functions directly; test the contract.

Draft test cases for each layer present in the spec:

BACKEND API tests (one per endpoint in the spec):
- Happy path: valid input → assert HTTP status + exact response body shape from spec
- Invalid input: missing/wrong fields → assert error status + error format from spec
- Auth: unauthenticated request → assert 401/403 as specified
- Boundary: empty list, max size, special characters in string fields

FRONTEND tests (if UI is in scope):
- Render with valid props → assert expected output is visible
- User interaction (click, type, submit) → assert state change or emitted event
- Error state → assert error UI renders correctly

E2E tests via Playwright (main journeys only, not per endpoint):
- Primary success flow end-to-end
- One critical failure journey

For each test case return:
- Test name (descriptive)
- Layer (backend-api / frontend / e2e)
- What it tests (one sentence)
- Input / precondition
- Expected outcome (be specific — status code, field names, UI text)

Do NOT write test code yet — return the structured list only.

Testing framework: <from status.json test_commands>
Language: <language>
```

**After getting back the list:** present it in plan mode. After approval, either spawn the QA agent again to write the code or write it yourself based on the approved list.

---

## Frontend Agent (Phase 3)

**When to spawn:** When the spec involves UI components, pages, routing, or user interaction.

**Brief template:**
```
You are a frontend developer implementing a feature based on a design spec.

Read the spec at: <path to docs/specs/<feature>.md>

Your responsibility: implement the frontend portion — <describe specifically: e.g., the LoginPage component and its form, the UserDashboard route, etc.>

Shared contracts (read before starting):
<list any shared type files, API interface files written by the orchestrator>

Tests you must make pass:
<path to test file(s)>

Run the tests after implementation with: <test command>
Report which tests pass and which still fail.

Tech stack: <framework, e.g., React, Vue, Svelte>
Style: <CSS approach, e.g., Tailwind, CSS Modules>
```

---

## Backend Agent (Phase 3)

**When to spawn:** When the spec involves API endpoints, business logic, or server-side processing.

**Brief template:**
```
You are a backend developer implementing a feature based on a design spec.

Read the spec at: <path to docs/specs/<feature>.md>

Your responsibility: implement the backend portion — <describe specifically: e.g., the /auth/login endpoint, the UserService class, etc.>

Shared contracts (read before starting):
<list any shared type files, API contracts written by the orchestrator>

Tests you must make pass:
<path to test file(s)>

Run the tests after implementation with: <test command>
Report which tests pass and which still fail.

Language/framework: <e.g., Node/Express, Python/FastAPI, Go/gin>
Database: <if applicable>
```

---

## Database Agent (Phase 3)

**When to spawn:** When the spec requires schema changes, migrations, or complex queries.

**Brief template:**
```
You are a database engineer implementing schema and query changes based on a design spec.

Read the spec at: <path to docs/specs/<feature>.md>
Focus on the Data Models section.

Your responsibility: <describe specifically: e.g., create the users table migration, write the UserRepository queries>

Tests you must make pass:
<path to test file(s)>

Run the tests after implementation with: <test command>

Database: <PostgreSQL, SQLite, MongoDB, etc.>
Migration tool: <e.g., Prisma, Flyway, Alembic>
```

---

## Full-Stack Agent (Phase 3)

**When to use instead of separate frontend/backend agents:** When the feature is narrow enough that splitting creates more coordination overhead than it saves, or when the frontend and backend are tightly coupled with shared state that can't be cleanly separated upfront.

**Brief template:**
```
You are a full-stack developer implementing a complete feature based on a design spec.

Read the spec at: <path to docs/specs/<feature>.md>

Implement the full feature end-to-end:
- Frontend: <describe>
- Backend: <describe>
<add database if applicable>

Tests you must make pass:
<path to test file(s)>

Run the tests after implementation with: <test command>
Report which tests pass and which still fail.

Tech stack: <full stack description>
```

---

## Tips for all agents

- Always include the spec path — agents must read it, not rely on the brief alone
- Always include specific test paths and the run command
- Always describe what "done" means (tests pass)
- Cross-agent dependencies must be resolved by the orchestrator (you) before spawning — don't ask agents to coordinate with each other
