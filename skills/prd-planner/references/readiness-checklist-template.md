# Readiness Checklist Template

Use this template when the planning process needs explicit stage gates: clear criteria for when a design, engineering plan, QA strategy, or overall document package is ready to move into the next stage.

This template helps answer questions such as:
- when is design ready for frontend implementation?
- when is engineering planning ready to move into execution?
- when is QA ready to move from strategy into detailed coverage or validation?
- when is the overall planning package sufficient to stop writing docs and start building?

Without readiness criteria, teams often fall into one of two traps:
- they start too early and create rework
- they keep documenting too long and delay execution

---

## 1. Core principle

A stage is “ready” not when every detail is perfect, but when:
- the next role can proceed without repeatedly asking for the same missing basics
- major boundaries and assumptions are explicit enough
- unresolved items are visible and controlled
- remaining uncertainty does not block the next stage’s main objective

Readiness is about **sufficiency for transition**, not total completeness.

---

## 2. Recommended readiness levels

For most product planning workflows, think in four levels:

### Level 1: Discovery-ready
The idea is clear enough to turn into a structured PRD effort.

### Level 2: Design / planning-ready
The product definition is clear enough that design and engineering planning can begin.

### Level 3: Execution-ready
The role-specific outputs are clear enough that implementation or test preparation can proceed.

### Level 4: Validation-ready
The implementation or plan is stable enough that QA, acceptance review, or release preparation can be performed meaningfully.

---

## 3. Universal readiness checklist

Use this lightweight version when the project is small or when you need one high-level gate only.

```text
## Stage
[Name of the stage]

## Ready when
- [criterion 1]
- [criterion 2]
- [criterion 3]

## Still allowed to be unresolved
- [acceptable unresolved item 1]
- [acceptable unresolved item 2]

## Not ready if
- [blocking problem 1]
- [blocking problem 2]
```

---

## 4. PRD-ready checklist

Use this when deciding whether discovery is sufficiently stable to generate the main PRD.

### Ready when
- product goal is clear
- target users / roles are identified
- core scenarios are identified
- scope and non-goals are at least directionally clear
- major structural decisions are stable enough to write against
- biggest unknowns are listed explicitly as open questions

### Still allowed to be unresolved
- lower-priority feature details
- future-phase enhancements
- secondary operational or reporting details

### Not ready if
- core user is still unclear
- core object model is still changing wildly
- scope boundary is still highly unstable
- user intent is still too ambiguous to write coherent requirements

---

## 5. Design-ready checklist

Use this when deciding whether the project is ready to hand over to Designer or whether a design round is sufficiently stable for frontend implementation planning.

### 5.1 Ready for design exploration
Use this gate before Designer round 1.

#### Ready when
- main PRD is coherent enough
- key user flows are identified
- core objects and relationships are understandable
- P0 scope is known
- key page or workflow list exists
- major business constraints are explicit

#### Still allowed to be unresolved
- detailed visual style
- final copywriting
- lower-priority page interactions

#### Not ready if
- PM cannot explain the main user flow clearly
- page list is still unstable
- core object relationships are still changing

### 5.2 Ready for frontend handoff after design
Use this gate after design output exists.

#### Ready when
- page structure is stable enough for implementation
- key interactions are defined
- major states are covered (empty/loading/error/validation/no-data and project-specific result states if relevant)
- PM-owned open decisions are clearly listed
- design output is no longer changing at the structural level every review

#### Still allowed to be unresolved
- minor visual polish
- low-priority microcopy refinements
- non-blocking secondary states

#### Not ready if
- frontend still cannot tell which structure is final enough to implement
- critical states are missing
- key interaction rules are still ambiguous

---

## 6. Engineering-ready checklist

Use this when deciding whether engineering planning is ready to move into implementation.

### 6.1 Ready for engineering planning
Use this gate before Engineering round 1.

#### Ready when
- main PRD exists
- core object model is stable enough
- main technical surfaces are visible (frontend / backend / data / integration)
- major product semantics that must not change are explicit
- acceptance framing exists at least in draft form

#### Still allowed to be unresolved
- detailed module names
- exact implementation order
- lower-priority optimization decisions

#### Not ready if
- product semantics are still being redefined every round
- core states or result semantics are not yet explicit
- design and product boundaries are still contradictory

### 6.2 Ready for implementation handoff
Use this gate after engineering planning output exists.

#### Ready when
- module boundaries are clear enough
- implementation order is clear enough
- key dependencies are known
- API / data / status semantics are stable enough to build against
- major risks and PM-owned decisions are explicitly listed

#### Still allowed to be unresolved
- non-blocking optimizations
- future-phase refactors
- lower-priority monitoring details

#### Not ready if
- frontend / backend / data responsibilities are still blurred
- key status semantics are still ambiguous
- dependencies are still hidden or contradictory
- engineering output keeps reopening confirmed product logic

---

## 7. QA-ready checklist

Use this when deciding whether QA should start or deepen validation work.

### 7.1 Ready for QA strategy work
Use this gate before QA round 1.

#### Ready when
- main PRD exists
- acceptance criteria or acceptance framing exists
- key states and failure semantics are visible enough
- major risk areas can be identified

#### Still allowed to be unresolved
- detailed UI design
- final API edge behavior in low-priority areas
- final automation scope

#### Not ready if
- the product still lacks stable acceptance framing
- state semantics are still undefined
- PM cannot explain what “correct behavior” means at a high level

### 7.2 Ready for detailed QA coverage or validation
Use this gate when moving beyond strategy into concrete validation planning.

#### Ready when
- design structure is stable enough for page-level validation
- engineering semantics are stable enough for state, API, and failure validation
- summary vs detail expectations are explicit when relevant
- major open product decisions are separated from testable behavior

#### Still allowed to be unresolved
- minor design polish
- low-priority edge-case copy changes
- future-phase coverage

#### Not ready if
- QA cannot tell which states are final enough to validate
- open PM decisions are mixed into supposed acceptance criteria
- engineering behavior is still described only at a vague level

---

## 8. Multi-agent handoff-ready checklist

Use this when deciding whether the document package is ready to be handed off to downstream agents.

### Ready when
- main PRD is stable enough
- role briefs exist or can be generated from stable material
- reading order for each role is clear
- confirmed product boundaries are explicit
- open questions are visible
- round-1 objectives for downstream roles are controlled and scoped

### Still allowed to be unresolved
- some round-2 decisions
- lower-priority optimization questions
- future-phase planning

### Not ready if
- PM still needs to verbally explain the basics to every role from scratch
- role boundaries are still unclear
- prompts would need to carry too much missing product definition themselves

---

## 9. Execution-ready checklist

Use this when deciding whether the planning package is sufficient to stop expanding docs and start execution tracking.

### Ready when
- downstream roles know what to read and what to do
- major handoffs and dependencies are clear
- PM can split work into execution units or tickets
- unresolved items are few enough to be managed during execution rather than blocking all work
- the next bottleneck is implementation, not document ambiguity

### Still allowed to be unresolved
- some low-priority UX refinements
- some future-phase backlog items
- some implementation-detail optimizations

### Not ready if
- starting execution would immediately create large-scale rework
- multiple roles are blocked on the same unresolved product ambiguity
- ownership remains unclear

---

## 10. Validation-ready checklist

Use this when deciding whether the system or implementation plan is ready for meaningful validation, acceptance review, or release preparation.

### Ready when
- acceptance expectations are explicit enough
- major product and technical semantics are stable enough
- QA can distinguish product ambiguity from implementation defect
- major state, failure, and result behaviors are known
- validation can proceed without PM redefining basics every day

### Still allowed to be unresolved
- minor polish issues
- low-severity edge cases
- future enhancement ideas

### Not ready if
- acceptance criteria are still moving heavily
- state or result behavior is still materially ambiguous
- cross-role expectations are still contradictory

---

## 11. Readiness review output template

Use this when the PM or planner wants to explicitly state whether a stage is ready.

```text
## Stage reviewed
[stage name]

## Ready status
[Ready / Conditionally ready / Not ready]

## What is already sufficient
- [sufficient item 1]
- [sufficient item 2]

## What is still unresolved but acceptable
- [acceptable unresolved item 1]
- [acceptable unresolved item 2]

## What is blocking readiness
- [blocking item 1]
- [blocking item 2]

## Recommendation
[Proceed / Proceed with conditions / Do not proceed yet]
```

---

## 12. Common failure modes

Avoid these mistakes:
- demanding perfection before moving to the next stage
- declaring readiness with major semantics still ambiguous
- mixing acceptable unresolved items with true blockers
- using “ready” to mean “finished” instead of “sufficient to transition”
- moving into execution when the real blocker is still product ambiguity
- delaying execution when only minor polish remains

---

## 13. Quick checklist

Before declaring any stage ready, check:
- [ ] are the main blockers explicit?
- [ ] are acceptable unresolved items separated from blockers?
- [ ] can the next role proceed without repeatedly asking for missing basics?
- [ ] are major boundaries and semantics stable enough?
- [ ] is this truly execution-ready, or only planning-ready?
