Your primary goal is to generate a new Blazor component for a .NET 9 application.

Always begin by asking clarifying questions if the user's initial prompt is not detailed enough.

**1. Clarify Component Requirements:**

If not provided by the user, ask for the following details:
- **Component Name:** (e.g., `UserProfileCard.razor`, `OrderFormComponent.razor`)
- **Purpose & Functionality:** What should the component do? What problem does it solve?
- **Input Parameters:**
    - Name, C# data type, and purpose of each parameter.
    - Should any parameters be mandatory (`[EditorRequired]`)?
- **Events:**
    - What events, if any, should the component emit (e.g., `OnSubmit`, `OnItemClicked`)?
    - What data (type) should each `EventCallback<T>` carry?
- **Interactivity & Data Needs:**
    - How interactive does this component need to be? (Helps determine render mode)
    - Will it fetch data? From where? (Helps determine services needed and render mode implications)
    - Does it need to maintain state across navigations or re-renders? (Consider `PersistentComponentState`)
- **User Interface (UI) Sketch/Description:** A brief idea of how it should look or what elements it should contain.

**2. Adhere to Blazor Best Practices (refer to #file:../instructions/blazor.instructions.md for full details):**

- **Project Version:** Ensure the component is compatible with .NET 9.
- **File Structure:**
    - Create a `.razor` file for the component.
    - Create a `.razor.cs` code-behind file if the component has significant logic.
    - Create isolated CSS (`.razor.css`) for component-specific styles.
- **Component Design:**
    - Follow Single Responsibility Principle (SRP).
    - Keep components small and focused.
    - Use `[Parameter]` and `[EditorRequired]` attributes correctly.
    - Use `EventCallback<T>` for events.
- **Render Modes (.NET 9):**
    - **Crucial:** Determine and apply the most appropriate render mode. Clearly state the chosen mode in your response and in the component file (e.g., `@rendermode InteractiveServer`, `@rendermode InteractiveWebAssembly`, `@rendermode InteractiveAuto`, or `@rendermode StaticServer` for static content).
    - Explain *why* the chosen render mode is suitable based on the component's interactivity, data fetching, and SSR requirements.
    - If `InteractiveAuto` or `InteractiveWebAssembly` is chosen, remind the user about potential initial download considerations for WASM.
- **State Management:**
    - For simple state, use component parameters and fields.
    - For more complex or shared state, discuss options like scoped services or `PersistentComponentState` (especially if prerendering and interactivity are involved). Refer to `blazor.instructions.md`.
- **Performance Optimization:**
    - Implement `ShouldRender` if applicable to prevent unnecessary re-renders.
    - Use `@key` for lists of components/elements.
    - Consider `<Virtualize>` for large datasets.
    - Leverage **Streaming Rendering** (`@attribute [StreamRendering(true)]`) for server-rendered components that await asynchronous data, to improve perceived load time.
- **Forms & Validation (if applicable):**
    - Use `EditForm`, Data Annotations, and built-in validation components.
- **JavaScript Interop:**
    - Minimize JS interop. Isolate JS calls in dedicated services or helper classes.
    - Follow best practices outlined in `blazor.instructions.md`.
- **Error Handling:**
    - Suggest using `<ErrorBoundary>` to wrap parts of the component or the whole component if it can encounter runtime errors that shouldn't break the entire page.
- **Security:**
    - Briefly mention security considerations if the component handles sensitive data or user input, directing to any architecture documents in the workspace.

**3. Generation Steps & Output:**

When generating the component, follow these steps and structure your output:

- **Provide the `.razor` file content.**
    ```razor
    // Example: MyComponent.razor
    @* If applicable, state the chosen render mode clearly *@
    @* @rendermode InteractiveServer *@
    @* @attribute [StreamRendering(true)] *@

    <h3>MyComponent</h3>
    <p>@Message</p>
    <button @onclick="HandleClick">Click me</button>

    @code {
        [Parameter]
        public string? Message { get; set; }

        [Parameter]
        public EventCallback<string> OnClick { get; set; }

        private async Task HandleClick()
        {
            // Component logic
            await OnClick.InvokeAsync("Button was clicked!");
        }

        // Implement lifecycle methods, ShouldRender, etc., as needed.
    }
    ```
- **Provide the `.razor.cs` file content (if applicable).**
    ```csharp
    // Example: MyComponent.razor.cs
    using Microsoft.AspNetCore.Components;
    using System.Threading.Tasks;

    namespace YourProject.Components
    {
        public partial class MyComponent
        {
            // Logic moved from @code block
            // [Parameter] public string Message { get; set; }
            // ...
        }
    }
    ```
- **Provide the `.razor.css` file content (if applicable for isolated styles).**
    ```css
    /* Example: MyComponent.razor.css */
    h3 {
        color: navy;
    }
    ```
- **Explain your choices:**
    - Justify the chosen render mode.
    - Explain any complex logic or design patterns used.
    - Highlight how performance or specific .NET 9 features (like streaming rendering or persistent state) were incorporated.
- **Suggest related files/code:**
    - If the component requires new services (e.g., for data fetching, state management), provide a basic structure or an example of how to register them.
    - If it uses DTOs (Data Transfer Objects), show their definitions.
- **Testing:**
    - **Strongly recommend** creating bUnit tests for the component.
    - Provide a simple bUnit test example for a key interaction if feasible.
    ```csharp
    // Example bUnit Test Snippet
     using Bunit;
     using Microsoft.VisualStudio.TestTools.UnitTesting; // Or Xunit, Nunit
     using YourProject.Components;
    
     [TestClass]
     public class MyComponentTests : TestContext
     {
       [TestMethod]
       public void MyComponent_RendersCorrectly_AndHandlesClick()
       {
         // Arrange
         var cut = RenderComponent<MyComponent>(parameters => parameters
           .Add(p => p.Message, "Test Message")
           .Add(p => p.OnClick, EventCallback.Factory.Create<string>(this, Assert.IsNotNull))
         );
    
         // Assert - initial render
         cut.Find("h3").MarkupMatches("<h3>MyComponent</h3>");
         cut.Find("p").MarkupMatches("<p>Test Message</p>");
    
         // Act - click button
         cut.Find("button").Click();
    
         // Assert - event callback was invoked (example, depends on actual event logic)
       }
     }
    ```

**4. Final Review & Iteration:**

- After presenting the generated component, ask the user for feedback.
- Be prepared to iterate on the design and implementation based on their input.

Remember to always consult the #file:../instructions/blazor.instructions.md for detailed Blazor development guidelines and .NET 9 best practices.
