---
description: Architecture Diagram Creation Rules for Nous
globs:
alwaysApply: false
version: 1.0
encoding: UTF-8
---

# Architecture Diagram Creation Rules

<ai_meta>
  <parsing_rules>
    - Process XML blocks first for structured data
    - Execute instructions in sequential order
    - Use templates as exact patterns
    - Request missing data rather than assuming
  </parsing_rules>
  <file_conventions>
    - encoding: UTF-8
    - line_endings: LF
    - indent: 2 spaces
    - markdown_headers: no indentation
  </file_conventions>
</ai_meta>

## Overview

<purpose>
  - Generate comprehensive architecture diagrams for software systems
  - Create visual documentation using MDX, Markdown, and Mermaid
  - Establish consistent diagramming standards across projects
</purpose>

<context>
  - Part of Nous framework
  - Triggered when architectural visualization is needed
  - Output used by developers, architects, and stakeholders
</context>

<prerequisites>
  - Write access to project root
  - Understanding of system architecture
  - User has architectural requirements or system description
  - Access to Mermaid diagram tools and validation
</prerequisites>

<process_flow>

<step number="1" name="gather_diagram_requirements">

### Step 1: Gather Diagram Requirements

<step_metadata>
  <required_inputs>
    - diagram_type: enum["system", "component", "deployment", "data_flow", "sequence", "class"]
    - system_description: string
    - components: array[object] (minimum: 2)
    - relationships: array[object] (minimum: 1)
    - output_format: enum["mermaid", "mdx", "markdown"]
  </required_inputs>
  <validation>blocking</validation>
</step_metadata>

<data_sources>
  <primary>user_direct_input</primary>
  <fallback_sequence>
    1. [tech-stack](.docs/product/tech-stack.md)
    2. [mission](.docs/product/mission.md)
    3. [GitHub Copilot Instructions](.github/copilot-instructions.md)
  </fallback_sequence>
</data_sources>

<error_template>
  Please provide the following missing information:
  1. What type of architecture diagram do you need? (system, component, deployment, data_flow, sequence, class)
  2. Brief description of the system or architecture
  3. List of main components/services (minimum 2)
  4. How these components relate to each other (minimum 1 relationship)
  5. Preferred output format (mermaid, mdx, markdown)
</error_template>

<instructions>
  ACTION: Collect all required inputs from user
  VALIDATION: Ensure all 5 inputs provided before proceeding
  FALLBACK: Check project documentation for system context
  ERROR: Use error_template if inputs missing
</instructions>

</step>

<step number="2" name="determine_diagram_strategy">

### Step 2: Determine Diagram Strategy

<step_metadata>
  <creates>
    - strategy: object
    - mermaid_type: string
  </creates>
</step_metadata>

<diagram_type_mapping>
  <system>
    <mermaid_type>architecture-beta</mermaid_type>
    <focus>high-level system overview</focus>
    <components>services, databases, external systems</components>
  </system>
  <component>
    <mermaid_type>flowchart</mermaid_type>
    <focus>internal component relationships</focus>
    <components>modules, classes, interfaces</components>
  </component>
  <deployment>
    <mermaid_type>architecture-beta</mermaid_type>
    <focus>infrastructure and hosting</focus>
    <components>servers, containers, networks</components>
  </deployment>
  <data_flow>
    <mermaid_type>flowchart</mermaid_type>
    <focus>data movement and processing</focus>
    <components>data stores, processors, flows</components>
  </data_flow>
  <sequence>
    <mermaid_type>sequenceDiagram</mermaid_type>
    <focus>interaction flow over time</focus>
    <components>actors, systems, messages</components>
  </sequence>
  <class>
    <mermaid_type>classDiagram</mermaid_type>
    <focus>object-oriented structure</focus>
    <components>classes, interfaces, relationships</components>
  </class>
</diagram_type_mapping>

<instructions>
  ACTION: Map user diagram_type to appropriate Mermaid diagram type
  VALIDATE: Ensure chosen type matches user requirements
  DOCUMENT: Record strategy for consistent application
</instructions>

</step>

<step number="3" name="create_directory_structure">

### Step 3: Create Directory Structure

<step_metadata>
  <creates>
    - directory: .docs/architecture/
    - files: 1-3
  </creates>
</step_metadata>

<file_structure>
  .docs/
  └── architecture/
      ├── [DIAGRAM_NAME].md         # Markdown with embedded mermaid diagrams file
      ├── [DIAGRAM_NAME].mermaid    # Pure Mermaid (if requested)
      └── [DIAGRAM_NAME].mdx        # MDX with components (if requested)
</file_structure>

<git_config>
  <commit_message>Add architecture diagram: [DIAGRAM_NAME]</commit_message>
  <tag>arch-[DIAGRAM_NAME]-v1.0.0</tag>
  <gitignore_consideration>false</gitignore_consideration>
</git_config>

<instructions>
  ACTION: Create directory structure as specified
  VALIDATION: Verify write permissions before creating
  PROTECTION: Confirm before overwriting existing files
</instructions>

</step>

<step number="4" name="generate_mermaid_diagram">

### Step 4: Generate Mermaid Diagram

<step_metadata>
  <creates>
    - diagram_code: string
  </creates>
  <validation>syntax_check</validation>
</step_metadata>

<mermaid_templates>
  <architecture>
    <template>
      architecture-beta
          [GROUP_DEFINITIONS]
          [SERVICE_DEFINITIONS]
          [EDGE_DEFINITIONS]
    </template>
    <group_syntax>group {id}({icon})[{title}] (in {parent})?</group_syntax>
    <service_syntax>service {id}({icon})[{title}] (in {parent})?</service_syntax>
    <edge_syntax>{serviceId}:{T|B|L|R} {<}?--{>}? {T|B|L|R}:{serviceId}</edge_syntax>
  </architecture>
  <flowchart>
    <template>
      flowchart {direction}
          [NODE_DEFINITIONS]
          [LINK_DEFINITIONS]
    </template>
    <node_syntax>{id}["{label}"] or {id}({shape})["{label}"]</node_syntax>
    <link_syntax>{nodeA} --> {nodeB} or {nodeA} -->|label| {nodeB}</link_syntax>
  </flowchart>
  <sequence>
    <template>
      sequenceDiagram
          [PARTICIPANT_DEFINITIONS]
          [MESSAGE_DEFINITIONS]
    </template>
    <participant_syntax>participant {alias} as {name}</participant_syntax>
    <message_syntax>{participant1} ->> {participant2}: {message}</message_syntax>
  </sequence>
  <class>
    <template>
      classDiagram
          [CLASS_DEFINITIONS]
          [RELATIONSHIP_DEFINITIONS]
    </template>
    <class_syntax>class {className} { [METHODS_AND_PROPERTIES] }</class_syntax>
    <relationship_syntax>{classA} --|> {classB} : {relationship}</relationship_syntax>
  </class>
</mermaid_templates>

<icon_guidelines>
  <architecture_icons>
    - cloud: cloud services, APIs
    - database: data storage
    - disk: file storage
    - internet: external connections
    - server: compute resources
  </architecture_icons>
  <flowchart_shapes>
    - rectangle: standard process
    - rounded: start/end points
    - diamond: decision points
    - circle: connectors
    - cylinder: databases
  </flowchart_shapes>
</icon_guidelines>

<instructions>
  ACTION: Generate Mermaid code using appropriate template
  APPLY: Use components and relationships from Step 1
  VALIDATE: Check syntax before proceeding
  OPTIMIZE: Ensure clear visual hierarchy and flow
</instructions>

</step>

<step number="5" name="validate_mermaid_syntax">

### Step 5: Validate Mermaid Syntax

<step_metadata>
  <validates>
    - mermaid_code: string
  </validates>
  <error_handling>retry_on_failure</error_handling>
</step_metadata>

<validation_rules>
  <syntax_check>
    - Proper diagram type declaration
    - Valid node/service definitions
    - Correct edge/link syntax
    - Proper grouping structure
  </syntax_check>
  <semantic_check>
    - All referenced IDs are defined
    - Edge connections are logical
    - Icon names are valid
    - Labels are clear and concise
  </semantic_check>
</validation_rules>

<error_correction>
  <common_fixes>
    - Fix missing quotes around labels
    - Correct invalid icon names
    - Resolve undefined node references
    - Fix malformed edge syntax
  </common_fixes>
</error_correction>

<instructions>
  ACTION: Use mermaid-diagram-validator tool to check syntax
  RETRY: Fix any validation errors and re-validate
  DOCUMENT: Log any corrections made
  CONTINUE: Only proceed when validation passes
</instructions>

</step>

<step number="6" name="create_markdown_file">

### Step 6: Create Main Markdown File

<step_metadata>
  <creates>
    - file: .docs/architecture/[DIAGRAM_NAME].md
  </creates>
</step_metadata>

<file_template>
  <header>
    # [DIAGRAM_TITLE]

    > Last Updated: [CURRENT_DATE]
    > Version: 1.0.0
    > Type: Architecture Diagram
  </header>
  <required_sections>
    - Overview
    - Architecture Diagram
    - Components
    - Data Flow
    - Deployment Notes
  </required_sections>
</file_template>

<section name="overview">
  <template>
    ## Overview

    [SYSTEM_DESCRIPTION]

    ### Purpose
    - [PRIMARY_PURPOSE]
    - [SECONDARY_PURPOSE]

    ### Scope
    - **Included:** [INCLUDED_COMPONENTS]
    - **Excluded:** [EXCLUDED_COMPONENTS]
  </template>
</section>

<section name="diagram">
  <template>
    ## Architecture Diagram

    ```mermaid
    [VALIDATED_MERMAID_CODE]
    ```

    ### Diagram Legend
    - [SYMBOL_1]: [MEANING_1]
    - [SYMBOL_2]: [MEANING_2]
  </template>
</section>

<section name="components">
  <template>
    ## Components

    ### [COMPONENT_CATEGORY]

    **[COMPONENT_NAME]**
    - **Purpose:** [COMPONENT_PURPOSE]
    - **Technology:** [TECH_STACK]
    - **Responsibilities:** [KEY_RESPONSIBILITIES]
    - **Dependencies:** [DEPENDENCIES]
  </template>
</section>

<section name="data_flow">
  <template>
    ## Data Flow

    ### [FLOW_NAME]
    1. [STEP_1]: [DESCRIPTION]
    2. [STEP_2]: [DESCRIPTION]
    3. [STEP_3]: [DESCRIPTION]

    **Data Formats:** [FORMATS]
    **Security Considerations:** [SECURITY_NOTES]
  </template>
</section>

<section name="deployment">
  <template>
    ## Deployment Notes

    ### Environment Requirements
    - [REQUIREMENT_1]
    - [REQUIREMENT_2]

    ### Scaling Considerations
    - [SCALING_POINT_1]
    - [SCALING_POINT_2]

    ### Monitoring Points
    - [MONITORING_1]: [METRICS]
    - [MONITORING_2]: [METRICS]
  </template>
</section>

<instructions>
  ACTION: Create comprehensive markdown file using all sections
  POPULATE: Use data from previous steps to fill templates
  FORMAT: Maintain consistent markdown structure
  EMBED: Include validated Mermaid diagram code
</instructions>

</step>

<step number="7" name="create_optional_formats">

### Step 7: Create Optional Output Formats

<step_metadata>
  <creates>
    - file: .docs/architecture/[DIAGRAM_NAME].mermaid (if requested)
    - file: .docs/architecture/[DIAGRAM_NAME].mdx (if requested)
  </creates>
  <conditional>based_on_output_format_request</conditional>
</step_metadata>

<mermaid_file_template>
  <content>
    [VALIDATED_MERMAID_CODE]
  </content>
  <comment_header>
    %% [DIAGRAM_TITLE]
    %% Generated: [CURRENT_DATE]
    %% Version: 1.0.0
  </comment_header>
</mermaid_file_template>

<mdx_file_template>
  <imports>
    import { Mermaid } from '@components/Mermaid'
    import { ArchitectureCard } from '@components/ArchitectureCard'
  </imports>
  <content>
    # [DIAGRAM_TITLE]

    <ArchitectureCard title="[TITLE]" description="[DESCRIPTION]">
      <Mermaid>
        {`[VALIDATED_MERMAID_CODE]`}
      </Mermaid>
    </ArchitectureCard>

    ## Interactive Components

    [INTERACTIVE_ELEMENTS]
  </content>
</mdx_file_template>

<instructions>
  ACTION: Create additional format files if requested
  VALIDATE: Ensure proper syntax for each format
  CONSISTENCY: Maintain same diagram content across formats
  OPTIMIZE: Add format-specific enhancements
</instructions>

</step>

<step number="8" name="preview_and_finalize">

### Step 8: Preview and Finalize

<step_metadata>
  <validates>
    - diagram_rendering: boolean
    - file_completeness: boolean
  </validates>
  <creates>
    - preview_link: string
  </creates>
</step_metadata>

<preview_checklist>
  <rendering>
    - [ ] Diagram renders without errors
    - [ ] All components are visible
    - [ ] Relationships are clear
    - [ ] Labels are readable
  </rendering>
  <content>
    - [ ] Documentation is complete
    - [ ] Technical details are accurate
    - [ ] Examples are relevant
    - [ ] Links work correctly
  </content>
</preview_checklist>

<finalization_steps>
  1. Use mermaid-diagram-preview tool to render diagram
  2. Review visual output for clarity and accuracy
  3. Validate all file formats are syntactically correct
  4. Confirm documentation completeness
  5. Generate git commit with descriptive message
</finalization_steps>

<instructions>
  ACTION: Preview diagram using mermaid-diagram-preview tool
  VALIDATE: Check all created files for completeness
  REVIEW: Ensure diagram meets user requirements
  FINALIZE: Prepare files for version control
</instructions>

</step>

</process_flow>

## Execution Summary

<final_checklist>
  <verify>
    - [ ] User requirements gathered and validated
    - [ ] Appropriate diagram type selected
    - [ ] Directory structure created
    - [ ] Mermaid diagram generated and validated
    - [ ] Main markdown documentation created
    - [ ] Optional formats created (if requested)
    - [ ] Diagram previewed and verified
    - [ ] All files ready for commit
  </verify>
</final_checklist>

<execution_order>
  1. Gather and validate diagram requirements
  2. Determine optimal diagram strategy
  3. Create directory structure
  4. Generate Mermaid diagram code
  5. Validate Mermaid syntax thoroughly
  6. Create comprehensive markdown documentation
  7. Create additional formats if requested
  8. Preview and finalize all outputs
</execution_order>

<quality_standards>
  <diagram_quality>
    - Clear visual hierarchy
    - Logical component grouping
    - Appropriate level of detail
    - Consistent styling
  </diagram_quality>
  <documentation_quality>
    - Complete component descriptions
    - Clear data flow explanations
    - Relevant technical details
    - Actionable deployment notes
  </documentation_quality>
</quality_standards>
