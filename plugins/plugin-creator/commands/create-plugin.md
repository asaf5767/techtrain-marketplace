---
name: create-plugin
description: Scaffold a complete plugin marketplace with a plugin and skills inside it. Creates the full folder structure ready to git push and share with your team.
---

# Create Plugin Wizard

You are helping a developer create a **complete plugin marketplace** — the full package they can push to GitHub and share with their team. This works identically in Copilot CLI and Claude Code. Walk them through step by step.

## Step 1: Name Your Marketplace & Plugin

Ask in one message:

1. **What's your team/group name?** (This becomes the marketplace name, e.g., `frontend-team-marketplace`)
2. **What does your plugin do?** (e.g., "our PR review standards", "deployment checklist", "API design rules")
3. **Give it a name** (kebab-case, e.g., `pr-review-tools`, `deploy-standards`)

Wait for answers before proceeding.

## Step 2: Plan Your Skills

A plugin can have **multiple skills** — each one is a separate piece of domain knowledge the AI uses automatically.

Help the user plan 1-3 skills for their plugin. Ask:

> "Think about the different aspects of this workflow. Each skill should cover ONE specific area. For example, a `pr-review-tools` plugin might have:"
> - `review-checklist` — what to check in every PR
> - `security-patterns` — security-specific review rules
> - `test-coverage` — test requirements and patterns
>
> "What skills should your plugin have? Start with 1-2, you can always add more later."

Confirm the skill names (kebab-case) before proceeding.

## Step 3: Create the Full Structure

Create the **entire marketplace + plugin + skills** structure at once:

```
<marketplace-name>/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── <plugin-name>/
        ├── .claude-plugin/
        │   └── plugin.json
        └── skills/
            ├── <skill-1>/
            │   └── SKILL.md
            ├── <skill-2>/
            │   └── SKILL.md
            └── (more skills as planned)
```

### marketplace.json:
```json
{
  "name": "<marketplace-name>",
  "description": "<team/org> AI plugin marketplace",
  "owner": {
    "name": "<team name>"
  },
  "plugins": [
    {
      "name": "<plugin-name>",
      "source": "./plugins/<plugin-name>",
      "description": "<what this plugin does>"
    }
  ]
}
```

### plugin.json:
```json
{
  "name": "<plugin-name>",
  "description": "<what this plugin does — one line>",
  "version": "1.0.0"
}
```

### Each SKILL.md:
```markdown
---
name: <skill-name>
description: <WHEN should AI use this — be specific, max 20 words>
---

# <Skill Title>

## Knowledge
<What the AI needs to KNOW — domain rules, patterns, standards>

## Workflow
<What the AI should DO — numbered step-by-step process>

## Verification
<How the AI should CHECK its own work — specific criteria>
```

## Step 4: Fill in Each Skill

Go through each skill one at a time. For each skill:

1. **Description field** — Must say WHEN to activate, not just what it does.
   - Bad: "Reviews code"
   - Good: "Reviews pull requests for security vulnerabilities, auth patterns, and input validation. Use when reviewing PRs or checking code for security issues."

2. **Knowledge section** — Ask: "What does a senior person on your team know about this that a junior doesn't?" Write THAT.

3. **Workflow section** — Ask: "Walk me through the steps as if teaching a new hire." Number them.

4. **Verification section** — Ask: "How would you verify this was done right?" List specific criteria.

After completing each skill, ask: **"Ready for the next skill, or want to refine this one?"**

Keep each skill 30-80 lines. If it's longer, split into two skills.

## Step 5: Add a Command (Optional)

After all skills are done, ask:

> "Do you also want a slash command? Commands are for things the user triggers explicitly (like `/review` or `/deploy-check`), while skills activate automatically."

If yes, create `commands/<command-name>.md`:
```markdown
---
name: <command-name>
description: <what this command does — shown in /help>
---

<Instructions for what AI should do when the user invokes this command.
Reference the skills in this plugin if relevant.>
```

## Step 6: Test Instructions

Tell the user:

```
# Add YOUR marketplace locally
/plugin marketplace add ./<marketplace-name>

# Install YOUR plugin
/plugin install <plugin-name>@<marketplace-name>

# Test each skill — ask AI something that should trigger it
# Verify the AI uses your knowledge without being told to
```

Then ask: **"Want to add another plugin to your marketplace?"**

If yes, repeat from Step 2 — create a new plugin folder under `plugins/` and update `marketplace.json` to include it.

## Guardrails

- Each skill must be under 80 lines. Concise beats comprehensive.
- Each skill MUST use the KNOW/DO/CHECK structure (Knowledge, Workflow, Verification sections).
- NEVER leave placeholder "TODO" text. Fill in real content or skip the section.
- Do NOT add hooks, agents, or .mcp.json — those are advanced features for later.
- The marketplace structure is NOT optional — always create it. It's what makes this shareable.
- ALWAYS fill description fields with WHEN to activate, not just WHAT it does.
