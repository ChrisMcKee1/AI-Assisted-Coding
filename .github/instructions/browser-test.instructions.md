---
description: Browser Testing Rules for Nous using Playwright
globs:
alwaysApply: false
version: 1.0
encoding: UTF-8
---

# Browser Testing Rules

<ai_meta>
  <parsing_rules>
    - Process XML blocks first for structured data
    - Execute instructions in sequential order
    - Use templates as exact patterns
    - Always take a snapshot before interacting with the browser
  </parsing_rules>
  <file_conventions>
    - encoding: UTF-8
    - line_endings: LF
    - indent: 2 spaces
    - markdown_headers: no indentation
  </file_conventions>
  <tooling>
    - sequential-thinking mcp
    - playwright mcp
  </tooling>
</ai_meta>

## Overview

<purpose>
  - Provide a structured workflow for conducting automated browser tests.
  - Ensure reliable and repeatable test execution using the Playwright MCP toolset.
  - Define best practices for interacting with web pages and verifying outcomes.
</purpose>

<context>
  - Part of the Nous framework for automated development and testing.
  - Triggered when a user requests to test a feature, workflow, or UI component in a browser.
  - Aims to simulate user actions and validate application behavior.
</context>

<prerequisites>
  - A clear request from the user describing the test scenario.
  - A running application accessible via a URL.
  - The Playwright MCP toolset must be available and configured.
</prerequisites>

<process_flow>

<step number="1" name="understand_test_request">

### Step 1: Understand the Test Request

<step_metadata>
  <purpose>Deconstruct the user's request into a clear test objective and a sequence of actions.</purpose>
  <inputs>
    - user_request: string (e.g., "Test the login flow with my credentials.")
  </inputs>
  <outputs>
    - test_objective: string
    - action_sequence: array[string]
  </outputs>
  <mcp_tooling>
    - sequential-thinking
  </mcp_tooling>
</step_metadata>

<analysis_tasks>
  <objective_identification>
    - What is the primary goal of this test? (e.g., "Verify successful login.")
  </objective_identification>
  <action_extraction>
    - What are the discrete user actions? (e.g., "Navigate to /login", "Enter username", "Enter password", "Click submit.")
  </action_extraction>
  <assertion_definition>
    - What is the expected outcome? (e.g., "The user is redirected to the dashboard.", "A welcome message is displayed.")
  </assertion_definition>
</analysis_tasks>

<instructions>
  ACTION: Analyze the user's request to define the test's objective, actions, and success criteria.
  UTILIZE: sequential-thinking mcp to break down the user's natural language request into a structured list of actions and expected outcomes.
  DOCUMENT: The resulting test plan before proceeding.
</instructions>

</step>

<step number="2" name="navigate_and_verify_initial_state">

### Step 2: Navigate and Verify Initial State

<step_metadata>
  <purpose>Open the browser to the correct starting URL and confirm the page is ready.</purpose>
  <inputs>
    - start_url: string
  </inputs>
  <mcp_tooling>
    - playwright
  </mcp_tooling>
</step_metadata>

<initialization_workflow>
  1. **Navigate**: Use `browser_navigate` to go to the starting URL.
  2. **Wait**: Use `browser_wait_for` with a reasonable timeout (e.g., 5 seconds) or for a key text element to appear to ensure the page has loaded.
  3. **Snapshot**: Use `browser_snapshot` to capture the initial state of the page. This is mandatory for identifying element references for subsequent actions.
</initialization_workflow>

<instructions>
  ACTION: Navigate to the specified URL and capture a snapshot of the initial page state.
  TOOL_CALL: `browser_navigate` with the target URL.
  TOOL_CALL: `browser_wait_for` to ensure the page is fully loaded.
  TOOL_CALL: `browser_snapshot` to get the accessibility tree for interaction.
  VALIDATE: The snapshot contains expected elements for the first action.
</instructions>

</step>

<step number="3" name="execute_test_sequence">

### Step 3: Execute Test Sequence

<step_metadata>
  <purpose>Systematically perform the user actions defined in the test plan.</purpose>
  <mcp_tooling>
    - sequential-thinking
    - playwright
  </mcp_tooling>
</step_metadata>

<interaction_best_practices>
  <general_workflow>
    For each action in the sequence:
    1. **Analyze Snapshot**: Review the latest `browser_snapshot` to find the correct `ref` for the target element.
    2. **Perform Action**: Call the appropriate Playwright tool (`browser_type`, `browser_click`, etc.).
    3. **Wait for Change**: If the action causes a page change or async update, use `browser_wait_for`.
    4. **Take New Snapshot**: Call `browser_snapshot` again to see the result of the action and prepare for the next step.
  </general_workflow>

  <tool_guidelines>
    - **Text Input**: ALWAYS use `browser_type` for filling in form fields. DO NOT use `browser_press_key` for this, as it is for single, non-character key presses (e.g., 'Enter', 'ArrowDown').
    - **Clicking**: Use `browser_click` for buttons, links, and other clickable elements.
    - **Dropdowns**: Use `browser_select_option` to choose one or more options from a `<select>` element.
    - **Pauses**: Use `browser_wait_for` to handle delays, waiting for text to appear/disappear, or for async operations to complete. Avoid fixed time delays where possible.
    - **Verification**: Use `browser_snapshot` to get the page content for verification. Check for expected text or elements in the snapshot output.
  </tool_guidelines>
</interaction_best_practices>

<instructions>
  ACTION: Execute the planned sequence of UI interactions step-by-step.
  UTILIZE: sequential-thinking mcp to manage the loop of "snapshot -> act -> wait -> snapshot".
  TOOL_CALL: Use the correct `playwright` tool for each interaction as per the guidelines.
  MANDATORY: Always use a fresh `browser_snapshot` to find element `ref` values before an action.
  BEST_PRACTICE: Prefer `browser_type` for text entry and `browser_click` for interactions. Use `browser_wait_for` to handle dynamic content.
</instructions>

</step>

<step number="4" name="verify_final_outcome">

### Step 4: Verify Final Outcome

<step_metadata>
  <purpose>Check if the final state of the application matches the expected outcome.</purpose>
  <inputs>
    - expected_outcome: string (e.g., "a 'Welcome' message is visible")
  </inputs>
  <mcp_tooling>
    - playwright
  </mcp_tooling>
</step_metadata>

<verification_methods>
  <text_presence>
    - Take a final `browser_snapshot`.
    - Scan the snapshot's accessibility tree for the expected text.
  </text_presence>
  <url_check>
    - Take a `browser_snapshot`.
    - Check the `url` property in the snapshot output to confirm redirection.
  </url_check>
  <element_state>
    - Take a `browser_snapshot`.
    - Check for the presence, absence, or attributes (e.g., `disabled`) of specific elements.
  </element_state>
</verification_methods>

<instructions>
  ACTION: Verify that the test's success criteria have been met.
  TOOL_CALL: `browser_snapshot` to get the final page state.
  ANALYZE: The snapshot output to confirm the presence of expected text, the correct URL, or the state of specific UI elements.
  COMPARE: The final state against the `expected_outcome` defined in Step 1.
</instructions>

</step>

<step number="5" name="report_results">

### Step 5: Report Results

<step_metadata>
  <purpose>Summarize the test execution and outcome for the user.</purpose>
  <outputs>
    - summary_report: string
  </outputs>
  <mcp_tooling>
    - sequential-thinking
    - playwright
  </mcp_tooling>
</step_metadata>

<report_template>
  ## Browser Test Results

  **Objective:** [TEST_OBJECTIVE]

  **Outcome:** ✅ Success / ❌ Failure

  **Summary:**
  [A brief, human-readable summary of what happened.]

  **Verification Steps:**
  - [✅/❌] Navigated to [START_URL].
  - [✅/❌] Performed action: [ACTION_1_DESCRIPTION].
  - [✅/❌] Performed action: [ACTION_2_DESCRIPTION].
  - [✅/❌] Verified final state: [ASSERTION_DESCRIPTION].

  **Final Screenshot:**
  [FILENAME_OF_FINAL_SCREENSHOT]
</report_template>

<instructions>
  ACTION: Generate a clear and concise report of the test results.
  TOOL_CALL: `browser_take_screenshot` to capture a visual record of the final state.
  UTILIZE: sequential-thinking mcp to structure the summary report based on the template.
  POPULATE: The report with the objective, a success/failure status, and a summary of the steps taken.
  FINALIZE: Close the browser using `browser_close` after the report is complete.
</instructions>

</step>

</process_flow>
