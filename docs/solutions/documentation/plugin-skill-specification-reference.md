---
title: "Plugin and Skill Specification Reference Documentation"
slug: "plugin-skill-specification-reference"
date: "2026-02-13"
category: "documentation"
component: "CLAUDE.md"
tags:
  - plugin-development
  - documentation
  - claude-code
  - marketplace
  - skill-specification
  - agent-specification
---

# Plugin and Skill Specification Reference Documentation

## Problem

The CLAUDE.md file had minimal documentation for plugin/skill creation:
- Only showed directory structure without field explanations
- Skill and Agent specifications mixed together
- Missing links to official specifications
- No field constraints documented

This made it difficult for developers to understand complete requirements when creating new plugins or skills.

## Investigation

The following sources were researched:

1. **Claude Code Plugins Reference** (https://code.claude.com/docs/en/plugins-reference)
   - Complete plugin manifest schema
   - Required and optional fields
   - Component path specifications

2. **Agent Skills Specification** (https://agentskills.io/specification)
   - SKILL.md frontmatter format
   - Name constraints
   - Progressive disclosure principle

3. **Existing Plugin Patterns**
   - `plugins/go-style-guide/` - Multi-skill plugin with agent
   - `plugins/chinese-style-guide/` - Simple single-skill plugin

## Solution

### Restructured CLAUDE.md

Split the Plugin Structure section into dedicated specification sections:

#### Plugin Specification Section

Added comprehensive `plugin.json` field tables:

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

#### Skill Specification Section

Added frontmatter constraints with validation examples:

| Field | Required | Constraints |
|-------|----------|-------------|
| `name` | Yes | 1-64 chars, lowercase + hyphens, no consecutive hyphens |
| `description` | Yes | 1-1024 chars, trigger-based |

Name validation table:

| Valid | Invalid | Reason |
|-------|---------|--------|
| `pdf-processing` | `PDF-Processing` | Uppercase not allowed |
| `code-review` | `-code` | Cannot start with hyphen |

Content organization recommendations (Overview → When to Use → Quick Reference → Details → References).

#### Agent Specification Section

Added AGENT.md format and content organization:
- Frontmatter fields (`name`, `description`)
- Recommended section order (Purpose, Value Prop, Dependencies, Checklist, Process, References)

#### Repository Conventions Section

Added trigger-based description pattern with Good/Avoid examples.

### Key Files Modified

- `CLAUDE.md` - Expanded from 80 lines to 190 lines

## Prevention Strategies

### Best Practices

1. **Link to official specs** - Always include source links for external specifications
2. **Use tables for fields** - Structured tables make requirements clear
3. **Show validation examples** - Valid/invalid examples teach constraints
4. **Document content organization** - Help developers structure their work

### Documentation Completeness Checklist

- [ ] All fields have type and description
- [ ] Required vs optional clearly marked
- [ ] Examples provided for each field
- [ ] Official specification links included
- [ ] Validation rules documented
- [ ] Content organization guidance provided

### When to Update

- When official specifications change
- When new field types are added
- When new component types are introduced

## Related Documentation

- `docs/solutions/plugin-development/go-style-guide-plugin.md` - Go plugin implementation example
- `docs/brainstorms/2026-02-13-plugin-skill-spec-reference-brainstorm.md` - Original brainstorm
- `docs/plans/2026-02-13-docs-plugin-skill-spec-reference-plan.md` - Implementation plan

## References

- [Claude Code Plugins Reference](https://code.claude.com/docs/en/plugins-reference)
- [Agent Skills Specification](https://agentskills.io/specification)