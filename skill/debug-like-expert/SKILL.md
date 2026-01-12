---
name: debug-like-expert
description: Methodical debugging protocol for complex issues when standard troubleshooting fails. Use when you need systematic root cause analysis with scientific rigor instead of quick fixes.
---

<objective>
This skill implements rigorous, scientific debugging methodology for complex problems. It emphasizes evidence-based investigation, systematic hypothesis testing, and verified solutions over guesswork and quick fixes.

Critical mindset: Apply MORE skepticism to code you wrote yourself. Cognitive biases about intended behavior often mask actual implementation errors. VERIFY, DON'T ASSUME.
</objective>

<essential_principles>
<principle name="verify-dont-assume">
Every hypothesis must be tested. Every fix must be validated.
Never assume you know what's wrong - prove it.
</principle>

<principle name="own-code-skepticism">
Be MORE suspicious of your own code than unfamiliar code.
You know what it SHOULD do, which blinds you to what it ACTUALLY does.
</principle>

<principle name="one-variable-at-a-time">
Change only one thing between tests.
Multiple changes make it impossible to identify what worked.
</principle>

<principle name="no-drive-by-fixes">
Never apply a fix without understanding WHY it works.
Unexplained fixes often mask deeper issues or create new ones.
</principle>

<principle name="read-completely">
Read complete relevant files, don't skim.
The bug is often in the part you skipped.
</principle>
</essential_principles>

<intake>
What are you debugging?

1. **Skill not working** - A skill produces wrong output or fails
2. **Code bug** - Application code behaving unexpectedly
3. **Integration failure** - Components not communicating correctly
4. **Performance issue** - Code works but too slowly
5. **Intermittent failure** - Works sometimes, fails sometimes

Provide:
- Exact error message or unexpected behavior
- Steps to reproduce
- What you've already tried
</intake>

<routing>
| Issue Type | Workflow | Domain Loading |
|------------|----------|----------------|
| Skill failure | workflows/debug-skill.md | Load skill architecture |
| Code bug | workflows/debug-code.md | Detect project type |
| Integration | workflows/debug-integration.md | Load relevant APIs |
| Performance | workflows/debug-performance.md | Load profiling tools |
| Intermittent | workflows/debug-intermittent.md | Load race condition patterns |
</routing>

<process>
<phase name="context-scan">
<description>Understand the environment and detect domains</description>
<actions>
- Identify project type (language, framework, architecture)
- Check for available domain expertise
- Note relevant tools and capabilities
- Establish baseline understanding
</actions>
<output>Context summary with available expertise</output>
</phase>

<phase name="evidence-gathering">
<description>Document the problem with precision</description>
<actions>
- Record EXACT error messages (copy, don't paraphrase)
- Document reproduction steps (specific, not general)
- Map execution path from entry to failure
- Identify what changed recently
- Research external context (API docs, known issues)
</actions>
<output>Evidence document with reproduction steps</output>
</phase>

<phase name="hypothesis-formation">
<description>Form testable hypotheses</description>
<actions>
- List possible causes based on evidence
- Rank by likelihood and ease of testing
- For each hypothesis, define:
  - What would prove it true?
  - What would prove it false?
  - How to test it?
</actions>
<output>Ranked hypothesis list with test plans</output>
</phase>

<phase name="systematic-testing">
<description>Test hypotheses one at a time</description>
<actions>
- Start with most likely hypothesis
- Design minimal test to prove/disprove
- Execute test, record result
- If disproved, move to next hypothesis
- If proved, proceed to solution
- NEVER skip this phase
</actions>
<output>Test results with confirmed root cause</output>
</phase>

<phase name="solution-development">
<description>Fix the root cause, not symptoms</description>
<actions>
- Implement MINIMAL change addressing root cause
- Verify fix resolves original issue
- Test reproduction steps (should pass now)
- Check for regressions and side effects
- Document why the fix works
</actions>
<output>Verified solution with explanation</output>
</phase>

<phase name="verification">
<description>Confirm the problem is truly solved</description>
<checklist>
- [ ] Original error no longer occurs
- [ ] Reproduction steps now pass
- [ ] No new errors introduced
- [ ] Edge cases considered
- [ ] Can explain WHY it works to someone else
</checklist>
<output>Verification report</output>
</phase>
</process>

<tools_usage>
Leverage these capabilities:
- **Read** - Complete file contents, not snippets
- **Grep** - Find patterns across codebase
- **Bash** - Run tests, check logs, execute code
- **WebSearch** - Research errors, find documentation
- **WebFetch** - Get API docs, known issues
- **Task/Explore** - Deep codebase analysis
</tools_usage>

<anti_patterns>
<anti_pattern name="guessing">
DON'T: Try random fixes hoping something works
DO: Form hypothesis, test, verify
</anti_pattern>

<anti_pattern name="skimming">
DON'T: Skim files looking for obvious issues
DO: Read complete relevant files thoroughly
</anti_pattern>

<anti_pattern name="multiple-changes">
DON'T: Change several things at once
DO: One change, one test, record result
</anti_pattern>

<anti_pattern name="assuming-understanding">
DON'T: Assume you know what the code does
DO: Trace execution and verify behavior
</anti_pattern>

<anti_pattern name="quick-fix-satisfaction">
DON'T: Accept a fix that works but you don't understand
DO: Keep investigating until you understand WHY
</anti_pattern>
</anti_patterns>

<success_criteria>
Debugging is complete when you can answer YES to all:
- [ ] Do you understand WHY the issue occurred?
- [ ] Have you verified the fix actually works?
- [ ] Do the original reproduction steps now pass?
- [ ] Have you checked for side effects?
- [ ] Could you explain the fix in a code review?
</success_criteria>

<workflow_index>
| Workflow | Purpose |
|----------|---------|
| workflows/debug-skill.md | Debug Claude Code skills |
| workflows/debug-code.md | Debug application code |
| workflows/debug-integration.md | Debug component communication |
</workflow_index>

<domain_index>
| Domain | Purpose |
|--------|---------|
| domains/skill-debugging.md | Skill-specific debugging patterns |
</domain_index>
