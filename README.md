# claude-skills

Personal [Claude Code](https://claude.com/claude-code) skills.

| Skill | What it does |
| ----- | ------------ |
| [modernize-design](skills/modernize-design/SKILL.md) | Redesign a component/page: first builds one HTML file with 2–3 design options, waits for your pick, then implements it with the project's own tokens and components. |

## Install

### Option 1 — Claude Code plugin (easiest, auto-updates)

From a terminal:

```bash
claude plugin marketplace add aiThanet/claude-skills
claude plugin install modernize-design@aithanet-skills
```

Or inside Claude Code:

```
/plugin marketplace add aiThanet/claude-skills
/plugin install modernize-design@aithanet-skills
```

Restart Claude Code. Update later with `claude plugin marketplace update aithanet-skills` (or `/plugin marketplace update aithanet-skills`).

### Option 2 — Copy the skill folder

macOS / Linux / Git Bash:

```bash
git clone --depth 1 https://github.com/aiThanet/claude-skills /tmp/claude-skills \
  && mkdir -p ~/.claude/skills \
  && cp -r /tmp/claude-skills/skills/modernize-design ~/.claude/skills/
```

Windows PowerShell:

```powershell
git clone --depth 1 https://github.com/aiThanet/claude-skills "$env:TEMP\claude-skills"
Copy-Item -Recurse "$env:TEMP\claude-skills\skills\modernize-design" "$HOME\.claude\skills\"
```

Use `.claude/skills/` inside a project instead of `~/.claude/skills/` to install for that project only.

### Option 3 — Ask Claude

Paste into Claude Code:

> Install the modernize-design skill from https://github.com/aiThanet/claude-skills into ~/.claude/skills

## Use

Ask "modernize the orders page", or run `/modernize-design` (plugin install: `/modernize-design:modernize-design`).
