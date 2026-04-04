# Agent Skills

[中文文档](README_CN.md)

A collection of AI agent skills — self-contained knowledge bases that can be loaded by AI coding assistants (e.g., Claude Code, Copilot CLI, Cursor) to gain expertise on specific topics.

## What is a Skill?

A skill is a structured Markdown document with YAML frontmatter that serves as a comprehensive reference on a topic. AI agents load skills to provide more accurate, context-aware assistance without needing to search documentation in real time.

Each skill lives in its own directory under `skills/<name>/` and consists of:

- **`SKILL.md`** — Entry point with frontmatter metadata and a quick-reference guide
- **`references/`** — Modular deep-dive files for detailed syntax, edge cases, and workflows

## Available Skills

| Skill    | Description                                                                                                                                                |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [psql][] | PostgreSQL interactive terminal reference — meta-commands, CLI options, formatting, data import/export, scripting, and advanced workflows                  |

[psql]: skills/psql/SKILL.md

## Installation

### npx skills (Recommended)

The [skills CLI](https://github.com/vercel-labs/skills) is a universal installer that supports Claude Code, Cursor, Copilot, Codex, and 40+ other agents.

```bash
# Install all skills from this repo
npx skills add chaunsin/agent

# Install a specific skill
npx skills add chaunsin/agent --skill psql

# List available skills before installing
npx skills add chaunsin/agent --list

# Install globally (available across all projects)
npx skills add chaunsin/agent -g

# Install to specific agents
npx skills add chaunsin/agent -a claude-code -a cursor

# Check if any installed skills have updates
npx skills check

# Update all installed skills to the latest version
npx skills update

# List all installed skills (project + global)
npx skills list

# Remove a specific skill
npx skills remove psql
```

### Manual

```bash
# Copy a skill into the Claude Code skills directory
cp -r skills/<skill-name> ~/.claude/skills/
```

## Usage

Once installed, skills activate automatically based on their trigger keywords (defined in the `description` frontmatter field). No manual configuration is needed — when you ask a question related to a skill's domain, the agent will load the relevant reference.

For example, with the psql skill installed, asking "how to list all tables in psql" or "psql \d commands" will trigger the skill.

## Repository Structure

```text
agent/
├── skills/                  # Skill directories
│   └── psql/                # PostgreSQL interactive terminal
│       ├── SKILL.md         # Skill entry point
│       └── references/      # Detailed reference files
├── CLAUDE.md                # Claude Code workspace instructions
├── LICENSE                  # Apache 2.0
└── README.md                # This file
```

## License

[Apache License 2.0](LICENSE)
