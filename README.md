# Claude Code Plugin Marketplace

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)

A collection of plugins for [Claude Code](https://claude.ai/code) that extend its capabilities with style guides, coding standards, and best practices.

## Available Plugins

### [chinese-style-guide](./plugins/chinese-style-guide/)

Chinese style guide for consistent formatting and spacing in Chinese text output.

**Features:**
- Proper spacing between Chinese and English text
- Correct punctuation usage (full-width vs half-width)
- Number and unit formatting guidelines

### [go-style-guide](./plugins/go-style-guide/)

Go style guide with coding standards, best practices, and testing patterns.

**Features:**
- **golang-principle** skill: Go coding standards (naming, error handling, API design)
- **golang-testing** skill: Testing patterns (table-driven tests, testify usage)
- **go-reviewer** agent: Automated Go code review

## Installation

### Add the Marketplace

```bash
/plugin marketplace add zoumo/dotclaude
```

### Install a Plugin

```bash
# Install Chinese Style Guide
/plugin install chinese-style-guide@zoumo-dotclaude

# Install Go Style Guide
/plugin install go-style-guide@zoumo-dotclaude
```

## Usage

Skills are invoked by name directly, without any plugin prefix:

```bash
/golang-principle    # Get Go coding standards guidance
/golang-testing      # Get Go testing patterns guidance
```

## Plugin Structure

```
plugins/
├── <plugin-name>/
│   ├── .claude-plugin/
│   │   └── plugin.json           # Plugin metadata
│   ├── README.md                  # Plugin documentation
│   ├── skills/                    # Skills directory
│   │   └── <skill-name>/
│   │       └── SKILL.md           # Skill definition
│   └── agents/                    # Agents directory (optional)
│       └── <agent-name>/
│           └── AGENT.md           # Agent definition
```

## Development

See [CLAUDE.md](./CLAUDE.md) for development guidelines and conventions.

## License

[Apache 2.0](LICENSE)