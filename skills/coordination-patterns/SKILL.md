---
name: coordination-patterns
description: Decision aid for shaping non-trivial work before starting it — helps pick between doing it solo, spawning subagents (single or parallel), write-then-verify loops, and plan-first approaches. Use this skill before any non-trivial coding, planning, refactoring, research, or multi-step task, especially when deciding whether to delegate to the Agent tool, whether to parallelize, or how to sequence the work. Trigger phrases include "implement X", "refactor Y", "research Z across the codebase", "migrate N services", "write a plan for", or any task that looks like it'll take more than a handful of tool calls. Based on the 5 multi-agent coordination patterns — Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, Shared State.
---

# Coordination Patterns

Before diving into a non-trivial task, pause and pick a *shape* for the work. Getting the shape right matters more than the tactics within it — a well-shaped task is easy; a badly-shaped one is painful no matter how carefully you execute.

This skill maps 5 coordination patterns onto the tools you actually have in Claude Code, and gives you a decision procedure for which to use.

## The decision procedure

Ask these questions in order. Stop at the first "yes":

### 1. Is the task small — under ~5 tool calls of direct work?

Just do it. Don't coordinate. The overhead of spawning subagents, writing plans, or setting up verification loops will exceed the time saved. Coordination patterns are for tasks whose complexity justifies their overhead.

### 2. Does the task have a clear acceptance criterion that a verifier can check?

Use **Generator-Verifier**. Produce output, then run the check. Iterate until it passes.

Concrete shapes in Claude Code:
- Write code → run tests / linter / type-checker → fix failures → repeat
- Draft code → spawn a `code-reviewer` Agent for independent review → address feedback
- Draft a plan or doc → re-read it from scratch as if a stranger wrote it → revise
- Generate SQL → run `EXPLAIN` and sample queries → refine

**Why this helps:** you catch problems while context is fresh. The verifier doesn't need to be smarter than the generator — the value is a second pass guided by explicit criteria.

**Avoid when:** you can't articulate what "good" looks like. Without explicit criteria the loop either accepts everything or rewrites forever. If the only criterion is "I'll know it when I see it," skip this pattern and ask the user to clarify what "done" means.

### 3. Does the task decompose into 2–6 bounded subtasks that don't need to share intermediate findings?

Use **Orchestrator-Subagent** — the default pattern. You are the orchestrator. Spawn Agent calls for each bounded piece, synthesize results yourself.

Concrete shapes in Claude Code:
- Broad codebase exploration → `Explore` subagent (keeps raw grep/read output out of your context)
- Independent research questions → parallel `general-purpose` subagents
- Architecture design → `Plan` subagent
- Independent review → `code-reviewer` subagent

**Well-shaped subagent tasks:**
- **Self-contained prompt**: brief the agent like a colleague who just walked in — it can't see your conversation
- **Bounded deliverable**: returns a specific artifact, not an open-ended investigation
- **Independent**: doesn't need to react to what another subagent found mid-flight

**Why this helps:** subagents use fresh context windows, so the raw tool output (file contents, search results) stays out of yours. Each returns a tight summary.

**Avoid when:** one subagent needs another's findings partway through. The orchestrator becomes a message-relay bottleneck — just do the work yourself.

### 4. Do you have N truly independent long-running items — e.g., migrate 5 services, refactor 10 files the same way?

Use **Agent Teams**. Same mechanism as Orchestrator-Subagent, but each Agent does *persistent, multi-step* work on one item end-to-end rather than a one-shot query.

Concrete shapes:
- Parallel migrations: one Agent per service, each doing discover → migrate → test
- Batch refactors: one Agent per file/module when the refactor pattern is uniform
- Parallel bug fixes across genuinely independent components

**Why this helps:** each worker accumulates deep context on its slice. Three agents with 30 focused turns each will outperform one agent juggling 90 turns of mixed context.

**Avoid when:** the items touch shared files (merge conflicts), or one worker's discovery should change how the others act (duplicated or wasted work). When in doubt, serialize — it's almost always better to do N things sequentially than to fight coordination problems.

### 5. Do the agents need to react to each other's findings in real time?

In Claude Code, this is a signal to **NOT delegate** — do it yourself.

The **Shared State** pattern (agents reading/writing a common store) and **Message Bus** pattern (pub/sub routing) assume infrastructure that a single Claude Code session doesn't have. If you find yourself wanting either, what you actually want is usually:

- **Serial execution by you**, using a scratch file or TaskCreate to keep your own notes
- **Plan-first**, then execute deterministically — write out the approach, then follow it

If the investigation really is collaborative and compounding (one finding reshapes the next), a single agent doing it all beats any attempt to split it across isolated subagent contexts.

## Common mistakes

**Spawning subagents for trivial tasks.** Every Agent call has cold-start cost — it re-derives context from scratch. If the task is a 1–2 tool-call lookup, just do it yourself.

**Spawning subagents that need your conversation context.** Subagents can't see this chat. If the task requires "given what we've been discussing…" you'll spend more tokens briefing them than doing it yourself.

**Parallelizing for its own sake.** Two Agent calls for two tasks isn't automatically faster than handling them yourself, and it multiplies the cold-start cost. Parallelize only when the tasks are truly independent *and* each is substantial enough to earn its overhead.

**Verifier without criteria.** "Review this code" with no checklist produces generic, low-signal feedback. Either hand the verifier explicit criteria (tests, rubric, style guide, user requirements) or skip the pattern.

**Plan → execute when the plan would be obvious.** If you already know the three steps, just do them. Plans earn their keep when the approach has real branches or tradeoffs to resolve before starting — not as ceremony.

**Writing a TaskCreate list for a 3-step task.** Task tracking is overhead; use it when there's real risk of losing track, not by default.

## Quick reference

| Signal | Pattern | Claude Code tool |
|---|---|---|
| Small task, under ~5 tool calls | Just do it | Direct tools |
| Quality-critical, testable output | Generator-Verifier | Write + test/lint; or Agent `code-reviewer` |
| 2–6 bounded independent subtasks | Orchestrator-Subagent | Agent (single or parallel) |
| N uniform long-running items | Agent Teams | Agent (persistent, one per item) |
| Findings must compound in real time | Don't delegate | Do it yourself; maybe a plan |
| Ambiguous approach, real tradeoffs | Plan-first | `Plan` subagent or ExitPlanMode |

## Default stance

When unsure, **start with Orchestrator-Subagent**. It's the most general pattern, uses tools you already have, and produces clean boundaries. Evolve only when you hit a specific failure mode:

- Subagents duplicating work → don't delegate; do it yourself
- Verifier missing real issues → sharpen the criteria, or drop the loop
- Subagent returning vague summaries → the prompt wasn't self-contained enough
- Parallel calls slower than sequential → the tasks weren't actually independent

Pattern choice is a hypothesis, not a commitment. If the shape isn't working after one or two rounds, reshape.
