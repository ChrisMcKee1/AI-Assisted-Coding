---
description: Blazor Component Creation Rules for Nous
globs:
alwaysApply: false
version: 2.0
encoding: UTF-8
---

# Blazor Component Creation Rules

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
  - Create well-structured Blazor components following spec-driven development
  - Implement proper separation of concerns with .razor, .razor.cs, and .razor.css files
  - Ensure comprehensive testing with bUnit framework
  - Follow Blazor best practices and performance optimization
</purpose>

<context>
  - Part of Nous framework
  - Executed when Blazor component creation is required
  - Follows spec-driven development methodology
  - Integrates with existing project structure and standards
</context>

<prerequisites>
  - Spec documentation exists in [specs](.docs/specs/)
  - Tasks defined in spec's tasks.md
  - .NET project with Blazor framework
  - Access to project structure and dependencies
  - Understanding of component requirements
</prerequisites>

<process_flow>

<step number="1" name="gather_component_requirements">

### Step 1: Gather Component Requirements

<step_metadata>
  <required_inputs>
    - component_name: string
    - component_purpose: string
    - parameters: array[object] (minimum: 0)
    - events: array[object] (minimum: 0)
    - render_mode: enum["Static", "InteractiveServer", "InteractiveWebAssembly", "InteractiveAuto"]
    - ui_description: string
  </required_inputs>
  <validation>blocking</validation>
</step_metadata>

<data_sources>
  <primary>user_direct_input</primary>
  <fallback_sequence>
    1. [spec file](.docs/specs/[SPEC_NAME]/spec.md)
    2. [tasks file](.docs/specs/[SPEC_NAME]/tasks.md)
    3. [mission](.docs/product/mission.md)
    4. [tech-stack](.docs/product/tech-stack.md)
  </fallback_sequence>
</data_sources>

<error_template>
  Please provide the following missing information:
  1. Component name (e.g., UserProfileCard, OrderFormComponent)
  2. Component purpose and functionality description
  3. Input parameters (name, type, required/optional, description)
  4. Events the component should emit (name, data type, purpose)
  5. Render mode preference (Static, InteractiveServer, InteractiveWebAssembly, InteractiveAuto)
  6. UI description or sketch of component appearance
  7. Data requirements (if component needs to fetch/display data)
</error_template>

<instructions>
  ACTION: Collect all required inputs from user
  VALIDATION: Ensure all inputs provided before proceeding
  FALLBACK: Check spec documentation for component context
  ERROR: Use error_template if inputs missing
</instructions>

</step>

<step number="2" name="analyze_project_structure">

### Step 2: Analyze Project Structure

<step_metadata>
  <analyzes>
    - project_type: string
    - dotnet_version: string
    - component_location: string
    - bunit_availability: boolean
  </analyzes>
</step_metadata>

<project_analysis>
  <component_location>
    <blazor_server>Components/ or Pages/</blazor_server>
    <blazor_webassembly>Components/ or Pages/</blazor_webassembly>
    <blazor_web_app>Components/ or Pages/</blazor_web_app>
    <fallback>Components/</fallback>
  </component_location>
  <dotnet_version_detection>
    <check_files>
      - *.csproj files for TargetFramework
      - global.json for SDK version
    </check_files>
  </dotnet_version_detection>
</project_analysis>

<bunit_detection>
  <check_for>
    - Test project with bUnit NuGet package
    - Existing component tests
    - Test project structure
  </check_for>
  <missing_action>request_permission_to_create</missing_action>
</bunit_detection>

<instructions>
  ACTION: Analyze project structure and configuration
  DETECT: .NET version, Blazor project type, and component location
  CHECK: For existing bUnit test project
  PREPARE: Component creation strategy based on findings
</instructions>

</step>

<step number="3" name="determine_render_mode">

### Step 3: Determine Render Mode Strategy

<step_metadata>
  <determines>
    - optimal_render_mode: string
    - performance_implications: array[string]
    - prerendering_strategy: string
  </determines>
</step_metadata>

<render_mode_decision_matrix>
  <static_server>
    <use_when>
      - No interactivity required
      - Pure display components
      - SEO-critical content
    </use_when>
    <benefits>Fast initial load, SEO-friendly</benefits>
  </static_server>
  <interactive_server>
    <use_when>
      - Real-time updates needed
      - Complex state management
      - Server-side data processing
    </use_when>
    <benefits>Low client payload, server processing power</benefits>
  </interactive_server>
  <interactive_webassembly>
    <use_when>
      - Offline capability required
      - Client-side heavy processing
      - Reduced server load
    </use_when>
    <benefits>Offline support, client-side processing</benefits>
  </interactive_webassembly>
  <interactive_auto>
    <use_when>
      - Best of both worlds needed
      - Progressive enhancement
      - Flexible deployment
    </use_when>
    <benefits>Fast initial load, then client-side performance</benefits>
  </interactive_auto>
</render_mode_decision_matrix>

<streaming_rendering>
  <use_when>
    - Component has async data loading
    - Improved perceived performance needed
    - Server-side rendering with async operations
  </use_when>
  <attribute>[StreamRendering(true)]</attribute>
</streaming_rendering>

<instructions>
  ACTION: Determine optimal render mode based on requirements
  EVALUATE: Performance implications and trade-offs
  DOCUMENT: Reasoning for render mode selection
  CONSIDER: Streaming rendering for async operations
</instructions>

</step>

<step number="4" name="create_component_structure">

### Step 4: Create Component File Structure

<step_metadata>
  <creates>
    - component.razor: markup and directives
    - component.razor.cs: code-behind logic
    - component.razor.css: isolated styles
  </creates>
</step_metadata>

<file_templates>
  <razor_file>
    <template>
      @using Microsoft.AspNetCore.Components
      @namespace [PROJECT_NAMESPACE].Components
      @inherits [COMPONENT_NAME]Base

      @* Render mode directive *@
      @rendermode [RENDER_MODE]

      @* Streaming rendering if applicable *@
      @attribute [StreamRendering(true)]

      <div class="[component-name-kebab]">
          [COMPONENT_MARKUP]
      </div>
    </template>
  </razor_file>
  <code_behind_file>
    <template>
      using Microsoft.AspNetCore.Components;

      namespace [PROJECT_NAMESPACE].Components;

      public partial class [COMPONENT_NAME] : ComponentBase
      {
          [PARAMETERS]
          [EVENTS]
          [PRIVATE_FIELDS]
          [LIFECYCLE_METHODS]
          [EVENT_HANDLERS]
          [PRIVATE_METHODS]
      }
    </template>
  </code_behind_file>
  <css_file>
    <template>
      /* [COMPONENT_NAME] Component Styles */
      .[component-name-kebab] {
          /* Component root styles */
      }

      /* Component-specific styles */
      .[component-name-kebab] .element {
          /* Element styles */
      }
    </template>
  </css_file>
</file_templates>

<blazor_best_practices>
  <component_design>
    - Follow Single Responsibility Principle
    - Keep components small and focused
    - Use proper parameter validation with [EditorRequired]
    - Implement EventCallback<T> for component events
  </component_design>
  <performance>
    - Override ShouldRender() when appropriate
    - Use @key directive for dynamic lists
    - Consider <Virtualize> for large datasets
    - Minimize unnecessary re-renders
  </performance>
  <accessibility>
    - Include proper ARIA attributes
    - Ensure keyboard navigation support
    - Use semantic HTML elements
    - Provide meaningful alt text for images
  </accessibility>
</blazor_best_practices>

<instructions>
  ACTION: Create three separate files following templates
  IMPLEMENT: Blazor best practices throughout
  ENSURE: Proper separation of concerns
  OPTIMIZE: For performance and accessibility
</instructions>

</step>

<step number="5" name="implement_component_logic">

### Step 5: Implement Component Logic

<step_metadata>
  <implements>
    - parameter_definitions: based on requirements
    - event_callbacks: for component interactions
    - lifecycle_methods: as needed
    - business_logic: in code-behind
  </implements>
</step_metadata>

<parameter_patterns>
  <basic_parameter>
    [Parameter] public string PropertyName { get; set; } = string.Empty;
  </basic_parameter>
  <required_parameter>
    [Parameter, EditorRequired] public string RequiredProperty { get; set; } = default!;
  </required_parameter>
  <cascading_parameter>
    [CascadingParameter] public ThemeContext Theme { get; set; } = default!;
  </cascading_parameter>
</parameter_patterns>

<event_patterns>
  <simple_event>
    [Parameter] public EventCallback OnItemClicked { get; set; }
  </simple_event>
  <typed_event>
    [Parameter] public EventCallback<ItemSelectedEventArgs> OnItemSelected { get; set; }
  </typed_event>
</event_patterns>

<lifecycle_methods>
  <on_initialized>
    protected override void OnInitialized()
    {
        // Component initialization logic
    }
  </on_initialized>
  <on_parameters_set>
    protected override void OnParametersSet()
    {
        // Logic when parameters change
    }
  </on_parameters_set>
  <on_after_render>
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            // First render logic
        }
    }
  </on_after_render>
</lifecycle_methods>

<state_management>
  <local_state>Use private fields and properties</local_state>
  <shared_state>Consider scoped services or state containers</shared_state>
  <persistent_state>Use PersistentComponentState for cross-render persistence</persistent_state>
</state_management>

<instructions>
  ACTION: Implement component logic in code-behind file
  FOLLOW: Parameter and event patterns
  ADD: Appropriate lifecycle methods
  CONSIDER: State management requirements
</instructions>

</step>

<step number="6" name="create_component_styles">

### Step 6: Create Component Styles

<step_metadata>
  <creates>
    - isolated_css: component-specific styles
    - responsive_design: mobile-first approach
    - theme_integration: consistent with app theme
  </creates>
</step_metadata>

<css_architecture>
  <isolation>
    <scope>component-level only</scope>
    <naming>kebab-case class names</naming>
    <organization>logical grouping of related styles</organization>
  </isolation>
  <responsive_design>
    <approach>mobile-first</approach>
    <breakpoints>standard CSS media queries</breakpoints>
    <flexibility>flexible layouts with CSS Grid/Flexbox</flexibility>
  </responsive_design>
  <accessibility>
    <focus_styles>visible focus indicators</focus_styles>
    <color_contrast>WCAG AA compliance</color_contrast>
    <motion>respect prefers-reduced-motion</motion>
  </accessibility>
</css_architecture>

<style_template>
  <root_component>
    .[component-name] {
        /* Component container styles */
        display: block;
        position: relative;
    }
  </root_component>
  <responsive_example>
    /* Mobile-first approach */
    .[component-name] .content {
        padding: 1rem;
    }

    @media (min-width: 768px) {
        .[component-name] .content {
            padding: 2rem;
        }
    }
  </responsive_example>
  <accessibility_example>
    .[component-name] button:focus {
        outline: 2px solid var(--focus-color);
        outline-offset: 2px;
    }

    @media (prefers-reduced-motion: reduce) {
        .[component-name] * {
            animation-duration: 0.01ms !important;
            transition-duration: 0.01ms !important;
        }
    }
  </accessibility_example>
</style_template>

<instructions>
  ACTION: Create isolated CSS file for component
  IMPLEMENT: Mobile-first responsive design
  ENSURE: Accessibility compliance
  INTEGRATE: With existing theme system
</instructions>

</step>

<step number="7" name="setup_bunit_testing">

### Step 7: Setup bUnit Testing

<step_metadata>
  <checks>existing_bunit_project</checks>
  <creates>bunit_project_if_missing</creates>
  <implements>component_tests</implements>
</step_metadata>

<bunit_project_setup>
  <project_structure>
    Tests/
    └── [ProjectName].Tests/
        ├── [ProjectName].Tests.csproj
        ├── Components/
        │   └── [ComponentName]Tests.cs
        └── TestBase.cs
  </project_structure>
  <required_packages>
    - bunit
    - Microsoft.AspNetCore.Components.Testing
    - xunit or NUnit (test framework)
    - Microsoft.NET.Test.Sdk
  </required_packages>
</bunit_project_setup>

<permission_request_template>
  I notice there's no bUnit test project in this solution. 
  
  Would you like me to:
  1. Create a new test project with bUnit setup
  2. Add the component tests to an existing test project
  3. Skip testing setup for now
  
  Creating a test project will include:
  - bUnit NuGet package
  - Test project structure
  - Base test class setup
  - Your component tests
</permission_request_template>

<test_patterns>
  <basic_render_test>
    [Fact]
    public void [ComponentName]_RendersCorrectly()
    {
        // Arrange
        var component = RenderComponent<[ComponentName]>(parameters => parameters
            .Add(p => p.Property, "test value"));

        // Assert
        component.Should().NotBeNull();
        component.Find("selector").Should().NotBeNull();
    }
  </basic_render_test>
  <parameter_test>
    [Theory]
    [InlineData("value1")]
    [InlineData("value2")]
    public void [ComponentName]_DisplaysParameter(string value)
    {
        // Arrange & Act
        var component = RenderComponent<[ComponentName]>(parameters => parameters
            .Add(p => p.Property, value));

        // Assert
        component.Find("selector").TextContent.Should().Contain(value);
    }
  </parameter_test>
  <event_test>
    [Fact]
    public void [ComponentName]_RaisesEventOnClick()
    {
        // Arrange
        var eventRaised = false;
        var component = RenderComponent<[ComponentName]>(parameters => parameters
            .Add(p => p.OnItemClicked, () => eventRaised = true));

        // Act
        component.Find("button").Click();

        // Assert
        eventRaised.Should().BeTrue();
    }
  </event_test>
</test_patterns>

<instructions>
  ACTION: Check for existing bUnit test project
  REQUEST: Permission to create test project if missing
  CREATE: Comprehensive component tests
  FOLLOW: Testing best practices with bUnit
</instructions>

</step>

<step number="8" name="validate_component">

### Step 8: Validate Component Implementation

<step_metadata>
  <validates>
    - compilation: boolean
    - tests_passing: boolean
    - accessibility: boolean
    - performance: boolean
  </validates>
</step_metadata>

<validation_checklist>
  <compilation>
    - [ ] Component compiles without errors
    - [ ] All references resolved correctly
    - [ ] No build warnings
  </compilation>
  <functionality>
    - [ ] Parameters work as expected
    - [ ] Events fire correctly
    - [ ] Render mode applied properly
    - [ ] Lifecycle methods execute
  </functionality>
  <testing>
    - [ ] All bUnit tests pass
    - [ ] Test coverage is adequate
    - [ ] Edge cases covered
  </testing>
  <standards>
    - [ ] Follows Blazor best practices
    - [ ] Accessibility requirements met
    - [ ] Performance optimized
    - [ ] Code style consistent
  </standards>
</validation_checklist>

<validation_tools>
  <build>dotnet build</build>
  <test>dotnet test</test>
  <accessibility>browser dev tools, accessibility scanner</accessibility>
</validation_tools>

<instructions>
  ACTION: Run comprehensive validation checks
  VERIFY: All checklist items completed
  FIX: Any issues found during validation
  DOCUMENT: Validation results
</instructions>

</step>

<step number="9" name="integration_verification">

### Step 9: Integration Verification

<step_metadata>
  <integrates>
    - project_structure: verify component placement
    - spec_requirements: validate against SRD
    - existing_components: check for conflicts
  </integrates>
</step_metadata>

<integration_checks>
  <file_placement>
    - Component files in correct directory
    - Namespace alignment with project structure
    - Proper file naming conventions
  </file_placement>
  <spec_compliance>
    - All SRD requirements implemented
    - Component meets acceptance criteria
    - User stories satisfied
  </spec_compliance>
  <system_integration>
    - No naming conflicts with existing components
    - Dependencies properly resolved
    - Services and DI working correctly
  </system_integration>
</integration_checks>

<instructions>
  ACTION: Verify component integrates properly with project
  CHECK: Spec requirements are fully met
  ENSURE: No conflicts with existing code
  VALIDATE: Component works in project context
</instructions>

</step>

<step number="10" name="documentation_and_completion">

### Step 10: Documentation and Completion

<step_metadata>
  <creates>
    - component_documentation: usage examples
    - completion_summary: structured report
  </creates>
</step_metadata>

<documentation_template>
  <component_readme>
    # [ComponentName]

    ## Purpose
    [COMPONENT_PURPOSE]

    ## Usage
    ```razor
    <[ComponentName] 
        Property="value"
        OnEvent="HandleEvent" />
    ```

    ## Parameters
    | Name | Type | Required | Description |
    |------|------|----------|-------------|
    | Property | string | Yes | Description |

    ## Events
    | Name | Type | Description |
    |------|------|-------------|
    | OnEvent | EventCallback | Description |

    ## Render Mode
    [RENDER_MODE] - [REASONING]

    ## Styling
    Component uses isolated CSS. Customize by overriding CSS custom properties.
  </component_readme>
</documentation_template>

<completion_summary_template>
  ## ✅ Blazor Component '[COMPONENT_NAME]' Successfully Created

  ### Files Created
  - 📄 `[ComponentName].razor` - Component markup and directives
  - ⚙️ `[ComponentName].razor.cs` - Component logic and code-behind
  - 🎨 `[ComponentName].razor.css` - Isolated component styles
  - 🧪 `[ComponentName]Tests.cs` - bUnit component tests

  ### Implementation Details
  - **Render Mode**: [RENDER_MODE]
  - **Parameters**: [PARAMETER_COUNT] parameters defined
  - **Events**: [EVENT_COUNT] events implemented
  - **Tests**: [TEST_COUNT] tests created and passing

  ### Performance Features
  - [PERFORMANCE_OPTIMIZATIONS]

  ### Accessibility Features
  - [ACCESSIBILITY_FEATURES]

  ### Integration Notes
  - ✅ Compiles without errors
  - ✅ All tests passing
  - ✅ Spec requirements met
  - ✅ No integration conflicts

  ### Usage Example
  ```razor
  <[ComponentName] 
      [EXAMPLE_USAGE] />
  ```

  ### Next Steps
  - Component is ready for use in your Blazor application
  - Consider adding to component library documentation
  - Review for additional optimization opportunities
</completion_summary_template>

<instructions>
  ACTION: Create component documentation
  GENERATE: Comprehensive completion summary
  PROVIDE: Usage examples and integration guidance
  HIGHLIGHT: Key features and implementation details
</instructions>

</step>

</process_flow>

## Execution Summary

<final_checklist>
  <verify>
    - [ ] Component requirements gathered and validated
    - [ ] Project structure analyzed
    - [ ] Optimal render mode determined
    - [ ] Three-file structure created (.razor, .razor.cs, .razor.css)
    - [ ] Component logic implemented following best practices
    - [ ] Isolated styles created with accessibility support
    - [ ] bUnit tests created and passing
    - [ ] Component validated and integrated
    - [ ] Documentation provided
    - [ ] Completion summary generated
  </verify>
</final_checklist>

<execution_order>
  1. Gather comprehensive component requirements
  2. Analyze project structure and detect bUnit availability
  3. Determine optimal render mode strategy
  4. Create proper file structure with separation of concerns
  5. Implement component logic in code-behind
  6. Create isolated CSS with responsive design
  7. Setup bUnit testing (request permission if needed)
  8. Validate component implementation thoroughly
  9. Verify integration with project and specs
  10. Generate documentation and completion summary
</execution_order>

## Blazor Best Practices Integration

<blazor_standards>
  <component_architecture>
    - Single Responsibility Principle for each component
    - Proper parameter validation and typing
    - Event-driven communication between components
    - Lifecycle method optimization
  </component_architecture>
  <performance_optimization>
    - Strategic ShouldRender() implementation
    - Efficient state management
    - Proper async/await patterns
    - Memory leak prevention
  </performance_optimization>
  <testing_strategy>
    - Comprehensive bUnit test coverage
    - Parameter validation testing
    - Event callback testing
    - Integration testing where appropriate
  </testing_strategy>
  <accessibility_compliance>
    - WCAG AA compliance
    - Keyboard navigation support
    - Screen reader compatibility
    - Focus management
  </accessibility_compliance>
</blazor_standards>
