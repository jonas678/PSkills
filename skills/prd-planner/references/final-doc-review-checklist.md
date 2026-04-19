# Final Document Review Checklist

Use this reference at the **end** of a `prd-planner` workflow, after the main docs, supporting docs, briefs, and prompt materials already exist.

Its purpose is to answer:
- is the generated document package internally consistent?
- is it complete enough for the next role to start?
- what should be fixed immediately before handoff?

This is a final audit-and-repair step, not a replacement for discovery, PRD writing, or readiness gating.

---

## 1. When to use this checklist

Use it when:
- the user asks whether the docs are “enough”
- the package is about to be sent to design / engineering / QA
- briefs, prompts, Jira docs, or review docs have already been generated
- multiple docs were produced over many rounds and may have drifted
- the PM wants a final cleanup pass before execution

---

## 2. Core audit principle

Do not only ask “does each document look reasonable in isolation?”

Also ask:
- do the documents still agree with each other?
- do later docs reflect newer confirmed decisions?
- do prompts and handoff docs still match the final frozen scope?

The final audit is about **package integrity**, not only document quality.

---

## 3. Audit dimensions

## 3.1 Completeness
Check whether the intended package is actually present.

Examples:
- PRD exists
- supporting page / flow docs exist when needed
- data / API doc exists when needed
- technical design exists when needed
- open questions / acceptance / review materials exist when needed
- role briefs exist when multi-agent handoff is intended
- prompt docs and PM playbook exist when PM-led orchestration is intended

## 3.2 Terminology consistency
Check whether the same object or concept is named consistently across docs.

Examples:
- Judge Agent vs Judge Config
- Run vs Execution
- Case Result vs Result Item
- Target Agent vs Tested Agent

If different names are intentional, make the mapping explicit.

## 3.3 Semantic consistency
Check whether key meanings remain stable across docs.

Examples:
- status definitions
- pass/fail merge logic
- cancellation semantics
- what is pre-created vs what is actual execution result
- what is MVP vs later phase

## 3.4 Scope consistency
Check whether all docs still reflect the same product boundary.

Examples:
- a PRD says black-box only, but prompt docs ask for white-box design
- acceptance checklist includes features that the PRD excluded
- engineering brief assumes implementation scope larger than frozen scope

## 3.5 Cross-reference and reading-order consistency
Check whether downstream docs point readers to the right upstream docs.

Examples:
- briefs reference the current readiness / freeze doc
- prompts reference the latest role briefs
- PM playbook references the latest prompt package

## 3.6 Role-handoff consistency
Check whether Designer / Engineering / QA / PM docs agree on:
- what is already confirmed
- what is still open
- what each role should start with
- what each role should not redefine

## 3.7 Readiness conclusion consistency
Check whether the package’s claimed readiness matches the actual state.

Examples:
- if the package says engineering can start, is the API / state model stable enough?
- if QA can start, are acceptance and status semantics explicit enough?

---

## 4. Repair rules

### Repair immediately when:
- the issue is wording-only
- the intended meaning is already confirmed elsewhere
- a doc forgot to reference the latest readiness or freeze rule
- a prompt or brief is clearly lagging behind confirmed scope
- a naming inconsistency can be fixed without changing product meaning

### Escalate instead of silently fixing when:
- scope would change
- acceptance meaning would change
- execution semantics are unclear
- ownership / role split would change
- two docs conflict and the latest confirmed decision is not obvious

In these cases, explicitly surface:
- the conflict
- the likely impact
- the recommended resolution
- what needs user confirmation

---

## 5. Suggested output format for the final audit

```text
## Audit scope
- [which docs were checked]

## What is already consistent
- [item]
- [item]

## Issues found
1. [issue]
   - affected docs:
   - severity:
   - recommendation:

## Repaired immediately
- [repair made]
- [repair made]

## Still requires confirmation
- [item]

## Final readiness conclusion
- design-ready: yes/no
- engineering-ready: yes/no
- QA-ready: yes/no
- PM handoff-ready: yes/no
```

---

## 6. Strong recommendation

Do not end a large `prd-planner` workflow with only:
- “here are your docs”

Prefer ending with:
- a final audit summary
- explicit repairs
- a readiness conclusion
- a short recommendation on what should happen next
