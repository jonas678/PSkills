# Phase 1: SPEC

Goal: get alignment on *what* is being built before a single line of code or test is written.

## Steps

**1a. Determine the change mode:**
- **Mode A (New Feature)** — the thing being built doesn't exist yet
- **Mode B (Enhancement/Update)** — modifying something that already exists

For Mode B: read the relevant existing files first. You can't design a change without understanding what's already there.

Update status: `phases.spec.status = "in_progress"`

**1b. Analyze and identify:**
- Components involved (frontend, backend, API, database, etc.)
- Main interfaces and data contracts (Mode B: current → target)
- Impact on surrounding code (Mode B: who calls what you're changing?)

**1c. Enter plan mode.** Present the SDD outline:
- Selected mode (A or B) and why
- Sections you'll write, main components, their interactions
- Any open questions or assumptions

Wait for approval. Do not write the spec file until the user approves the outline.

**1d. Exit plan mode.** Write the spec to `docs/specs/<feature-slug>.md` using `references/sdd-template.md`.

**1e. Confirm test commands.** Before closing Phase 1, ask the user to confirm the exact commands for each layer involved:

| Layer | Example command |
|-------|----------------|
| Frontend unit/component | `npm test` or `npx vitest` |
| Backend unit/integration | `pytest`, `go test ./...`, `npm test` |
| E2E (Playwright) | `PLAYWRIGHT_HTML_OPEN=never npx playwright test` |

Only ask for layers that the spec actually involves. Store the confirmed commands — they will be used verbatim for the RED check in Phase 2 and the GREEN check in Phase 3.

**1f. Update status.json:**
```json
{
  "mode": "new-feature | enhancement",
  "spec_file": "docs/specs/<feature-slug>.md",
  "test_commands": {
    "frontend": "npm test",
    "backend": "pytest",
    "e2e": "PLAYWRIGHT_HTML_OPEN=never npx playwright test"
  },
  "phases": {
    "spec": { "status": "completed", "completed_at": "<ISO timestamp>" }
  }
}
```

Only include keys for layers that exist. Tell the user where the spec was saved. Ask if they want to proceed to Phase 2.
