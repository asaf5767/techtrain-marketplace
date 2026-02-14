---
name: create-plugin
description: Scaffold a new Claude Code plugin with skills and optionally a marketplace. Walks you through the full setup step by step.
---

# Create Plugin Wizard

You are helping a developer create a Claude Code plugin. This works with Claude Code, Copilot CLI, and GitHub Copilot — the file structure is the same. Walk them through this step by step.

## Step 1: Understand the Intent

Ask the user these 3 questions (one message, wait for answers):

1. **What workflow does this plugin automate?** (e.g., "PR review with our team's checklist", "deployment pre-flight check")
2. **Who is it for?** (just you, your team, your org)
3. **What should the AI KNOW, DO, and CHECK?**
   - KNOW = domain rules, patterns, standards
   - DO = step-by-step workflow
   - CHECK = how to verify its own output

Wait for their answers before proceeding.

## Step 2: Choose Components

Based on their answers, recommend:

| Component | Create if... |
|-----------|-------------|
| **Skill** (`skills/<name>/SKILL.md`) | AI should activate this AUTOMATICALLY based on context |
| **Command** (`commands/<name>.md`) | User should invoke this EXPLICITLY via slash command |

Default recommendation: **start with a skill**. Most workflows benefit from auto-activation.

Confirm the plugin name (kebab-case, e.g., `pr-review-standards`) before proceeding.

## Step 3: Scaffold the Plugin

Create this folder structure in the current directory:

```
<plugin-name>/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── <skill-name>/
        └── SKILL.md
```

If they also want a command, add:
```
├── commands/
│   └── <command-name>.md
```

### plugin.json content:
```json
{
  "name": "<plugin-name>",
  "description": "<one-line description of what this plugin does>",
  "version": "1.0.0"
}
```

### SKILL.md structure:
```markdown
---
name: <skill-name>
description: <when should AI use this — be specific, max 20 words>
---

# <Skill Title>

## Knowledge
<What the AI needs to KNOW — domain rules, patterns, standards specific to this team/project>

## Workflow
<What the AI should DO — numbered step-by-step process>

## Verification
<How the AI should CHECK its own work — specific criteria>
```

### Command .md structure (if applicable):
```markdown
---
name: <command-name>
description: <what this command does — shown in /help>
---

<Instructions for what AI should do when user invokes this command>
```

## Step 4: Fill in the Content

After creating the skeleton files, help the user fill in each section:

- **KNOW section**: Ask "What does a senior person on your team know about this that a junior doesn't?" Write THAT.
- **DO section**: Ask "Walk me through the steps as if you're teaching a new hire." Number them.
- **CHECK section**: Ask "How would you verify this was done right?" List specific criteria.
- **Description field**: Must be specific about WHEN to activate. Bad: "Reviews code". Good: "Reviews pull requests for security vulnerabilities, auth patterns, and input validation. Use when reviewing PRs or checking code for security issues."

Keep skills 30-80 lines. If it's longer, they're writing documentation, not a skill.

## Step 5: Wrap in a Marketplace

Ask: "Do you want to wrap this in a marketplace so your team can install it with one command?"

If yes (recommended), restructure:

```
<marketplace-name>/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── <plugin-name>/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            └── <skill-name>/
                └── SKILL.md
```

### marketplace.json:
```json
{
  "name": "<marketplace-name>",
  "description": "<team/org name> AI plugin marketplace",
  "owner": {
    "name": "<team name>"
  },
  "plugins": [
    {
      "name": "<plugin-name>",
      "source": "./plugins/<plugin-name>",
      "description": "<plugin description>"
    }
  ]
}
```

## Step 6: Test Instructions

Tell the user to test:

```bash
# Add YOUR marketplace locally
/plugin marketplace add ./<marketplace-name>

# Install YOUR plugin
/plugin install <plugin-name>@<marketplace-name>

# Test it — ask something that should trigger the skill
```

If using Copilot CLI or GitHub Copilot: the skill works when placed in `.claude/skills/` — both tools read this location.

## Guardrails

- Create ONE skill per plugin. Keep it simple.
- Skills must be under 80 lines. Concise beats comprehensive.
- Do NOT add hooks, agents, or .mcp.json — those are advanced features.
- NEVER leave placeholder "TODO" text. Fill in real content or skip the section.
- ALWAYS use the KNOW/DO/CHECK structure in skills.
