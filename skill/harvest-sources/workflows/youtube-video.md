<workflow name="youtube-video-extraction">
<objective>
Extract knowledge, concepts, and actionable techniques from YouTube videos. Transform video content into structured documentation suitable for skill creation.
</objective>

<trigger>
Activated when source URL matches `youtube.com/watch*` or `youtu.be/*` patterns, or user explicitly selects YouTube video analysis.
</trigger>

<process>
<phase name="metadata-extraction">
<description>Gather video metadata and context</description>
<actions>
- Fetch video page via WebFetch
- Extract: title, channel name, publish date, duration
- Parse: video description for links, timestamps, resources
- Note: view count, like ratio (quality indicators)
- Identify: video category and tags if available
</actions>
<output>Video metadata summary</output>
</phase>

<phase name="description-parsing">
<description>Deep parse video description</description>
<actions>
- Extract all URLs mentioned (tools, resources, repos)
- Parse timestamp chapters if present
- Identify: code repositories linked
- Note: tools and technologies mentioned
- Extract: social links and additional resources
</actions>
<output>Resource compilation from description</output>
</phase>

<phase name="transcript-analysis">
<description>Analyze video transcript for content</description>
<actions>
- Attempt to fetch transcript via WebFetch on transcript services
- If unavailable, note limitation and work with description + title
- Parse transcript for:
  - Key concept introductions
  - Step-by-step instructions
  - Tool/command mentions
  - Code snippets discussed
  - Tips and best practices
- Create timestamp-indexed notes
</actions>
<output>Transcript-based knowledge extraction</output>
</phase>

<phase name="concept-identification">
<description>Identify and structure main concepts</description>
<actions>
- List all technologies/tools mentioned
- Identify teaching progression (beginner → advanced)
- Extract definitions and explanations
- Note relationships between concepts
- Identify prerequisites mentioned
</actions>
<output>Concept hierarchy and definitions</output>
</phase>

<phase name="actionable-extraction">
<description>Extract actionable techniques and steps</description>
<actions>
- Identify all "how to" segments
- Extract step-by-step processes
- Note command-line instructions
- Capture configuration steps
- Document troubleshooting tips mentioned
</actions>
<output>Actionable technique catalog</output>
</phase>

<phase name="supplementary-research">
<description>Enrich with additional context</description>
<actions>
- WebSearch for mentioned tools/technologies
- Fetch linked resources from description
- Cross-reference with official documentation
- Verify current accuracy of instructions
</actions>
<output>Enriched knowledge with current context</output>
</phase>

<phase name="knowledge-synthesis">
<description>Compile into skill-ready format</description>
<actions>
- Structure findings using harvested-knowledge template
- Organize by concept and actionability
- Include resource links
- Add viewing recommendations
</actions>
<output>Complete harvested knowledge document</output>
</phase>
</process>

<youtube_specific_patterns>
<pattern name="video-types">
Recognize and adapt for:
- Tutorial: Focus on steps, commands, configurations
- Explainer: Focus on concepts, definitions, relationships
- Review: Focus on pros/cons, comparisons, recommendations
- Live coding: Focus on process, debugging, real-world patterns
- Conference talk: Focus on architecture, best practices, case studies
</pattern>

<pattern name="timestamp-chapters">
When chapters are present:
- Use as primary structure for notes
- Each chapter becomes a concept section
- Enables targeted re-watching recommendations
</pattern>

<pattern name="code-in-videos">
For videos with code:
- Check description for GitHub links
- Note repository names mentioned
- Extract visible code snippets if possible
- Document file names and structures shown
</pattern>
</youtube_specific_patterns>

<limitations>
<limitation>
Transcript may not be available for all videos. Fall back to:
- Detailed description parsing
- Title and metadata analysis
- WebSearch for video summaries/reviews
- Linked resource analysis
</limitation>

<limitation>
Visual-only content (diagrams, UI demos) cannot be directly extracted.
Note these as "visual demonstration" with context.
</limitation>
</limitations>

<tools>
Primary: WebFetch (for video page, description, transcript attempts)
Secondary: WebSearch (for supplementary context and tool documentation)
Support: Read (for any downloaded transcript files)
</tools>

<success_markers>
- Video metadata fully captured
- Description thoroughly parsed for resources
- Main concepts identified and structured
- Actionable steps extracted where present
- Linked resources cataloged
- Knowledge encoded in standard format
</success_markers>
</workflow>
