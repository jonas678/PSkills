# SDD-TDD Workflow Plugin

A Claude Code plugin that enforces a spec-first, test-driven development workflow with plan mode human gates and multi-agent coordination.

## Skills included

### `/sdd-tdd`
Three-phase workflow: **Spec → Test → Implement**

- **Phase 1 (SPEC)**: Writes a Software Design Document (SDD) with plan mode approval before any code is written. Supports both new features (Mode A) and enhancements to existing systems (Mode B).
- **Phase 2 (TEST)**: A QA agent drafts test cases from the spec. Tests are approved via plan mode, then written. Tests must be **RED** (failing) before implementation starts.
- **Phase 3 (IMPLEMENT)**: Specialized agents (frontend, backend, database) implement in parallel. Tests must be **GREEN** before the phase completes.

Features:
- Phase state tracked in `.claude/sdd-tdd-status.json` — resume mid-flow across sessions
- Project-scoped hook reminds you of incomplete phases at end of each turn
- Hook has zero effect on other projects

### `coordination-patterns`
Decision aid for shaping non-trivial work. Helps you choose between doing work solo, spawning subagents, write-then-verify loops, or plan-first approaches. Based on 5 patterns: Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, Shared State.

## Installation

```bash
claude plugin install https://github.com/jonas678/PSkills
```

## Usage

```
/sdd-tdd <feature description>
```

The skill will:
1. Check for an existing in-progress flow (resume if found)
2. Set up `.claude/sdd-tdd-status.json` and install a project-scoped hook
3. Walk you through each phase with plan mode gates

## Project structure

```
skills/
├── sdd-tdd/
│   ├── SKILL.md                  ← orchestration (loaded always)
│   └── references/
│       ├── setup.md              ← hook + status file setup
│       ├── phase-1-spec.md       ← loaded only during Phase 1
│       ├── phase-2-test.md       ← loaded only during Phase 2
│       ├── phase-3-implement.md  ← loaded only during Phase 3
│       ├── sdd-template.md       ← Mode A/B SDD templates
│       └── agent-roles.md        ← QA, frontend, backend, DB agent briefs
└── coordination-patterns/
    └── SKILL.md
```
