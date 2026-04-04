# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is an AI agent skills repository. Each skill is a self-contained knowledge base under `skills/<name>/` that agents can load to gain expertise on a topic.

## Repository Structure

- `skills/<name>/SKILL.md` — Skill entry point with frontmatter (name, description, metadata) and the main reference content
- `skills/<name>/references/` — Modular deep-dive files that the main SKILL.md links to for detailed syntax, edge cases, and workflows
- `testdata/` — Source material and reference data used during skill authoring (gitignored)

## Skill Authoring Conventions

- SKILL.md uses YAML frontmatter with `name`, `description`, and `metadata` (author, version) fields
- The `description` field doubles as the trigger — it lists keywords and scenarios where the skill should activate
- Main SKILL.md provides a comprehensive quick reference; detailed content is split into `references/*.md` files
- All content is written in English regardless of the user's language
- Cross-references between SKILL.md and reference files use relative paths

## Git Conventions

- Commit messages use conventional commit prefixes: `docs:`, `refactor:`, `init`
- Chinese commit messages are also used — follow the style of recent commits in the branch
