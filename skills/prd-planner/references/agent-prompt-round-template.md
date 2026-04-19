# Agent Prompt Round Template

Use this template when a PRD or handoff package is already in place and the next step is to coordinate one or more downstream agents across multiple rounds.

This template is designed to standardize prompt construction for iterative work. It helps avoid these common problems:
- using one generic prompt for every round
- asking round 2 to regenerate round 1
- failing to protect confirmed decisions
- unclear reading order and unclear output contract
- vague PM follow-up prompts such as “continue optimizing”

---

## 1. When to use this template

Use this template when:
- the user is going to send work to Designer, Engineering, QA, or other downstream agents
- the work is expected to require more than one round
- each round should build on prior outputs rather than restart from scratch
- PM feedback, design feedback, or cross-agent feedback must be incorporated explicitly

Do not use this template too early if the product boundary is still highly unclear. First stabilize requirement discovery and the main planning checkpoint.

---

## 2. Core rule

A prompt round template should always answer these questions clearly:
1. Who is the agent in this round?
2. What must the agent read before working?
3. What facts are already confirmed?
4. What is the objective of this round only?
5. What should the agent not repeat or reopen?
6. What output format is expected?

If any of these are missing, later rounds become unstable.

---

## 3. Recommended round model

A strong default round model for multi-agent work is:

### Round 1
Goal:
- read the docs
- understand the system boundary
- produce the first structured output

### Round 2
Goal:
- revise based on PM feedback
- absorb other agent outputs
- refine only the changed or unresolved parts

### Round 3
Goal:
- converge across roles
- resolve remaining conflicts
- turn refined outputs into execution-ready or validation-ready form

### Optional later rounds
Use only when needed for:
- implementation handoff
- release preparation
- acceptance closure
- post-review gap filling

Do not over-engineer the number of rounds unless the project complexity truly requires it.

---

## 4. Round 1 prompt template

```text
You are the [Role] Agent for the [Project Name] project.

## Role
[Describe what this role is responsible for in this round.]

## Required reading order
Read these materials in order before doing the work:
1. [doc 1]
2. [doc 2]
3. [doc 3]

Optional references if needed:
- [optional doc A]
- [optional doc B]

## Confirmed facts
These facts are already considered confirmed and should not be silently changed:
- [fact 1]
- [fact 2]
- [fact 3]

## Round-1 objective
[State exactly what this round should produce.]

## Output requirements
Please output:
1. [output item 1]
2. [output item 2]
3. [output item 3]

## Constraints
- do not redefine confirmed product boundaries
- do not expand scope beyond this round’s objective
- explicitly label assumptions and open questions

## Output format
[Specify markdown / table / sectioned doc / checklist / etc.]

Before starting, briefly state what you read, what you understand, and how you plan to structure the output.
```

---

## 5. Round 2 prompt template

```text
You are continuing as the [Role] Agent for the [Project Name] project.

## Base documents
Continue using these base materials:
1. [doc 1]
2. [doc 2]
3. [doc 3]

## Round-1 output summary
Use your prior output as existing input:
[insert summary of round-1 result]

## PM feedback
Please incorporate the following PM review feedback:
[insert PM feedback]

## Cross-agent inputs
Also consider these additional inputs if relevant:
- [designer / engineering / QA / other role input]

## Confirmed items
These items are already confirmed and should not be reopened:
- [confirmed item 1]
- [confirmed item 2]

## Round-2 objective
[State what this revision round must accomplish.]

## Do not repeat
- do not restate unchanged sections in full
- do not regenerate the entire round-1 output
- do not reopen confirmed items unless explicitly requested

## Output requirements
Please output only the changed, added, or unresolved parts, including:
1. [changed part 1]
2. [changed part 2]
3. [remaining decision items]

## Output format
[delta-oriented markdown / revised table / issue list / annotated update]

Before starting, briefly summarize your understanding of the round-1 result, the new feedback, and what you will revise in this round.
```

---

## 6. Round 3 prompt template

```text
You are continuing as the [Role] Agent for the [Project Name] project.

## Base materials
Use the existing document package and previous round outputs.

## Round-2 result summary
[insert summary of round-2 output]

## Remaining issues
These are the remaining conflicts, gaps, or unresolved items:
- [item 1]
- [item 2]

## Cross-role convergence inputs
Please consider:
- [input from another role]
- [PM decision update]
- [technical / design / QA clarification]

## Confirmed items
Do not reopen these:
- [confirmed item 1]
- [confirmed item 2]

## Round-3 objective
[Example: finalize execution-ready structure, close major ambiguities, and prepare for implementation handoff.]

## Do not repeat
- do not re-output prior rounds in full
- do not revisit closed design or product choices
- do not expand scope into unrelated future enhancements

## Output requirements
Please provide:
1. final unresolved-items resolution proposal
2. cross-role alignment notes
3. implementation-ready / validation-ready deltas
4. remaining PM decision list if any

## Output format
[convergence memo / final delta list / readiness checklist / implementation handoff note]
```

---

## 7. Role-specific adjustments

### Designer rounds usually emphasize
- information architecture
- page structure
- interaction design
- state coverage
- PM-owned design decisions that still need confirmation

### Engineering rounds usually emphasize
- module boundaries
- implementation order
- interface alignment
- database and backend dependencies
- risks and blocked items

### QA rounds usually emphasize
- test strategy
- state semantics
- abnormal scenarios
- acceptance coverage
- validation gaps caused by design or engineering changes

---

## 8. Round objective guidance by stage

### If the project is still early
Round 1 should emphasize structure, not final detail.

### If PM has already reviewed outputs
Round 2 should emphasize deltas, not rewrites.

### If the project is nearing execution
Round 3 should emphasize readiness, dependencies, and unresolved blockers.

### If the project is nearing release
Later rounds should emphasize acceptance, regression, rollback, and operational readiness rather than fresh design exploration.

---

## 9. Output style guidance

### Good round prompts are scoped
Bad:
- continue improving everything
- make it better
- refine the whole document

Better:
- revise only the run overview page layout and state coverage based on PM comments
- refine only the backend/database dependency section using the updated designer structure
- add only the missing QA coverage for summary/detail mismatch and judge_failed behavior

### Good round prompts preserve prior work
Always tell the agent what should be preserved.

### Good round prompts are delta-aware
A later round should almost never ask for a full fresh document unless that is explicitly desired.

---

## 10. Anti-patterns

Avoid these mistakes:
- sending the same prompt in round 1 and round 2
- omitting the round-1 summary in later rounds
- omitting confirmed items
- omitting “do not repeat” instructions
- mixing revision requests with open decisions without distinguishing them
- asking agents to silently make PM-owned product decisions
- letting later rounds reopen already stabilized semantics

---

## 11. Quick checklist

Before sending a round prompt, check:
- [ ] role is explicit
- [ ] required reading order is explicit
- [ ] confirmed facts/items are explicit
- [ ] round-specific objective is explicit
- [ ] do-not-repeat guidance is explicit
- [ ] output format is explicit
- [ ] prior-round summary is included when relevant
- [ ] cross-agent inputs are included when relevant
