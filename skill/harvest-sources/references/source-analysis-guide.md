<reference name="source-analysis-guide">
<objective>
Comprehensive guide for analyzing different source types effectively.
Reference this when encountering complex or unusual sources.
</objective>

<toc>
1. Analysis Mindset
2. Source Type Deep Dives
3. Common Challenges
4. Quality Assessment
5. Knowledge Encoding Best Practices
</toc>

<section name="analysis-mindset">
<principle name="depth-over-breadth">
Always prefer thorough analysis of key components over shallow coverage of everything.
A well-documented pattern is more valuable than a list of features.
</principle>

<principle name="context-is-king">
Never extract code or patterns without context. Isolated snippets are nearly useless.
Always capture: why it exists, when to use it, what it depends on.
</principle>

<principle name="verify-dont-assume">
Cross-reference claims with documentation. Check version compatibility.
Note uncertainty explicitly rather than presenting assumptions as facts.
</principle>

<principle name="think-like-implementer">
Ask: "If I were implementing this tomorrow, what would I need to know?"
Focus on practical, actionable knowledge.
</principle>
</section>

<section name="source-type-deep-dives">
<source_type name="github-repositories">
<priority_files>
In order of importance:
1. README.md - Overview and quick start
2. docs/ or documentation/ - Detailed guides
3. examples/ - Working implementations
4. src/index.* or src/main.* - Entry points
5. package.json/requirements.txt/Cargo.toml - Dependencies
6. .github/workflows/ - CI/CD patterns
7. tests/ - Usage patterns and edge cases
</priority_files>

<hidden_gems>
Often-overlooked valuable content:
- CHANGELOG.md - Feature evolution, breaking changes
- CONTRIBUTING.md - Architecture insights
- .env.example - Configuration options
- Makefile/scripts/ - Common operations
- Issue templates - Common problems
- Pull request templates - Contribution patterns
</hidden_gems>

<red_flags>
Signs of potentially outdated/unreliable repos:
- No updates in 2+ years
- Unresolved critical issues
- Deprecated dependencies
- Incomplete documentation
- No tests
</red_flags>
</source_type>

<source_type name="youtube-videos">
<high_value_indicators>
- Clear chapter markers
- Linked code repository
- Active comment section with author responses
- Part of a series
- Recent upload with current tools
</high_value_indicators>

<extraction_priorities>
1. Commands and configurations shown
2. Step-by-step processes
3. Tool recommendations
4. Gotchas mentioned
5. Alternative approaches discussed
</extraction_priorities>

<limitations_handling>
When transcript unavailable:
- Parse description thoroughly
- Check for community transcripts
- Search for written summaries
- Focus on linked resources
- Note visual-only content gaps
</limitations_handling>
</source_type>

<source_type name="blog-articles">
<credibility_signals>
Indicators of reliable content:
- Author with verifiable expertise
- Working code examples
- Recent publication date
- Links to official documentation
- Acknowledges limitations
- Updated based on feedback
</credibility_signals>

<extraction_priorities>
1. Core technique/pattern
2. Complete code examples
3. Configuration requirements
4. Common pitfalls mentioned
5. Alternative approaches
6. Prerequisites stated
</extraction_priorities>

<warning_signs>
Be cautious with:
- Outdated version references
- No code examples
- Contradicts official docs
- SEO-heavy, content-light
- No author attribution
</warning_signs>
</source_type>

<source_type name="n8n-workflows">
<analysis_approach>
1. Start with trigger - understand what initiates the workflow
2. Follow main path - trace the happy path first
3. Map error handling - identify fallback paths
4. Catalog integrations - list external services
5. Extract custom code - focus on Function nodes
6. Note credentials needed - document requirements
</analysis_approach>

<reusability_focus>
Most reusable components:
- Error handling patterns
- Data transformation logic
- Integration configurations
- Notification patterns
- Conditional logic structures
</reusability_focus>

<security_considerations>
Never expose or store:
- API keys or tokens
- Passwords or secrets
- Personal identifiable data
- Internal URLs or endpoints

Document only credential types and permission requirements.
</security_considerations>
</source_type>
</section>

<section name="common-challenges">
<challenge name="access-restrictions">
<problem>Content behind paywalls, logins, or rate limits</problem>
<approaches>
- Check for free alternatives (official docs, GitHub)
- Use WebSearch to find summaries or reviews
- Document what IS accessible
- Note limitations clearly in output
- Suggest alternatives to user
</approaches>
</challenge>

<challenge name="outdated-content">
<problem>Source uses deprecated APIs, old versions, or obsolete patterns</problem>
<approaches>
- Cross-reference with current documentation
- Note version differences explicitly
- Extract concepts that remain relevant
- Document required updates
- Flag uncertainty about current applicability
</approaches>
</challenge>

<challenge name="incomplete-information">
<problem>Source lacks context, examples, or complete explanations</problem>
<approaches>
- Supplement with WebSearch
- Find related official documentation
- Note gaps explicitly
- Don't fill gaps with assumptions
- Provide pointers for further research
</approaches>
</challenge>

<challenge name="complex-codebases">
<problem>Large repositories with unclear entry points</problem>
<approaches>
- Start with documentation
- Look for examples/ directory
- Check tests for usage patterns
- Focus on public API surface
- Use Task tool for deep exploration
</approaches>
</challenge>
</section>

<section name="quality-assessment">
<assessment_framework>
Rate harvested sources on these dimensions:

<dimension name="completeness">
1 - Major gaps in documentation
2 - Missing important sections
3 - Covers basics adequately
4 - Comprehensive coverage
5 - Exhaustive with examples
</dimension>

<dimension name="accuracy">
1 - Contains errors
2 - Partially accurate
3 - Generally correct
4 - Verified accurate
5 - Cross-referenced and confirmed
</dimension>

<dimension name="currency">
1 - Severely outdated
2 - Somewhat outdated
3 - Reasonably current
4 - Up to date
5 - Cutting edge
</dimension>

<dimension name="actionability">
1 - Theoretical only
2 - Limited practical guidance
3 - Basic implementation possible
4 - Clear implementation path
5 - Ready to implement immediately
</dimension>
</assessment_framework>
</section>

<section name="knowledge-encoding-best-practices">
<practice name="structured-not-verbose">
Use clear structure with concise content. XML tags provide organization;
content should be dense and valuable.
</practice>

<practice name="examples-are-essential">
Every pattern and concept should have at least one concrete example.
Abstract descriptions without examples are insufficient.
</practice>

<practice name="preserve-context">
Code snippets need surrounding context: imports, setup, expected inputs/outputs.
Isolated code is rarely useful.
</practice>

<practice name="attribution-matters">
Always cite sources completely. Include URLs, dates, and authors.
This enables verification and updates.
</practice>

<practice name="note-limitations">
Explicitly document what you couldn't find, access, or verify.
Transparency about gaps is more valuable than false completeness.
</practice>

<practice name="skill-ready-focus">
Always ask: "Can this be turned into a skill?" Structure findings
to enable direct skill creation when possible.
</practice>
</section>
</reference>
