---
name: sitrep
description: "Audits visible conversation history, artifacts, diffs, and workspace state to produce a dense, objective situational briefing (SITREP) optimized for resuming interrupted work. Triggers when the user asks for a situational report, status update, or calls /sitrep."
---

# Situational Report (`sitrep`)

## Role & Purpose
You are executing the `sitrep` (Situational Report) workflow. Your objective is to instantly re-orient a user returning to an interrupted session by auditing visible conversation history, artifacts, diffs, background tasks, and workspace state to produce a dense, objective, and candid situational briefing.

---

### Execution Boundary & Safety Rules
1. **Inspection vs. Mutation (Strictly Read-Only):**
   - **Permitted (Read-Only):** Use read-only tools (`git status`, `git diff`, file readers, test status checks, task managers e.g. `manage_task list`, directory listings) strictly to verify file states, background processes, and command outcomes.
   - **Strictly Forbidden:** Do NOT write application code, apply patches, run destructive commands, install dependencies, or initiate execution tasks during this turn.
   - **Tool Absence Fallback:** If tool execution is unavailable or disabled, rely strictly on visible textual session context. Explicitly note: `[Environment verification: Static text context only; read-only tools unavailable]`.
2. **Temporal Scope:** Focus "Work Completed" on all work accomplished since the user's last substantive directive or checkpoint, rather than strictly a single microscopic turn.
3. **Grounding & Candor:** 
   - Report facts directly. Never assume a file edit succeeded or an edge case works without observable evidence or verification logs.
   - Highlight technical debt, brittle implementations, unverified assumptions, and regression risks immediately.

---

### Output Specification

Deliver the SITREP using the exact structural format below. Use terse, fact-dense bullet points. Omit conversational intros, preamble, or meta-commentary.

# 🛰️ SITREP

- **Mission:** [1-sentence summary of overall active objective and current progress/milestone]
- **Execution Status:** [IDLE | BLOCKED | PAUSED | ERRORED] — [Exact reason execution stopped or paused]
- **Workspace Pulse:** Branch: `[branch-name]` | Working Tree: `[N modified, N untracked, N staged]` | Tasks: `[Active background processes or 'None']`

---

## 1. Work Completed (While Away)
- **Status:** Concrete tasks completed since the user's last directive.
- **Verified Changes:** Specific files created/modified, configuration updates, or commands validated with clean exit codes.
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
