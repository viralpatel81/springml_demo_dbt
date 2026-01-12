<workflow name="blog-article-parsing">
<objective>
Parse blog posts, technical articles, and documentation pages to extract knowledge, code examples, and techniques. Structure findings for skill creation.
</objective>

<trigger>
Activated when source URL points to a blog, documentation site, or written article, or user explicitly selects blog/article analysis.
</trigger>

<process>
<phase name="content-retrieval">
<description>Fetch and validate article content</description>
<actions>
- Fetch article via WebFetch
- Verify content accessibility (no paywall, login required)
- Extract: title, author, publish date
- Identify: article type (tutorial, opinion, reference, case study)
- Note: estimated reading time / content length
</actions>
<output>Article metadata and accessibility status</output>
</phase>

<phase name="structure-analysis">
<description>Map article structure and sections</description>
<actions>
- Identify heading hierarchy (H1, H2, H3)
- Map section flow and organization
- Note: table of contents if present
- Identify: introduction, body sections, conclusion
- Locate: code blocks, images, diagrams
</actions>
<output>Article structure map</output>
</phase>

<phase name="content-extraction">
<description>Deep parse article content</description>
<actions>
- Extract main thesis/purpose
- Parse each section for key points
- Identify: definitions and explanations
- Extract: examples and use cases
- Note: author opinions vs facts
- Capture: warnings, tips, best practices
</actions>
<output>Section-by-section knowledge extraction</output>
</phase>

<phase name="code-extraction">
<description>Extract and contextualize code snippets</description>
<actions>
- Identify all code blocks
- Determine language for each snippet
- Extract surrounding context (what problem it solves)
- Note: dependencies and prerequisites
- Identify: complete vs partial examples
- Check for: linked repositories or gists
</actions>
<output>Contextualized code snippet library</output>
</phase>

<phase name="reference-gathering">
<description>Catalog all references and links</description>
<actions>
- Extract all outbound links
- Categorize: documentation, tools, related articles
- Identify: primary sources cited
- Note: version numbers mentioned
- Compile: resource list for further reading
</actions>
<output>Reference compilation</output>
</phase>

<phase name="pattern-identification">
<description>Identify reusable patterns and techniques</description>
<actions>
- Abstract specific examples to general patterns
- Identify: architectural decisions
- Extract: naming conventions
- Note: error handling approaches
- Document: configuration patterns
</actions>
<output>Pattern catalog</output>
</phase>

<phase name="knowledge-synthesis">
<description>Compile into skill-ready format</description>
<actions>
- Apply harvested-knowledge template
- Structure by concept hierarchy
- Include all code with context
- Add implementation guidance
</actions>
<output>Complete harvested knowledge document</output>
</phase>
</process>

<article_specific_patterns>
<pattern name="article-types">
Adapt extraction for:
- Tutorial: Emphasize steps, order, prerequisites
- Reference: Focus on completeness, options, parameters
- Opinion/Analysis: Note author perspective, separate facts
- Case Study: Extract problem, solution, results, lessons
- Comparison: Structure as feature matrix
</pattern>

<pattern name="code-context">
For each code block, capture:
- What problem it solves
- Prerequisites (imports, setup)
- Expected input/output
- Gotchas or edge cases mentioned
- Alternative approaches discussed
</pattern>

<pattern name="documentation-sites">
For official documentation:
- Note version being documented
- Extract API signatures
- Capture configuration options
- Document default values
- Note deprecation warnings
</pattern>
</article_specific_patterns>

<quality_signals>
Assess article quality:
- Author expertise indicators
- Publication date (recency)
- Comments/discussion quality
- Links to working examples
- Updates/corrections noted
</quality_signals>

<tools>
Primary: WebFetch (for article content)
Secondary: WebSearch (for context, author background, related articles)
Support: Read (for downloaded content analysis)
</tools>

<success_markers>
- Article fully retrieved and parsed
- Structure mapped with all sections
- Key concepts extracted with context
- Code snippets captured with explanations
- References and resources cataloged
- Patterns abstracted from specifics
- Knowledge encoded in standard format
</success_markers>
</workflow>
