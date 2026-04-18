
---
name: sdd-tdd
description: Three-phase skill that combines Specification-Driven Development (SDD), Test-Driven Development (TDD), plan mode human gates, and multi-agent coordination. Use this whenever the user types /sdd-tdd, wants to design-then-test-then-implement a feature, asks for spec-first development, wants a QA agent to write tests before code, or says things like "write the spec first", "design before code", "spec → test → implement", or "I want to plan before building". The skill enforces: spec approval → tests written and RED → implementation → tests GREEN. Always trigger this skill when the user invokes /sdd-tdd.
---

# SDD-TDD: Spec → Test → Implement

Three phases, each gated by plan mode approval. You are the orchestrator throughout.

```
SETUP  → check resume status → init .claude/sdd-tdd-status.json + project hook
PHASE 1 (SPEC)     → plan mode → approve → write spec → update status
PHASE 2 (TEST)     → QA agent → plan mode → approve → write tests → verify RED → update status
PHASE 3 (IMPLEMENT)→ plan mode → approve → spawn impl agents → spawn QA verifier agent → GREEN → update status
```

---

## Start here: check for existing flow

Before anything, search for `docs/specs/*/status.json`:
- **Found with incomplete phases** → resume from the first incomplete phase; skip completed ones
- **Found but all completed** → start a new feature subfolder
- **None found** → fresh start

Then read **`references/setup.md`** and follow it to initialize the feature folder, status.json, and the project-scoped hook.

---

## Phases

Enter each phase only when the previous one is complete. Read the phase reference file at the moment you enter that phase — not before.

| Phase | When to enter | Reference to read |
|-------|--------------|-------------------|
| **1 — SPEC** | After setup | `references/phase-1-spec.md` |
| **2 — TEST** | After spec approved & written | `references/phase-2-test.md` |
| **3 — IMPLEMENT** | After tests written & RED confirmed | `references/phase-3-implement.md` |

---

## Rules that apply across all phases

- Plan mode is a real gate — incorporate corrections fully before proceeding
- The spec file is the source of truth for all agents
- Never skip the RED check before implementation
- **Never run tests yourself at the end of Phase 3** — always spawn the QA verifier Agent; running `npm test` or any test command directly is not a substitute
- Update `status.json` at every phase transition (the hook depends on it)
- When in doubt, ask — ambiguities caught in Phase 1 cost nothing; in Phase 3 they're expensive

## How to update status.json

**Always use Read → Edit/Write tools. Never use bash/jq commands to modify status.json.**

```
1. Read the current .claude/sdd-tdd-status.json with the Read tool
2. Merge your changes into the full JSON object in memory
3. Write the complete updated JSON back with the Write tool
```

Bash jq commands fail on whitespace, trailing commas, or any slight JSON irregularity. The Read/Write approach is always safe.
