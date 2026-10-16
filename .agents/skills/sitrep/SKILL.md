---
name: sitrep
description: "Audits visible conversation history, artifacts, diffs, and workspace state to produce a dense, objective situational briefing (SITREP) optimized for resuming interrupted work. Triggers when the user asks for a situation report, status update, or calls /sitrep."
---

# Situation Report (`sitrep`)

## Role & Purpose
You are executing the `sitrep` (Situation Report) workflow. Your objective is to instantly re-orient a user returning to an interrupted session by auditing visible conversation history, artifacts, diffs, background tasks, and workspace state to produce an ergonomically scannable, strictly grounded, and candid situational briefing.

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

---

### Epistemic Integrity & Visual Receipts (Strict Grounding)
Maintain 100% factual accuracy by embedding receipts cleanly at the visual layer (in badges, pills, and compact bullet metadata) so the UI remains sleek and uncluttered:

1. **The "Receipts" Standard (No Unverified Claims):**
   - Never claim `🟢 TESTED & PASSING` without embedding the exact verifiable command and exit code inline: e.g. `🟢 TESTED & PASSING (via pytest tests/auth/ • exit 0)`.
   - If code was edited or created without running a test/build command in the transcript, strictly mark as `🟡 UNVERIFIED DRAFT (Code written, unexecuted)`.
2. **Anti-Pseudometric Standard (No Fabricated Progress):**
   - Never invent arbitrary completion percentages. Calculate progress ONLY against explicit checklist items (e.g. from a plan or prompt).
   - If no formal checklist exists, report progress strictly by observable units: `Progress: In-flight ([N] verified actions taken)`.
3. **Disk Truth vs. Chat Memory:**
   - Every file reported as created or modified MUST appear in live `git status -s`. Never report conversational intent as completed work.
4. **Surface Silent Pivots Cleanly:**
   - Audit visible logs for non-zero tool exit codes. If an attempted approach failed and you pivoted, disclose it in one concise bullet (`⚠️ *Pivot:* [failed command] -> [fallback taken]`).
5. **Side-Effect & Scope-Drift Check:**
   - Briefly flag unprompted files modified by formatters, lockfile generators, or build tools (`📦 *Side-effects:* [e.g. package-lock.json modified]`).

---

### Fast Short-Circuit (Trivial / No-Change Turns)
If **no tool calls, code modifications, or background tasks** have occurred since the last user turn (e.g., user ran `/sitrep` twice or after a purely informational exchange), do NOT emit the full template. Output this compact 4-line heartbeat card:

```markdown
# 🛰️ SITREP (Heartbeat — No State Change)
- **Mission:** [1-sentence summary of active goal]
- **Status:** 🟢 IDLE — No actions, edits, or commands executed since last turn.
- **Workspace:** Clean | Branch: `[branch]` | Verification: `[🟢 Tested (via cmd) | 🟡 Draft | 🔴 Failing]`
Awaiting instruction: Confirm next action or provide a new directive.
```

---

### UX-Optimized Output Specification

Deliver the SITREP using the structured format below. Keep receipts embedded inline in badges and pills to maintain a clean visual hierarchy. Format all file references as clickable links (e.g. `[filename](file:///path/to/file)` or `./relative/path`).

# 🛰️ SITREP

**Mission:** [1-sentence summary of overall active objective]
**Progress:** `[████████░░] N%` (Step X of Y: [Milestone]) *(or: `In-flight ([N] verified actions taken)` if unchunked)*
**Status:** [🟢 IDLE (Task complete) | ⏸️ PAUSED (Milestone review) | 🛑 BLOCKED (Decision required) | 💥 ERRORED (Failure encountered)] — [Reason]
**Verification:** [🟢 TESTED & PASSING (via `[command]` • exit 0) | 🟡 UNVERIFIED DRAFT (Code written, unexecuted) | 🔴 FAILING (via `[command]` • exit [code])]
**Blast Radius:** [Low | Med | High] ([N] created, [N] modified, [N] deletions | Git revert: [Safe / Dirty])
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
Chronological narrative (2–4 bullets) of what was accomplished and verified since the user's last directive, grounded strictly in live disk and log evidence:
- Specific files created or modified formatted as clickable links with change stats: `[filename](file:///...)` `(+N, -N)`.
- Commands validated with clean exit codes: e.g. `[command]` (exit 0).
- *(If applicable)* `⚠️ Pivot: Attempted [cmd] (exit [code]); switched to [workaround].`
- *(If applicable)* `📦 Side-effects: [Unprompted files touched, e.g. lockfiles or cache].`
*(If no work was completed: "No state changes detected in this segment.")*

---

### 🛡️ Progressive Health Check (Collapsible Signal)
- **If issues exist:**
  - **Low Confidence & Fragile Areas:** Component, doubt source, and atomic remedy.
  - **Known Bugs & Regressions:** Errors or failing tests observed during this session.
  - **Latent Risks & Misses:** Boundary conditions or omitted requirements.
- **If NO issues exist:**
  - Collapse all three above into this single clean line:
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
