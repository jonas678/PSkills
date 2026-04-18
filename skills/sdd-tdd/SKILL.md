---
name: sdd-tdd
description: Three-phase skill that combines Software Design Documents (SDD), Test-Driven Development (TDD), plan mode human gates, and multi-agent coordination. Use this whenever the user types /sdd-tdd, wants to design-then-test-then-implement a feature, asks for spec-first development, wants a QA agent to write tests before code, or says things like "write the spec first", "design before code", "spec → test → implement", or "I want to plan before building". The skill enforces: spec approval → tests written and RED → implementation → tests GREEN. Always trigger this skill when the user invokes /sdd-tdd.
---

# SDD-TDD: Spec → Test → Implement

Three phases, each gated by plan mode approval. You are the orchestrator throughout.

```
SETUP  → check resume status → init .claude/sdd-tdd-status.json + project hook
PHASE 1 (SPEC)     → plan mode → approve → write spec → update status
PHASE 2 (TEST)     → QA agent → plan mode → approve → write tests → verify RED → update status
PHASE 3 (IMPLEMENT)→ plan mode → approve → spawn agents → verify GREEN → update status
```

---

## Start here: check for existing flow

Before anything, check if `.claude/sdd-tdd-status.json` exists:
- **All phases completed** → archive it to `.claude/sdd-tdd-status.<feature-slug>.json`, then start fresh
- **Some phases incomplete** → resume from the first incomplete phase; skip completed ones
- **No file** → fresh start

Then read **`references/setup.md`** and follow it to initialize status.json and install the project-scoped hook.

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
- Update `status.json` at every phase transition (the hook depends on it)
- When in doubt, ask — ambiguities caught in Phase 1 cost nothing; in Phase 3 they're expensive
