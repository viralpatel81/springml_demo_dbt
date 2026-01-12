<workflow name="debug-skill">
<objective>
Systematically debug Claude Code skills that aren't working as expected.
</objective>

<trigger>
A skill produces wrong output, fails to activate, or behaves unexpectedly.
</trigger>

<process>
<phase name="symptom-documentation">
<description>Document exactly what's happening</description>
<questions>
- What skill is failing?
- What was the expected behavior?
- What actually happened?
- What was the exact input/invocation?
- Any error messages?
</questions>
<actions>
- Record exact invocation context
- Capture complete output (not summarized)
- Note any partial success
</actions>
<output>Symptom report</output>
</phase>

<phase name="skill-structure-analysis">
<description>Examine skill structure for issues</description>
<checklist>
- [ ] YAML frontmatter valid? (name ≤64, description ≤1024)
- [ ] Name matches directory name?
- [ ] No markdown headings in body?
- [ ] Required sections present? (objective, quick_start/intake, success_criteria)
- [ ] All XML tags properly closed?
- [ ] SKILL.md under 500 lines?
- [ ] Workflows/references exist if referenced?
</checklist>
<actions>
- Read SKILL.md completely
- Check all referenced files exist
- Validate XML structure
</actions>
<output>Structure analysis report</output>
</phase>

<phase name="content-analysis">
<description>Analyze skill content for logical issues</description>
<focus_areas>
- Is the objective clear and achievable?
- Does the intake/routing match user needs?
- Are process steps actionable and complete?
- Do success criteria match the objective?
- Are there gaps in the workflow?
</focus_areas>
<actions>
- Trace through skill workflow manually
- Identify assumptions that might be wrong
- Look for missing steps or unclear instructions
</actions>
<output>Content analysis report</output>
</phase>

<phase name="hypothesis-formation">
<description>Form testable hypotheses</description>
<common_skill_issues>
| Symptom | Likely Cause | Test |
|---------|--------------|------|
| Skill won't activate | Name/description mismatch, YAML error | Check YAML validity |
| Wrong workflow triggered | Routing logic error | Trace routing table |
| Incomplete output | Missing process steps | Walk through process |
| Unexpected behavior | Unclear instructions | Re-read as if new |
| Referenced file not found | Path error, missing file | Check file paths |
</common_skill_issues>
<output>Hypothesis list with tests</output>
</phase>

<phase name="testing">
<description>Test each hypothesis</description>
<actions>
- Start with structural issues (easiest to verify)
- Test one hypothesis at a time
- Record results for each test
- Stop when root cause confirmed
</actions>
<output>Test results with confirmed cause</output>
</phase>

<phase name="fix-implementation">
<description>Fix the identified issue</description>
<actions>
- Make minimal change to fix root cause
- Maintain skill structure requirements
- Update any affected references
- Document what was changed and why
</actions>
<output>Fixed skill files</output>
</phase>

<phase name="verification">
<description>Verify the fix works</description>
<actions>
- Re-invoke skill with original input
- Verify expected behavior occurs
- Test edge cases
- Run audit-skill for compliance check
</actions>
<output>Verification report</output>
</phase>
</process>

<common_fixes>
<fix issue="yaml-error">
Problem: Invalid YAML frontmatter
Solution: Ensure name is lowercase-hyphenated, ≤64 chars; description ≤1024 chars
</fix>

<fix issue="markdown-headings">
Problem: Markdown headings (#) in skill body
Solution: Replace all headings with semantic XML tags
</fix>

<fix issue="unclosed-tags">
Problem: XML tags not properly closed
Solution: Find and close all open tags; use editor with XML validation
</fix>

<fix issue="missing-reference">
Problem: Referenced workflow/file doesn't exist
Solution: Create missing file or update reference to correct path
</fix>

<fix issue="unclear-routing">
Problem: User intent doesn't match routing
Solution: Add more routing options or clarify intake options
</fix>
</common_fixes>

<success_markers>
- Root cause identified and documented
- Fix implemented with minimal changes
- Skill re-tested and working
- Audit passes
</success_markers>
</workflow>
