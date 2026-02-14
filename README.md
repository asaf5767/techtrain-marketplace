# techtrain-marketplace

Course marketplace for the **Context-Driven AI Orchestration** workshop.

## What's Inside

| Plugin | What It Does | How to Use |
|--------|-------------|------------|
| **plugin-creator** | Scaffolds new plugins and marketplaces | `/techtrain:create-plugin` |
| **hello-skill** | Sample plugin — minimal reference implementation | Browse the code to see the pattern |

## Quick Start

```bash
# Add this marketplace
/plugin marketplace add instructor-gh-user/techtrain-marketplace

# Install the plugin creator
/plugin install plugin-creator@techtrain-marketplace

# Create your own plugin
/techtrain:create-plugin
```

## Plugin Structure Reference

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json       ← Name, description, version
└── skills/
    └── my-skill/
        └── SKILL.md      ← KNOW / DO / CHECK
```

## Marketplace Structure Reference

```
my-marketplace/
├── .claude-plugin/
│   └── marketplace.json  ← Lists your plugins
└── plugins/
    └── my-plugin/        ← Your plugin(s) go here
```

## Testing Locally

```bash
# Test a marketplace from a local folder
/plugin marketplace add ./my-marketplace

# Test a plugin directly (no marketplace needed)
claude --plugin-dir ./my-plugin
```

## Tool Compatibility

The skill files (SKILL.md) work across:
- **Claude Code** — via plugins or `.claude/skills/`
- **Copilot CLI** — reads `.claude/skills/` automatically
- **GitHub Copilot** — use `.github/instructions/` with same content
