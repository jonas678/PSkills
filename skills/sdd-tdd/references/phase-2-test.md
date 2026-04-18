# Phase 2: TEST

Goal: write tests from the spec (not from the implementation), then confirm they fail. Tests that pass before implementation wasn't testing anything new.

## Steps

**2a.** Update `docs/specs/<feature-slug>/status.json` using Read → merge → Write (never bash/jq): set `phases.test.status = "in_progress"` in the full JSON and write it back.

**2b. Read the team from status.json** (`team` field). The QA agent's responsibilities and which layers to cover were decided and approved in Phase 1 — no re-derivation needed here.

**2c. Spawn the QA agent.** The agent starts cold — brief it fully:
- Path to the spec file
- Testing framework, language, and test commands from `status.json → test_commands`
- Layers to cover: read from `status.json → team` (frontend, backend, E2E — only what was decided)
- Coverage requirements per layer (see below)
- Return format: structured list of test case descriptions only (no code yet)

**Coverage requirements by layer:**

*Backend API tests* — the most critical for spec-driven TDD. For each endpoint in the spec:
- Call via HTTP transport level (Supertest, httpx, `net/http/httptest`, etc.) — not unit tests on internal functions
- Assert exact response: status code, body shape, field names and types
- Negative cases: invalid input → error format and status code from spec
- Auth cases: unauthenticated → 401/403 as specified

*Frontend tests*:
- Render with valid props → assert expected output
- User interactions (click, type) → assert state changes or emitted events
- Error states → assert error UI renders correctly

*E2E tests (Playwright)* — full user flows only, not per-endpoint:
- Main success journey described in the spec
- One or two critical failure journeys

See `references/agent-roles.md` → QA Agent section for the brief template.

**2d. Write the test cases to `docs/specs/<feature-slug>/test-cases.md`** using the Write tool. This is the raw QA agent output — descriptions only, no code. Structure it exactly as returned: grouped by layer, with test name, what it tests, input/precondition, and expected outcome for each entry.

**2e. Enter plan mode.** Present the QA agent's proposed test cases (reference the saved file):
- Each test name and purpose
- Any coverage gaps or risky areas you notice

Wait for user to approve, add, or remove tests. Update `docs/specs/<feature-slug>/test-cases.md` to reflect any changes made during approval before exiting plan mode.

**2f. Exit plan mode.** Write the actual test code based on the approved list. Save to the project's test directory.

**2g. Run the tests** using the commands from `status.json → test_commands`. They MUST be RED (all failing).

If any pass unexpectedly: investigate whether the feature partially exists. Tell the user — do not proceed until you understand why.

**2h. Update `docs/specs/<feature-slug>/status.json`** using Read → merge → Write (never bash/jq):
- `test_cases_file`: `"docs/specs/<feature-slug>/test-cases.md"`
- `test_files`: list of written test file paths
- `phases.test.status`: `"completed"`
- `phases.test.completed_at`: current ISO timestamp
- `phases.test.red_verified`: `true`

Confirm RED status to the user. Ask if they want to proceed to Phase 3.
