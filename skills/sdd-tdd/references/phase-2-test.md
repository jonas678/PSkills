# Phase 2: TEST

Goal: write tests from the spec (not from the implementation), then confirm they fail. Tests that pass before implementation wasn't testing anything new.

## Steps

**2a.** Update status: `phases.test.status = "in_progress"`

**2b. Spawn a QA agent** to draft test case descriptions. The agent starts cold — brief it fully:
- Path to the spec file
- Testing framework and language (ask user if unknown)
- Coverage required: happy path, edge cases, error/failure cases, boundary conditions
- Return format: structured list of descriptions only (no code yet)

See `references/agent-roles.md` → QA Agent section for the brief template.

**2c. Enter plan mode.** Present the QA agent's proposed test cases:
- Each test name and purpose
- Any coverage gaps or risky areas you notice

Wait for user to approve, add, or remove tests.

**2d. Exit plan mode.** Write the actual test code based on the approved list. Save to the project's test directory.

**2e. Run the tests.** They MUST be RED (all failing).

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
