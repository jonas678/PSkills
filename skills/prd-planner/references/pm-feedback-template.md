# PM Feedback Packaging Template

Use this template after round 1 outputs have been produced by Designer, Engineering, QA, or other downstream agents, and the PM needs to turn review comments into structured inputs for round 2.

This template is designed to prevent vague feedback such as “optimize this more” or “revise based on the discussion.”

Instead, it helps the PM produce feedback that is:
- actionable
- role-specific
- delta-oriented
- safe for multi-agent collaboration

---

## 1. When to use this template

Use this template when:
- a first-round output already exists
- the PM has reviewed it
- the next step is revision, refinement, or convergence
- the next prompt should modify the previous result rather than regenerate from scratch

Do not use this template before the first meaningful output exists.

---

## 2. Core rule

A good PM feedback package should always distinguish among:
1. what is already confirmed
2. what must be revised
3. what is still open for decision

If these three are mixed together, the next-round agent usually reopens closed decisions or rewrites too much.

---

## 3. Minimal feedback package structure

For most projects, the PM’s round-2 input package should contain these sections:

1. round-1 output summary
2. confirmed items
3. revision items
4. open decision items
5. round-2 objective
6. what not to repeat
7. output format requirement

This minimal structure is usually enough to drive a good round-2 prompt.

---

## 4. Universal PM feedback template

```text
## Round-1 Output Summary
[Summarize what the agent already produced. Keep this concise but concrete.]

## Confirmed Items
[These items are considered decided. The next round should not reopen them.]

## Revision Items
[These items must be revised in round 2. Be concrete about what is wrong and what should improve.]

## Open Decision Items
[These items are still not fully decided. The agent may help narrow them, but should not silently decide them as final unless asked.]

## Round-2 Objective
[State what this round should accomplish.]

## Do Not Repeat
[List what the agent should not fully restate or regenerate.]

## Output Requirement
[Specify whether the next round should output only deltas, a revised section list, a decision table, etc.]
```

---

## 5. Stronger PM feedback version

Use this version when the project is more complex or multiple agents are involved.

```text
## Role
[Designer / Engineering / QA / Other]

## Round-1 Output Summary
- [key point 1]
- [key point 2]
- [key point 3]

## What Is Good and Should Be Preserved
- [preserve item 1]
- [preserve item 2]

## Confirmed Items
- [confirmed item 1]
- [confirmed item 2]

## Revision Items
| Area | Current issue | Expected revision |
|---|---|---|
| | | |

## Open Decision Items
| Topic | Current options | What help is needed |
|---|---|---|
| | | |

## Cross-Agent Inputs to Consider
- [Designer / Engineering / QA output that should influence this round]

## Round-2 Objective
[Write one concise objective sentence.]

## Do Not Repeat
- [do not regenerate full document]
- [do not reopen confirmed item X]
- [do not restate unchanged sections]

## Expected Output Format
[delta list / revised section only / table / annotated update / issue list]
```

---

## 6. Designer-focused PM feedback template

Use this when reviewing design outputs such as information architecture, low-fidelity wireframes, page structure, or key interactions.

```text
## Designer Round-1 Summary
[Summarize what the designer produced.]

## Confirmed Design Directions
- [page structure confirmed]
- [navigation pattern confirmed]
- [comparison layout direction confirmed]

## Design Revisions Needed
| Page / Flow | Current issue | Revision needed |
|---|---|---|
| | | |

## Open Design Decisions
| Topic | Current options | PM wants designer to do what |
|---|---|---|
| | | |

## Round-2 Goal
[Example: refine P0 structure, improve state coverage, and reduce ambiguity for frontend handoff.]

## Do Not Repeat
- do not redraw unchanged pages from scratch
- do not restate fully confirmed information architecture
- do not silently decide PM-owned product decisions

## Expected Output
Only output revised parts, newly added state notes, changed interaction notes, and remaining PM decision points.
```

---

## 7. Engineering-focused PM feedback template

Use this when reviewing engineering planning outputs such as module boundaries, implementation sequencing, API alignment, or risk analysis.

```text
## Engineering Round-1 Summary
[Summarize what engineering produced.]

## Confirmed Technical Directions
- [module split confirmed]
- [execution priority confirmed]
- [data model direction confirmed]

## Revisions Needed
| Area | Current issue | Expected revision |
|---|---|---|
| Frontend | | |
| Backend | | |
| Database | | |

## Open Technical Decisions
| Topic | Current options | What PM wants next |
|---|---|---|
| | | |

## Cross-Agent Inputs
- [designer changes that engineering must absorb]
- [QA risks that engineering should address]

## Round-2 Goal
[Example: refine implementation dependencies and align engineering breakdown with confirmed design structure.]

## Do Not Repeat
- do not rewrite the full round-1 plan
- do not reopen confirmed product semantics
- do not restate unchanged module descriptions

## Expected Output
Only output revised module split, changed dependencies, additional risks, updated implementation order, and remaining PM confirmation items.
```

---

## 8. QA-focused PM feedback template

Use this when reviewing QA outputs such as test strategy, risk framing, state-transition coverage, abnormal-scenario coverage, or acceptance planning.

```text
## QA Round-1 Summary
[Summarize what QA produced.]

## Confirmed QA Directions
- [state-semantics focus confirmed]
- [risk areas confirmed]
- [acceptance framing confirmed]

## Revisions Needed
| Area | Current issue | Expected revision |
|---|---|---|
| Strategy | | |
| State testing | | |
| Error handling | | |
| UI / API coverage | | |

## Open QA Decisions
| Topic | Current options | What PM wants next |
|---|---|---|
| | | |

## Cross-Agent Inputs
- [designer structure changes]
- [engineering interface or status definitions]

## Round-2 Goal
[Example: add page-level and interface-level coverage based on the first design and engineering outputs.]

## Do Not Repeat
- do not rewrite the whole round-1 strategy
- do not restate unchanged principles
- do not over-expand into implementation-specific automation unless asked

## Expected Output
Only output added coverage, revised risk points, updated validation strategy, and remaining PM / engineering clarification items.
```

---

## 9. PM writing rules for feedback packaging

### Good PM feedback is specific
Bad:
- improve this section
- think deeper
- make it more complete

Better:
- keep the overall structure, but split the run result page into summary and drill-down layers
- preserve the current module split, but clarify the dependency between result aggregation and case-result storage
- keep the test strategy framing, but add explicit validation for summary/detail mismatch and judge_failed handling

### Good PM feedback protects confirmed decisions
Explicitly say what is already confirmed.

### Good PM feedback limits scope
Round 2 should not secretly become a brand-new round-1 rewrite.

### Good PM feedback is role-aware
Designer feedback should emphasize usability and structure.
Engineering feedback should emphasize implementation consequences and boundaries.
QA feedback should emphasize risk coverage and validation scope.

---

## 10. Common failure modes

Watch for these mistakes:
- mixing confirmed items with open items
- giving only high-level dissatisfaction with no actionable revision request
- forgetting to include the round-1 output summary
- not telling the agent what not to repeat
- reopening already closed product decisions unintentionally
- asking one role to silently resolve another role’s owner-level decision

---

## 11. Quick checklist

Before sending PM feedback into round 2, check:
- [ ] round-1 output summary included
- [ ] confirmed items listed
- [ ] revision items concrete
- [ ] open decision items separated
- [ ] round-2 objective is single-round scoped
- [ ] “do not repeat” section included
- [ ] output format requirement included
- [ ] cross-agent inputs included when relevant
