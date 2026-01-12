<template name="skill-draft">
<description>
Template for drafting a new skill directly from harvested knowledge.
Use this when the harvested source contains enough information to create
a complete skill without additional research.
</description>

<usage>
1. Complete the harvested-knowledge template first
2. Use the skill_ready section to populate this template
3. Expand with details from other harvested sections
4. Review against skill best practices before finalizing
</usage>

<template_content>
```markdown
---
name: {skill-name-from-harvest}
description: {description-from-harvest - ensure it includes what AND when}
---

<objective>
{Expand the summary from harvested knowledge into a clear objective statement.
1-3 paragraphs explaining what this skill does and why it matters.}
</objective>

<quick_start>
{Immediate actionable guidance - the fastest path to using this skill.
Derived from implementation_guidance.steps in harvested knowledge.}
</quick_start>

<process>
{Convert suggested_workflow from harvested knowledge into detailed steps}

<step name="{step-name}">
{Step description and actions}
</step>

<!-- Additional steps -->
</process>

<patterns>
{Import relevant patterns from harvested knowledge}

<pattern name="{pattern-name}">
<when>{When to use this pattern}</when>
<how>{How to implement}</how>
<example>{Example from harvested source}</example>
</pattern>

<!-- Additional patterns -->
</patterns>

<code_reference>
{Import key code snippets from harvested knowledge}

<snippet name="{name}" language="{lang}">
<purpose>{purpose}</purpose>
<code>
{code}
</code>
</snippet>

<!-- Additional snippets -->
</code_reference>

<configuration>
{Import relevant configurations from harvested knowledge}
</configuration>

<success_criteria>
{Derive from implementation_guidance.verification in harvested knowledge}

- [ ] {Criterion 1}
- [ ] {Criterion 2}
- [ ] {Criterion 3}
</success_criteria>

<troubleshooting>
{Import from implementation_guidance.troubleshooting in harvested knowledge}

<issue problem="{problem}">
<solution>{solution}</solution>
</issue>

<!-- Additional issues -->
</troubleshooting>

<resources>
{Import from resources section of harvested knowledge}

| Resource | URL | Purpose |
|----------|-----|---------|
| {name} | {url} | {description} |
</resources>

<attribution>
Skill derived from: {source_url from harvested knowledge}
Harvested: {harvested_date}
Original author: {author if known}
</attribution>
```
</template_content>

<mapping_guide>
How to map harvested knowledge sections to skill sections:

| Harvested Section | Skill Section |
|-------------------|---------------|
| summary | objective |
| implementation_guidance.steps | quick_start, process |
| patterns | patterns |
| code_snippets | code_reference |
| configurations | configuration |
| implementation_guidance.verification | success_criteria |
| implementation_guidance.troubleshooting | troubleshooting |
| resources | resources |
| metadata | attribution |
</mapping_guide>

<quality_checklist>
Before finalizing skill draft:

- [ ] Name follows verb-noun pattern
- [ ] Description includes what AND when
- [ ] Objective is clear and compelling
- [ ] Quick_start provides immediate value
- [ ] Process steps are actionable
- [ ] Success criteria are measurable
- [ ] Attribution is complete
- [ ] No markdown headings in XML body
- [ ] All XML tags properly closed
</quality_checklist>
</template>
