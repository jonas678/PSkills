# Cross-Role Interface Template

Use this template when a PRD workflow involves multiple downstream roles or agents and their outputs must feed one another cleanly.

This template is designed to answer a common coordination problem:
- Designer, Engineering, and QA each produce useful outputs
- but the interfaces between those outputs are unclear
- so PM has to manually translate everything each round

The goal of this template is to make role-to-role handoff explicit.

---

## 1. When to use this template

Use this template when:
- multiple roles are working from the same PRD package
- one role’s output materially affects another role’s next step
- PM wants to reduce ambiguity in cross-role collaboration
- later-round prompts depend on inputs produced by other agents

This is especially useful in:
- designer → frontend / engineering handoff
- engineering → QA handoff
- QA → PM / engineering risk feedback loops
- PM-led multi-agent coordination rounds

---

## 2. Core principle

Role outputs should not only say “what I produced.”
They should also say:
- what other roles need from this output
- what has changed that affects other roles
- what is confirmed versus still open
- what downstream roles should not assume

A good cross-role interface reduces translation work by PM.

---

## 3. Default role interface map

For a typical new product build, the most important interfaces are:

1. **PM → Designer**
2. **PM → Engineering**
3. **PM → QA**
4. **Designer → Engineering**
5. **Designer → QA**
6. **Engineering → QA**
7. **QA → Engineering**
8. **QA → PM**
9. **Engineering → PM**
10. **Designer → PM**

Not every project needs all of these in formal written form, but the larger the project, the more useful this becomes.

---

## 4. Interface template structure

For any role-to-role handoff, prefer this structure:

```text
## From
[Role A]

## To
[Role B]

## Purpose of this handoff
[Why this output matters to the receiving role]

## What is being handed over
- [artifact 1]
- [artifact 2]
- [artifact 3]

## Confirmed items
- [confirmed item 1]
- [confirmed item 2]

## Open items
- [open item 1]
- [open item 2]

## What changed since last round
- [change 1]
- [change 2]

## What the receiving role should do with this
- [expected action 1]
- [expected action 2]

## What the receiving role must not assume
- [non-assumption 1]
- [non-assumption 2]
```

This structure is intentionally simple and should be reusable across roles.

---

## 5. Designer → Engineering interface template

Use this when design outputs must be translated into frontend and technical implementation planning.

### Designer should provide
- page structure or layout summary
- key interaction rules
- state coverage notes
- page-level assumptions
- decisions still requiring PM confirmation
- any design changes that affect field visibility, workflow order, or interaction logic

### Engineering needs from Designer
- stable page and flow structure
- key interaction behaviors that affect API or state handling
- state list that frontend must render
- any changed assumptions that may impact data requirements or component structure

### Suggested template

```text
## From
Designer

## To
Engineering

## Purpose of this handoff
Translate approved or draft design structure into implementation planning inputs.

## What is being handed over
- page structure updates
- key interaction behavior notes
- state coverage list
- newly introduced design assumptions

## Confirmed design items
- [confirmed structure item]
- [confirmed interaction item]

## Open design items
- [open PM-owned decision]
- [still unresolved behavior]

## What changed since last round
- [layout changed]
- [interaction changed]
- [state coverage expanded]

## What Engineering should do with this
- update frontend implementation split
- align API/state handling with new interaction design
- identify backend or data dependencies introduced by design updates

## What Engineering must not assume
- open PM-owned design decisions are not final
- visual grouping does not automatically imply backend ownership or data structure
```

---

## 6. Designer → QA interface template

Use this when design output changes how QA should think about page coverage, state coverage, and interaction validation.

### Designer should provide
- page list and flow changes
- state additions
- error / empty / loading / validation behavior expectations
- risky UX transitions or conditional interactions

### QA needs from Designer
- what should be visible to users
- which states and transitions must be validated
- where interaction ambiguity still exists

### Suggested template

```text
## From
Designer

## To
QA

## Purpose of this handoff
Help QA convert design output into page-level and interaction-level validation points.

## What is being handed over
- updated page/flow structure
- UI state definitions
- interaction notes affecting validation

## Confirmed items
- [confirmed page behavior]
- [confirmed state behavior]

## Open items
- [state still pending PM confirmation]
- [interaction still pending engineering feasibility]

## What changed since last round
- [new state added]
- [flow order changed]
- [detail view behavior changed]

## What QA should do with this
- update page-level test coverage
- add state and interaction validation
- flag any remaining ambiguity that blocks test design

## What QA must not assume
- draft design behavior is not final unless marked confirmed
- visual grouping alone does not define backend behavior
```

---

## 7. Engineering → QA interface template

Use this when engineering outputs define status handling, API behavior, failure modes, data behavior, or execution semantics that QA must validate.

### Engineering should provide
- module or interface changes
- state semantics
- error categories
- request/response or data contract changes
- implementation constraints relevant to QA

### QA needs from Engineering
- what is technically implemented or planned
- what system-level failures must be distinguishable
- what API/data/state behavior must be validated

### Suggested template

```text
## From
Engineering

## To
QA

## Purpose of this handoff
Translate implementation structure into QA validation targets.

## What is being handed over
- module / API / data updates
- state and status handling rules
- failure-category handling
- execution or service behavior changes

## Confirmed engineering items
- [confirmed state semantics]
- [confirmed contract or dependency]

## Open items
- [implementation still pending]
- [PM-owned product behavior still unresolved]

## What changed since last round
- [status handling updated]
- [API field changed]
- [failure category clarified]

## What QA should do with this
- update API-level validation
- update state-transition tests
- add abnormal-case coverage where new failure semantics exist

## What QA must not assume
- planned implementation is not equal to already shipped behavior
- unresolved PM behavior should not be treated as final acceptance criteria
```

---

## 8. QA → Engineering interface template

Use this when QA identifies risks, missing validation, ambiguity, or inconsistency that engineering must absorb.

### QA should provide
- concrete risk findings
- missing validations
- summary/detail mismatch concerns
- status ambiguity concerns
- category or contract mismatches

### Engineering needs from QA
- what is risky
- what is currently untestable or ambiguous
- what changes might reduce release risk

### Suggested template

```text
## From
QA

## To
Engineering

## Purpose of this handoff
Feed validation risks and ambiguity findings back into implementation planning.

## What is being handed over
- test risk findings
- missing coverage areas
- status / failure ambiguity issues
- acceptance blockers

## Confirmed QA findings
- [validated risk]
- [confirmed mismatch]

## Open QA concerns
- [possible issue not yet resolved]
- [needs engineering clarification]

## What changed since last round
- [new risk discovered]
- [previous risk narrowed or closed]

## What Engineering should do with this
- clarify implementation behavior
- adjust contracts or state handling if needed
- identify whether this is product, design, or implementation debt

## What Engineering must not assume
- QA concern is not automatically a code bug; it may be a product or design ambiguity
- unresolved acceptance ambiguity should not be silently patched without PM alignment
```

---

## 9. QA → PM interface template

Use this when QA needs PM decisions or needs to expose validation risks that are actually product-level issues.

### QA should provide
- acceptance risk summary
- product ambiguity affecting testability
- unresolved expected behavior
- prioritization implications

### Suggested template

```text
## From
QA

## To
PM

## Purpose of this handoff
Escalate product-level ambiguity or validation risk that requires PM decision.

## What is being handed over
- acceptance risk summary
- ambiguous behavior list
- testability blockers

## Confirmed QA concerns
- [confirmed issue]
- [confirmed mismatch]

## Open decisions needed from PM
- [decision 1]
- [decision 2]

## What PM should do with this
- confirm product behavior
- decide acceptance expectation
- decide whether scope or priority changes are needed

## What PM must not assume
- lack of a test case may indicate unclear product definition, not weak QA effort
```

---

## 10. Engineering → PM interface template

Use this when engineering needs to expose dependency, feasibility, sequencing, or implementation-scope issues back to PM.

### Engineering should provide
- blocked dependencies
- scope consequences
- sequencing implications
- technical tradeoff requiring PM choice

---

## 11. Designer → PM interface template

Use this when design needs PM decisions about structure, priority, workflow choice, or unresolved UX tradeoffs.

### Designer should provide
- design tradeoffs
- PM-owned decisions
- points where business logic and UX structure are coupled

---

## 12. Common cross-role mistakes

Avoid these mistakes:
- passing only a finished artifact with no explanation of what changed
- not distinguishing confirmed items from open items
- assuming another role will infer dependencies automatically
- using visual structure as proof of product semantics
- using engineering constraints as proof of final product behavior without PM confirmation
- using QA concerns as proof of implementation defect when the real issue is product ambiguity

---

## 13. Quick checklist

Before treating a cross-role handoff as complete, check:
- [ ] the sending role is explicit
- [ ] the receiving role is explicit
- [ ] what changed is explicit
- [ ] confirmed vs open items are separated
- [ ] expected downstream action is explicit
- [ ] non-assumptions are explicit
