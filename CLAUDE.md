# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin marketplace repository hosted at `github.com/zoumo/dotclaude`. The marketplace contains plugins that extend Claude Code's capabilities:

- **chinese-style-guide**: Chinese text formatting and spacing guidelines
- **go-style-guide**: Go coding standards, best practices, and testing patterns with automated code review agent

## Plugin Development Reference

### Plugin Specification

> Source: [Claude Code Plugins Reference](https://code.claude.com/docs/en/plugins-reference)

#### Directory Structure

```
plugins/
├── <plugin-name>/
│   ├── .claude-plugin/
│   │   └── plugin.json           # Plugin manifest (required)
│   ├── README.md                  # Plugin documentation
│   ├── skills/                    # Skills directory
│   ├── agents/                    # Agents directory (optional)
│   └── hooks/                     # Hooks directory (optional)
```

#### Plugin Manifest (plugin.json)

**Required Fields:**

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `name` | string | Unique identifier (kebab-case) | `"go-style-guide"` |

**Metadata Fields:**

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `version` | string | Semantic version | `"1.0.0"` |
| `description` | string | Plugin purpose | `"Go coding standards"` |
| `author` | object | Author info | `{"name": "dev", "email": "..."}` |
| `keywords` | array | Discovery tags | `["go", "style"]` |

**Component Paths:**

| Field | Type | Description |
|-------|------|-------------|
| `skills` | string\|array | Skill directory paths |
| `agents` | string\|array | Agent file paths |
| `hooks` | string\|array\|object | Hook configuration |

---

### Skill Specification

> Source: [Agent Skills Specification](https://agentskills.io/specification)

#### Directory Structure

```
skills/
└── <skill-name>/
    ├── SKILL.md          # Required: Skill definition
    ├── references/       # Optional: Reference documents
    └── scripts/          # Optional: Executable scripts
```

#### SKILL.md Format

**Frontmatter Fields:**

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | 1-64 chars, lowercase + hyphens, no consecutive hyphens, matches directory name |
| `description` | Yes | 1-1024 chars, trigger-based |
| `license` | No | License name or file reference |
| `metadata` | No | Arbitrary key-value mapping |

**Name Validation:**

| Valid | Invalid | Reason |
|-------|---------|--------|
| `pdf-processing` | `PDF-Processing` | Uppercase not allowed |
| `code-review` | `-code` | Cannot start with hyphen |
| `data-analysis` | `data--analysis` | Consecutive hyphens |

#### Content Organization

Recommended section order:

1. **Overview** - Brief description with external links
2. **When to Use** - Activation scenarios
3. **Quick Reference** - Table with Rule | Good | Avoid
4. **Detailed Rules** - Grouped by category
5. **References** - External documentation

**Progressive Disclosure:** Keep SKILL.md under 500 lines. Move detailed content to `references/`.

---

### Agent Specification

#### Directory Structure

```
agents/
└── <agent-name>/
    └── AGENT.md          # Required: Agent definition
```

#### AGENT.md Format

**Frontmatter Fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Agent identifier (kebab-case) |
| `description` | Yes | When to invoke this agent |

#### Content Organization

Recommended section order:

1. **Purpose** - Agent's role
2. **Value Prop** - When to use
3. **Dependencies** - Related skills
4. **Checklist** - Review items (checkbox format)
5. **Review Process** - Step-by-step process
6. **References** - External links

---

### Marketplace Configuration

The top-level `.claude-plugin/marketplace.json` defines the marketplace:

```json
{
  "name": "zoumo-dotclaude",
  "metadata": {
    "description": "Personal Claude Code plugins marketplace"
  },
  "repository": "https://github.com/zoumo/dotclaude",
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./plugins/plugin-name",
      ...
    }
  ]
}
```

**Important:** The `source` field must be a relative path starting with `./` from the repository root to the plugin directory. Do NOT use `pluginRoot` in metadata - it is not supported and will cause validation errors. Always use relative paths like `./plugins/plugin-name`.

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

---

### Repository Conventions

#### Description Pattern

Use **trigger-based** descriptions - explain *when* to use:

| Good | Avoid |
|------|-------|
| "Use when writing, refactoring Go code" | "Go style guide" |
| "Triggers on PDF, forms, document extraction" | "Helps with PDFs" |

#### Good/Avoid Table Format

```markdown
| Rule | Good | Avoid |
|------|------|-------|
| Package name | `package bytes` | `package byte_utils` |
```

### Git Workflow

- Commit messages follow convention: `docs:`, `feat:`, `fix:`, `refactor:`, `test:`, `chore:`