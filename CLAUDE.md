# Skill Creation Pipeline

This repository contains a complete pipeline for harvesting knowledge from external sources and transforming it into Claude Code skills.

## Purpose

Convert external sources (YouTube videos, GitHub repos, blog articles, n8n workflows) into production-ready Claude Code skills through a systematic pipeline.

## Pipeline Overview

```
Source → harvest-sources → create-agent-skills → audit-skill → test → heal-skill
```

## Directory Structure

```
skill/
├── harvest-sources/        # Extract knowledge from external sources
├── create-agent-skills/    # Build skills from harvested knowledge
├── debug-like-expert/      # Methodical debugging for complex issues
├── commands/
│   ├── audit-skill.md      # Validate skill compliance
│   └── heal-skill.md       # Fix skills based on execution feedback
└── agents/
    └── skill-auditor.md    # Subagent for compliance audits
```

## Quick Start

1. **Harvest a source**: Provide a URL (GitHub, YouTube, blog, n8n) to `harvest-sources`
2. **Create skill**: Use `create-agent-skills` with the harvested output
3. **Audit**: Run `audit-skill` to validate compliance
4. **Test**: Invoke the skill and observe behavior
5. **Heal**: Use `heal-skill` if issues are found

## Skill Best Practices

- YAML frontmatter: name ≤64 chars, description ≤1024 chars
- Pure XML structure (no markdown headings in skill body)
- Required sections: `<objective>`, `<quick_start>` or `<intake>`, `<success_criteria>`
- Keep SKILL.md under 500 lines (use progressive disclosure)

## Ignored Files

The dbt project files in this repo are legacy and not relevant to skill development. They are excluded via .gitignore.
