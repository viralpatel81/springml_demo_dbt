<workflow name="n8n-workflow-decoding">
<objective>
Decode n8n automation workflows to extract integration patterns, node configurations, and automation logic. Transform into reusable knowledge for skill creation.
</objective>

<trigger>
Activated when source URL matches `*.n8n.io/*`, contains n8n workflow JSON, or user explicitly selects n8n workflow analysis.
</trigger>

<process>
<phase name="workflow-retrieval">
<description>Fetch and parse workflow definition</description>
<actions>
- Fetch workflow page or JSON via WebFetch
- Parse workflow structure (nodes, connections)
- Extract: workflow name, description, tags
- Identify: trigger type (manual, webhook, schedule, etc.)
- Note: workflow complexity (node count, branches)
</actions>
<output>Workflow metadata and structure overview</output>
</phase>

<phase name="node-cataloging">
<description>Catalog all nodes and their purposes</description>
<actions>
- List all nodes in execution order
- For each node extract:
  - Node type (HTTP Request, Function, IF, etc.)
  - Node name and purpose
  - Key configuration parameters
  - Credential requirements
- Group nodes by category (triggers, actions, logic, output)
</actions>
<output>Node catalog with configurations</output>
</phase>

<phase name="flow-mapping">
<description>Map data flow and logic paths</description>
<actions>
- Trace execution paths from trigger to outputs
- Identify: branching logic (IF nodes, Switch nodes)
- Map: data transformations between nodes
- Note: error handling paths
- Document: loops and iterations
</actions>
<output>Execution flow diagram (textual)</output>
</phase>

<phase name="integration-extraction">
<description>Extract integration patterns</description>
<actions>
- List all external services connected
- Document: API endpoints used
- Extract: authentication patterns
- Note: rate limiting considerations
- Identify: webhook configurations
</actions>
<output>Integration pattern catalog</output>
</phase>

<phase name="code-extraction">
<description>Extract custom code and expressions</description>
<actions>
- Parse Function nodes for JavaScript code
- Extract: expression syntax used in fields
- Document: data transformation logic
- Note: custom error handling code
- Capture: utility functions defined
</actions>
<output>Code snippet library from workflow</output>
</phase>

<phase name="pattern-abstraction">
<description>Abstract reusable automation patterns</description>
<actions>
- Identify: common node combinations
- Abstract: error handling patterns
- Extract: data validation patterns
- Document: notification patterns
- Note: scheduling patterns
</actions>
<output>Reusable automation pattern library</output>
</phase>

<phase name="knowledge-synthesis">
<description>Compile into skill-ready format</description>
<actions>
- Apply harvested-knowledge template
- Structure by automation capability
- Include node configurations
- Add implementation guidance
</actions>
<output>Complete harvested knowledge document</output>
</phase>
</process>

<n8n_specific_patterns>
<pattern name="common-triggers">
Recognize trigger patterns:
- Webhook: External event-driven
- Schedule: Time-based automation
- Manual: User-initiated
- Email: Inbox monitoring
- Database: Data change triggers
</pattern>

<pattern name="node-categories">
Group nodes for analysis:
- Core: IF, Switch, Merge, Function, Set
- Communication: Email, Slack, Discord, Telegram
- Data: HTTP Request, Databases, Spreadsheets
- Storage: S3, Google Drive, Dropbox
- Development: Git, GitHub, GitLab
- AI: OpenAI, Anthropic, Custom LLM
</pattern>

<pattern name="data-transformation">
Common transformation patterns:
- JSON parsing and construction
- Array operations (map, filter, reduce)
- Date/time formatting
- String manipulation
- Data merging from multiple sources
</pattern>

<pattern name="error-handling">
Error handling patterns:
- Try/Catch with Error Trigger
- Fallback paths
- Retry logic
- Notification on failure
- Logging and monitoring
</pattern>
</n8n_specific_patterns>

<credential_handling>
IMPORTANT: Never extract or expose actual credentials.
Document only:
- Credential type required
- Required permissions/scopes
- Configuration pattern
- Environment variable recommendations
</credential_handling>

<tools>
Primary: WebFetch (for workflow pages and JSON)
Secondary: WebSearch (for n8n documentation, node references)
Support: Read (for downloaded workflow files)
</tools>

<success_markers>
- Workflow structure fully parsed
- All nodes cataloged with purposes
- Execution flow mapped clearly
- Integrations documented
- Custom code extracted
- Patterns abstracted for reuse
- Knowledge encoded in standard format
</success_markers>
</workflow>
