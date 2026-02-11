# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin marketplace repository hosted at `github.com/zoumo/dotclaude`. The marketplace contains plugins that extend Claude Code's capabilities, primarily the `chinese-style-guide` plugin for Chinese text formatting.

## Plugin Structure

```
plugins/
├── <plugin-name>/
│   ├── .claude-plugin/
│   │   └── plugin.json           # Plugin metadata (name, description, version, author)
│   ├── README.md                  # Plugin documentation with installation/usage instructions
│   └── skills/
│       └── <skill-name>/
│           └── SKILL.md           # Skill definition with YAML frontmatter
```

### Marketplace Configuration

The top-level `.claude-plugin/marketplace.json` defines the marketplace:

```json
{
  "name": "zoumo-dotclaude",
  "metadata": { "pluginRoot": "./plugins" },
  "repository": "https://github.com/zoumo/dotclaude",
  "plugins": [...]
}
```

### Skill Definition Format

Skills use YAML frontmatter at the top of `SKILL.md`:

```yaml
---
name: <skill-name>
description: <when this skill should be triggered>
---
```

### Plugin Installation (Marketplace)

Plugins are installed via Claude Marketplace, not manual placement:

```bash
/plugin marketplace add zoumo/dotclaude
/plugin install <plugin-name>@zoumo-dotclaude
```

### Skill Invocation

Skills are invoked by name directly—no plugin prefix needed:

```
/<skill-name>
```

## Plugin Development Documentation

When writing or modifying plugins:

1. **Plugin README.md** must include installation instructions for Claude Marketplace
2. **plugin.json** must contain: `name`, `description`, `version`, `author`
3. **SKILL.md** needs: YAML frontmatter (name, description), clear Good/Avoid examples in tables

### Git Workflow

- When creating a new plan, use `planning-with-files:plan` skill
- Store planning files in `./docs/plans/<plan-name>/`
- Commit messages follow convention: `docs:`, `feat:`, `fix:`, `refactor:`, `test:`, `chore:`