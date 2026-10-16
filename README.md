# `sitrep` — Situational Report Skill

A lightweight, high-candor Situational Report (`sitrep`) workflow for AI coding assistants.

When invoked via `/sitrep` (or upon asking for a status report), the assistant audits recent conversation history, diffs, artifacts, and workspace state to produce a dense, objective, read-only situational briefing.

---

## ⚡ The Fastest Way: Just Ask Your Agent!

If you are already inside an AI coding assistant session, simply paste this prompt into the chat:

> **"Install the `sitrep` skill from https://github.com/HeWhoRoams/sitrep so I can call `/sitrep` for situational reports."**

Or provide the raw skill file URL directly:
> **"Fetch https://raw.githubusercontent.com/HeWhoRoams/sitrep/main/SKILL.md and save it as a skill/rule in this workspace so I can use `/sitrep`."**

Any capable agent will read the repository or raw file and place it into the correct configuration directory for your current environment automatically.

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
> "Run a situational report before we proceed."

The agent will inspect your working tree and diffs using read-only tools and deliver a concise, fact-dense situational briefing concluding with:
`Awaiting instruction: Confirm immediate next steps, provide input on listed items, or redirect focus.`
