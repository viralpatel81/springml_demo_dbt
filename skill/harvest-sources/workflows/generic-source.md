<workflow name="generic-source-analysis">
<objective>
Provide adaptive analysis for web sources that don't fit the specific categories (GitHub, YouTube, blog, n8n). Extract maximum knowledge using flexible analysis techniques.
</objective>

<trigger>
Activated when source URL doesn't match specific patterns or user selects generic analysis for an unfamiliar source type.
</trigger>

<process>
<phase name="source-reconnaissance">
<description>Analyze and classify the source</description>
<actions>
- Fetch source via WebFetch
- Analyze page structure and content type
- Determine: Is it a tool? Documentation? Forum? API? App?
- Identify: Primary content format (text, code, data, interactive)
- Note: Access limitations or requirements
</actions>
<output>Source classification and characteristics</output>
</phase>

<phase name="adaptive-strategy">
<description>Select appropriate extraction strategy</description>
<actions>
Based on source classification, adapt approach:

If documentation/wiki:
- Map navigation structure
- Extract core concepts
- Catalog API references

If forum/discussion:
- Identify key answers/solutions
- Extract code snippets
- Note common problems solved

If tool/application:
- Document features and capabilities
- Extract configuration options
- Note integration possibilities

If data source/API:
- Document endpoints
- Extract schema information
- Note authentication requirements
</actions>
<output>Tailored extraction strategy</output>
</phase>

<phase name="content-extraction">
<description>Execute adaptive content extraction</description>
<actions>
- Apply selected strategy
- Extract primary content elements
- Identify: Key concepts and definitions
- Capture: Code or configuration examples
- Note: Relationships and dependencies
- Document: Limitations and gotchas
</actions>
<output>Extracted content by category</output>
</phase>

<phase name="context-enrichment">
<description>Enrich with supplementary research</description>
<actions>
- WebSearch for additional context
- Find related documentation
- Identify: Official resources
- Note: Community resources
- Cross-reference information
</actions>
<output>Enriched context</output>
</phase>

<phase name="pattern-identification">
<description>Abstract patterns from specifics</description>
<actions>
- Identify reusable patterns
- Abstract to general principles
- Document implementation approaches
- Note best practices discovered
</actions>
<output>Pattern catalog</output>
</phase>

<phase name="knowledge-synthesis">
<description>Compile into skill-ready format</description>
<actions>
- Apply harvested-knowledge template
- Adapt structure to content type
- Include all relevant examples
- Add implementation guidance
</actions>
<output>Complete harvested knowledge document</output>
</phase>
</process>

<source_type_detection>
<heuristics>
Detect source type by analyzing:
- URL patterns and domain
- Page structure and metadata
- Content characteristics
- Interactive elements
- File types present
</heuristics>

<common_types>
| Pattern | Likely Type | Approach |
|---------|-------------|----------|
| */docs/* or */documentation/* | Documentation | Reference extraction |
| */api/* or */reference/* | API Docs | Endpoint cataloging |
| Stack Overflow, Reddit, etc. | Forum | Solution extraction |
| */releases/* or version numbers | Changelog | Feature tracking |
| Form-heavy, interactive | Web App | Feature documentation |
| JSON/XML heavy | Data/API | Schema extraction |
</common_types>
</source_type_detection>

<fallback_strategies>
When content is difficult to parse:
1. Focus on visible text content
2. Extract all links for resource mapping
3. Use WebSearch for context about the source
4. Document what IS accessible
5. Note limitations clearly
</fallback_strategies>

<tools>
Primary: WebFetch (universal content retrieval)
Secondary: WebSearch (context and documentation)
Support: Read (for any downloaded files)
</tools>

<success_markers>
- Source type identified and classified
- Appropriate strategy selected and executed
- Primary content extracted
- Context enriched with research
- Patterns identified where possible
- Knowledge encoded in available format
- Limitations clearly documented
</success_markers>
</workflow>
