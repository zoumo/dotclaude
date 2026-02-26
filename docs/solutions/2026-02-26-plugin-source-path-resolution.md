# Solution: Plugin Source Path Resolution with pluginRoot

**Date:** 2026-02-26
**Status:** Resolved

## Problem

Plugin installation failed with error:

```
Failed to install plugin "chinese-style-guide@zoumo-dotclaude": Source path does not exist: /Users/jim/.claude/plugins/marketplaces/zoumo-dotclaude/chinese-style-guide
```

The `plugins/` directory was missing from the resolved path.

## Root Cause

Incorrect `source` field configuration when `pluginRoot` is set in `marketplace.json`.

**Incorrect configuration:**
```json
{
  "metadata": { "pluginRoot": "./plugins" },
  "plugins": [
    { "source": "./chinese-style-guide" }  // WRONG: ./ prefix when pluginRoot is set
  ]
}
```

**Why it failed:** When `pluginRoot` is defined, the `source` field should be a simple directory name (no `./` prefix). The `./` prefix caused the path resolution to skip the `pluginRoot` directory.

## Solution

Remove the `./` prefix from `source` fields when `pluginRoot` is configured:

```json
{
  "metadata": { "pluginRoot": "./plugins" },
  "plugins": [
    { "source": "chinese-style-guide" }  // CORRECT: no ./ prefix
  ]
}
```

**Path resolution:**
- `pluginRoot` + `source` = `./plugins/chinese-style-guide`

## Key Insight

According to Claude Code plugin documentation:

> `"./plugins"` lets you write `"source": "formatter"` instead of `"source": "./plugins/formatter"`

The `pluginRoot` field acts as a base directory that is prepended to all relative source paths. When you include `./` in the source, it may be interpreted differently by the path resolver.

## Prevention

1. **Rule:** When `pluginRoot` is set, `source` fields should be simple directory names without `./` prefix
2. **Validation:** Source field should match `^[a-z][a-z0-9-]*[a-z0-9]$` (kebab-case)
3. **Pattern:** Use `source: "plugin-name"` not `source: "./plugin-name"`

## Files Changed

- `.claude-plugin/marketplace.json` - Removed `./` prefix from both plugin source fields

## Verification

After fix:
```bash
/plugin marketplace update zoumo-dotclaude
/plugin install chinese-style-guide@zoumo-dotclaude
```

## References

- [Claude Code Plugin Marketplaces Documentation](https://code.claude.com/docs/en/plugin-marketplaces)
- Commit: `62c2ec1` - fix: remove ./ prefix from plugin source paths