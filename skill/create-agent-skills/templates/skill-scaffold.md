<template name="skill-scaffold">
<description>
Basic scaffold for creating a new skill. Copy and fill in the sections.
</description>

<simple_skill_template>
```markdown
---
name: {skill-name}
description: {What it does in third person. When to use it. Include trigger phrases.}
---

<objective>
{1-3 paragraphs explaining what this skill accomplishes and why it matters.
Be specific about the value it provides.}
</objective>

<quick_start>
{Immediate actionable guidance. The fastest path to using this skill.
What does the user need to provide? What happens first?}
</quick_start>

<process>
<step name="{step-1-name}">
{Description of what happens in this step}
<actions>
- Action 1
- Action 2
</actions>
</step>

<step name="{step-2-name}">
{Description of what happens in this step}
<actions>
- Action 1
- Action 2
</actions>
</step>

<!-- Add more steps as needed -->
</process>

<success_criteria>
{Skill is complete when:}
- [ ] {Measurable criterion 1}
- [ ] {Measurable criterion 2}
- [ ] {Measurable criterion 3}
</success_criteria>

<troubleshooting>
<issue problem="{common problem}">
<solution>{How to resolve it}</solution>
</issue>
</troubleshooting>
```
</simple_skill_template>

<complex_skill_template>
```markdown
---
name: {skill-name}
description: {What it does in third person. When to use it. Include trigger phrases.}
---

<objective>
{1-3 paragraphs explaining what this skill accomplishes and why it matters.}
</objective>

<essential_principles>
<principle name="{principle-1}">
{Core principle that guides this skill}
</principle>
<principle name="{principle-2}">
{Another core principle}
</principle>
</essential_principles>

<intake>
{Present options to the user:}

1. **{Option 1}** - {Description}
2. **{Option 2}** - {Description}
3. **{Option 3}** - {Description}
</intake>

<routing>
| User Intent | Workflow | Notes |
|-------------|----------|-------|
| {intent 1} | workflows/{workflow-1}.md | {when to use} |
| {intent 2} | workflows/{workflow-2}.md | {when to use} |
</routing>

<quick_reference>
{Essential information that should always be visible}
</quick_reference>

<workflow_index>
| Workflow | Purpose |
|----------|---------|
| workflows/{name}.md | {purpose} |
</workflow_index>

<success_criteria>
- [ ] {Criterion 1}
- [ ] {Criterion 2}
- [ ] {Criterion 3}
</success_criteria>
```
</complex_skill_template>

<workflow_file_template>
```markdown
<workflow name="{workflow-name}">
<objective>
{What this specific workflow accomplishes}
</objective>

<trigger>
{When this workflow is activated}
</trigger>

<process>
<phase name="{phase-1}">
<description>{What happens}</description>
<actions>
- Action 1
- Action 2
</actions>
<output>{What this phase produces}</output>
</phase>

<!-- More phases -->
</process>

<success_markers>
- {Marker 1}
- {Marker 2}
</success_markers>
</workflow>
```
</workflow_file_template>
</template>
