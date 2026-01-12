<domain name="skill-debugging">
<objective>
Domain expertise for debugging Claude Code skills specifically.
</objective>

<skill_architecture_knowledge>
<component name="yaml-frontmatter">
Location: Top of SKILL.md
Required fields: name, description
Constraints:
- name: ≤64 chars, lowercase-hyphenated, matches directory
- description: ≤1024 chars, third person, includes what AND when
Common errors:
- Special characters in name
- Description too long
- Missing required fields
</component>

<component name="xml-body">
Location: After YAML frontmatter
Structure: Pure XML, no markdown headings
Required tags: objective, quick_start OR intake, success_criteria
Common errors:
- Markdown headings (#, ##) in body
- Unclosed XML tags
- Missing required sections
</component>

<component name="router-pattern">
Used for: Complex skills with multiple workflows
Structure: SKILL.md routes to workflows/ based on intent
Common errors:
- Routing table references non-existent workflow
- Intake options don't match routing
- Circular references
</component>

<component name="progressive-disclosure">
Pattern: SKILL.md → workflows/ → references/
Rule: Keep SKILL.md under 500 lines
Common errors:
- SKILL.md too long (not split)
- References more than one level deep
- Missing index tables
</component>
</skill_architecture_knowledge>

<validation_checklist>
<category name="yaml">
- [ ] name field present and ≤64 characters
- [ ] name is lowercase with hyphens only
- [ ] name matches directory name (for complex skills)
- [ ] description field present and ≤1024 characters
- [ ] description includes what AND when to use
- [ ] description is third person
</category>

<category name="structure">
- [ ] No markdown headings (#, ##, ###) in body
- [ ] All XML tags properly opened and closed
- [ ] SKILL.md is under 500 lines
- [ ] Required sections present: objective, quick_start/intake, success_criteria
</category>

<category name="content">
- [ ] Objective clearly states what and why
- [ ] Intake/quick_start provides clear entry point
- [ ] Process steps are actionable
- [ ] Success criteria are measurable
- [ ] All referenced files exist
</category>

<category name="routing">
- [ ] Routing table matches intake options
- [ ] All workflow files exist
- [ ] Workflows have proper structure
- [ ] No circular dependencies
</category>
</validation_checklist>

<debugging_patterns>
<pattern name="invocation-failure">
Symptom: Skill doesn't activate when expected
Debug steps:
1. Check skill is in correct location (~/.claude/skills/ or project)
2. Verify YAML name matches expected trigger
3. Check description includes trigger phrases
4. Verify no YAML syntax errors
</pattern>

<pattern name="wrong-output">
Symptom: Skill runs but produces unexpected output
Debug steps:
1. Trace through process steps manually
2. Check for missing or unclear steps
3. Verify assumptions in the workflow
4. Look for edge cases not handled
</pattern>

<pattern name="partial-execution">
Symptom: Skill starts but doesn't complete
Debug steps:
1. Identify where execution stops
2. Check for blocking conditions
3. Verify all required inputs available
4. Look for infinite loops or missing exits
</pattern>

<pattern name="reference-not-found">
Symptom: Error about missing file or reference
Debug steps:
1. Check file path in reference
2. Verify file exists at path
3. Check for typos in path
4. Ensure relative paths are correct
</pattern>
</debugging_patterns>

<repair_procedures>
<procedure name="fix-yaml">
1. Extract current YAML frontmatter
2. Validate against constraints
3. Fix: name (lowercase, hyphens, ≤64)
4. Fix: description (≤1024, what+when, third person)
5. Rewrite YAML block
</procedure>

<procedure name="fix-xml-structure">
1. Remove all markdown headings
2. Replace with appropriate XML tags
3. Ensure all tags are closed
4. Validate nesting is correct
</procedure>

<procedure name="fix-references">
1. List all file references in SKILL.md
2. Check each file exists
3. Create missing files or fix paths
4. Update index tables
</procedure>
</repair_procedures>
</domain>
