---
description: Generate Status Report for Nous Spec-Driven Development
globs:
alwaysApply: false
version: 1.0
encoding: UTF-8
---

# Generate Status Report for Nous Framework

<ai_meta>
  <parsing_rules>
    - Process XML blocks first for structured data
    - Execute instructions in sequential order
    - Parse all spec documents systematically
    - Aggregate data before generating report
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
  - Generate comprehensive project status reports
  - Analyze all specification documents in .docs/specs/
  - Provide architectural overview of system progress
  - Track completion status across all features
</purpose>

<context>
  - Part of Nous framework
  - Provides high-level project visibility
  - Assists architects and project managers
  - Consolidates distributed spec information
</context>

<prerequisites>
  - Product documentation exists in .docs/product/
  - One or more spec folders exist in .docs/specs/
  - Access to:
    - [mission](../../.docs/product/mission.md)
    - [roadmap](../../.docs/product/roadmap.md)
    - [tech-stack](../../.docs/product/tech-stack.md)
    - [decisions](../../.docs/product/decisions.md)
</prerequisites>

<process_flow>

<step number="1" name="discover_documentation">

### Step 1: Discover Documentation Structure

<step_metadata>
  <scans>
    - .docs/product/ directory
    - .docs/specs/ directory
  </scans>
  <catalogs>all available documentation files</catalogs>
</step_metadata>

<discovery_tasks>
  <product_documents>
    - mission.md: product vision and goals
    - roadmap.md: planned features and phases
    - tech-stack.md: technology decisions
    - decisions.md: architectural decisions
    - code-style.md: development standards
  </product_documents>
  <spec_folders>
    - Pattern: YYYY-MM-DD-spec-name/
    - Contents: spec.md, tasks.md, sub-specs/
    - Status indicators in task checkboxes
  </spec_folders>
</discovery_tasks>

<folder_structure_analysis>
  <expected_structure>
    .docs/
    ├── product/
    │   ├── mission.md
    │   ├── roadmap.md
    │   ├── tech-stack.md
    │   ├── decisions.md
    │   └── code-style.md
    └── specs/
        ├── YYYY-MM-DD-spec-name-1/
        │   ├── spec.md
        │   ├── tasks.md
        │   └── sub-specs/
        │       ├── technical-spec.md
        │       ├── api-spec.md
        │       ├── database-schema.md
        │       └── tests.md
        └── YYYY-MM-DD-spec-name-2/
            └── ...
  </expected_structure>
</folder_structure_analysis>

<instructions>
  ACTION: Scan .docs/ directory structure
  CATALOG: All found documentation files
  IDENTIFY: Missing expected documents
  PREPARE: File list for systematic parsing
</instructions>

</step>

<step number="2" name="parse_product_documentation">

### Step 2: Parse Product Documentation

<step_metadata>
  <reads>core product documents</reads>
  <extracts>strategic context</extracts>
</step_metadata>

<parsing_targets>
  <mission_analysis>
    - Product vision statement
    - Target user personas
    - Core value propositions
    - Success metrics
  </mission_analysis>
  <roadmap_analysis>
    - Completed phases
    - Current phase progress
    - Upcoming features
    - Timeline estimates
  </roadmap_analysis>
  <tech_stack_analysis>
    - Current technologies
    - Architecture decisions
    - Infrastructure setup
    - Development tools
  </tech_stack_analysis>
  <decisions_log>
    - Strategic decisions made
    - Technical debt items
    - Architectural changes
    - Process improvements
  </decisions_log>
</parsing_targets>

<data_extraction>
  <mission_data>
    - vision: string
    - target_users: array[string]
    - value_props: array[string]
  </mission_data>
  <roadmap_data>
    - phases: array[{name, status, features}]
    - current_phase: string
    - completion_percentage: number
  </roadmap_data>
  <tech_data>
    - frontend: string
    - backend: string
    - database: string
    - infrastructure: array[string]
  </tech_data>
  <decisions_data>
    - decisions: array[{id, date, title, status, category}]
    - recent_decisions: array[decision]
  </decisions_data>
</data_extraction>

<instructions>
  ACTION: Read and parse all product documentation
  EXTRACT: Structured data from each document
  VALIDATE: Data completeness and consistency
  STORE: Parsed data for report generation
</instructions>

</step>

<step number="3" name="analyze_spec_folders">

### Step 3: Analyze Specification Folders

<step_metadata>
  <iterates>through all spec folders</iterates>
  <extracts>status and progress data</extracts>
</step_metadata>

<spec_analysis_process>
  <folder_processing>
    FOR each folder in .docs/specs/:
      1. PARSE folder name for date and spec name
      2. READ spec.md for requirements and scope
      3. READ tasks.md for completion status
      4. READ sub-specs/ for technical details
      5. CALCULATE completion percentage
      6. IDENTIFY blocked or overdue items
  </folder_processing>
</spec_analysis_process>

<spec_data_extraction>
  <basic_info>
    - spec_name: string
    - creation_date: date
    - status: enum[Planning, InProgress, Completed, Blocked]
    - priority: enum[Low, Medium, High, Critical]
  </basic_info>
  <requirements_data>
    - overview: string
    - user_stories: array[string]
    - scope_items: array[string]
    - out_of_scope: array[string]
    - deliverables: array[string]
  </requirements_data>
  <progress_data>
    - total_tasks: number
    - completed_tasks: number
    - in_progress_tasks: number
    - blocked_tasks: number
    - completion_percentage: number
  </progress_data>
  <technical_data>
    - new_dependencies: array[string]
    - database_changes: boolean
    - api_changes: array[string]
    - testing_requirements: array[string]
  </technical_data>
</spec_data_extraction>

<status_determination>
  <status_rules>
    - Planning: tasks.md not created or no tasks checked
    - InProgress: some tasks checked, some unchecked
    - Completed: all tasks checked
    - Blocked: explicit blocking indicators or overdue
  </status_rules>
  <completion_calculation>
    completion_percentage = (checked_tasks / total_tasks) * 100
  </completion_calculation>
  <priority_inference>
    - High: affects core user workflows
    - Medium: enhances existing features
    - Low: nice-to-have improvements
    - Critical: security or stability issues
  </priority_inference>
</status_determination>

<instructions>
  ACTION: Process each spec folder systematically
  EXTRACT: All relevant data points
  CALCULATE: Progress metrics and status
  IDENTIFY: Issues and blockers
</instructions>

</step>

<step number="4" name="identify_system_patterns">

### Step 4: Identify System-Wide Patterns

<step_metadata>
  <analyzes>cross-spec patterns</analyzes>
  <identifies>architectural trends</identifies>
</step_metadata>

<pattern_analysis>
  <technology_trends>
    - Most frequently used technologies
    - New technology adoption patterns
    - Deprecated or replaced technologies
    - Infrastructure evolution
  </technology_trends>
  <feature_patterns>
    - Feature types being developed
    - User story patterns
    - Common integration points
    - Recurring technical requirements
  </feature_patterns>
  <development_patterns>
    - Average spec completion time
    - Common blocking factors
    - Test coverage trends
    - Quality metrics
  </development_patterns>
  <architectural_evolution>
    - Database schema evolution
    - API design patterns
    - Service architecture changes
    - Performance optimization trends
  </architectural_evolution>
</pattern_analysis>

<risk_assessment>
  <technical_risks>
    - Outdated dependencies
    - Architectural debt
    - Performance bottlenecks
    - Security concerns
  </technical_risks>
  <project_risks>
    - Consistently blocked specs
    - Resource allocation issues
    - Timeline slippage patterns
    - Scope creep indicators
  </project_risks>
</risk_assessment>

<instructions>
  ACTION: Analyze patterns across all specs
  IDENTIFY: System-wide trends and risks
  EVALUATE: Architectural health
  HIGHLIGHT: Areas needing attention
</instructions>

</step>

<step number="5" name="generate_executive_summary">

### Step 5: Generate Executive Summary

<step_metadata>
  <creates>high-level overview</creates>
  <targets>architects and managers</targets>
</step_metadata>

<summary_components>
  <project_health>
    - Overall completion percentage
    - Active specs count
    - Velocity metrics
    - Quality indicators
  </project_health>
  <key_metrics>
    - Features completed this month
    - Features in progress
    - Blocked items requiring attention
    - Technical debt indicators
  </key_metrics>
  <strategic_alignment>
    - Mission alignment assessment
    - Roadmap progress evaluation
    - Resource allocation effectiveness
    - Timeline adherence
  </strategic_alignment>
</summary_components>

<executive_summary_template>
  # Project Status Executive Summary

  > Generated: [CURRENT_DATE]
  > Report Period: [DATE_RANGE]
  > Project Health: [HEALTH_INDICATOR]

  ## Key Metrics

  - **Total Specifications**: [TOTAL_SPECS]
  - **Completed**: [COMPLETED_COUNT] ([COMPLETION_PERCENTAGE]%)
  - **In Progress**: [IN_PROGRESS_COUNT]
  - **Blocked**: [BLOCKED_COUNT]
  - **Average Completion Time**: [AVG_DAYS] days

  ## Strategic Alignment

  **Mission Progress**: [ALIGNMENT_ASSESSMENT]
  **Roadmap Adherence**: [TIMELINE_STATUS]
  **Resource Utilization**: [EFFICIENCY_METRIC]

  ## Immediate Actions Required

  1. [ACTION_ITEM_1]
  2. [ACTION_ITEM_2]
  3. [ACTION_ITEM_3]
</executive_summary_template>

<health_indicators>
  <green>
    - >80% specs on track
    - <5% blocked items
    - Strong velocity trend
  </green>
  <yellow>
    - 60-80% specs on track
    - 5-15% blocked items
    - Declining velocity
  </yellow>
  <red>
    - <60% specs on track
    - >15% blocked items
    - Critical issues present
  </red>
</health_indicators>

<instructions>
  ACTION: Generate executive summary
  CALCULATE: Key performance metrics
  ASSESS: Overall project health
  RECOMMEND: Priority actions
</instructions>

</step>

<step number="6" name="create_detailed_status_sections">

### Step 6: Create Detailed Status Sections

<step_metadata>
  <creates>comprehensive status report</creates>
  <organizes>by functional areas</organizes>
</step_metadata>

<report_sections>
  <active_specifications>
    - Current work in progress
    - Completion status
    - Estimated completion dates
    - Resource requirements
  </active_specifications>
  <completed_features>
    - Recently completed specs
    - Delivered value
    - Performance metrics
    - User impact assessment
  </completed_features>
  <blocked_items>
    - Blocked specifications
    - Blocking factors
    - Resolution strategies
    - Priority recommendations
  </blocked_items>
  <upcoming_work>
    - Next planned specifications
    - Resource requirements
    - Dependencies
    - Timeline estimates
  </upcoming_work>
</report_sections>

<section_templates>
  <active_specs_template>
    ## Active Specifications

    ### [SPEC_NAME] ([COMPLETION_PERCENTAGE]%)
    **Created**: [DATE] | **Priority**: [PRIORITY] | **ETA**: [ESTIMATED_DATE]

    **Scope**: [BRIEF_DESCRIPTION]

    **Progress**:
    - ✅ [COMPLETED_TASK_COUNT] completed
    - 🔄 [IN_PROGRESS_TASK_COUNT] in progress
    - ⏳ [PENDING_TASK_COUNT] pending

    **Next Actions**: [NEXT_STEPS]

    ---
  </active_specs_template>

  <completed_features_template>
    ## Recently Completed Specifications

    ### [SPEC_NAME] ✅
    **Completed**: [DATE] | **Duration**: [DAYS] days

    **Delivered**:
    - [DELIVERABLE_1]
    - [DELIVERABLE_2]

    **Impact**: [USER_IMPACT_SUMMARY]

    ---
  </completed_features_template>

  <blocked_items_template>
    ## Blocked Specifications ⚠️

    ### [SPEC_NAME]
    **Blocked Since**: [DATE] | **Priority**: [PRIORITY]

    **Blocking Factor**: [BLOCKING_REASON]

    **Impact**: [IMPACT_DESCRIPTION]

    **Proposed Resolution**: [RESOLUTION_STRATEGY]

    **Owner**: [RESPONSIBLE_PARTY]

    ---
  </blocked_items_template>
</section_templates>

<instructions>
  ACTION: Create detailed status sections
  ORGANIZE: By functional categories
  PROVIDE: Actionable information
  HIGHLIGHT: Critical items needing attention
</instructions>

</step>

<step number="7" name="generate_technical_insights">

### Step 7: Generate Technical Insights

<step_metadata>
  <analyzes>technical patterns and trends</analyzes>
  <provides>architectural guidance</provides>
</step_metadata>

<technical_analysis>
  <architecture_evolution>
    - Database schema changes
    - API design patterns
    - Service architecture updates
    - Integration patterns
  </architecture_evolution>
  <technology_adoption>
    - New dependencies introduced
    - Technology stack evolution
    - Performance optimization patterns
    - Security enhancements
  </technology_adoption>
  <code_quality_trends>
    - Test coverage evolution
    - Code complexity trends
    - Technical debt accumulation
    - Refactoring opportunities
  </code_quality_trends>
  <performance_metrics>
    - Feature development velocity
    - Bug introduction rates
    - Deployment frequency
    - Mean time to resolution
  </performance_metrics>
</technical_analysis>

<technical_insights_template>
  ## Technical Insights

  ### Architecture Evolution

  **Database Changes**:
  - [SCHEMA_CHANGE_SUMMARY]
  - Impact on existing systems: [IMPACT_ASSESSMENT]

  **API Evolution**:
  - New endpoints: [ENDPOINT_COUNT]
  - Breaking changes: [BREAKING_CHANGES]
  - Version strategy: [VERSIONING_APPROACH]

  ### Technology Stack Updates

  **New Dependencies**:
  - [DEPENDENCY_1]: [JUSTIFICATION]
  - [DEPENDENCY_2]: [JUSTIFICATION]

  **Deprecated Technologies**:
  - [DEPRECATED_1]: [REPLACEMENT_PLAN]
  - [DEPRECATED_2]: [MIGRATION_STATUS]

  ### Quality Metrics

  **Test Coverage**: [COVERAGE_PERCENTAGE]% ([TREND])
  **Technical Debt**: [DEBT_ASSESSMENT]
  **Performance**: [PERFORMANCE_TREND]

  ### Recommendations

  1. **[RECOMMENDATION_1]**: [RATIONALE]
  2. **[RECOMMENDATION_2]**: [RATIONALE]
  3. **[RECOMMENDATION_3]**: [RATIONALE]
</technical_insights_template>

<instructions>
  ACTION: Analyze technical patterns across specs
  GENERATE: Architectural insights
  PROVIDE: Technical recommendations
  HIGHLIGHT: Quality and performance trends
</instructions>

</step>

<step number="8" name="create_timeline_visualization">

### Step 8: Create Timeline Visualization

<step_metadata>
  <creates>project timeline overview</creates>
  <visualizes>progress and milestones</visualizes>
</step_metadata>

<timeline_components>
  <historical_progress>
    - Completed specifications by month
    - Major milestones achieved
    - Decision points and pivots
    - Performance trend lines
  </historical_progress>
  <current_state>
    - Active work streams
    - Current sprint progress
    - Immediate deliverables
    - Resource allocation
  </current_state>
  <future_projections>
    - Planned specifications
    - Estimated delivery dates
    - Resource requirements
    - Risk factors
  </future_projections>
</timeline_components>

<timeline_template>
  ## Project Timeline

  ### Historical Progress (Last 6 Months)

  ```
  [MONTH-6] |████████░░| 80% - [SPECS_COMPLETED] specs completed
  [MONTH-5] |██████████| 100% - [SPECS_COMPLETED] specs completed
  [MONTH-4] |██████░░░░| 60% - [SPECS_COMPLETED] specs completed
  [MONTH-3] |████████░░| 80% - [SPECS_COMPLETED] specs completed
  [MONTH-2] |██████████| 100% - [SPECS_COMPLETED] specs completed
  [MONTH-1] |███████░░░| 70% - [SPECS_COMPLETED] specs completed
  ```

  ### Current Month Progress

  ```
  Week 1: ████████░░ 80% complete
  Week 2: ██████░░░░ 60% complete
  Week 3: ████░░░░░░ 40% complete
  Week 4: ░░░░░░░░░░ 0% planned
  ```

  ### Upcoming Milestones

  - **[DATE]**: [MILESTONE_1]
  - **[DATE]**: [MILESTONE_2]
  - **[DATE]**: [MILESTONE_3]

  ### Velocity Metrics

  - **Average Specs/Month**: [VELOCITY]
  - **Trend**: [IMPROVING/STABLE/DECLINING]
  - **Capacity Utilization**: [UTILIZATION_PERCENTAGE]%
</timeline_template>

<instructions>
  ACTION: Create timeline visualization
  CALCULATE: Historical velocity metrics
  PROJECT: Future completion estimates
  HIGHLIGHT: Key milestones and risks
</instructions>

</step>

<step number="9" name="generate_recommendations">

### Step 9: Generate Strategic Recommendations

<step_metadata>
  <analyzes>patterns and risks</analyzes>
  <provides>actionable recommendations</provides>
</step_metadata>

<recommendation_categories>
  <immediate_actions>
    - Critical blockers to resolve
    - Overdue specifications
    - Resource reallocation needs
    - Process improvements
  </immediate_actions>
  <strategic_initiatives>
    - Architectural improvements
    - Technology upgrades
    - Capacity planning
    - Quality enhancements
  </strategic_initiatives>
  <risk_mitigation>
    - Technical debt reduction
    - Knowledge sharing needs
    - Dependency management
    - Performance optimization
  </risk_mitigation>
</recommendation_categories>

<recommendation_template>
  ## Strategic Recommendations

  ### Immediate Actions (Next 2 Weeks)

  #### 🔴 Critical
  1. **[ACTION_1]**
     - **Issue**: [PROBLEM_DESCRIPTION]
     - **Impact**: [IMPACT_ASSESSMENT]
     - **Solution**: [RECOMMENDED_ACTION]
     - **Owner**: [RESPONSIBLE_PARTY]
     - **Timeline**: [TIMEFRAME]

  #### 🟡 Important
  2. **[ACTION_2]**
     - **Issue**: [PROBLEM_DESCRIPTION]
     - **Solution**: [RECOMMENDED_ACTION]
     - **Timeline**: [TIMEFRAME]

  ### Strategic Initiatives (Next Quarter)

  #### Architecture & Technology
  - **[INITIATIVE_1]**: [DESCRIPTION_AND_RATIONALE]
  - **[INITIATIVE_2]**: [DESCRIPTION_AND_RATIONALE]

  #### Process Improvements
  - **[IMPROVEMENT_1]**: [DESCRIPTION_AND_EXPECTED_OUTCOME]
  - **[IMPROVEMENT_2]**: [DESCRIPTION_AND_EXPECTED_OUTCOME]

  ### Risk Mitigation

  #### Technical Risks
  - **[RISK_1]**: [MITIGATION_STRATEGY]
  - **[RISK_2]**: [MITIGATION_STRATEGY]

  #### Project Risks
  - **[RISK_1]**: [MITIGATION_STRATEGY]
  - **[RISK_2]**: [MITIGATION_STRATEGY]
</recommendation_template>

<prioritization_matrix>
  <high_impact_high_effort>Strategic initiatives</high_impact_high_effort>
  <high_impact_low_effort>Quick wins - prioritize first</high_impact_low_effort>
  <low_impact_high_effort>Avoid unless strategic</low_impact_high_effort>
  <low_impact_low_effort>Consider if resources available</low_impact_low_effort>
</prioritization_matrix>

<instructions>
  ACTION: Generate prioritized recommendations
  CATEGORIZE: By urgency and impact
  PROVIDE: Clear action plans
  ASSIGN: Responsible parties and timelines
</instructions>

</step>

<step number="10" name="compile_final_report">

### Step 10: Compile Final Status Report

<step_metadata>
  <assembles>all report sections</assembles>
  <creates>comprehensive document</creates>
</step_metadata>

<report_structure>
  <header_section>
    - Report metadata
    - Executive summary
    - Key metrics dashboard
  </header_section>
  <status_sections>
    - Active specifications
    - Completed features
    - Blocked items
    - Upcoming work
  </status_sections>
  <analysis_sections>
    - Technical insights
    - Timeline visualization
    - Pattern analysis
    - Risk assessment
  </analysis_sections>
  <action_sections>
    - Strategic recommendations
    - Immediate actions
    - Resource requirements
    - Success metrics
  </action_sections>
</report_structure>

<final_report_template>
  # Project Status Report

  > **Generated**: [CURRENT_DATE]  
  > **Period**: [REPORT_PERIOD]  
  > **Next Review**: [NEXT_REVIEW_DATE]

  ## Executive Summary

  [EXECUTIVE_SUMMARY_CONTENT]

  ## Project Health Dashboard

  | Metric | Current | Target | Trend |
  |--------|---------|---------|-------|
  | Completion Rate | [RATE]% | [TARGET]% | [TREND] |
  | Active Specs | [COUNT] | [CAPACITY] | [TREND] |
  | Blocked Items | [COUNT] | 0 | [TREND] |
  | Velocity | [SPECS_PER_MONTH] | [TARGET] | [TREND] |

  ## Current Status

  [ACTIVE_SPECIFICATIONS_CONTENT]

  [COMPLETED_FEATURES_CONTENT]

  [BLOCKED_ITEMS_CONTENT]

  ## Technical Analysis

  [TECHNICAL_INSIGHTS_CONTENT]

  ## Timeline & Progress

  [TIMELINE_VISUALIZATION_CONTENT]

  ## Strategic Recommendations

  [RECOMMENDATIONS_CONTENT]

  ## Appendices

  ### A. Specification Inventory
  [COMPLETE_SPEC_LIST]

  ### B. Technical Debt Register
  [TECHNICAL_DEBT_ITEMS]

  ### C. Decision Log Summary
  [RECENT_DECISIONS]

  ---

  **Report Prepared By**: Nous Framework  
  **Next Status Review**: [NEXT_REVIEW_DATE]
</final_report_template>

<report_metadata>
  <filename>status-report-YYYY-MM-DD.md</filename>
  <location>.docs/reports/</location>
  <retention>keep last 12 reports</retention>
  <format>markdown with tables and charts</format>
</report_metadata>

<instructions>
  ACTION: Compile all sections into final report
  VALIDATE: Report completeness and accuracy
  FORMAT: Professional document structure
  SAVE: To .docs/reports/ directory
</instructions>

</step>

</process_flow>

## Report Quality Standards

<quality_standards>
  <accuracy>
    - Data must be current and verified
    - Calculations must be correct
    - Status assessments must be objective
  </accuracy>
  <completeness>
    - All specs must be analyzed
    - No critical information omitted
    - Recommendations must be actionable
  </completeness>
  <clarity>
    - Executive summary for leadership
    - Technical details for architects
    - Action items clearly prioritized
  </clarity>
  <timeliness>
    - Report reflects current state
    - Trends based on recent data
    - Projections include latest changes
  </timeliness>
</quality_standards>

## Error Handling

<error_scenarios>
  <missing_files>
    <condition>Expected documentation files not found</condition>
    <action>Note missing files and continue with available data</action>
  </missing_files>
  <corrupted_data>
    <condition>Spec files have invalid format</condition>
    <action>Report parsing issues and skip corrupted files</action>
  </corrupted_data>
  <empty_directories>
    <condition>No spec folders found</condition>
    <action>Generate minimal report with recommendations to create specs</action>
  </empty_directories>
</error_scenarios>

## Execution Summary

<final_checklist>
  <verify>
    - [ ] All .docs/specs/ folders analyzed
    - [ ] Product documentation parsed
    - [ ] Status calculations accurate
    - [ ] Recommendations prioritized
    - [ ] Report format professional
    - [ ] Action items clearly defined
    - [ ] Technical insights provided
    - [ ] Timeline visualization created
  </verify>
</final_checklist>

<success_criteria>
  - Architects have clear project visibility
  - Blockers and risks clearly identified
  - Progress trends accurately represented
  - Actionable recommendations provided
  - Technical health assessed
  - Resource needs identified
</success_criteria>
