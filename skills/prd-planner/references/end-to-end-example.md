# End-to-End Example: Using `prd-planner` from Vague Idea to Multi-Agent Handoff

This example shows how `prd-planner` can be used end to end, not only to write a PRD, but to guide a PM from vague idea → clarified requirements → document package → multi-agent handoff → review rounds → execution transition.

---

## 1. Example scenario

A PM says:

> I want to build an Agent testing platform. Users configure an agent API template, upload test cases, run cases in batch, and use a judge agent to evaluate responses.

At this point, the idea is still incomplete.

---

## 2. Stage 1: Requirement intake and clarification

### What `prd-planner` should do
Do **not** jump straight into a full PRD.

Instead:
- identify this as a 0-to-1 system
- recognize that it may also fit the internal platform / test-eval scenario guidance
- clarify target users, core objects, reuse model, and evaluation boundary
- ask high-impact questions in small batches

### Typical clarification questions
Examples:
- Is a test suite tied to one agent, or reusable across multiple agents?
- Is MVP black-box only, or does it include white-box / semi-white-box workflow evaluation?
- Do test cases support multi-turn conversations?
- Does the result model need only pass/fail, or also score and failure category?
- Who are the main users: engineering, QA, prompt team, or product?

### Outcome of this stage
A structured checkpoint instead of a full PRD.

---

## 3. Stage 2: Requirement checkpoint

After one or more clarification rounds, `prd-planner` should summarize:
- project goal
- target users
- core scenarios
- core objects
- MVP scope
- non-goals
- assumptions
- open questions
- recommended direction

### Example checkpoint conclusions
- test suites are reusable across multiple agents
- MVP is black-box only
- test cases support multi-turn conversation structure
- case results are pre-created as execution records when a run is created
- judge failures should be distinguished as a separate category

This checkpoint should be confirmed before the main PRD is written.

---

## 4. Stage 3: Main PRD generation

Once the checkpoint is stable, `prd-planner` should produce the main PRD.

Typical output:
- product background
- goals
- users and roles
- core scenarios
- core object model
- functional scope
- MVP vs later phases
- risks and constraints
- acceptance direction

At this stage, the product is defined in a stable enough way for downstream documentation.

---

## 5. Stage 4: Supporting document package

After the main PRD is stable, `prd-planner` can expand into supporting docs such as:
- page requirements
- data model and API
- backend / execution technical design
- open questions list
- acceptance checklist
- task breakdown or review materials

### Example outputs for this scenario
- Agent config page requirements
- test suite and case editor definitions
- run result and comparison page definitions
- run / case_result data model
- execution engine behavior and status semantics
- acceptance checklist for MVP black-box evaluation scope

This stage turns the PRD into role-usable planning materials.

---

## 6. Stage 5: Multi-agent handoff package

If the PM wants to use downstream AI agents or specialist roles, `prd-planner` can continue into handoff preparation.

Typical outputs:
- Designer brief
- Engineering brief
- QA brief
- agent prompt document
- PM review playbook

### Example role split
- Designer Agent
- Engineering Agent
  - Frontend / Backend / Database as sub-roles
- QA Agent
- PM as orchestrator

This stage is about making the work executable by downstream roles, not just well documented.

---

## 7. Stage 6: Round-1 prompts

Next, `prd-planner` can generate or recommend round-1 prompts.

### Designer round 1 usually focuses on
- information architecture
- low-fidelity structure
- page layout and key interactions
- state coverage
- PM-owned design decision points

### Engineering round 1 usually focuses on
- frontend / backend / database split
- implementation sequencing
- API / data / execution dependencies
- risk list
- PM confirmation points

### QA round 1 usually focuses on
- test strategy
- state semantics
- abnormal scenario framing
- result trustworthiness risks
- acceptance framing

This prevents round 1 from going too deep too early.

---

## 8. Stage 7: PM reviews round-1 outputs

After the first round comes back, `prd-planner` can help the PM review the outputs.

### PM should organize the review into
- confirmed items
- revision items
- open decision items

### Example review focus
#### Designer
- Is the page structure clear?
- Are complex pages usable?
- Are key states covered?

#### Engineering
- Are module boundaries clear?
- Are product semantics preserved?
- Are dependencies and risks exposed?

#### QA
- Are state semantics covered?
- Is result trustworthiness considered?
- Are summary/detail mismatches and failure categories accounted for?

---

## 9. Stage 8: Round-2 packaging

Now `prd-planner` can help package first-round outputs into second-round inputs.

Round-2 prompts should typically include:
- round-1 output summary
- PM feedback
- confirmed items not to reopen
- current revision target
- what not to repeat
- delta-oriented output format

This is what turns vague feedback such as “improve this” into executable second-round work.

---

## 10. Stage 9: Cross-role coordination

At this point, `prd-planner` can also help define role interfaces such as:
- Designer → Engineering
- Designer → QA
- Engineering → QA
- QA → Engineering
- QA → PM

### Example
Designer → Engineering may pass:
- page structure
- interaction rules
- state coverage
- PM-owned unresolved design decisions

Engineering → QA may pass:
- state semantics
- failure categories
- API/data behavior
- implementation constraints

This reduces manual translation work by PM.

---

## 11. Stage 10: Readiness gating

Before execution begins, `prd-planner` can help judge whether the project is ready to move forward.

Typical gates:
- PRD-ready
- design-ready
- engineering-ready
- QA-ready
- multi-agent handoff-ready
- execution-ready
- validation-ready

### Example execution-ready conditions
- core object relationships are stable
- major states and result semantics are explicit
- supporting docs are sufficient
- role briefs exist
- prompt rounds exist if needed
- unresolved items are visible and manageable
- the main bottleneck has shifted from ambiguity to implementation

At this point, the recommendation should be to move into execution rather than continue expanding docs.

---

## 12. Practical summary

This example shows that `prd-planner` can support a full path like this:

1. vague idea
2. clarification rounds
3. requirement checkpoint
4. main PRD
5. supporting documents
6. role briefs
7. prompt rounds
8. PM review
9. PM feedback packaging
10. cross-role coordination
11. readiness gating
12. execution transition

The skill is therefore best used as a **planning and handoff system**, not only a PRD-writing helper.
