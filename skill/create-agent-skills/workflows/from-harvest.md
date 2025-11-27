<workflow name="create-from-harvested-knowledge">
<objective>
Transform harvested knowledge output into a properly structured, production-ready skill.
This workflow bridges harvest-sources and skill creation.
</objective>

<trigger>
User has `<harvested_source>` XML output from harvest-sources skill and wants to create a skill from it.
</trigger>

<process>
<phase name="parse-harvest">
<description>Extract and validate harvested knowledge</description>
<actions>
- Read the `<harvested_source>` XML provided
- Extract metadata: source_url, source_type, title
- Validate `<skill_ready>` section exists
- Extract: suggested_skill_name, suggested_description, core_capabilities
- Note any gaps or missing information
</actions>
<output>Parsed harvest data ready for skill creation</output>
</phase>

<phase name="determine-skill-type">
<description>Decide between simple and complex skill structure</description>
<decision_criteria>
Use SIMPLE skill (single SKILL.md) when:
- Single workflow path
- Content fits under 500 lines
- No branching logic needed
- Limited reference material

Use COMPLEX skill (router pattern) when:
- Multiple workflow paths
- Extensive reference material
- Branching based on user intent
- Code snippets or templates needed
</decision_criteria>
<output>Skill type decision with justification</output>
</phase>

<phase name="map-content">
<description>Map harvested sections to skill structure</description>
<mapping>
| Harvested Section | Skill Section | Notes |
|-------------------|---------------|-------|
| summary | objective | Expand with significance |
| key_concepts | context OR references/ | Based on volume |
| patterns | patterns | Include all examples |
| code_snippets | code_reference OR templates/ | Based on reusability |
| implementation_guidance.steps | process | Convert to phases |
| implementation_guidance.verification | success_criteria | Convert to checklist |
| implementation_guidance.troubleshooting | troubleshooting | Keep as-is |
| resources | resources OR references/ | Based on volume |
| skill_ready.suggested_workflow | process OR workflows/ | Based on complexity |
</mapping>
<output>Content mapping plan</output>
</phase>

<phase name="generate-yaml">
<description>Create valid YAML frontmatter</description>
<actions>
- Use suggested_skill_name (convert to lowercase-hyphenated)
- Validate name ≤ 64 characters
- Use suggested_description, enhance if needed
- Ensure description includes WHAT and WHEN
- Validate description ≤ 1024 characters
</actions>
<template>
```yaml
---
name: {skill-name}
description: {Description that includes what it does AND when to use it. Use third person.}
---
```
</template>
<output>Valid YAML frontmatter</output>
</phase>

<phase name="build-structure">
<description>Create skill file structure</description>
<actions>
For SIMPLE skill:
- Create single SKILL.md with all content

For COMPLEX skill:
- Create SKILL.md with router and essential principles
- Create workflows/ for each major process
- Create references/ for extensive documentation
- Create templates/ for reusable outputs
</actions>
<output>File structure created</output>
</phase>

<phase name="populate-content">
<description>Fill in skill content from harvested knowledge</description>
<actions>
- Write objective from summary (expand significance)
- Create intake or quick_start section
- Convert implementation steps to process phases
- Transfer patterns with examples
- Include code snippets with context
- Build success_criteria from verification steps
- Add troubleshooting if available
</actions>
<output>Skill content populated</output>
</phase>

<phase name="validate-structure">
<description>Verify skill meets all requirements</description>
<checklist>
- [ ] YAML frontmatter valid
- [ ] Name matches directory (if complex skill)
- [ ] No markdown headings in XML body
- [ ] Required sections present (objective, quick_start/intake, success_criteria)
- [ ] All XML tags closed
- [ ] SKILL.md under 500 lines
- [ ] References one level deep
</checklist>
<output>Validation report</output>
</phase>

<phase name="add-attribution">
<description>Document source attribution</description>
<actions>
- Add `<attribution>` section at end of SKILL.md
- Include source URL, harvest date, original author
- Note any modifications or enhancements made
</actions>
<output>Attribution added</output>
</phase>
</process>

<integration_with_harvest>
Expected input format from harvest-sources:

```xml
<harvested_source>
  <metadata>...</metadata>
  <summary>...</summary>
  <key_concepts>...</key_concepts>
  <patterns>...</patterns>
  <code_snippets>...</code_snippets>
  <implementation_guidance>...</implementation_guidance>
  <skill_ready>
    <suggested_skill_name>...</suggested_skill_name>
    <suggested_description>...</suggested_description>
    <core_capabilities>...</core_capabilities>
    <suggested_workflow>...</suggested_workflow>
  </skill_ready>
</harvested_source>
```

The `<skill_ready>` section is specifically designed for this workflow.
</integration_with_harvest>

<success_markers>
- Harvested knowledge fully parsed
- Appropriate skill type selected
- Content correctly mapped to skill structure
- Valid YAML frontmatter generated
- All required sections populated
- Structure validated
- Attribution documented
</success_markers>
</workflow>
