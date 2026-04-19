# PSkills — The Full-Stack AI Software Studio Plugin

A complete Claude Code plugin suite that turns your terminal into a multi-agent software studio. It provides an end-to-end pipeline from **Product Planning (PM)** to **Design**, **Software Architecture**, **Test-Driven Development (TDD)**, and **QA Verification**.

## Installation

**Claude Code Plugin (recommended)**

From within Claude Code, first add the marketplace:

```text
/plugin marketplace add jonas678/PSkills
```

Then install the plugin:

```text
/plugin install PSkills@PSkills
```

*(Note: Ensure you are running the latest version of Claude Code that supports the `/plugin` slash commands).*

Or install manually (for development):

```bash
git clone https://github.com/jonas678/PSkills /tmp/PSkills
cp -r /tmp/PSkills/skills/* ~/.claude/skills/
```

---

## 🌟 The Golden Workflow (How it all connects)

The true power of PSkills is that these skills perfectly interlock using Claude Code's **Agent Teams (Swarm)** mechanism. You (the human) act as the Product Director, while Claude plays all the expert roles.

Here is the ultimate end-to-end workflow:

### 👑 Stage 1: PM Planning & Discovery (`/prd-planner` + `Explore` Agent)
You open the main Claude Code session and type `/prd-planner I want to update our user login to add JWT`.
Claude acts as your PM. For existing projects (Brownfield), it first spawns a **System Analyst** (using the `Explore` subagent) to map out how the code currently works, keeping your PM context clean. 
Then, it asks clarifying questions, scopes the MVP, and outputs a rock-solid `PRD.md` with explicit "As-Is" and "To-Be" sections, along with Role Briefs for the downstream agents.

### 🚀 Stage 2: Swarm Initialization & Design (`TeamCreate` + `/frontend-design`)
You tell Claude to execute the handoff. Claude uses `TeamCreate` to spawn a Swarm of agents in the background.
- The **Designer Agent** wakes up, reads the PRD, and natively invokes the **`/frontend-design`** skill. It writes high-fidelity, bold HTML/React prototypes into your project.
- The **Engineering Agent** wakes up, reads the PRD, and invokes Phase 1 of **`/sdd-tdd`** to write the Software Design Document (SDD) defining API contracts and database schemas.

### 🔴 Stage 3: TDD Red Phase (`/qa-engineer` Mode 2)
Before the Engineer is allowed to write business logic, the Engineering Agent awakens the **QA Agent** (using **`/qa-engineer`**).
- QA reads the SDD and PRD.
- QA writes rigorous automated tests (Jest, Pytest, Playwright).
- QA runs the tests. They **FAIL (RED)** because the feature isn't built yet. QA hands the failing logs to the Engineer.

### 🟢 Stage 4: Implementation & Green Verification (`/sdd-tdd` + `/playwright-cli`)
- The **Engineering Agent** writes the actual backend/frontend code to make the tests pass.
- Once done, the **QA Agent** is called back for **Mode 3 (Independent Verification)**. QA runs the test suite (calling **`/playwright-cli`** for E2E UI tests).
- If it's GREEN, the feature is done! If it's RED, QA kicks the error logs back to the Engineer via `SendMessage`.

---

## Skills included

### `/prd-planner`
**The PM-first Product Requirement Planner**
Turn vague product requests into structured planning outputs that can progress from discovery to PRD, supporting documents, and team handoff.
- **Requirement clarification**: Ask high-impact questions first to shape new systems or existing feature enhancements.
- **Handoff & Execution**: Generate role-specific briefs (Designer, Engineering, QA) with direct-copy prompts.
- **Automated Swarm Teams**: Automatically sets up an Agent Team (using `TeamCreate` and `run_in_background`) so the PM session orchestrates downstream Designer, QA, and Engineering agents without context pollution.
- **Seamlessly integrates with `/sdd-tdd`**: Passes the generated `PRD.md` and `Engineering-Brief.md` directly to the development workflow.

### `/qa-engineer`
**The Dedicated Quality Assurance Agent**
A specialized agent skill designed to enforce Test-Driven Development (TDD) and objective verification. Use this standalone or spawn it as an agent during a team Swarm.
- **Mode 1 (Strategy)**: Reads PRDs/Briefs to output structured test dimensions, risk models, and edge cases.
- **Mode 2 (TDD Red)**: Reads SDDs/Specs to write failing test cases (unit/e2e/integration). Strictly prohibited from writing feature implementation.
- **Mode 3 (TDD Green / Verify)**: Independently runs test suites (using `/playwright-cli` when needed) and reports PASS/FAIL logs back to the orchestrator. Strictly prohibited from fixing bugs itself to maintain verification integrity.

### `/frontend-design`
**The Distinctive Frontend Designer (Official Anthropic Skill)**
An officially maintained Anthropic skill integrated into the `PSkills` workflow. It guides the creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics.
- **Bold Design Aesthetics**: Prioritizes unique typography, extreme aesthetics (minimalism, brutalism, luxury), and memorable color palettes.
- **Frontend Code Generation**: Creates actual, working HTML/CSS/JS or React/Vue components with meticulous attention to motion and layout.
- **Swarm Integration**: Seamlessly works inside the `/prd-planner` and `/sdd-tdd` team Swarms.

### `/sdd-tdd`
**Spec → Test → Implement (The Engineering Core)**
A Three-phase engineering workflow:
- **Phase 1 (SPEC)**: Reads PRDs from `/prd-planner` and produces a technical Software Design Document (SDD).
- **Phase 2 (TEST)**: Spawns the `/qa-engineer` agent to draft test cases and verify they are **RED** (failing) before implementation starts.
- **Phase 3 (IMPLEMENT)**: Spawns implementation agents (frontend, backend, database) to write code. Finally, spawns a QA agent as an independent verifier to ensure tests are **GREEN**.

### `coordination-patterns`
Decision aid for shaping non-trivial work. Helps choose between doing work solo, spawning subagents, write-then-verify loops, or Swarm/Team Swarm approaches. Recognizes `TeamCreate` and `SendMessage` as the ultimate primitives for complex multi-agent message buses.

### `playwright-cli`
Automates browser interactions and Playwright test workflows within Claude Code sessions. Natively invoked by `/qa-engineer` during E2E test verification.

### `/karpathy-guidelines`
**The Behavioral Anti-Slop Filter**
A set of foundational behavioral rules derived from Andrej Karpathy's observations on LLM coding pitfalls.
- **Simplicity First**: Prevents overcomplication, bloated abstractions, and speculative feature creep.
- **Surgical Changes**: Stops Claude from randomly refactoring adjacent code or deleting comments it doesn't fully understand.
- **Goal-Driven Execution**: Enforces a "define success criteria -> loop until verified" mindset, perfectly complementing the TDD workflow.