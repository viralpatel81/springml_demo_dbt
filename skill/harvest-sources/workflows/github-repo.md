<workflow name="github-repo-analysis">
<objective>
Systematically analyze a GitHub repository to extract its structure, patterns, code architecture, and reusable components. Transform this knowledge into a format suitable for skill creation.
</objective>

<trigger>
Activated when source URL matches `github.com/*` pattern or user explicitly selects GitHub repository analysis.
</trigger>

<process>
<phase name="initial-reconnaissance">
<description>Gather high-level repository information</description>
<actions>
- Fetch repository main page via WebFetch
- Extract: repo name, description, primary language, stars, forks
- Identify: README location, documentation folders, license
- Note: last update date, contributor count, activity level
</actions>
<output>Repository metadata summary</output>
</phase>

<phase name="structure-mapping">
<description>Map the complete repository structure</description>
<actions>
- Fetch directory tree (use GitHub API or web scraping)
- Identify key directories: src/, lib/, docs/, tests/, examples/
- Locate configuration files: package.json, requirements.txt, Cargo.toml, etc.
- Find workflow files: .github/workflows/, Makefile, scripts/
- Map entry points: main files, index files, CLI entry points
</actions>
<output>Directory structure with annotations</output>
</phase>

<phase name="documentation-extraction">
<description>Parse all documentation thoroughly</description>
<actions>
- Fetch and parse README.md completely
- Extract installation instructions
- Document usage examples
- Identify configuration options
- Parse any docs/ folder contents
- Extract inline code documentation patterns
</actions>
<output>Consolidated documentation summary</output>
</phase>

<phase name="code-pattern-analysis">
<description>Identify architectural and coding patterns</description>
<actions>
- Analyze main source files for architecture patterns
- Identify: design patterns used (factory, singleton, observer, etc.)
- Document: error handling approaches
- Extract: API structures and interfaces
- Note: testing patterns and strategies
- Identify: dependency injection or configuration patterns
</actions>
<output>Pattern catalog with examples</output>
</phase>

<phase name="dependency-mapping">
<description>Catalog all dependencies and integrations</description>
<actions>
- Parse dependency manifests (package.json, requirements.txt, etc.)
- Categorize: runtime vs dev dependencies
- Identify: key integrations (APIs, services, databases)
- Note: version constraints and compatibility
</actions>
<output>Dependency tree with purposes</output>
</phase>

<phase name="reusable-extraction">
<description>Extract reusable components and snippets</description>
<actions>
- Identify utility functions and helpers
- Extract configuration templates
- Document reusable patterns with context
- Capture workflow automations
- Note CLI commands and scripts
</actions>
<output>Reusable component library</output>
</phase>

<phase name="knowledge-synthesis">
<description>Synthesize findings into skill-ready format</description>
<actions>
- Consolidate all phases into structured output
- Apply harvested-knowledge template
- Generate skill suggestions
- Document implementation guidance
</actions>
<output>Complete harvested knowledge document</output>
</phase>
</process>

<github_specific_patterns>
<pattern name="readme-sections">
Common README sections to extract:
- Overview / Description
- Installation / Getting Started
- Usage / Examples
- Configuration / Options
- API Reference
- Contributing
- License
</pattern>

<pattern name="repo-types">
Recognize and adapt analysis for:
- Library/Package: Focus on API, exports, usage patterns
- CLI Tool: Focus on commands, flags, configuration
- Web App: Focus on routes, components, state management
- API Service: Focus on endpoints, authentication, data models
- Framework: Focus on conventions, lifecycle, extensibility
- Plugin/Extension: Focus on hooks, integration points
</pattern>

<pattern name="config-files">
Key configuration files to analyze:
- .env.example - Environment variables
- config/*.json - Application configuration
- .github/workflows/*.yml - CI/CD patterns
- Dockerfile - Containerization approach
- docker-compose.yml - Service orchestration
</pattern>
</github_specific_patterns>

<tools>
Primary: WebFetch (for GitHub pages and raw files)
Secondary: Task with Explore agent (for deep codebase analysis if cloned)
Support: WebSearch (for additional context on dependencies/patterns)
</tools>

<success_markers>
- Repository structure fully mapped
- README and docs comprehensively parsed
- Key code patterns identified with examples
- Dependencies cataloged with purposes
- Reusable components extracted
- Knowledge encoded in standard format
</success_markers>
</workflow>
