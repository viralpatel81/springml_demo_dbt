---
name: audit-skill
description: Audit a skill for compliance with best practices, YAML standards, and pure XML structure. Use after creating or modifying a skill.
arguments: skill-path
---

<objective>
Invoke the skill-auditor agent to perform a comprehensive compliance audit on the skill at the specified path. The audit validates structure, content, and adherence to skill architecture best practices.
</objective>

<process>
<step name="invoke-auditor">
Activate the skill-auditor agent with the provided skill path.

```
Audit the skill at: $ARGUMENTS
```

The agent will:
1. Read and analyze SKILL.md
2. Check YAML frontmatter compliance
3. Validate XML structure
4. Verify required sections exist
5. Check file references
6. Assess content quality
</step>

<step name="receive-report">
The auditor returns a structured report with:
- Overall compliance score
- Section-by-section analysis
- Specific issues found
- Recommended fixes
- Priority ranking of issues
</step>

<step name="present-findings">
Present the audit findings to the user with:
- Summary of compliance status
- List of issues by severity
- Specific file locations and line numbers
- Recommended actions
</step>
</process>

<expected_output>
```
SKILL AUDIT REPORT: {skill-name}
================================

Overall Status: {PASS | NEEDS ATTENTION | FAIL}
Compliance Score: {X}/100

YAML FRONTMATTER
----------------
[✓] Name valid (≤64 chars, lowercase-hyphenated)
[✓] Description valid (≤1024 chars, includes what+when)
[✗] Issue: {description of issue}

XML STRUCTURE
-------------
[✓] No markdown headings
[✓] All tags closed
[✗] Issue: {description of issue}

REQUIRED SECTIONS
-----------------
[✓] objective present
[✓] quick_start OR intake present
[✗] success_criteria missing

FILE REFERENCES
---------------
[✓] All referenced workflows exist
[✗] Missing: references/missing-file.md

RECOMMENDATIONS
---------------
1. [HIGH] {recommendation}
2. [MEDIUM] {recommendation}
3. [LOW] {recommendation}
```
</expected_output>

<success_criteria>
- [ ] Skill-auditor agent successfully invoked
- [ ] Skill path correctly passed to agent
- [ ] Audit report generated with all categories
- [ ] Specific issues identified with locations
- [ ] Actionable recommendations provided
</success_criteria>

<usage>
```
/audit-skill path/to/skill
/audit-skill skill/harvest-sources
/audit-skill ~/.claude/skills/my-skill
```
</usage>
