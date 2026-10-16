# `sitrep` — Situational Report Skill

A lightweight, high-candor Situational Report (`sitrep`) workflow for AI coding assistants (such as Google Antigravity, Claude Code, and Gemini Agent).

When invoked via `/sitrep` or upon asking for a status report, the assistant audits recent conversation history, diffs, artifacts, and workspace state to produce a dense, objective, read-only situational briefing.

---

## Features

- **Strict Read-Only Execution:** Uses read-only commands (`git status`, `git diff`, directory inspection) to verify state without mutating code or configuration.
- **High Candor & Grounding:** Zero fluff or cheerleading. Surfaces technical debt, brittle code, unverified assumptions, and regressions directly.
- **Structured 6-Part Briefing:**
  1. **Recent Activity (Completed Work)**
  2. **Low Confidence & Revisit List**
  3. **Human Input Required (Blockers & Decisions)**
  4. **Immediate Next (Tactical: Turn N+1)**
  5. **Horizon Next (Strategic: Turns N+2 to N+5)**
  6. **Gaps, Potential Misses & Regressions**

---

## Installation

### Option 1: Global Installation (Machine-Wide)

To make `/sitrep` available across all workspaces and projects on your machine, copy `SKILL.md` into your global config directory:

```bash
# Windows
mkdir -Force "$HOME\.gemini\config\skills\sitrep"
cp SKILL.md "$HOME\.gemini\config\skills\sitrep\SKILL.md"

# Linux / macOS
mkdir -p ~/.gemini/config/skills/sitrep
cp SKILL.md ~/.gemini/config/skills/sitrep/SKILL.md
```

### Option 2: Project Repository Installation (Shared with Team)

To check the skill directly into any existing repository so that anyone opening the repo in Antigravity can use `/sitrep`:

```bash
# In your project repository root:
mkdir -p .agents/skills/sitrep
cp path/to/SKILL.md .agents/skills/sitrep/SKILL.md
git add .agents/skills/sitrep/SKILL.md
git commit -m "feat(skills): add /sitrep situational report skill"
```

---

## Usage

Simply run:

```text
/sitrep
```

or ask:

> "Give me a sitrep on where we are right now."
