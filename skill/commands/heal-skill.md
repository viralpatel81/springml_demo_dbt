---
name: heal-skill
description: Fix a skill based on what actually worked during execution. Use after a skill fails and you've found a working solution.
---

<objective>
Analyze what went wrong with a skill during execution, identify the fix that worked, and update the skill documentation to prevent future failures. This creates a feedback loop where skills improve based on real-world usage.
</objective>

<process>
<step name="skill-detection">
Identify which skill needs healing by examining:
- Recent conversation context
- SKILL.md files mentioned or used
- Task history and error messages
- User indication of which skill failed

If unclear, ask: "Which skill needs healing?"
</step>

<step name="reflection">
Analyze the failure and fix:
- What was the skill supposed to do?
- What actually happened (the failure)?
- How was the fix discovered?
- What change made it work?
- Why did the original approach fail?

Document the root cause, not just the symptom.
</step>

<step name="file-scan">
Examine the skill's file structure:
```
skill-name/
├── SKILL.md
├── workflows/
├── references/
└── templates/
```

Read all relevant files to understand:
- Current documentation
- Where the gap or error exists
- What needs to change
</step>

<step name="change-proposal">
Present proposed changes in diff format:

```
FILE: skill-name/SKILL.md

BEFORE (lines X-Y):
{current content}

AFTER:
{corrected content}

REASON: {why this change fixes the issue}
```

For each affected file, show:
- Exact location of change
- Current (incorrect) content
- Proposed (correct) content
- Explanation of why
</step>

<step name="approval-gate">
Present options to user:

1. **Apply with commit** - Make changes and commit with message
2. **Apply without commit** - Make changes, don't commit
3. **Revise** - Modify the proposed changes
4. **Cancel** - Abandon healing

WAIT for explicit user approval before making any changes.
</step>

<step name="apply-changes">
If approved:
1. Apply all proposed changes
2. Verify files are syntactically valid
3. Run quick validation (YAML, XML structure)
4. If "with commit" selected, create commit

Commit message format:
```
heal({skill-name}): {brief description of fix}

- {specific change 1}
- {specific change 2}
```
</step>

<step name="verification">
After applying changes:
1. Read modified files to confirm changes
2. Run audit-skill for compliance check
3. Report success or any remaining issues
</step>
</process>

<allowed_operations>
This command can:
- Read any file (to understand current state)
- Edit skill documentation files
- Execute: `ls`, `git status`, `git add`, `git commit`

This command cannot:
- Delete files
- Execute arbitrary commands
- Make changes without approval
</allowed_operations>

<healing_patterns>
<pattern name="missing-step">
Problem: Skill missing a step that was needed
Fix: Add the missing step to process
Document: Why this step is necessary
</pattern>

<pattern name="unclear-instruction">
Problem: Instructions ambiguous, led to wrong action
Fix: Rewrite instructions with specificity
Document: What the confusion was
</pattern>

<pattern name="wrong-assumption">
Problem: Skill assumed something that wasn't true
Fix: Remove assumption or add validation
Document: What the correct behavior should be
</pattern>

<pattern name="missing-edge-case">
Problem: Skill didn't handle a specific scenario
Fix: Add handling for the edge case
Document: When this scenario occurs
</pattern>

<pattern name="incorrect-reference">
Problem: Referenced wrong file or tool
Fix: Correct the reference
Document: What the correct reference is
</pattern>
</healing_patterns>

<success_criteria>
- [ ] Failed skill identified
- [ ] Root cause understood (not just symptom)
- [ ] Proposed changes address root cause
- [ ] User approved changes
- [ ] Changes applied successfully
- [ ] Skill passes validation after healing
</success_criteria>

<usage>
After a skill fails and you've found a fix:
```
/heal-skill
```

The command will detect the relevant skill from context.
If multiple skills involved, it will ask which to heal.
</usage>
