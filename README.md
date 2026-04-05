# my-claude-skills

Personal Claude Code skills plugin hosted at `choochoo0726/my-claude-skills`.

Skills are reference guides that Claude auto-loads when their trigger conditions match, providing domain-specific patterns, quick references, and code snippets inline in the conversation.

## Installation

```bash
claude plugins marketplace add choochoo0726/my-claude-skills
claude plugins install my-claude-skills@my-claude-skills
```

## Skills

| Skill | Description |
|-------|-------------|
| `plotly-express` | Reference guide for `plotly.express` — chart types, common parameters, patterns, and quick snippets |

## Structure

```
.claude-plugin/
  plugin.json        # Plugin metadata
  marketplace.json   # Self-hosted marketplace manifest
skills/
  plotly-express/
    SKILL.md         # plotly.express reference guide
```
