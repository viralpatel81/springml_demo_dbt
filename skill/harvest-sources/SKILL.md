---
name: harvest-sources
description: Research and analyze external sources (GitHub repos, YouTube videos, blog articles, n8n workflows) and encode them into structured knowledge suitable for skill creation. Use when you need to learn from and document an external resource.
---

<objective>
This skill enables systematic harvesting of knowledge from external sources and encoding it into a structured format ready for skill creation. It transforms scattered information from various source types into well-organized, actionable knowledge bases.

The harvesting process follows a rigorous research methodology: identify the source type, deep-dive into the content, extract patterns and key concepts, and encode findings into a standardized format that can be directly used to create new skills.
</objective>

<intake>
What would you like to harvest?

1. **GitHub Repository** - Analyze repo structure, code patterns, documentation, and workflows
2. **YouTube Video** - Extract concepts, tutorials, and step-by-step processes from video content
3. **Blog Article / Documentation** - Parse written content for techniques, patterns, and knowledge
4. **n8n Workflow** - Decode automation workflows, nodes, and integration patterns
5. **Other URL** - Generic analysis for any web-accessible resource

Please provide the URL and select the source type (or I'll auto-detect).
</intake>

<routing>
| User Intent | Workflow | Progressive Disclosure |
|-------------|----------|------------------------|
| GitHub repo analysis | workflows/github-repo.md | Load repo structure patterns, code analysis methods |
| YouTube video extraction | workflows/youtube-video.md | Load transcript analysis, concept extraction |
| Blog/article parsing | workflows/blog-article.md | Load content parsing, knowledge extraction |
| n8n workflow decoding | workflows/n8n-workflow.md | Load node analysis, automation patterns |
| Generic URL | workflows/generic-source.md | Load adaptive analysis methods |
</routing>

<process>
<step name="source-identification">
Identify and validate the source:
- Detect source type from URL pattern
- Verify accessibility (fetch the resource)
- Determine scope and depth of content
- Identify any authentication requirements
</step>

<step name="deep-research">
Execute comprehensive analysis based on source type:

For GitHub repos:
- Map repository structure (directories, key files)
- Analyze README and documentation
- Extract code patterns and architectures
- Identify dependencies and integrations
- Document configuration patterns
- Catalog reusable components

For YouTube videos:
- Extract video metadata (title, description, chapters)
- Analyze transcript for key concepts
- Identify step-by-step processes
- Note tool/technology mentions
- Extract actionable techniques

For blog articles:
- Parse main content structure
- Extract code snippets and examples
- Identify key concepts and definitions
- Document techniques and patterns
- Note references and citations

For n8n workflows:
- Decode workflow JSON structure
- Map node connections and flow
- Document trigger conditions
- Extract integration patterns
- Identify reusable automation blocks
</step>

<step name="knowledge-encoding">
Transform findings into structured format:
- Create semantic sections with XML tags
- Organize by concept hierarchy
- Include concrete examples
- Document edge cases and gotchas
- Prepare for skill template injection
</step>

<step name="output-generation">
Generate harvested knowledge document:
- Use template from templates/harvested-knowledge.md
- Include source attribution
- Provide skill-ready structure
- Add implementation guidance
</step>
</process>

<tools_usage>
This skill leverages these Claude Code capabilities:

- **WebFetch** - Retrieve and parse web content
- **Task with Explore agent** - Deep codebase analysis for GitHub repos
- **Grep/Glob** - Pattern matching in fetched content
- **Read** - Process downloaded files
- **WebSearch** - Supplement with additional context when needed

Critical: Always verify content accessibility before deep analysis. Handle rate limits and access restrictions gracefully.
</tools_usage>

<output_format>
The harvested knowledge is encoded using this structure:

```xml
<harvested_source>
  <metadata>
    <source_url>original URL</source_url>
    <source_type>github|youtube|blog|n8n|other</source_type>
    <harvested_date>ISO date</harvested_date>
    <title>source title</title>
  </metadata>

  <summary>
    High-level overview of what was learned (2-3 paragraphs)
  </summary>

  <key_concepts>
    <concept name="concept-name">
      Description and explanation
    </concept>
    <!-- Additional concepts -->
  </key_concepts>

  <patterns>
    <pattern name="pattern-name">
      <description>What this pattern does</description>
      <implementation>How to implement it</implementation>
      <example>Concrete example</example>
    </pattern>
    <!-- Additional patterns -->
  </patterns>

  <code_snippets>
    <snippet name="snippet-name" language="lang">
      <purpose>What this code does</purpose>
      <code>actual code</code>
    </snippet>
    <!-- Additional snippets -->
  </code_snippets>

  <skill_ready>
    <suggested_skill_name>proposed name</suggested_skill_name>
    <suggested_description>skill description</suggested_description>
    <core_workflow>main process steps</core_workflow>
  </skill_ready>
</harvested_source>
```
</output_format>

<success_criteria>
Harvesting is complete when:
- [ ] Source has been fully accessed and parsed
- [ ] All major concepts have been identified and documented
- [ ] Patterns are extracted with concrete examples
- [ ] Code snippets are captured with context
- [ ] Knowledge is structured in skill-ready format
- [ ] Source attribution is properly documented
- [ ] Output can be directly used by create-agent-skills
</success_criteria>

<quick_reference>
**Quick harvest workflow:**
1. Receive URL from user
2. Auto-detect source type (or confirm with user)
3. Fetch and parse content
4. Execute type-specific deep analysis
5. Encode findings in structured XML format
6. Output harvested knowledge document
7. Optionally trigger skill creation

**Source type detection patterns:**
- `github.com/*` → GitHub repository
- `youtube.com/watch*` or `youtu.be/*` → YouTube video
- `*.n8n.io/*` or n8n JSON → n8n workflow
- Other URLs → Blog/article or generic analysis
</quick_reference>

<anti_patterns>
- DO NOT shallow-skim sources - always deep-dive
- DO NOT ignore error handling patterns in code
- DO NOT skip documentation and comments
- DO NOT harvest without proper attribution
- DO NOT assume - verify all extracted information
- DO NOT proceed if source is inaccessible - report and suggest alternatives
</anti_patterns>

<workflow_index>
| Workflow | Purpose |
|----------|---------|
| workflows/github-repo.md | Detailed GitHub repository analysis |
| workflows/youtube-video.md | YouTube video content extraction |
| workflows/blog-article.md | Blog and documentation parsing |
| workflows/n8n-workflow.md | n8n automation workflow decoding |
| workflows/generic-source.md | Adaptive analysis for other sources |
</workflow_index>

<template_index>
| Template | Purpose |
|----------|---------|
| templates/harvested-knowledge.md | Main output format template |
| templates/skill-draft.md | Draft skill from harvested content |
</template_index>
