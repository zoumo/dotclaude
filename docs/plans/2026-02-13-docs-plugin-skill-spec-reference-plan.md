---
title: Plugin & Skill Specification Reference Documentation
type: docs
date: 2026-02-13
updated: 2026-02-13
---

# Plugin & Skill Specification Reference Documentation

## Overview

重构 CLAUDE.md，将当前的 Plugin Structure 章节拆分为多个详细章节，分别说明 Plugin、Skill、Agent 的完整结构和规范。

## Problem Statement

当前 CLAUDE.md 的 Plugin Structure 章节过于简略：
1. 仅展示目录结构，缺少字段说明
2. Skill 和 Agent 混在一起说明
3. 缺少官方规范的链接和关键约束

## Proposed Solution

将内容重构为以下章节：

```
## Plugin Development Reference

### Plugin Specification
- 目录结构
- plugin.json 字段说明（表格）
- 规范链接

### Skill Specification
- 目录结构
- SKILL.md frontmatter 字段说明
- 内容组织建议
- 规范链接

### Agent Specification
- 目录结构
- AGENT.md frontmatter 字段说明
- 内容组织建议

### Marketplace Configuration
- marketplace.json 结构说明

### Repository Conventions
- 描述模式（trigger-based）
- Good/Avoid 表格格式
```

## Technical Approach

### File to Modify

`CLAUDE.md` - 重构 lines 12-67 (Plugin Structure 章节)

### Content Structure

#### 1. Plugin Specification Section

```markdown
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
```

#### 2. Skill Specification Section

```markdown
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
```

#### 3. Agent Specification Section

```markdown
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
```

#### 4. Repository Conventions Section

```markdown
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
```

## Acceptance Criteria

- [x] Plugin Specification 章节包含目录结构和字段表格
- [x] Skill Specification 章节包含 frontmatter 约束和内容组织建议
- [x] Agent Specification 章节包含结构和内容建议
- [x] Marketplace Configuration 保持独立章节
- [x] Repository Conventions 包含描述模式和表格格式
- [x] 所有规范包含官方来源链接

## References

- [Claude Code Plugins Reference](https://code.claude.com/docs/en/plugins-reference)
- [Agent Skills Specification](https://agentskills.io/specification)