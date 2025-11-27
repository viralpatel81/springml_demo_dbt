# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Does

A skill creation pipeline that transforms external sources (YouTube videos, GitHub repos, blog articles, n8n workflows) into production-ready Claude Code skills.

## Pipeline Flow

```
Source URL → harvest-sources → create-agent-skills → audit-skill → heal-skill
```

1. **harvest-sources**: Extracts knowledge from URLs, outputs `<harvested_source>` XML
2. **create-agent-skills**: Transforms harvested knowledge into skill files
3. **audit-skill**: Validates compliance (invokes skill-auditor agent)
4. **heal-skill**: Fixes skills based on execution feedback
5. **debug-like-expert**: Deep debugging for complex failures

## Architecture

Skills follow a router pattern with progressive disclosure:

```
skill-name/
├── SKILL.md              # Router + essential principles (≤500 lines)
├── workflows/            # Step-by-step procedures
├── references/           # Domain knowledge
└── templates/            # Output structures
```

**Key constraint**: SKILL.md uses pure XML structure—no markdown headings (`#`, `##`) in the body. Use semantic tags like `<objective>`, `<process>`, `<success_criteria>`.

## Skill File Requirements

YAML frontmatter:
- `name`: lowercase-hyphenated, ≤64 chars, matches directory name
- `description`: ≤1024 chars, third person, includes WHAT it does AND WHEN to use it

Required XML sections:
- `<objective>` - Purpose and significance
- `<intake>` or `<quick_start>` - Entry point
- `<success_criteria>` - Verification checklist

## Data Flow Between Skills

harvest-sources outputs a `<skill_ready>` section containing:
- `suggested_skill_name`
- `suggested_description`
- `core_capabilities`
- `suggested_workflow`

create-agent-skills consumes this via `workflows/from-harvest.md` to generate compliant skill files.

## Ignored Content

The dbt project files (models/, data/, tests/, etc.) are legacy and excluded via .gitignore.
