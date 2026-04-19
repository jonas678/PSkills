# Prompt Round Guidance

Use this reference when the user wants downstream AI-agent work to happen in structured rounds rather than one generic prompt.

This guidance helps prevent a common failure mode:
- every downstream round behaves like a fresh start
- already confirmed decisions get reopened
- second-round prompts cause full rewrites instead of targeted revision

---

## 1. When to use this reference

Use this reference when:
- the user wants role-specific prompts
- the user wants multi-round collaboration
- PM will review outputs and send structured follow-up prompts
- multiple roles must coordinate over time

This is especially useful when:
- Designer, Engineering, and QA are all involved
- first-round outputs need PM review before refinement
- later rounds should incorporate outputs from other roles

---

## 2. Core principle

Do not rely on one generic prompt when the work clearly needs multiple rounds.

Different rounds should have different jobs:
- round 1 creates first structured output
- round 2 revises using PM feedback
- round 3 or later converges using cross-role outputs, implementation realities, or validation needs

Round design should reduce repetition and prevent decision drift.

---

## 3. Recommended round model

A strong default round model is:

### Round 1
- read the docs
- understand the current scope
- produce first structured output
- surface missing or unclear decisions

### Round 2
- revise using PM feedback
- preserve confirmed decisions
- output only deltas and updated sections
- surface remaining blocked items

### Round 3+
- converge using outputs from other roles
- refine implementation / validation / integration details
- reduce ambiguity before build or review
- prepare final delivery or validation materials

Not every project needs all rounds, but most multi-role efforts need at least round 1 and round 2.

---

## 4. Round-1 guidance

Round 1 should usually include:
- role definition
- required reading list
- confirmed product facts
- current objective
- explicit output format
- constraints on changing product scope or semantics

Round 1 should favor:
- structure over polish
- coverage over over-detailing
- early risk surfacing over pretending everything is already clear

Round 1 should not usually ask for:
- final polish
- full production implementation
- full validation detail before the design or engineering structure exists

---

## 5. Round-2 guidance

Round 2 should explicitly include:
- summary of round-1 outputs
- PM feedback
- confirmed items that must not be reopened
- specific revision targets
- instructions on what not to repeat
- output requirements focused on deltas, not full rewrites

A good round-2 prompt should make it easy for the downstream role to answer:
- what changed
- what remains unchanged
- what is still blocked
- what depends on another role

---

## 6. Round-3 and later guidance

Later rounds are often for:
- absorbing outputs from other roles
- implementation-focused refinement
- validation-focused refinement
- closing remaining ambiguities
- preparing release or handoff readiness

At this stage, prompts should become narrower, not broader.

The later the round, the more the prompt should focus on:
- deltas
- integration
- unresolved dependencies
- concrete next actions

---

## 7. PM playbook expectations

When PM is orchestrating prompt rounds, provide explicit support for:
- recommended send order
- what to expect in round 1 for each role
- what to review role by role
- how to package feedback into round 2
- how to stop agents from repeating already settled sections

PM should avoid vague follow-up instructions like:
- “optimize this more”
- “make it better”
- “continue”

Instead, PM feedback should be structured as:
- confirmed items
- revision items
- open decision items

---

## 8. Prompt design anti-patterns

Watch for these mistakes:
- one generic prompt reused for all rounds
- no reading order
- no explicit output contract
- no distinction between confirmed vs open items
- no instruction about what not to repeat
- letting each round silently redefine the system
- asking for implementation depth too early

---

## 9. Suggested prompt skeleton

A strong prompt by round often includes:
- role identity
- required reading list
- confirmed facts
- round goal
- output format
- do-not-reopen items
- delta-only instruction when applicable
- questions that still require confirmation

---

## 10. Suggested next references

After using this reference, commonly useful next references are:
- `agent-prompt-round-template.md`
- `pm-feedback-template.md`
- `multi-agent-delivery-guidance.md`
- `readiness-checklist-template.md`
