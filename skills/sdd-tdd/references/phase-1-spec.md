# Phase 1: SPEC

Goal: align on *what* is being built and *how* it will be delivered before a single line of code or test is written. The plan mode output is the design document — it drives Phases 2 and 3.

## Steps

**1a. Determine the change mode and prerequisites:**
- **Mode A (New Feature)** — the thing being built doesn't exist yet
- **Mode B (Enhancement/Update)** — modifying something that already exists

*Important Integration Note:* If the user has already used the **prd-planner** skill to generate a PRD and handoff briefs (like `Engineering-Brief.md`), **do not reinvent the wheel**. Read those documents first to act as the foundation for this Spec. The SDD should focus strictly on technical architecture, API contracts, and implementation details rather than re-litigating product boundaries.

For Mode B: read the relevant existing files first. You can't design a change without understanding what's already there.

Update status: `phases.spec.status = "in_progress"`

**1b. Apply coordination-patterns** to decide how to do the analysis:
- If the codebase is small or the relevant files are obvious → do it yourself with Read/Grep
- If Mode B and the impact area is large or unclear → use an **Explore subagent** to map affected files before starting (keeps raw search output out of your context)

**1c. Enter plan mode.** Present the design document. The required sections below must always appear. Add any others that help the user evaluate and approve the work — the plan mode can include anything useful.

---

### Required sections

**Refined Requirements / Input Review**
- **Note:** If a `PRD.md` or `Engineering-Brief.md` exists (e.g. generated via `prd-planner`), explicitly reference it here instead of restating it.
- User story and acceptance criteria
- What is explicitly out of scope
- Ambiguities resolved before proceeding

**As-Is** *(skip only if Mode A with no related existing code)*
- What currently exists: components, files, interfaces, behavior
- Pain points or gaps this change addresses

**Technical Design (SDD)**
- This section is the core Software Design Document (SDD).
- API contracts, data models, database schemas, component boundaries
- Key architectural decisions and tradeoffs
- Any non-obvious implementation choices

**To-Be**
- Target state after the feature lands
- What changes vs. stays the same
- Before/after for any interface or contract that changes

**Team Composition & Coordination**

Apply the coordination-patterns decision procedure to Phase 3 *before* writing this section. Ask:

- Are there 2–6 bounded domains the orchestrator will direct one at a time or in parallel? → **Orchestrator-Subagent**: orchestrator spawns each agent, collects output, decides next step.
- Are there N truly independent long-running items where each agent should run the full cycle autonomously (implement → run own tests → fix failures → report)? → **Agent Teams**: agents work end-to-end without orchestrator check-ins between steps.

Then state in the plan:
- Chosen coordination pattern and why
- Which agents will be spawned (QA in Phase 2 + Phase 3 verifier, plus implementation agents)
- What each agent is responsible for
- How Agent Teams agents differ from Orchestrator-Subagent agents in their briefing: Agent Teams agents are briefed to run their full test suite and fix failures autonomously before reporting back; Orchestrator-Subagent agents stop and return after each implementation step for the orchestrator to synthesize
- Cross-agent dependencies and how they'll be resolved upfront (shared types, sequencing)

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

**1d. Exit plan mode.** Write the spec to `docs/specs/<feature-slug>/spec.md` using the **Write tool**. The approved plan content is still in your context — write the spec directly from it. Plan mode does not save files; do not attempt to copy or reference any plan file path. No fixed template; the plan's structure IS the spec structure.

**1e. Confirm test commands.** Ask the user to confirm the exact commands for each layer the spec involves:

| Layer | Example command |
|-------|----------------|
| Frontend unit/component | `npm test` or `npx vitest` |
| Backend unit/integration | `pytest`, `go test ./...`, `npm test` |
| E2E (Playwright) | `PLAYWRIGHT_HTML_OPEN=never npx playwright test` |

Only ask for layers the spec actually involves. Store these — they will be used verbatim in Phase 2 (RED check) and Phase 3 (GREEN check).

**1f. Update status.json** using Read → merge → Write (never bash/jq):

1. Read `docs/specs/<feature-slug>/status.json`
2. Merge these fields into the full JSON:
   - `mode`: `"new-feature"` or `"enhancement"`
   - `spec_file`: `"docs/specs/<feature-slug>/spec.md"`
   - `test_commands`: only layers that exist (backend, frontend, e2e)
   - `team`: one entry per agent decided in the plan
   - `coordination_pattern`: `"orchestrator-subagent"` or `"agent-teams"`
   - `phases.spec.status`: `"completed"`
   - `phases.spec.completed_at`: current ISO timestamp
3. Write the complete JSON back with the Write tool

Tell the user where the spec was saved. Ask if they want to proceed to Phase 2.
