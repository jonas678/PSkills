# Output Quality Rules

Use this reference when the planning work needs to produce outputs that are not only structured, but also concrete enough for downstream product, design, engineering, QA, or PM work.

This reference exists to prevent a common failure mode:
- outputs look organized, but are still too vague to use
- recommendations are not clearly labeled
- assumptions and open questions are buried
- frontend, backend, or impact sections read like placeholders rather than actionable planning material

---

## 1. When to use this reference

Use this reference when:
- the user wants implementation-usable planning outputs
- the user asks for page-level, data-level, API-level, or technical detail
- the user wants team-ready document packages
- the user wants handoff materials
- a PRD has already been drafted and now needs expansion depth

---

## 2. Core principle

A good output is not only well-structured. It must also be:
- specific
- usable
- explicit about uncertainty
- explicit about recommendations
- explicit about what changed vs what remains open

Prefer material that a downstream role can actually work from.

---

## 3. General output quality rules

Across outputs:
- prefer tables and bullets over vague narrative when clarity improves
- distinguish confirmed facts, assumptions, open questions, and recommendations
- do not present inferred structure as if it were user-confirmed truth
- avoid generic statements like “the system should be user-friendly” unless they are translated into concrete product expectations
- make scope boundaries and non-goals visible
- make acceptance framing visible when the output is intended for execution handoff

---

## 4. PRD output quality rules

A strong PRD should usually make explicit:
- background / problem
- goals and non-goals
- target users / roles
- core scenarios
- scope
- assumptions
- risks and dependencies
- acceptance framing

For change work, also include:
- current state
- target state
- unchanged scope
- compatibility / migration considerations

---

## 5. Frontend detail standard

When expanding frontend or page requirements, try to include:
- page or interaction name
- entry point
- target user role
- page goal
- layout regions
- major controls and components
- primary user flow
- validation rules
- empty / loading / error / success / edge states
- project-specific special states when relevant
- edge cases and exception handling

The page detail should be sufficient for:
- design exploration
- frontend implementation planning
- QA scenario identification

---

## 6. Backend detail standard

When expanding backend or service planning, try to include:
- endpoint path and method
- endpoint purpose
- caller
- request parameters
- response fields
- validation rules
- permission checks
- business rules
- error handling
- idempotency / retry / concurrency behavior when relevant
- logging / monitoring / audit expectations
- upstream and downstream dependencies
- synchronous vs asynchronous steps when relevant

If details are not user-confirmed, mark them as:
- `Recommendation`
- `Assumption`
- `To be confirmed`

---

## 7. Impact analysis standard

Always identify what is:
- added
- modified
- removed
- refactored

Prefer a concrete table such as:

| Module | Type | Action | Description |
|---|---|---|---|

Also try to distinguish:
- impacted modules
- unaffected modules
- rollout-sensitive modules
- migration-sensitive modules

---

## 8. PM deliverable expansion quality rules

When the user is acting as PM and team-ready output is clearly needed, avoid stopping at a single PRD.

Consider expanding into:
- PRD main document
- page requirements
- data / API appendix
- technical appendix
- open questions tracker
- acceptance checklist
- review materials
- role briefs
- prompt materials
- PM orchestration materials
- readiness or final audit docs when appropriate

The package should separate:
- confirmed decisions
- assumptions
- recommendations
- still-open items

---

## 9. Common low-quality output patterns

Watch for these signs of weak output:
- high-level wording with no actionable structure
- assumptions mixed into confirmed facts
- open questions hidden instead of surfaced
- page descriptions that do not describe flows or states
- backend sections that name capabilities but not behaviors
- impact analysis with no changed-vs-unchanged distinction
- handoff docs that do not tell downstream roles what not to redefine

---

## 10. Suggested companion references

After using this reference, commonly useful next references are:
- `prd-template.md`
- `change-prd-template.md`
- `api-spec-template.md`
- `doc-package-sequencing-template.md`
- `final-doc-review-checklist.md`
