# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

This is TJ's personal Obsidian vault containing notes about work, personal projects, family, and reference material.

## Work Context

"Work" means **MailRoute** - an email filtering/security service.

- Server names like `001.lax`, `s02.mia`, `es01.lax` = MailRoute infrastructure
- References to `controlpanel`, `webadmin`, `amavisd`, `postfix`, `dovecot` = work systems
- Datacenters: LAX (Los Angeles), MIA (Miami), SAN (San Diego)
- Providers: Psychz, Quadranet, Cogent, Hivelocity, Ionos

## TODO System

TODO files to be consolidated into:
- `TODO - Work.md` - MailRoute/infrastructure tasks
- `TODO - Personal.md` - Family, finance, personal tasks

Format: Use `- [ ]` checkbox format for actionable items.

When asked to "remind me to X" or "add a TODO for X":
- Work-related (servers, mailroute, infrastructure) → `TODO - Work.md`
- Personal → `TODO - Personal.md`

## Sensitive Data

- `Private/` folder contains sensitive credentials and is excluded via `.claudeignore`
- Never commit passwords, keys, or credentials to the repo
- If you find sensitive data in notes, flag it for the user

## File Conventions

- Notes are markdown files in the root directory
- Attachments go in `Attachments/`
- Use descriptive filenames (no need for date prefixes unless relevant)

## Git Commits

When committing:
- Do NOT add "Generated with Claude Code" footers
- Do NOT add "Co-Authored-By" lines
- Keep commit messages simple and descriptive

## Common Tasks

**Scanning TODOs**: Read all `TODO*.md` and `TODO*.txt` files to get full picture of outstanding tasks.

**Finding work notes**: Search for MailRoute, server names, or infrastructure terms.

**Finding personal notes**: Look for family names (Ian, Bridget, Jenette, Denise) or personal topics.
