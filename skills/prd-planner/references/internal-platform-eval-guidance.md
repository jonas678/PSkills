# Internal Platform / Eval Guidance

Use this reference when the product is an internal platform, QA tool, developer tool, evaluation system, or test orchestration product.

This guidance helps prevent a common failure mode:
- the product is described as if it were a normal end-user app
- the operator, execution model, evaluator model, and reusable assets remain vague
- black-box vs semi-white-box vs white-box scope stays ambiguous

---

## 1. When to use this reference

Use this reference when the request involves:
- internal tools
- platform workflows
- QA or test systems
- evaluation systems
- developer productivity tools
- systems that configure, execute, compare, or analyze targets

This reference is scenario-specific guidance, not the main definition of the overall skill.

---

## 2. Core principle

For internal platform and eval-style products, clarify the operating model before expanding the PRD.

This usually means making explicit:
- who operates the system
- what gets configured once
- what gets reused
- what gets executed per run
- what gets generated per execution
- how evaluation works
- how results are analyzed
- what level of internal visibility is in or out of scope

---

## 3. Product framing questions

Before going deeper, clarify:
- Is this an internal tool, platform capability, or productized system?
- Who is the real operator?
- Who consumes the outputs?
- Is the primary goal regression testing, comparison, observability, debugging, release gating, or something else?
- Is the tool mainly about setup, execution, evaluation, analysis, or all of these?

Without this framing, downstream docs often drift.

---

## 4. Core object model

Try to clarify the relationships among:
- target system or target agent
- reusable test suite
- test case
- test run
- execution result
- judge or evaluator
- optional traces or step-level records

Important distinctions often include:
- reusable asset vs execution record
- configuration object vs output object
- target under test vs evaluator of the result

---

## 5. Execution model

Explicitly define:
- what gets configured once
- what is reused across targets or runs
- what gets selected per run
- what gets created per execution
- what is tracked before execution completes
- what becomes the final execution result

This is critical for avoiding confusion between:
- scheduled work
- execution tracking records
- final outcome records

---

## 6. Evaluation model

Clarify:
- whether rule-based assertions exist
- whether semantic judge / evaluator exists
- how final pass / fail is determined
- whether evaluator failures count as system failures or content failures
- what failure categories are needed

If multiple layers of evaluation exist, make the merge logic explicit.

---

## 7. Analysis model

Clarify which result views are required:
- target-centric view
- case-centric view
- run summary view
- failure drill-down view
- trend or version comparison if relevant

This helps stabilize the page model and data model early.

---

## 8. Scope boundary model

If the system evaluates AI agents or workflows, explicitly distinguish:
- black-box final-output evaluation
- semi-white-box key-step evaluation
- white-box workflow / path / trace validation

Do not let the product stay ambiguous here.

This distinction can materially change:
- object model
- data model
- UI structure
- execution pipeline
- future extension strategy

---

## 9. Common failure modes

Watch for these mistakes:
- binding reusable test assets to one target when they should be independent
- confusing execution records with final results
- confusing evaluator configuration with target configuration
- leaving black-box vs white-box scope undefined
- describing outputs only as scores without enough drill-down semantics
- defining pages before the underlying operational model is stable

---

## 10. Suggested next references

After using this reference, commonly useful next references are:
- `prd-template.md`
- `doc-package-sequencing-template.md`
- `multi-agent-delivery-guidance.md` if downstream execution materials are needed
- `final-doc-review-checklist.md` if a large package is being finalized
