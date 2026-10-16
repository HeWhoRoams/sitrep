---
name: sitrep
description: "Audits visible conversation history, artifacts, diffs, and workspace state to produce a dense, objective situational briefing (SITREP) optimized for resuming interrupted work. Triggers when the user asks for a situation report, status update, or calls /sitrep."
---

# Situation Report (`sitrep`)

## Role & Purpose
You are executing the `sitrep` (Situation Report) workflow. Your objective is to instantly re-orient a user returning to an interrupted session by auditing visible conversation history, artifacts, diffs, background tasks, and workspace state to produce an ergonomically scannable, objective, and candid situational briefing.

---

### Execution Boundary & Safety Rules
1. **Inspection vs. Mutation (Strictly Read-Only):**
   - **Permitted (Read-Only):** Strictly inspect state using fast read-only queries (`git status -s`, `git diff --stat`, `git log -1 --oneline`, task inspection e.g. `manage_task list`, directory listings).
   - **Strictly Forbidden:** Do NOT write application code, apply patches, run destructive commands, install dependencies, or trigger builds/tests during this turn. Report *last known* build/test states.
   - **Tool Absence Fallback:** If tool execution is unavailable or disabled, rely strictly on visible textual session context. Explicitly note: `[Environment verification: Static text context only; read-only tools unavailable]`.
2. **Fast-Triage Tool Protocol (Low Latency):**
   - Keep response time under 5 seconds. Never dump verbose full `git diff` outputs into context; use `git diff --stat` or targeted line checks.
   - Limit tool calls to 2–3 rapid queries total. Do not read entire source files unless verifying an unverified edit.
3. **Temporal Scope:** Focus the recap on all work accomplished since the user's last substantive directive or checkpoint, rather than strictly a single microscopic turn.
4. **Grounding & Candor:** 
   - Report facts directly. Never assume a file edit succeeded or an edge case works without observable evidence or verification logs.
   - Highlight technical debt, brittle implementations, unverified assumptions, and regression risks immediately.

---

### Fast Short-Circuit (Trivial / No-Change Turns)
If **no tool calls, code modifications, or background tasks** have occurred since the last user turn (e.g., user ran `/sitrep` twice or after a purely informational exchange), do NOT emit the full template. Output this compact 4-line heartbeat card:

```markdown
# 🛰️ SITREP (Heartbeat — No State Change)
- **Mission:** [1-sentence summary of active goal]
- **Status:** 🟢 IDLE — No actions, edits, or commands executed since last turn.
- **Workspace:** Clean | Branch: `[branch]` | Verification: `[🟢 Tested | 🟡 Draft | 🔴 Failing]`
Awaiting instruction: Confirm next action or provide a new directive.
```

---

### UX-Optimized Output Specification

Deliver the SITREP using the structured format below. Use terse, fact-dense bullet points. Format all file references as clickable links (e.g. `[filename](file:///path/to/file)` or `./relative/path`).

# 🛰️ SITREP

**Mission:** [1-sentence summary of overall active objective]
**Progress:** `[████████░░] N%` (Step X of Y: [Current Milestone Name])
**Status:** [🟢 IDLE (Task complete) | ⏸️ PAUSED (Awaiting milestone review) | 🛑 BLOCKED (Decision required) | 💥 ERRORED (Failure encountered)] — [Reason]
**Verification:** [🟢 TESTED & PASSING | 🟡 UNVERIFIED DRAFT | 🔴 FAILING / BROKEN]
**Blast Radius:** [Low | Med | High] ([N] files created, [N] modified, [N] deletions | Git revert: [Safe / Dirty])
**Workspace Pulse:** Branch: `[branch-name]` | Tasks: `[Active/completed background tasks & subagents, or 'None']`

---

### 🚨 Human Decision Required (Dynamic: Show FIRST if BLOCKED)
*(Only include this section at the very top if `Status` is 🛑 BLOCKED. Otherwise omit or keep in standard order.)*
- Present blockers as **Decision Cards** with 1-character selectors:
  - **[A] Option Title:** Tradeoffs / rationale.
  - **[B] Option Title:** Tradeoffs / rationale.
- Explicit prompt: `👉 Reply 'A' or 'B' to immediately unblock.`

---

### ⏱️ While You Were Away
Chronological narrative (2–4 bullets) of what was accomplished and verified since the user's last directive:
- Micro-timeline of key milestones reached and commands validated with clean exit codes.
- Specific files created or modified formatted as clickable links.
*(If no work was completed: "No state changes detected in this segment.")*

---

### 🛡️ Progressive Health Check (Collapsible Signal)
- **If issues exist:**
  - **Low Confidence & Fragile Areas:** Component, doubt source, and atomic remedy.
  - **Known Bugs & Regressions:** Errors or failing tests observed during this session.
  - **Latent Risks & Misses:** Boundary conditions or omitted requirements.
- **If NO issues exist:**
  - Collapse all three above into this single line:
    `✅ Health: No blockers, open bugs, fragile assumptions, or regressions detected.`

---

### 👉 Immediate Next Steps (Tactical: Turn N+1)
Ordered list (1, 2, 3) of atomic, testable actions ready for immediate execution upon approval. Each item must have a clear scope and expected outcome.

### 🔭 Horizon Next (Strategic: Turns N+2 to N+5)
High-level list (2–3 bullets) of downstream tasks, integration steps, or cleanup milestones to maintain alignment with the overall objective.

---

### Post-Report Handoff
Conclude the response precisely with this single line:
`Awaiting instruction: Reply '1' to execute [Step 1 title], 'continue' to run immediate steps, or provide specific direction.`
