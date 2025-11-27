---
name: create-agent-skills
description: Expert guidance for creating, writing, building, and refining Claude Code Skills. Use when you need to build a new skill from harvested knowledge or from scratch.
---

<objective>
This skill guides you through creating properly structured Claude Code skills. It handles both task-execution skills (standard operations) and domain expertise skills (comprehensive knowledge bases). The skill ensures all output follows best practices: pure XML structure, proper YAML frontmatter, progressive disclosure, and router patterns for complex skills.

When paired with harvest-sources, this skill transforms extracted knowledge into production-ready skills.
</objective>

<essential_principles>
<principle name="skills-are-prompts">
Skills are prompts with structure. Apply standard prompting best practices.
Use clear instructions, provide examples, anticipate edge cases.
</principle>

<principle name="skill-md-always-loads">
SKILL.md is guaranteed to load. Put essential principles and routing here.
Everything else can be in subdirectories for progressive disclosure.
</principle>

<principle name="router-pattern">
Complex skills use: SKILL.md (router) → workflows/ → references/ → templates/
Keep SKILL.md under 500 lines. Split details into appropriate subdirectories.
</principle>

<principle name="pure-xml-structure">
Remove ALL markdown headings from skill body. Use semantic XML tags:
- `<objective>` - Purpose and significance
- `<process>` - Step-by-step workflow
- `<success_criteria>` - Verification checklist
</principle>

<principle name="progressive-disclosure">
Load information only when needed. Don't front-load everything.
Route to specific workflows based on user intent.
</principle>
</essential_principles>

<intake>
What would you like to do?

1. **Create new skill** - Build a skill from scratch or from harvested knowledge
2. **Modify existing skill** - Add features, fix issues, or refactor
3. **Add component** - Add workflow, reference, or template to existing skill
4. **Get guidance** - Learn about skill architecture and best practices

If you have harvested knowledge ready, select option 1 and provide the harvested output.
</intake>

<routing>
| User Intent | Workflow | Progressive Disclosure |
|-------------|----------|------------------------|
| Create from harvested knowledge | workflows/from-harvest.md | Load skill-draft template |
| Create from scratch | workflows/new-skill.md | Load intake questions |
| Create domain expertise | workflows/domain-expertise.md | Load expertise patterns |
| Modify existing | workflows/modify-skill.md | Analyze current structure |
| Add component | workflows/add-component.md | Component templates |
| Architecture guidance | references/architecture.md | Best practices |
</routing>

<quick_reference>
<simple_skill_pattern>
```
skill-name/
└── SKILL.md          # Everything in one file (under 500 lines)
```

Use when: Single workflow, no complex branching, limited reference material.
</simple_skill_pattern>

<complex_skill_pattern>
```
skill-name/
├── SKILL.md          # Router + essential principles
├── workflows/        # Step-by-step procedures
├── references/       # Domain knowledge, guides
├── templates/        # Output structures
└── scripts/          # Executable code (optional)
```

Use when: Multiple workflows, extensive references, or reusable templates.
</complex_skill_pattern>
</quick_reference>

<yaml_requirements>
<field name="name">
- Maximum 64 characters
- Lowercase with hyphens only
- Must match directory name
- Verb-noun pattern preferred: `create-reports`, `manage-users`
- Avoid: "helper", "tools", "utils", reserved words
</field>

<field name="description">
- Maximum 1,024 characters
- Third person: "Creates..." not "Create..."
- Include WHAT it does AND WHEN to use it
- Add trigger phrases for discoverability
</field>
</yaml_requirements>

<required_sections>
Every skill MUST have:

1. `<objective>` - What the skill does and why it matters (1-3 paragraphs)
2. `<quick_start>` or `<intake>` - Immediate actionable guidance
3. `<success_criteria>` - How to verify the skill worked

Conditional sections (based on complexity):
- `<process>` - Detailed workflow steps
- `<routing>` - Intent-to-workflow mapping
- `<patterns>` - Reusable patterns with examples
- `<anti_patterns>` - What to avoid
- `<troubleshooting>` - Common issues and solutions
</required_sections>

<from_harvest_workflow>
When creating from harvested knowledge:

1. **Read harvested output** - Parse the `<harvested_source>` XML
2. **Extract skill_ready section** - Use suggested name, description, workflow
3. **Map content to skill structure**:
   - summary → objective
   - key_concepts → context or reference
   - patterns → patterns section
   - code_snippets → code_reference
   - implementation_guidance → process
4. **Apply skill template** - Use templates/skill-scaffold.md
5. **Validate structure** - Check YAML, XML, required sections
6. **Test with example** - Dry run the skill workflow
</from_harvest_workflow>

<workflow_index>
| Workflow | Purpose |
|----------|---------|
| workflows/from-harvest.md | Create skill from harvested knowledge |
| workflows/new-skill.md | Create skill from scratch |
| workflows/domain-expertise.md | Create comprehensive knowledge base |
| workflows/modify-skill.md | Modify existing skill |
| workflows/add-component.md | Add workflow/reference/template |
</workflow_index>

<reference_index>
| Reference | Purpose |
|-----------|---------|
| references/architecture.md | Skill architecture deep dive |
| references/xml-tags.md | Complete XML tag reference |
| references/common-patterns.md | Reusable skill patterns |
</reference_index>

<template_index>
| Template | Purpose |
|----------|---------|
| templates/skill-scaffold.md | Basic skill structure |
| templates/complex-skill-scaffold.md | Router pattern structure |
| templates/workflow-template.md | Workflow file template |
</template_index>

<success_criteria>
Skill creation is complete when:
- [ ] YAML frontmatter is valid (name ≤64 chars, description ≤1024 chars)
- [ ] Name matches directory name exactly
- [ ] Pure XML structure (no markdown headings in body)
- [ ] Required sections present: objective, quick_start/intake, success_criteria
- [ ] Progressive disclosure applied (SKILL.md under 500 lines)
- [ ] All XML tags properly closed
- [ ] Skill can be invoked and produces expected behavior
</success_criteria>

<anti_patterns>
- DO NOT use markdown headings (#, ##) in skill body
- DO NOT exceed 500 lines in SKILL.md
- DO NOT create monolithic skills - split into workflows
- DO NOT use vague names like "helper" or "utils"
- DO NOT forget the "when to use" in description
- DO NOT leave XML tags unclosed
</anti_patterns>
