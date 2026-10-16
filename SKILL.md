---
name: sitrep
description: "Audits visible conversation history, artifacts, diffs, and workspace state to produce a dense, objective situational briefing (SITREP) optimized for resuming interrupted work. Triggers when the user asks for a situation report, status update, or calls /sitrep."
---

# Situation Report (`sitrep`)

## Role & Purpose
You are executing the `sitrep` (Situation Report) workflow. Your objective is to instantly re-orient a user returning to an interrupted session by auditing visible conversation history, artifacts, diffs, background tasks, and workspace state to produce a dense, objective, and candid situational briefing.

---

### Execution Boundary & Safety Rules
1. **Inspection vs. Mutation (Strictly Read-Only):**
   - **Permitted (Read-Only):** Strictly inspect state using fast read-only queries (`git status -s`, `git diff --stat`, `git log -1 --oneline`, task inspection e.g. `manage_task list`, directory listings).
   - **Strictly Forbidden:** Do NOT write application code, apply patches, run destructive commands, install dependencies, or trigger builds/tests during this turn. Report *last known* build/test states.
   - **Tool Absence Fallback:** If tool execution is unavailable or disabled, rely strictly on visible textual session context. Explicitly note: `[Environment verification: Static text context only; read-only tools unavailable]`.
2. **Fast-Triage Tool Protocol (Low Latency):**
   - Avoid slow, heavyweight inspections. Never dump verbose full `git diff` outputs into context; use `git diff --stat` or targeted line checks.
   - Limit tool calls to 2–3 rapid queries total. Do not read entire source files unless verifying an unverified edit.
3. **Temporal Scope:** Focus "Work Completed" on all work accomplished since the user's last substantive directive or checkpoint, rather than strictly a single microscopic turn.
4. **Grounding & Candor:** 
   - Report facts directly. Never assume a file edit succeeded or an edge case works without observable evidence or verification logs.
   - Highlight technical debt, brittle implementations, unverified assumptions, and regression risks immediately.

---

### Fast Short-Circuit (Trivial / No-Change Turns)
If **no tool calls, code modifications, or background tasks** have occurred since the last user turn (e.g., user ran `/sitrep` twice or after a purely informational exchange), do NOT emit the full 6-section template. Instead, output this compact 4-line heartbeat card:

```markdown
# 🛰️ SITREP (Heartbeat — No State Change)
- **Mission:** [1-sentence summary of active goal]
- **Status:** IDLE — No actions, edits, or commands executed since last turn.
- **Workspace:** Clean | Branch: `[branch]` | Verification: `[🟢 Tested | 🟡 Draft | 🔴 Failing]`
Awaiting instruction: Confirm next action or provide a new directive.
```

---

### Standard Output Specification

Deliver the SITREP using the structured format below. Use terse, fact-dense bullet points. Omit conversational intros, preamble, or meta-commentary. Format all file references as clickable links (e.g. `[filename](file:///path/to/file)` or `./relative/path`).

# 🛰️ SITREP

- **Mission:** [1-sentence summary of overall active objective and current progress/milestone]
- **Execution Status:** [IDLE | BLOCKED | PAUSED | ERRORED] — [Exact reason execution stopped or paused]
- **Verification Confidence:** [🟢 TESTED & PASSING | 🟡 UNVERIFIED DRAFT | 🔴 FAILING / BROKEN]
  - *🟢 Tested & Passing: Tests or build validated with exit code 0.*
  - *🟡 Unverified Draft: Code edited/created, but has NOT yet been compiled, executed, or tested.*
  - *🔴 Failing / Broken: Build failed, tests failed, or runtime error encountered.*
- **Workspace Pulse:** Branch: `[branch-name]` | Working Tree: `[N modified, N untracked, N staged]` | Tasks: `[Active/completed background tasks & subagents, or 'None']`

---

> [!IMPORTANT]
> **Dynamic Hierarchy Rule:**
> If **Execution Status** is **BLOCKED**, move **Section 3 (Human Input Required)** immediately below the Workspace Pulse block and prefix it with `🚨 BLOCKER / DECISION REQUIRED` so the human sees the roadblock first. Otherwise, maintain standard numerical order.

## 1. Work Completed (While Away)
- **Status:** Concrete tasks completed since the user's last directive.
- **Verified Changes:** Specific files created/modified (as clickable links), configuration updates, or commands validated with clean exit codes.
*(If no work was completed: "No state changes detected in this segment.")*

## 2. Low Confidence & Revisit List
Items completed or attempted that remain fragile, unverified, or reliant on assumptions. For each item:
- **Item:** Component, function, or architectural decision.
- **Doubt Source:** Exact failure mode, unhandled edge case, missing test coverage, or unverified output.
- **Proposed Remedy:** The atomic action or test required to establish certainty.
*(If all recent work is definitively verified: "None. All recent work verified against requirements.")*

## 3. Human Input Required (Blockers & Decisions)
Decisions or missing inputs that halt forward progress or branch architecture.
- Format each as a concrete binary choice (A vs. B), a numbered selection list, or an explicit parameter/secret request.
- Do NOT ask open-ended or speculative questions.
*(If clear to proceed autonomously: "No immediate human blockers.")*

## 4. Immediate Next (Tactical: Turn N+1)
Ordered list (1, 2, 3) of atomic, testable actions ready for immediate execution upon approval. Each item must have a clear scope and expected outcome.

## 5. Horizon Next (Strategic: Turns N+2 to N+5)
High-level list (2–4 bullets) of downstream tasks, integration steps, or cleanup milestones to maintain alignment with the overall objective.

## 6. Gaps, Potential Misses & Regressions
- **Known Bugs / Regressions:** Unresolved errors, breaking changes, or failing tests observed during this session.
- **Latent Edge Cases:** Fragile assumptions, scaling limits, security considerations, or boundary conditions not yet covered.
- **Omissions:** Requirements from prior user prompts that were skipped, postponed, or partially completed.
*(If none detected: "No open bugs, latent regressions, or obvious blind spots identified.")*

---

### Post-Report Handoff
Conclude the response precisely with this single line:
`Awaiting instruction: Reply '1' to execute [Step 1 title], 'continue' to run immediate steps, or provide specific direction.`
