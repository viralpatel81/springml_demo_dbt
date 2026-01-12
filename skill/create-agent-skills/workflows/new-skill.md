<workflow name="create-new-skill">
<objective>
Create a new skill from scratch through guided discovery and structured development.
</objective>

<trigger>
User wants to create a skill but doesn't have harvested knowledge - starting from an idea or requirement.
</trigger>

<process>
<phase name="discovery">
<description>Understand what the skill should do</description>
<questions>
1. **What task or problem does this skill solve?**
   - Be specific about the outcome

2. **When should someone use this skill?**
   - What triggers indicate this skill is needed?

3. **What inputs does it need?**
   - User-provided data, files, context?

4. **What outputs does it produce?**
   - Files, decisions, transformed data?

5. **Are there multiple paths or workflows?**
   - Different modes, options, or branches?

6. **What tools or capabilities does it use?**
   - WebFetch, Bash, Read/Write, Task agents?
</questions>
<output>Skill requirements document</output>
</phase>

<phase name="naming">
<description>Determine skill name and description</description>
<guidelines>
Name pattern: verb-noun
- Good: `generate-reports`, `analyze-code`, `deploy-services`
- Bad: `report-helper`, `code-tools`, `my-skill`

Description must include:
- WHAT it does (functionality)
- WHEN to use it (triggers)
</guidelines>
<output>Name and description</output>
</phase>

<phase name="structure-decision">
<description>Choose simple or complex structure</description>
<decision_tree>
Q: Multiple distinct workflows or modes?
├─ YES → Complex (router pattern)
└─ NO → Q: More than 500 lines of content?
         ├─ YES → Complex (split into references)
         └─ NO → Simple (single SKILL.md)
</decision_tree>
<output>Structure decision</output>
</phase>

<phase name="outline">
<description>Create skill outline</description>
<simple_outline>
```
SKILL.md
├── YAML frontmatter
├── <objective>
├── <quick_start>
├── <process>
│   └── <step> (for each major step)
├── <success_criteria>
└── <troubleshooting> (optional)
```
</simple_outline>

<complex_outline>
```
skill-name/
├── SKILL.md
│   ├── YAML frontmatter
│   ├── <objective>
│   ├── <essential_principles>
│   ├── <intake>
│   ├── <routing>
│   ├── <quick_reference>
│   └── <success_criteria>
├── workflows/
│   └── {workflow-name}.md (for each path)
├── references/
│   └── {reference-name}.md (for detailed docs)
└── templates/
    └── {template-name}.md (for outputs)
```
</complex_outline>
<output>Skill outline</output>
</phase>

<phase name="write-content">
<description>Write skill content section by section</description>
<order>
1. YAML frontmatter (name, description)
2. objective (what and why)
3. intake OR quick_start (entry point)
4. process OR routing (main workflow)
5. success_criteria (verification)
6. Supporting sections as needed
</order>
<output>Draft skill content</output>
</phase>

<phase name="validate">
<description>Validate skill structure and content</description>
<checklist>
- [ ] YAML: name ≤64 chars, description ≤1024 chars
- [ ] No markdown headings (#) in body
- [ ] Required sections: objective, quick_start/intake, success_criteria
- [ ] All XML tags properly closed
- [ ] SKILL.md ≤500 lines
- [ ] Process steps are actionable
- [ ] Success criteria are measurable
</checklist>
<output>Validated skill</output>
</phase>

<phase name="test">
<description>Test skill with example invocation</description>
<actions>
- Simulate skill invocation
- Walk through the workflow
- Verify output matches expectations
- Note any gaps or issues
</actions>
<output>Test results</output>
</phase>
</process>

<success_markers>
- Requirements clearly defined
- Appropriate structure selected
- All required sections written
- Structure validated
- Skill tested with example
</success_markers>
</workflow>
