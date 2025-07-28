# Tips & Tricks for AI Assisted Coding with GitHub Copilot

## General Best Practices

- **Use Clear, Descriptive Prompts**  
  Frame your comments or stub code with precise intent to guide Copilot’s suggestions.
- **Iterate and Refine**  
  If the first suggestion isn’t ideal, use markdown files to work through the problem with the AI. Don't just assume it understands how to build good code, instruct it on how!
- **Leverage Context Blocks**  
  Provide related function signatures or comments so Copilot can infer correct patterns.
- **Approve Carefully**  
  Always review and test generated code; Copilot can sometimes hallucinate or suggest insecure patterns.
- **Combine with Documentation**  
  Remind the AI to reach out to Context7 on the regular to validate it's thoughts and plans against real documentation.

## Workflow-Specific Tips for This Framework

- **Start with Requirements in Markdown**  
  Before using the `Plan` or `Act` commands, write detailed requirements and architectural notes in markdown files (e.g., `projectBrief.md`, `docs/`). This gives Copilot and the AI agent the context needed for accurate planning and implementation.
- **Follow the Core Workflow Commands**  
  Use the provided commands (`Plan`, `Act`, `Research`, etc.) as described in the README to trigger structured workflows. This ensures Copilot leverages both live documentation and persistent project memory.
- **Sync ConPort Regularly**  
  After major changes or at the end of a session, run the `conport_sync_routine` to persist new knowledge, decisions, and context. This keeps the AI's memory up to date and prevents context loss.
- **Document Decisions and Patterns**  
  When you make architectural or implementation decisions, log them using the appropriate workflow or markdown files. This helps Copilot and the agent avoid repeating past mistakes and improves future suggestions.
- **Use Semantic Search for Context**  
  When you need to reference past decisions, requirements, or patterns, use the semantic search features (or ask the agent to do so) to retrieve relevant context from ConPort memory. This is especially useful for large or long-running projects.
- **Update the Project Glossary**  
  As you introduce new terms or concepts, add them to the project glossary. This helps Copilot understand domain-specific language and improves code suggestions.
- **Reset Context When Needed**  
  If Copilot starts to drift or lose track of the workflow, reset the chat and re-initialize context using the workflow commands. This helps maintain alignment with the defined process.
- **Leverage Auto-Documentation**  
  Allow the agent to auto-generate diagrams and keep architecture docs in sync with code changes. This ensures Copilot always has the latest architectural context.

## Common Pitfalls to Avoid

- **Over-accepting Suggestions**  
  Blindly trusting suggestions can introduce bugs, security issues, or licensing conflicts.
- **Fragmented Context**  
  Jumping between files without context often leads to mismatched or incomplete code completions.
- **Static Imports Only**  
  Copilot might suggest unused imports; remove or consolidate imports to keep code clean.
- **Ignoring Edge Cases**  
  Generated code may skip error handling or boundary checks—always verify exceptional paths.
- **Outdated Context**  
  After refactoring, invalid or stale comments can mislead Copilot; update related comments and stubs.
- **Context overflow**  
  Reset the chat context often. Tell the AI to update the ConPort status, and then start a new chat. This can help if things go off the rails.

## Troubleshooting Scenarios

### 1. Suggestion Drift After Refactor
**Issue:** Copilot continues suggesting outdated patterns post-refactor.
**Resolution:**  
- Update in-file comments to reflect new APIs or signatures.  
- Restart your IDE or clear Copilot cache if issues persist.

### 2. Performance Hangs on Large Files
**Issue:** Autocomplete lags in big modules.
**Resolution:**  
- Split large files into smaller, focused modules.  
- Disable Copilot in specific file types if not needed.

### 3. Incorrect Language Features
**Issue:** Copilot suggests syntax not supported by your project’s runtime or compiler.
**Resolution:**  
- Specify target language version in comments or config.  
- Add a small code comment with `// @ts-check` or similar hints.

### 4. Licensing or Legal Worries
**Issue:** Concern over code origin or license compatibility.
**Resolution:**  
- Vet suggestions for existing licenses.  
- Find, specify, and instruct which packages you want allowed!

### 5. Missing Dependencies
**Issue:** Suggestions reference packages not installed.
**Resolution:**  
- Preemptively install common dependencies.  
- Add an import stub comment (e.g., `import { } from 'lodash';`) to guide Copilot.

---

*These tips aim to empower your AI Assisted Coding experience and help you avoid common frustrations. Happy coding!*