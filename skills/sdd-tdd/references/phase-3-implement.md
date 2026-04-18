# Phase 3: IMPLEMENT

Goal: make the failing tests pass. The team and coordination pattern were decided and approved in Phase 1 — execute that decision.

## Steps

**3a.** Update status using Read → merge → Write (never bash/jq): set `phases.implement.status = "in_progress"` in the full JSON and write it back.

**3b. Read from status.json:**
- `team` — which agents to spawn and what each owns
- `coordination_pattern` — how to run them (`"orchestrator-subagent"` or `"agent-teams"`)

If the spec scope changed substantially since Phase 1, note it and confirm with the user before proceeding.

**3c. Enter plan mode.** Present the implementation plan:
- Coordination pattern being used and what it means for execution
- Which agents will run, what each implements
- Which test files each agent owns
- Shared contracts the orchestrator will write before spawning (types, API interfaces)
- Sequencing (parallel where independent, sequential where one depends on another)

Wait for user approval.

**3d. Exit plan mode.** Write shared types, interfaces, or API contracts to files before spawning. Agents cannot coordinate with each other mid-flight — all shared state must be resolved upfront.

**3e. Spawn implementation agents** following the chosen pattern:

### Orchestrator-Subagent

The orchestrator directs each agent step-by-step and synthesizes results between spawns.

- Spawn agents in parallel if truly independent, sequentially if one depends on another's output
- Each agent brief: implement the assigned scope, run the test command, report which tests pass and which fail
- After each agent returns: review output, resolve any issues yourself before spawning the next

### Agent Teams

Each agent runs the full cycle autonomously — implement → run tests → fix failures → report final state. The orchestrator only reviews the final reports.

- Spawn all agents in parallel (they are truly independent end-to-end workers)
- Each agent brief must explicitly say:
  - Implement the assigned scope
  - Run the test command
  - Fix any test failures yourself until all assigned tests pass or you exhaust reasonable fixes
  - Report final test results (pass/fail counts + any remaining failures with details)
- After all agents return: review the reports; if any failures remain, make a targeted fix yourself and re-run the affected tests

For both patterns, every agent brief must include:
- Spec file path (agents must read it)
- Test case descriptions file path (`status.json → test_cases_file`)
- Test file path(s) they own
- Test run command
- Any shared contract files written in 3d

See `references/agent-roles.md` for Frontend, Backend, Database, and Full-Stack brief templates.

**3f. Spawn QA agent as verifier.** After all implementation agents complete:
- Brief: spec path, all test files, all test run commands
- QA runs the full test suite and reports per-test pass/fail with failure details
- QA does NOT fix failures — it reports what's failing and why

If RED after QA verification: read QA's report, make a targeted fix, re-spawn QA to verify again. Do not re-implement from scratch.

**3g. Update status.json** using Read → merge → Write (never bash/jq):
- `phases.implement.status`: `"completed"`
- `phases.implement.completed_at`: current ISO timestamp
- `phases.implement.green_verified`: `true`

Tell the user: implementation complete, tests GREEN. The hook will go silent — all phases done.
