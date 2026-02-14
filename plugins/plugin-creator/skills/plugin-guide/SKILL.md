---
name: plugin-guide
description: Claude Code plugin and marketplace structure guide. Activate when user asks about creating, structuring, or troubleshooting plugins or marketplaces.
---

# Claude Code Plugin & Marketplace Guide

## Plugin Structure

A plugin is a folder with:
- `.claude-plugin/plugin.json` — name, description, version
- `skills/<name>/SKILL.md` — model-invoked (AI uses automatically based on context)
- `commands/<name>.md` — user-invoked slash commands
- Optionally: `agents/`, `hooks/`, `.mcp.json`

## Marketplace Structure

A marketplace is a Git repo with:
- `.claude-plugin/marketplace.json` — catalog listing plugins
- `plugins/` — contains plugin directories
- Each plugin in `marketplace.json` needs: name, source (relative path), description

## SKILL.md Format

```yaml
---
name: skill-name
description: When to use this — be specific (max 20 words)
---
```

Body uses KNOW / DO / CHECK:
- **Knowledge**: Domain rules and patterns
- **Workflow**: Numbered steps
- **Verification**: How to check the output

## Key Commands

```
/plugin marketplace add owner/repo          # Add marketplace from GitHub
/plugin marketplace add ./local-path        # Add local marketplace
/plugin install plugin-name@marketplace     # Install a plugin
claude --plugin-dir ./path                  # Test plugin directly
```

## Common Issues

| Problem | Fix |
|---------|-----|
| Skill doesn't activate | Make `description` more specific about WHEN to use it |
| JSON parse error | Check for trailing commas, wrong quotes, missing braces |
| Plugin not found | Verify `source` path in marketplace.json is correct |
| Skill too long | Cut to 30-80 lines. One good example beats ten rules |

## Tool Compatibility

These skill files work across tools:
- **Claude Code**: `.claude/skills/<name>/SKILL.md` or via plugin
- **Copilot CLI**: Reads `.claude/skills/` automatically
- **GitHub Copilot**: `.github/instructions/<name>.instructions.md` (same content, different location)
