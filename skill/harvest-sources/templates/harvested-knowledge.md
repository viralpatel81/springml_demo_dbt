<template name="harvested-knowledge">
<description>
Standard output template for all harvested sources. This format ensures consistency
and compatibility with skill creation workflows.
</description>

<usage>
Fill in each section based on the harvested content. Remove sections that don't apply
to the specific source type. Add custom sections if the source requires it.
</usage>

<template_content>
```xml
<harvested_source>
  <metadata>
    <source_url>{URL of the harvested source}</source_url>
    <source_type>{github|youtube|blog|n8n|documentation|forum|other}</source_type>
    <harvested_date>{YYYY-MM-DD}</harvested_date>
    <title>{Source title or name}</title>
    <author>{Author/creator if known}</author>
    <last_updated>{Last update date if known}</last_updated>
  </metadata>

  <summary>
    {2-3 paragraph high-level overview of what this source contains and teaches.
    Focus on the main value proposition and key takeaways.}
  </summary>

  <key_concepts>
    <concept name="{concept-name-1}">
      <definition>{Clear definition of the concept}</definition>
      <importance>{Why this concept matters}</importance>
      <related>{Related concepts or prerequisites}</related>
    </concept>

    <concept name="{concept-name-2}">
      <definition>{Clear definition of the concept}</definition>
      <importance>{Why this concept matters}</importance>
      <related>{Related concepts or prerequisites}</related>
    </concept>

    <!-- Add more concepts as needed -->
  </key_concepts>

  <patterns>
    <pattern name="{pattern-name-1}">
      <description>{What this pattern accomplishes}</description>
      <when_to_use>{Situations where this pattern applies}</when_to_use>
      <implementation>
        {Step-by-step implementation guidance}
      </implementation>
      <example>
        {Concrete example from the source}
      </example>
      <gotchas>
        {Common pitfalls or edge cases}
      </gotchas>
    </pattern>

    <!-- Add more patterns as needed -->
  </patterns>

  <code_snippets>
    <snippet name="{snippet-name}" language="{language}">
      <purpose>{What this code does}</purpose>
      <prerequisites>{Required imports, setup, dependencies}</prerequisites>
      <code>
{actual code here - preserve formatting}
      </code>
      <usage_notes>{How to adapt this code}</usage_notes>
    </snippet>

    <!-- Add more snippets as needed -->
  </code_snippets>

  <configurations>
    <config name="{config-name}">
      <file>{Configuration file name}</file>
      <purpose>{What this configuration controls}</purpose>
      <content>
{configuration content}
      </content>
      <options>
        <option name="{option}" default="{default}">{description}</option>
        <!-- More options -->
      </options>
    </config>

    <!-- Add more configurations as needed -->
  </configurations>

  <resources>
    <resource type="{documentation|tool|repository|article}">
      <name>{Resource name}</name>
      <url>{Resource URL}</url>
      <description>{Why this resource is valuable}</description>
    </resource>

    <!-- Add more resources as needed -->
  </resources>

  <implementation_guidance>
    <prerequisites>
      {What needs to be in place before implementing}
    </prerequisites>

    <steps>
      <step order="1">{First implementation step}</step>
      <step order="2">{Second implementation step}</step>
      <!-- More steps -->
    </steps>

    <verification>
      {How to verify successful implementation}
    </verification>

    <troubleshooting>
      <issue problem="{common problem}">
        <solution>{How to resolve}</solution>
      </issue>
      <!-- More issues -->
    </troubleshooting>
  </implementation_guidance>

  <skill_ready>
    <suggested_skill_name>{verb-noun format skill name}</suggested_skill_name>
    <suggested_description>
      {Description following skill description guidelines - what it does AND when to use it}
    </suggested_description>
    <core_capabilities>
      <capability>{Primary capability 1}</capability>
      <capability>{Primary capability 2}</capability>
      <!-- More capabilities -->
    </core_capabilities>
    <suggested_workflow>
      {High-level workflow steps for the skill}
    </suggested_workflow>
    <integration_points>
      {How this skill could integrate with other skills or tools}
    </integration_points>
  </skill_ready>

  <notes>
    <note type="{limitation|caveat|update|opinion}">
      {Additional notes about the harvested content}
    </note>
    <!-- More notes as needed -->
  </notes>
</harvested_source>
```
</template_content>

<section_guidelines>
<guideline section="metadata">
Always complete. Use "unknown" for missing fields rather than omitting.
</guideline>

<guideline section="summary">
Write for someone who hasn't seen the source. Include the "so what" - why does this matter?
</guideline>

<guideline section="key_concepts">
Limit to 5-10 most important concepts. Quality over quantity.
</guideline>

<guideline section="patterns">
Abstract from specific implementations to reusable patterns. Include concrete examples.
</guideline>

<guideline section="code_snippets">
Only include code that's directly reusable. Add context about adaptation needs.
</guideline>

<guideline section="skill_ready">
This section is the bridge to skill creation. Be specific about capabilities.
</guideline>
</section_guidelines>
</template>
