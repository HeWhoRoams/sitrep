# `sitrep` — Situation Report Skill

Do you ever kick off a task in your AI Agent, come back hours later and wonder what the heck you were doing? Here's a solution.

A lightweight, high-candor Situation Report (`sitrep`) workflow for AI coding assistants.

When invoked via `/sitrep` (or upon asking for a status report), the assistant audits recent conversation history, diffs, artifacts, and workspace state to produce a dense, objective, read-only Situation briefing.

---

## ⚡ The Fastest Way: Just Ask Your Agent!

If you are already inside an AI coding assistant session, simply paste this prompt into the chat:

> **"Install the `sitrep` skill from https://github.com/HeWhoRoams/sitrep so I can call `/sitrep` for Situation reports."**

Or provide the raw skill file URL directly:
> **"Fetch https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md and save it as a skill/rule in this workspace so I can use `/sitrep`."**

Any capable agent will read the repository or raw file and place it into the correct configuration directory for your current environment automatically.

---

## Features

- **Sub-2s Visual Re-orientation:** Heads-Up Display featuring an upfront **Mission Objective**, **Visual Progress Meter** (`[████████░░] 80%`), and **Execution Status** badge.
- **Blast Radius & Reversibility:** Psychological safety at a glance — instantly shows files created, modified, deletions, and git revert safety.
- **Chronological "While You Were Away" Timeline:** Tells the story of what happened while you stepped away rather than dumping a raw file table.
- **Epistemic Integrity & Visual Receipts:** Badges embed verified command proofs inline (`🟢 TESTED & PASSING (via pytest • exit 0)` vs `🟡 UNVERIFIED DRAFT`). No fabricated percentages or unverified claims.
- **Dynamic Decision Cards:** If `BLOCKED`, roadblocks jump to the very top formatted as interactive cards with 1-character selectors (`Reply 'A' or 'B'`).
- **Progressive Silence:** When no issues exist, collapses empty sections into a single clean line (`✅ Health: No blockers, open bugs, or regressions detected`).
- **Fast-Triage Tool Protocol:** Sub-5 second execution. Uses lightweight queries (`git status -s`, `git diff --stat`) capped at 2–3 read calls.
- **No-Change Short-Circuit:** If invoked with zero new state changes, returns a compact 4-line heartbeat card.
- **Strict Read-Only Safety:** Strictly audits state without mutating files, running builds, or executing application code.
- **Zero-Friction Re-entry:** Numbers immediate next actions so you can resume work by replying with a single character (`1` or `continue`).

---

## Installation by AI Agent

### 1. Google Antigravity & Gemini CLI (`agy`)

Antigravity natively discovers skills in global configuration and workspace folders:

#### Global (All Projects on Machine)
- **Windows (PowerShell):**
  ```powershell
  mkdir -Force "$HOME\.gemini\config\skills\sitrep"
  curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o "$HOME\.gemini\config\skills\sitrep\SKILL.md"
  ```
- **macOS / Linux:**
  ```bash
  mkdir -p ~/.gemini/config/skills/sitrep
  curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o ~/.gemini/config/skills/sitrep/SKILL.md
  ```

#### Workspace Project (Team / Repo Level)
```bash
mkdir -p .agents/skills/sitrep
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o .agents/skills/sitrep/SKILL.md
git add .agents/skills/sitrep/SKILL.md
git commit -m "feat(skills): add /sitrep skill"
```

---

### 2. Claude Code

Claude Code discovers skills in `~/.claude/skills/` (global) or `.claude/skills/` (project), and project instructions via `CLAUDE.md`:

#### Global Installation
```bash
mkdir -p ~/.claude/skills/sitrep
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o ~/.claude/skills/sitrep/SKILL.md
```

#### Project Installation
```bash
mkdir -p .claude/skills/sitrep
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o .claude/skills/sitrep/SKILL.md
```

*Alternatively, append the prompt from `SKILL.md` to your project's `CLAUDE.md` under a `/sitrep` command section.*

---

### 3. Cursor

Add `sitrep` as a Cursor Rule so the agent can execute it on demand via `@sitrep` or chat:

#### Project Rule (`.cursor/rules/`)
Create `.cursor/rules/sitrep.mdc`:
```bash
mkdir -p .cursor/rules
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o .cursor/rules/sitrep.mdc
```
*(Or append the contents of `SKILL.md` to `.cursorrules` in your repository root).*

---

### 4. Windsurf / Cascade

Add `sitrep` to your Cascade workspace rules:

```bash
mkdir -p .windsurf/rules
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o .windsurf/rules/sitrep.md
```
*(Or append the prompt from `SKILL.md` into your `.windsurfrules` file).*

---

### 5. Cline / Roo Code

Add `sitrep` to your project rules or custom instructions:

```bash
# Append as a project-level instruction:
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" >> .clinerules
```
In Roo Code, you can also add `sitrep` as a dedicated Read-Only custom mode in `.roomodes`.

---

### 6. GitHub Copilot (VS Code, JetBrains, CLI)

Add `sitrep` to your repository instructions for GitHub Copilot:

```bash
mkdir -p .github
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" >> .github/copilot-instructions.md
```

---

### 7. Aider

Tell Aider to keep `SKILL.md` in read-only context during sessions:

```bash
# Add to your project's .aider.conf.yml
read:
  - ".agents/skills/sitrep/SKILL.md"
```
Or run aider with:
```bash
aider --read .agents/skills/sitrep/SKILL.md
```

---

### 8. OpenCode / Generic Agents (`AGENTS.md`)

Many modern agents automatically parse `AGENTS.md` at the project root:

```bash
mkdir -p .agents/skills/sitrep
curl -sSL "https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md" -o .agents/skills/sitrep/SKILL.md
```

---

## Usage

Once installed, simply run:

```text
/sitrep
```

or ask:

> "Give me a sitrep on where we are right now."
> "Run a Situation report before we proceed."

The agent will inspect your working tree and diffs using read-only tools and deliver a concise, fact-dense Situation briefing concluding with:
`Awaiting instruction: Reply '1' to execute [Step 1 title], 'continue' to run immediate steps, or provide specific direction.`
