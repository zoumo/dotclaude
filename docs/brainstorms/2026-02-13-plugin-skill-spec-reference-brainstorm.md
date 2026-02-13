# Plugin & Skill Specification Reference Documentation

**Date:** 2026-02-13
**Status:** Brainstorm Complete

---

## What We're Building

A specification reference section in CLAUDE.md that documents both the official Claude Code plugin manifest schema and the Agent Skills specification from agentskills.io. This will guide the creation and optimization of plugins and skills in this repository.

The documentation will use a **spec-first organization** that clearly separates official requirements from repository conventions, making it easy to understand what's mandatory vs what's preferred.

---

## Why This Approach

**Spec-First Organization** was chosen because:

1. **Clear attribution** - Developers can distinguish spec requirements from repo conventions
2. **Easier updates** - When official specs change, the affected sections are clearly identified
3. **Compliance visibility** - Easy to check if a plugin/skill meets official requirements
4. **Learning-friendly** - New contributors understand both layers of requirements

---

## Key Decisions

### 1. Document Structure

```
## Plugin Development Reference

### Official Specifications
#### Plugin Manifest Schema (Claude Code)
- Source: https://code.claude.com/docs/en/plugins-reference
- Required fields, metadata fields, component paths

#### Agent Skills Specification (agentskills.io)
- Source: https://agentskills.io/specification
- SKILL.md frontmatter, name constraints, progressive disclosure

### Repository Conventions
- Description patterns (trigger-based)
- Good/Avoid table format
- Agent checklist structure
- Code example standards
```

### 2. Scope Coverage

| Aspect | Included | Source |
|--------|----------|--------|
| Plugin manifest schema | Yes | Claude Code docs |
| SKILL.md specification | Yes | agentskills.io |
| AGENT.md format | Yes | Repo patterns + best practices |
| Hooks/MCP/LSP | No | Advanced features, future scope |

### 3. Content Format

- **Tables** for field specifications (field, type, required, description)
- **Code blocks** for examples (JSON frontmatter, markdown structure)
- **Quick reference** for common patterns
- **Links** to original specification sources

### 4. Location

Update existing `CLAUDE.md` by expanding the "Plugin Development Documentation" section into a comprehensive reference.

---

## Open Questions

None remaining. All scope decisions have been resolved:

- ~~Output type~~ → Reference document
- ~~Coverage scope~~ → Plugin manifest, Skill spec, Agent definitions
- ~~Location~~ → Update CLAUDE.md
- ~~Approach~~ → Spec-first documentation

---

## Next Steps

1. Run `/workflows:plan` to implement the updated CLAUDE.md
2. Apply spec-first structure to the documentation
3. Include complete field tables from both specifications
4. Add repo conventions with examples from existing plugins