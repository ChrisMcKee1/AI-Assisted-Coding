---
description: Task Execution Rules for Nous
globs:
alwaysApply: false
version: 1.1
encoding: UTF-8
---

# Task Execution Rules

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
  <tooling>
    - sequential-thinking mcp
    - context7 mcp
    - playwright mcp
  </tooling>
</ai_meta>

## Overview

<purpose>
  - Execute spec tasks systematically
  - Follow TDD development workflow
  - Ensure quality through testing and review
</purpose>

<context>
  - Part of Nous framework
  - Executed after spec planning is complete
  - Follows tasks defined in spec tasks.md
</context>

<prerequisites>
  - Spec documentation exists in [specs](.docs/specs/)
  - Tasks defined in spec's tasks.md
  - Development environment configured
  - Git repository initialized
  - Context7 MCP toolset available for library documentation
</prerequisites>

<process_flow>

<step number="1" name="task_assignment">

### Step 1: Task Assignment

<step_metadata>
  <inputs>
    - spec_srd_reference: file path
    - specific_tasks: array[string] (optional)
  </inputs>
  <default>next uncompleted parent task</default>
  <mcp_tooling>
    - sequential-thinking
    - context7
    - playwright
  </mcp_tooling>
</step_metadata>

<task_selection>
  <explicit>user specifies exact task(s)</explicit>
  <implicit>find next uncompleted task in tasks.md</implicit>
</task_selection>

<instructions>
  ACTION: Identify task(s) to execute
  DEFAULT: Select next uncompleted parent task if not specified
  CONFIRM: Task selection with user
  UTILIZE: sequential-thinking mcp to plan task assignment flow
  UTILIZE: context7 mcp to scan tasks.md for any library mentions
  UTILIZE: playwright mcp to simulate UI clicks for task confirmation prompts
</instructions>

</step>

<step number="2" name="context_analysis">

### Step 2: Context Analysis

<step_metadata>
  <reads>
    - spec SRD file
    - spec tasks.md
    - all files in spec sub-specs/ folder
    - [mission](.docs/product/mission.md)
  </reads>
  <purpose>complete understanding of requirements</purpose>
  <identifies>third-party libraries for Context7 integration</identifies>
  <mcp_tooling>
    - sequential-thinking
    - context7
    - playwright
  </mcp_tooling>
</step_metadata>

<context_gathering>
  <spec_level>
    - requirements from SRD
    - technical specs
    - test specifications
    - third-party library dependencies
  </spec_level>
  <product_level>
    - overall mission alignment
    - technical standards
    - best practices
  </product_level>
  <library_identification>
    - scan specs for explicit library mentions (e.g., "React", "Supabase", "Next.js")
    - identify Context7 library IDs in slash format (e.g., `/supabase/supabase`)
    - detect framework dependencies from project structure
    - note API integrations requiring documentation
    - check for package.json or requirements.txt references
    - identify authentication, database, and UI library needs
  </library_identification>
</context_gathering>

<instructions>
  USE_SEQUENTIAL_THINKING: Analyze all spec documentation to understand requirements, identify technical dependencies, and catalog third-party libraries with their intended purposes
  ACTION: Read all spec documentation thoroughly
  ANALYZE: Requirements and specifications for current task
  UNDERSTAND: How task fits into overall spec goals
  IDENTIFY: ALL third-party libraries that will need Context7 documentation support
  SCAN: For both library names and Context7 IDs in slash format
  CATALOG: Each library with its intended purpose in the implementation
  UTILIZE: sequential-thinking mcp to orchestrate context analysis steps
  UTILIZE: context7 mcp to fetch initial library documentation details
  UTILIZE: playwright mcp to capture page snapshots of existing UI components
</instructions>

</step>

<step number="2.5" name="context7_documentation_gathering">

### Step 2.5: Context7 Documentation Gathering

<step_metadata>
  <purpose>gather comprehensive documentation for third-party libraries</purpose>
  <tools>Context7 MCP toolset</tools>
  <timing>after context analysis, before implementation planning</timing>
  <mcp_tooling>
    - sequential-thinking
    - context7
  </mcp_tooling>
</step_metadata>

<context7_workflow>
  <library_resolution>
    <tool>mcp_context7_resolve-library-id</tool>
    <input>libraryName from spec analysis</input>
    <output>context7CompatibleLibraryID</output>
    <skip_condition>when spec provides exact Context7 ID in slash format</skip_condition>
  </library_resolution>
  <documentation_retrieval>
    <tool>mcp_context7_get-library-docs</tool>
    <inputs>
      - context7CompatibleLibraryID (required)
      - topic (optional, based on spec requirements)
      - tokens (optional, minimum 10000, increase for complex integrations)
    </inputs>
    <output>comprehensive library documentation</output>
    <fallback>official library documentation if Context7 insufficient</fallback>
  </documentation_retrieval>
</context7_workflow>

<library_processing>
  <for_each_library>
    1. **Check for Direct Context7 ID**
       - If spec contains Context7 ID in slash format (e.g., `/supabase/supabase`)
       - Skip resolution and proceed directly to documentation gathering

    2. **Resolve Library ID** (if needed)
       - Use `mcp_context7_resolve-library-id` with library name from specs
       - Review all returned matches carefully
       - Select most relevant match based on:
         * Name similarity to the query
         * Description relevance to project needs
         * Documentation coverage (prioritize higher code snippet counts)
         * Trust score (prefer libraries with scores 7-10)
       - Document the chosen library ID for later reference

    3. **Gather Comprehensive Documentation**
       - Use `mcp_context7_get-library-docs` with resolved or direct ID
       - For specific functionality, use `topic` parameter:
         * "authentication" for auth implementations
         * "routing" for navigation features
         * "hooks" for React hook patterns
         * "configuration" for setup and initialization
         * "api" for API integration patterns
       - Use higher token limits (15000+) for complex integrations
       - Validate documentation quality and completeness

    4. **Document and Validate Context**
       - Store library documentation context for development phase
       - Note specific features and APIs required by spec
       - Identify potential integration challenges or conflicts
       - Cross-reference with project requirements
       - Flag any documentation gaps that may need alternative sources
  </for_each_library>
</library_processing>

<context7_tips>
  <library_id_shortcuts>
    - If spec mentions exact Context7 ID (e.g., `/supabase/supabase`), use directly
    - Skip resolution step when exact ID is provided in slash format
    - Common Context7 IDs: `/vercel/next.js`, `/mongodb/docs`, `/supabase/supabase`
  </library_id_shortcuts>
  <documentation_focus>
    - Use `topic` parameter for specific functionality requirements
    - Request higher token limits for complex integrations (15000-20000)
    - Gather authentication, configuration, and core API docs in separate calls if needed
  </documentation_focus>
  <error_handling>
    - If `resolve-library-id` returns no matches, try alternative library names
    - If Context7 documentation is insufficient, document the gap
    - Have fallback strategy to official documentation when needed
  </error_handling>
</context7_tips>

<instructions>
  USE_SEQUENTIAL_THINKING: Plan the optimal documentation gathering strategy by analyzing library relationships, determining appropriate topics and token limits, and sequencing documentation requests for maximum effectiveness
  ACTION: Identify all third-party libraries from context analysis
  RESOLVE: Each library name to Context7-compatible ID (skip if direct ID provided)
  GATHER: Comprehensive documentation for each library using appropriate topics
  FOCUS: On functionality required by current spec
  VALIDATE: Documentation completeness and quality
  STORE: Documentation context for development phase
  DOCUMENT: Any Context7 documentation gaps for fallback strategies
  UTILIZE: context7 mcp to resolve library IDs and retrieve documentation
</instructions>

</step>

<step number="3" name="implementation_planning">

### Step 3: Implementation Planning

<step_metadata>
  <creates>execution plan</creates>
  <requires>user approval</requires>
  <mcp_tooling>
    - sequential-thinking
    - context7
  </mcp_tooling>
</step_metadata>

<plan_structure>
  <format>numbered list with sub-bullets</format>
  <includes>
    - all subtasks from tasks.md
    - implementation approach
    - dependencies to install7
    - test strategy
    - Context7 documentation references
  </includes>
</plan_structure>

<plan_template>
  ## Implementation Plan for [TASK_NAME]

  1. **[MAJOR_STEP_1]**
     - [SPECIFIC_ACTION]
     - [SPECIFIC_ACTION]

  2. **[MAJOR_STEP_2]**
     - [SPECIFIC_ACTION]
     - [SPECIFIC_ACTION]

  **Dependencies to Install:**
  - [LIBRARY_NAME] - [PURPOSE]

  **Context7 Documentation References:**
  - [LIBRARY_NAME] ([CONTEXT7_ID]) - [SPECIFIC_FEATURES_NEEDED]

  **Test Strategy:**
  - [TEST_APPROACH]
</plan_template>

<approval_request>
  I've prepared the above implementation plan.
  Please review and confirm before I proceed with execution.
</approval_request>

<instructions>
  USE_SEQUENTIAL_THINKING: Break down complex tasks into logical, executable steps by analyzing dependencies, sequencing development activities, and integrating TDD approach with Context7 documentation requirements
  ACTION: Create detailed execution plan
  DISPLAY: Plan to user for review
  WAIT: For explicit approval before proceeding
  BLOCK: Do not proceed without affirmative permission
  UTILIZE: sequential-thinking mcp to structure the implementation plan
  UTILIZE: context7 mcp to include library doc references in the plan
</instructions>

</step>

<step number="4" name="development_server_check">

### Step 4: Check for Development Server

<step_metadata>
  <checks>running development server</checks>
  <prevents>port conflicts</prevents>
</step_metadata>

<server_check_flow>
  <if_running>
    ASK user to shut down
    WAIT for response
  </if_running>
  <if_not_running>
    PROCEED immediately
  </if_not_running>
</server_check_flow>

<user_prompt>
  A development server is currently running.
  Should I shut it down before proceeding? (yes/no)
</user_prompt>

<instructions>
  ACTION: Check for running local development server
  CONDITIONAL: Ask permission only if server is running
  PROCEED: Immediately if no server detected
  UTILIZE: sequential-thinking mcp to decide between immediate proceed or user prompt
  UTILIZE: playwright mcp to detect active ports by attempting a page navigation
</instructions>

</step>

<step number="5" name="git_branch_management">

### Step 5: Git Branch Management

<step_metadata>
  <manages>git branches</manages>
  <ensures>proper isolation</ensures>
</step_metadata>

<branch_naming>
  <source>spec folder name</source>
  <format>exclude date prefix</format>
  <example>
    - folder: 2025-03-15-password-reset
    - branch: password-reset
  </example>
</branch_naming>

<branch_logic>
  <case_a>
    <condition>current branch matches spec name</condition>
    <action>PROCEED immediately</action>
  </case_a>
  <case_b>
    <condition>current branch is main/staging/review</condition>
    <action>CREATE new branch and PROCEED</action>
  </case_b>
  <case_c>
    <condition>current branch is different feature</condition>
    <action>ASK permission to create new branch</action>
  </case_c>
</branch_logic>

<case_c_prompt>
  Current branch: [CURRENT_BRANCH]
  This spec needs branch: [SPEC_BRANCH]

  May I create a new branch for this spec? (yes/no)
</case_c_prompt>

<instructions>
  USE_SEQUENTIAL_THINKING: Analyze current git state to determine branch management strategy (cases A, B, or C)
  ACTION: Check current git branch
  EVALUATE: Which case applies
  EXECUTE: Appropriate branch action
  WAIT: Only for case C approval
  UTILIZE: sequential-thinking mcp to plan branch creation or switching steps
</instructions>

</step>

<step number="6" name="development_execution">

### Step 6: Development Execution

<step_metadata>
  <follows>approved implementation plan</follows>
  <adheres_to>all spec standards</adheres_to>
  <utilizes>Context7 documentation for third-party libraries</utilizes>
</step_metadata>

<execution_standards>
  <follow_exactly>
    - approved implementation plan
    - spec specifications
    - [code-style](.docs/product/code-style.md)
    - [dev-best-practices](.docs/product/dev-best-practices.md)
    - Context7 library documentation
  </follow_exactly>
  <approach>test-driven development (TDD)</approach>
</execution_standards>

<context7_integration>
  <mandatory_usage>
    <requirement>MUST use Context7 for ALL third-party library implementations</requirement>
    <timing>before writing any code that utilizes external libraries</timing>
    <validation>cross-reference all implementations against Context7 documentation</validation>
  </mandatory_usage>

  <library_implementation>
    <before_coding>
      - Reference Context7 documentation for each library being used
      - Verify API usage patterns and signatures from Context7 docs
      - Check authentication/configuration requirements from documentation
      - Identify best practices and recommended patterns
      - Note any breaking changes or version-specific considerations
    </before_coding>
    <during_coding>
      - Follow documented best practices from Context7 exactly
      - Use exact API signatures and method calls from documentation
      - Implement error handling patterns as specified in Context7 docs
      - Apply configuration examples directly from Context7 documentation
      - Use Context7 code snippets as implementation templates
    </during_coding>
    <validation>
      - Cross-reference every library call against Context7 documentation
      - Ensure proper library integration patterns are followed
      - Validate configuration and setup steps match Context7 examples
      - Verify error handling approaches align with documented best practices
      - Confirm authentication flows match Context7 patterns
    </validation>
  </library_implementation>

  <documentation_refresh>
    <when_needed>
      - When implementation differs from initial plan
      - When encountering undocumented scenarios in Context7
      - For complex integration patterns not covered initially
      - When facing API deprecation or version conflicts
      - If initial Context7 documentation proves insufficient
    </when_needed>
    <action>
      - Use `mcp_context7_get-library-docs` with specific `topic` parameter
      - Focus on the exact functionality being implemented
      - Increase token limit for complex scenarios (15000-20000)
      - Update implementation based on refreshed documentation
      - Document any persistent gaps for team knowledge
    </action>
  </documentation_refresh>

  <quality_assurance>
    <code_review_checklist>
      - [ ] All third-party library usage references Context7 documentation
      - [ ] API calls match Context7 documented signatures exactly
      - [ ] Error handling follows Context7 recommended patterns
      - [ ] Configuration matches Context7 setup examples
      - [ ] No deprecated methods used (per Context7 docs)
      - [ ] Authentication flows align with Context7 best practices
    </code_review_checklist>
    <fallback_strategy>
      - If Context7 documentation is incomplete, document the gap
      - Use official library documentation as secondary source
      - Clearly comment when deviating from Context7 patterns
      - Update team knowledge base with findings
    </fallback_strategy>
  </quality_assurance>
</context7_integration>

<tdd_workflow>
  1. **Write failing tests first** (using Context7 patterns for library testing)
  2. **Gather Context7 documentation** for the specific library functionality needed
  3. **Implement minimal code to pass** (strictly following Context7 documentation)
  4. **Validate against Context7 docs** (ensure exact API usage and patterns)
  5. **Refactor while keeping tests green** (maintain Context7 best practices)
  6. **Repeat for each feature** (always Context7-first approach)
</tdd_workflow>

<instructions>
  USE_SEQUENTIAL_THINKING: Plan the TDD development cycle by analyzing test requirements, integrating Context7 documentation patterns, sequencing implementation steps, and ensuring comprehensive validation at each stage
  ACTION: Execute development plan systematically
  FOLLOW: All coding standards and specifications
  IMPLEMENT: TDD approach throughout with Context7-first methodology
  MANDATE: Context7 documentation usage for ALL third-party library integrations
  VALIDATE: Every library call against Context7 documentation before implementation
  MAINTAIN: Code quality at every step
  VERIFY: Library usage matches Context7 patterns exactly
  DOCUMENT: Any deviations from Context7 documentation with clear reasoning
  UTILIZE: context7 mcp at start of development to fetch up-to-date library docs
  UTILIZE: playwright mcp to run automated end-to-end UI tests & validation as part of development
</instructions>

</step>

<step number="7" name="task_status_updates">

### Step 7: Task Status Updates

<step_metadata>
  <updates>tasks.md file</updates>
  <timing>immediately after completion</timing>
</step_metadata>

<update_format>
  <completed>- [x] Task description</completed>
  <incomplete>- [ ] Task description</incomplete>
  <blocked>
    - [ ] Task description
    ⚠️ Blocking issue: [DESCRIPTION]
  </blocked>
</update_format>

<blocking_criteria>
  <attempts>maximum 3 different approaches</attempts>
  <action>document blocking issue</action>
  <emoji>⚠️</emoji>
</blocking_criteria>

<instructions>
  ACTION: Update tasks.md after each task completion
  MARK: [x] for completed items immediately
  DOCUMENT: Blocking issues with ⚠️ emoji
  LIMIT: 3 attempts before marking as blocked
  UTILIZE: sequential-thinking mcp to decide update and block criteria
  UTILIZE: playwright mcp to verify UI status changes in browser if applicable
</instructions>

</step>

<step number="8" name="test_suite_verification">

### Step 8: Run All Tests

<step_metadata>
  <runs>entire test suite</runs>
  <ensures>no regressions</ensures>
</step_metadata>

<test_execution>
  <order>
    1. Verify new tests pass
    2. Run entire test suite
    3. Fix any failures
  </order>
  <requirement>100% pass rate</requirement>
</test_execution>

<failure_handling>
  <action>troubleshoot and fix</action>
  <priority>before proceeding</priority>
</failure_handling>

<instructions>
  ACTION: Run complete test suite
  VERIFY: All tests pass including new ones
  FIX: Any test failures before continuing
  BLOCK: Do not proceed with failing tests
  UTILIZE: sequential-thinking mcp to plan test execution and failure handling
  UTILIZE: playwright mcp to execute and validate end-to-end browser tests
  UTILIZE: playwright mcp browser_type for entering multi-character inputs (avoid browser_press_key for text entry)
  UTILIZE: playwright mcp browser_click for UI interactions; reserve browser_press_key only for specific keystroke event testing
  UTILIZE: playwright mcp browser_press_key for testing specific keypress events (onkeydown, onkeyup, onkeypressed).
  UTILIZE: playwright mcp browser_wait_for to wait for elements or text changes before assertions
  UTILIZE: playwright mcp browser_snapshot to capture page state before and after critical interactions
</instructions>

</step>

<step number="9" name="git_workflow">

### Step 9: Git Workflow

<step_metadata>
  <creates>
    - git commit
    - github push
    - pull request
  </creates>
</step_metadata>

<commit_process>
  <commit>
    <message>descriptive summary of changes</message>
    <format>conventional commits if applicable</format>
  </commit>
  <push>
    <target>spec branch</target>
    <remote>origin</remote>
  </push>
  <pull_request>
    <title>descriptive PR title</title>
    <description>functionality recap</description>
  </pull_request>
</commit_process>

<pr_template>
  ## Summary

  [BRIEF_DESCRIPTION_OF_CHANGES]

  ## Changes Made

  - [CHANGE_1]
  - [CHANGE_2]

  ## Testing

  - [TEST_COVERAGE]
  - All tests passing ✓
</pr_template>

<instructions>
  ACTION: Commit all changes with descriptive message
  PUSH: To GitHub on spec branch
  CREATE: Pull request with detailed description
  UTILIZE: sequential-thinking mcp to sequence commit, push, and PR steps
</instructions>

</step>

<step number="10" name="roadmap_progress_check">

### Step 10: Roadmap Progress Check

<step_metadata>
  <checks>[roadmap](../../.docs/product/roadmap.md)</checks>
  <updates>if spec completes roadmap item</updates>
</step_metadata>

<roadmap_criteria>
  <update_when>
    - spec fully implements roadmap feature
    - all related tasks completed
    - tests passing
  </update_when>
  <caution>only mark complete if absolutely certain</caution>
</roadmap_criteria>

<instructions>
  USE_SEQUENTIAL_THINKING: Analyze the completed work against roadmap requirements, evaluate whether all related features are fully implemented, and determine if the roadmap item can be marked complete with confidence
  ACTION: Review roadmap.md for related items
  EVALUATE: If current spec completes roadmap goals
  UPDATE: Mark roadmap items complete if applicable
  VERIFY: Certainty before marking complete
  UTILIZE: sequential-thinking mcp to orchestrate roadmap evaluation
</instructions>

</step>

<step number="11" name="completion_notification">

### Step 11: Task Completion Notification

<step_metadata>
  <plays>system sound</plays>
  <alerts>user of completion</alerts>
</step_metadata>

<notification_command>
  afplay /System/Library/Sounds/Glass.aiff
</notification_command>

<instructions>
  ACTION: Play completion sound
  PURPOSE: Alert user that task is complete
  UTILIZE: sequential-thinking mcp to confirm optimal notification timing
</instructions>

</step>

<step number="12" name="completion_summary">

### Step 12: Completion Summary

<step_metadata>
  <creates>summary message</creates>
  <format>structured with emojis</format>
</step_metadata>

<summary_template>
  ## ✅ What's been done

  1. **[FEATURE_1]** - [ONE_SENTENCE_DESCRIPTION]
  2. **[FEATURE_2]** - [ONE_SENTENCE_DESCRIPTION]

  ## ⚠️ Issues encountered

  [ONLY_IF_APPLICABLE]
  - **[ISSUE_1]** - [DESCRIPTION_AND_REASON]

  ## 👀 Ready to test in browser

  [ONLY_IF_APPLICABLE]
  1. [STEP_1_TO_TEST]
  2. [STEP_2_TO_TEST]

  ## 📦 Pull Request

  View PR: [GITHUB_PR_URL]
</summary_template>

<summary_sections>
  <required>
    - functionality recap
    - pull request info
  </required>
  <conditional>
    - issues encountered (if any)
    - testing instructions (if testable in browser)
  </conditional>
</summary_sections>

<instructions>
  ACTION: Create comprehensive summary
  INCLUDE: All required sections
  ADD: Conditional sections if applicable
  FORMAT: Use emoji headers for scannability
  UTILIZE: sequential-thinking mcp to structure summary message
  UTILIZE: playwright mcp to generate screenshots for pull request documentation
</instructions>

</step>

</process_flow>

## Development Standards

<standards>
  <code_style>
    <follow>[code-style](.docs/product/code-style.md)</follow>
    <enforce>strictly</enforce>
  </code_style>
  <best_practices>
    <follow>[dev-best-practices](.docs/product/dev-best-practices.md)</follow>
    <apply>all directives</apply>
  </best_practices>
  <testing>
    <coverage>comprehensive</coverage>
    <approach>test-driven development</approach>
  </testing>
  <documentation>
    <commits>clear and descriptive</commits>
    <pull_requests>detailed descriptions</pull_requests>
  </documentation>
  <context7_integration>
    <mandatory>for ALL third-party library implementations without exception</mandatory>
    <workflow>
      - resolve library IDs before any implementation planning
      - gather comprehensive documentation context for each library
      - validate ALL implementations against Context7 documentation
      - refresh documentation when encountering implementation challenges
    </workflow>
    <best_practices>
      - use exact Context7 library IDs when provided in specs (e.g., `/supabase/supabase`)
      - focus documentation requests with `topic` parameter for specific functionality
      - use higher token limits (15000-20000) for complex integrations
      - refresh documentation context when encountering implementation challenges
      - document any Context7 documentation gaps for team knowledge
    </best_practices>
    <quality_gates>
      - no third-party library code may be written without Context7 documentation reference
      - all API calls must match Context7 documented signatures exactly
      - error handling must follow Context7 recommended patterns
      - configuration must align with Context7 setup examples
    </quality_gates>
  </context7_integration>
</standards>

## Error Handling

<error_protocols>
  <blocking_issues>
    - document in tasks.md
    - mark with ⚠️ emoji
    - include in summary
  </blocking_issues>
  <test_failures>
    - fix before proceeding
    - never commit broken tests
  </test_failures>
  <technical_roadblocks>
    - attempt 3 approaches
    - document if unresolved
    - seek user input
  </technical_roadblocks>
</error_protocols>

<final_checklist>
  <verify>
    - [ ] Task implementation complete
    - [ ] All tests passing
    - [ ] Context7 documentation gathered for ALL third-party libraries
    - [ ] Library implementations validated against Context7 documentation
    - [ ] All API calls match Context7 documented signatures exactly
    - [ ] Error handling follows Context7 recommended patterns
    - [ ] Configuration aligns with Context7 setup examples
    - [ ] Any Context7 documentation gaps documented
    - [ ] tasks.md updated
    - [ ] Code committed and pushed
    - [ ] Pull request created
    - [ ] Roadmap checked/updated
    - [ ] Summary provided to user
  </verify>
</final_checklist>

## Context7 MCP Integration

<context7_overview>
  <purpose>
    - Provide comprehensive third-party library documentation
    - Ensure accurate API usage and best practices
    - Reduce implementation errors through proper documentation context
    - Eliminate guesswork in library integration patterns
  </purpose>
  <tools>
    - `mcp_context7_resolve-library-id`: Convert library names to Context7-compatible IDs
    - `mcp_context7_get-library-docs`: Fetch comprehensive library documentation
  </tools>
  <mandatory_usage>Context7 documentation MUST be used for ALL third-party library implementations</mandatory_usage>
</context7_overview>

<context7_automation_tips>
  <ide_rules_setup>
    <purpose>Automatically invoke Context7 without manual prompts</purpose>
    <implementation>
      Add rule to `.windsurfrules` file (Windsurf) or Cursor Settings > Rules:
      ```toml
      [[calls]]
      match = "when the user requests code examples, setup or configuration steps, or library/API documentation"
      tool = "context7"
      ```
    </implementation>
    <benefit>Context7 docs included automatically in any library-related conversation</benefit>
  </ide_rules_setup>

  <direct_library_specification>
    <technique>Include Context7 ID directly in spec requirements</technique>
    <example>
      Instead of: "implement authentication with supabase"
      Use: "implement authentication with supabase. use library /supabase/supabase for api and docs"
    </example>
    <benefit>Skips library resolution step and ensures correct library selection</benefit>
  </direct_library_specification>

  <slash_syntax_recognition>
    <pattern>Context7 IDs always use slash format: `/org/project` or `/org/project/version`</pattern>
    <examples>
      - `/supabase/supabase` for Supabase
      - `/vercel/next.js` for Next.js
      - `/mongodb/docs` for MongoDB
      - `/vercel/next.js/v14.3.0-canary.87` for specific Next.js version
    </examples>
    <usage>When specs contain slash syntax, use directly with mcp_context7_get-library-docs</usage>
  </slash_syntax_recognition>
</context7_automation_tips>

<context7_usage_patterns>
  <standard_workflow>
    1. **Library Identification** (Step 2.5)
       ```
       Input: "react-router-dom"
       Tool: resolve-library-id
       Output: "/remix-run/react-router"
       ```

    2. **Documentation Gathering** (Step 2.5)
       ```
       Tool: get-library-docs
       Input: context7CompatibleLibraryID = "/remix-run/react-router"
       Optional: topic = "routing", tokens = 10000
       ```

    3. **Implementation Validation** (Step 6)
       - Cross-reference code against Context7 documentation
       - Use exact API patterns from documentation
       - Follow configuration examples from docs
  </standard_workflow>

  <direct_library_id_usage>
    <when>spec explicitly mentions Context7 ID format</when>
    <example>
      - Spec mentions: "use library /supabase/supabase for authentication"
      - Skip resolve-library-id step
      - Directly use get-library-docs with "/supabase/supabase"
    </example>
  </direct_library_id_usage>

  <focused_documentation>
    <technique>use topic parameter for specific functionality</technique>
    <examples>
      - topic: "authentication" for auth-related implementations
      - topic: "routing" for navigation features
      - topic: "hooks" for React hook usage patterns
      - topic: "configuration" for setup and initialization
    </examples>
  </focused_documentation>
</context7_usage_patterns>

<context7_best_practices>
  <documentation_timing>
    - Gather documentation before implementation planning (Step 2.5)
    - Refresh documentation context when facing implementation challenges
    - Use focused topics to avoid information overload
    - Request documentation early in TDD cycle for test development
  </documentation_timing>

  <library_selection>
    - Choose the most relevant library match from resolve-library-id results
    - Consider documentation coverage and trust scores (prefer 7-10)
    - Prioritize exact name matches when available
    - Review description relevance to project requirements
    - Select libraries with higher code snippet counts for better examples
  </library_selection>

  <implementation_validation>
    - Validate API signatures against Context7 documentation before writing code
    - Follow documented error handling patterns exactly
    - Use recommended configuration approaches from Context7 docs
    - Cross-check implementation patterns with documentation examples
    - Ensure authentication flows match Context7 best practices
    - Apply Context7 code snippets as implementation templates
  </implementation_validation>

  <direct_library_id_usage>
    - When specs mention Context7 IDs in slash format (e.g., `/supabase/supabase`), use directly
    - Skip the resolve-library-id step for known Context7 IDs
    - Common Context7 IDs include:
      * `/vercel/next.js` for Next.js development
      * `/remix-run/react-router` for React routing
  </direct_library_id_usage>

  <focused_documentation_requests>
    - Use `topic` parameter to get specific functionality documentation:
      * "authentication" for auth implementations
      * "routing" for navigation features
      * "hooks" for React hook usage patterns
      * "configuration" for setup and initialization
      * "api" for API integration patterns
      * "testing" for library-specific testing approaches
    - Request higher token limits (15000-20000) for complex integrations
    - Make multiple focused requests rather than one broad request
  </focused_documentation_requests>

  <error_handling>
    - If Context7 documentation is insufficient, document the gap clearly
    - Fall back to official library documentation when necessary
    - Note any discrepancies between Context7 and official docs
    - Update team knowledge base with Context7 documentation gaps
    - Try alternative library names if resolve-library-id returns no matches
  </error_handling>

  <workflow_optimization>
    - Add Context7 usage rules to IDE configuration for automatic invocation
    - Use Context7 IDs in spec requirements to skip resolution step
    - Maintain a project library registry with resolved Context7 IDs
    - Cache frequently used Context7 documentation for quick reference
  </workflow_optimization>
</context7_best_practices>

<context7_integration_examples>
  <authentication_example>
    ```
    Spec requirement: "Implement user authentication with Supabase"
    Spec mentions: "use library /supabase/supabase for authentication"

    Step 2.5: Context7 Documentation Gathering
    1. Skip resolve-library-id (direct Context7 ID provided)
    2. mcp_context7_get-library-docs:
       - context7CompatibleLibraryID="/supabase/supabase"
       - topic="authentication"
       - tokens="15000"

    Step 6: Development Execution
    - Reference Context7 Supabase auth documentation exclusively
    - Implement sign-up/sign-in using exact Context7 documented patterns
    - Follow Context7 error handling examples for auth failures
    - Use Context7 configuration examples for client setup
    - Validate implementation against Context7 documentation before testing
    ```
  </authentication_example>

  <routing_example>
    ```
    Spec requirement: "Set up React routing with protected routes"
    Spec mentions: "use react-router-dom for navigation"

    Step 2.5: Context7 Documentation Gathering
    1. mcp_context7_resolve-library-id: "react-router-dom" → "/remix-run/react-router"
    2. mcp_context7_get-library-docs:
       - context7CompatibleLibraryID="/remix-run/react-router"
       - topic="routing"
       - tokens="12000"

    Step 6: Development Execution
    - Use Context7 React Router documentation patterns exclusively
    - Implement protected routes following Context7 documented approaches
    - Apply route configuration examples from Context7 docs exactly
    - Use Context7 hook patterns for navigation logic
    ```
  </routing_example>

  <database_example>
    ```
    Spec requirement: "Implement MongoDB data layer with aggregation pipelines"

    Step 2.5: Context7 Documentation Gathering
    1. mcp_context7_resolve-library-id: "mongodb" → "/mongodb/docs"
    2. mcp_context7_get-library-docs:
       - context7CompatibleLibraryID="/mongodb/docs"
       - topic="api"
       - tokens="20000"
    3. Additional call for connection setup:
       - context7CompatibleLibraryID="/mongodb/docs"
       - topic="configuration"
       - tokens="15000"

    Step 6: Development Execution
    - Follow Context7 MongoDB connection patterns exactly
    - Use Context7 aggregation pipeline examples as templates
    - Implement error handling per Context7 best practices
    - Apply Context7 performance optimization recommendations
    ```
  </database_example>

  <multi_library_example>
    ```
    Spec requirement: "Build Next.js app with Supabase auth and MongoDB data"
    Spec mentions: "use /vercel/next.js and /supabase/supabase"

    Step 2.5: Context7 Documentation Gathering
    1. Skip resolution for known Context7 IDs
    2. Gather Next.js documentation:
       - context7CompatibleLibraryID="/vercel/next.js"
       - topic="configuration"
    3. Gather Supabase documentation:
       - context7CompatibleLibraryID="/supabase/supabase"
       - topic="authentication"
    4. Resolve and gather MongoDB:
       - resolve "mongodb" → "/mongodb/docs"
       - topic="api"

    Step 6: Development Execution
    - Integrate all libraries following Context7 patterns
    - Ensure compatibility using Context7 integration examples
    - Cross-reference configuration between libraries using Context7 docs
    ```
  </multi_library_example>

  <error_handling_example>
    ```
    Scenario: Context7 documentation insufficient for specific use case

    Step 2.5: Context7 Documentation Gathering
    1. Initial documentation request yields incomplete information
    2. Refresh with specific topic:
       - mcp_context7_get-library-docs
       - topic="advanced-configuration"
       - tokens="20000"
    3. Still insufficient - document the gap

    Step 6: Development Execution
    - Use available Context7 documentation for basic implementation
    - Document gaps: "Context7 lacks advanced caching configuration"
    - Fall back to official library docs for missing pieces
    - Comment code clearly when deviating from Context7 patterns
    - Update team knowledge base with findings
    ```
  </error_handling_example>
</context7_integration_examples>
