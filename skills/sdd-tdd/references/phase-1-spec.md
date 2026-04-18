# Phase 1: SPEC

Goal: align on *what* is being built and *how* it will be delivered before a single line of code or test is written. The plan mode output is the design document — it drives Phases 2 and 3.

## Steps

**1a. Determine the change mode:**
- **Mode A (New Feature)** — the thing being built doesn't exist yet
- **Mode B (Enhancement/Update)** — modifying something that already exists

For Mode B: read the relevant existing files first. You can't design a change without understanding what's already there.

Update status: `phases.spec.status = "in_progress"`

**1b. Apply coordination-patterns** to decide how to do the analysis:
- If the codebase is small or the relevant files are obvious → do it yourself with Read/Grep
- If Mode B and the impact area is large or unclear → use an **Explore subagent** to map affected files before starting (keeps raw search output out of your context)

**1c. Enter plan mode.** Present the design document. The required sections below must always appear. Add any others that help the user evaluate and approve the work — the plan mode can include anything useful.

---

### Required sections

**Refined Requirements**
- User story and acceptance criteria
- What is explicitly out of scope
- Ambiguities resolved before proceeding — if anything is unclear, resolve it here, not in Phase 3

**As-Is** *(skip only if Mode A with no related existing code)*
- What currently exists: components, files, interfaces, behavior
- Pain points or gaps this change addresses

**Technical Design**
- API contracts, data models, component boundaries
- Key architectural decisions and tradeoffs
- Any non-obvious implementation choices

**To-Be**
- Target state after the feature lands
- What changes vs. stays the same
- Before/after for any interface or contract that changes

**Team Composition & Coordination**
- Which agents will be spawned in Phase 2 and Phase 3
- What each agent is responsible for
- Cross-agent dependencies and how they'll be resolved (shared types, sequencing)
- Example: "QA (Phase 2 author + Phase 3 verifier) + Frontend Agent + Backend Agent in parallel"

---

### Optional sections — add as needed

Add any of these if they help the user evaluate the design:

- **Key Decisions** — ADR-style entries (Decision / Why / Alternatives considered)
- **Non-Functional Requirements** — performance targets, security constraints, error handling expectations
- **Impact Analysis** *(Mode B)* — ripple effects, callers affected, breaking vs. non-breaking
- **Rollback Plan** *(Mode B)* — how to undo if something goes wrong in production
- **Open Questions** — anything still unresolved; resolve before Phase 2 or explicitly accept as assumptions
- **Diagrams or examples** — sequence diagrams, data flow, UI sketches in pseudocode, example requests/responses

---

Wait for approval. Incorporate any corrections fully. Do not write the spec file until the user approves.

**1d. Exit plan mode.** Write the spec to `docs/specs/<feature-slug>.md`. The spec is a formalization of the approved plan — use the same sections the plan used. No fixed template; the plan's structure IS the spec structure.

**1e. Confirm test commands.** Ask the user to confirm the exact commands for each layer the spec involves:

| Layer | Example command |
|-------|----------------|
| Frontend unit/component | `npm test` or `npx vitest` |
| Backend unit/integration | `pytest`, `go test ./...`, `npm test` |
| E2E (Playwright) | `PLAYWRIGHT_HTML_OPEN=never npx playwright test` |

Only ask for layers the spec actually involves. Store these — they will be used verbatim in Phase 2 (RED check) and Phase 3 (GREEN check).

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
  "team": {
    "qa": "Phase 2 test author + Phase 3 verifier",
    "frontend": "implements <describe scope, e.g. LoginPage component and routing>",
    "backend": "implements <describe scope, e.g. /auth/login and /auth/logout endpoints>"
  },
  "phases": {
    "spec": { "status": "completed", "completed_at": "<ISO timestamp>" }
  }
}
```

Only include team members and test commands for layers that exist. Tell the user where the spec was saved. Ask if they want to proceed to Phase 2.
