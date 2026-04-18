# Phase 2: TEST

Goal: write tests from the spec (not from the implementation), then confirm they fail. Tests that pass before implementation wasn't testing anything new.

## Steps

**2a.** Update status: `phases.test.status = "in_progress"`

**2b. Spawn a QA agent** to draft test case descriptions. The agent starts cold — brief it fully:
- Path to the spec file
- Testing framework, language, and test commands from `status.json → test_commands`
- Layers involved: frontend, backend, E2E (only what the spec covers)
- Coverage required per layer (see below)
- Return format: structured list of descriptions only (no code yet)

**Coverage requirements by layer:**

*Backend API tests* — the most critical for spec-driven TDD. For each endpoint in the spec's Components/Interface section:
- Call the endpoint via HTTP (Supertest, httpx, `net/http/httptest`, etc.) — not unit tests on internal functions
- Assert exact response: status code, response body shape, field names and types
- Negative cases: invalid input → assert the error format and status code defined in the spec
- Auth cases: unauthenticated request → assert 401/403 as specified

*Frontend tests* — component/unit level:
- Render with valid props → assert expected output
- User interactions (click, type) → assert state changes or emitted events
- Error states → assert error UI renders correctly

*E2E tests (Playwright)* — full user flows only, not per-endpoint:
- The main success journey described in the spec
- One or two critical failure journeys

See `references/agent-roles.md` → QA Agent section for the brief template.

**2c. Enter plan mode.** Present the QA agent's proposed test cases:
- Each test name and purpose
- Any coverage gaps or risky areas you notice

Wait for user to approve, add, or remove tests.

**2d. Exit plan mode.** Write the actual test code based on the approved list. Save to the project's test directory.

**2e. Run the tests** using the commands confirmed in Phase 1 (`status.json → test_commands`). They MUST be RED (all failing).

If any pass unexpectedly: investigate whether the feature partially exists. Tell the user — do not proceed until you understand why.

**2f. Update status.json:**
```json
{
  "test_files": ["<path to test file(s)>"],
  "phases": {
    "test": { "status": "completed", "completed_at": "<ISO timestamp>", "red_verified": true }
  }
}
```

Confirm RED status to the user. Ask if they want to proceed to Phase 3.
