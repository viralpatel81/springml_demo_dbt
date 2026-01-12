<reference name="skill-architecture">
<objective>
Deep dive into skill architecture, patterns, and best practices.
Reference this for advanced skill design decisions.
</objective>

<toc>
1. Core Architecture Concepts
2. File Organization Patterns
3. XML Tag Reference
4. Progressive Disclosure Strategies
5. Integration Patterns
</toc>

<section name="core-architecture">
<concept name="skills-as-prompts">
Skills are fundamentally prompts with structure. Everything that makes a good prompt makes a good skill:
- Clear instructions
- Specific examples
- Anticipated edge cases
- Defined success criteria

The XML structure provides semantic organization, not formatting.
</concept>

<concept name="guaranteed-loading">
SKILL.md always loads when the skill is invoked. This guarantee means:
- Essential principles belong in SKILL.md
- Routing logic belongs in SKILL.md
- Critical context belongs in SKILL.md

Everything else can be progressively disclosed from subdirectories.
</concept>

<concept name="router-pattern">
Complex skills act as routers:
1. User invokes skill
2. SKILL.md loads (intake, routing table)
3. Based on intent, specific workflow loads
4. Workflow may load additional references

This keeps initial context small while enabling deep functionality.
</concept>
</section>

<section name="file-organization">
<pattern name="simple-skill">
```
skill-name/
└── SKILL.md
```
Best for: Single-purpose skills under 500 lines.
</pattern>

<pattern name="standard-complex">
```
skill-name/
├── SKILL.md           # Router + principles
├── workflows/         # Action sequences
├── references/        # Knowledge bases
└── templates/         # Output formats
```
Best for: Multi-workflow skills with reference material.
</pattern>

<pattern name="with-scripts">
```
skill-name/
├── SKILL.md
├── workflows/
├── references/
├── templates/
└── scripts/           # Executable code
    ├── validate.sh
    └── transform.py
```
Best for: Skills that need to execute code.
</pattern>

<guideline name="depth-limit">
Keep references ONE level deep from SKILL.md:
- Good: `references/api-guide.md`
- Bad: `references/api/v2/endpoints/users.md`

If you need deeper organization, consolidate into fewer, larger files.
</guideline>
</section>

<section name="xml-tag-reference">
<required_tags>
| Tag | Purpose | Location |
|-----|---------|----------|
| `<objective>` | What and why | SKILL.md |
| `<quick_start>` OR `<intake>` | Entry point | SKILL.md |
| `<success_criteria>` | Verification | SKILL.md |
</required_tags>

<common_tags>
| Tag | Purpose | When to Use |
|-----|---------|-------------|
| `<essential_principles>` | Core rules | Complex skills |
| `<routing>` | Intent mapping | Multi-workflow |
| `<process>` | Step sequence | Any workflow |
| `<phase>` | Process subdivision | Detailed workflows |
| `<step>` | Single action | Simple processes |
| `<patterns>` | Reusable solutions | Reference material |
| `<anti_patterns>` | What to avoid | Guidance |
| `<troubleshooting>` | Problem solving | Any skill |
| `<code_reference>` | Code snippets | Technical skills |
| `<attribution>` | Source credit | Derived skills |
</common_tags>

<custom_tags>
You can create custom semantic tags. Guidelines:
- Use lowercase with underscores: `<my_custom_tag>`
- Be consistent within the skill
- Name should indicate content type
</custom_tags>
</section>

<section name="progressive-disclosure">
<strategy name="intake-routing">
1. Present high-level options in intake
2. Route to specific workflow based on selection
3. Workflow loads only relevant references
4. References load on-demand

Result: User sees only what they need.
</strategy>

<strategy name="summary-then-detail">
1. SKILL.md contains summaries and quick references
2. Full details in references/
3. Link from summary to detail: "See references/deep-dive.md"

Result: Quick answers available, depth accessible.
</strategy>

<strategy name="conditional-sections">
Use notes about when sections apply:
```xml
<advanced_usage>
For complex scenarios only. Skip if basic usage meets your needs.
...
</advanced_usage>
```

Result: Users self-select appropriate depth.
</strategy>
</section>

<section name="integration-patterns">
<pattern name="skill-chaining">
Skills can reference other skills:
```xml
<integration>
After harvesting, invoke create-agent-skills with the output.
</integration>
```
Design skills to produce outputs consumable by other skills.
</pattern>

<pattern name="tool-usage">
Document which Claude Code tools the skill uses:
```xml
<tools_usage>
- WebFetch: Retrieve external content
- Task/Explore: Deep codebase analysis
- Read/Write: File operations
</tools_usage>
```
Helps users understand capabilities and limitations.
</pattern>

<pattern name="output-format">
Define clear output formats:
```xml
<output_format>
This skill produces a `<harvested_source>` XML document...
</output_format>
```
Enables downstream processing and chaining.
</pattern>
</section>
</reference>
