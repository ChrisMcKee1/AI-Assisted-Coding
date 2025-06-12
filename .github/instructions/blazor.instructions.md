---
applyTo: '**/*.razor, **/*.razor.cs, **/*.razor.css'
---
Role Definition:
 - Blazor Expert
 - .NET 9 Specialist
 - Web UI/UX Developer
 - Performance Optimizer

General:
  Description: >
    This instruction set guides the AI in developing Blazor applications
    using .NET 9, focusing on best practices, component design, performance,
    state management, security, and testing. Adherence to these guidelines
    will ensure robust, maintainable, and high-performing Blazor applications.
    .NET 9 enhances Blazor with features like improved server-side rendering (SSR)
    with streaming rendering, flexible component render modes, persistent component state,
    WebAssembly (WASM) performance optimizations, and more refined development experiences.
  Requirements:
    - Utilize .NET 9 for all Blazor project development.
    - Follow Blazor best practices for component design, architecture, and render modes.
    - Implement efficient state management techniques, including persistent state where appropriate.
    - Prioritize performance optimization leveraging .NET 9 features for both server and WASM scenarios.
    - Ensure security best practices are applied throughout the application.
    - Write comprehensive unit and integration tests for Blazor components.
    - Leverage new .NET 9 features relevant to Blazor development for optimal outcomes.

Key Concepts & Best Practices (.NET 9):

  - Component Design:
    - Single Responsibility Principle (SRP): Components should have a single, well-defined purpose.
        ```csharp
        // Good: A component focused solely on displaying user details.
        // UserDetails.razor
        // ... displays user information ...

        // Bad: A component that fetches user data, displays it, and handles user editing.
        ```
    - Small Components: Break down complex UIs into smaller, reusable components.
    - Parameters: Use `[Parameter]` attribute for inputs. Define `[EditorRequired]` for mandatory parameters.
        ```csharp
        // Good: Clearly defined parameters
        // File: GreetingCard.razor
         <p>Hello, @Name!</p>
         @code {
           [Parameter]
           [EditorRequired]
           public string Name { get; set; }
         }
        ```
    - EventCallbacks: Use `EventCallback` for component outputs/events. Prefer `EventCallback<T>` for typed events.
        ```csharp
        // Good: Typed EventCallback
        // File: ChildComponent.razor
         <button @onclick="NotifyParent">Click Me</button>
         @code {
           [Parameter]
           public EventCallback<string> OnClick { get; set; }
        
           private async Task NotifyParent()
           {
             await OnClick.InvokeAsync("Button Clicked!");
           }
         }
        ```
    - Cascading Parameters: Use for sharing data down the component hierarchy (e.g., themes, user info), but sparingly to avoid over-coupling.
    - Component Lifecycle: Understand and use lifecycle methods (`OnInitializedAsync`, `OnParametersSetAsync`, `ShouldRender`, `OnAfterRenderAsync`) appropriately.
        - Perform async operations in `OnInitializedAsync` or `OnParametersSetAsync`.
        - Minimize work in `OnAfterRenderAsync`, primarily for JS interop after DOM is ready.
    - Templated Components: Use `RenderFragment` and `RenderFragment<T>` for creating flexible, reusable components with customizable layouts.
    - Render Modes (.NET 9): Understand and apply appropriate render modes for components or the entire application.
        - `@rendermode InteractiveServer`: Renders the component interactively on the server using Blazor Server.
        - `@rendermode InteractiveWebAssembly`: Renders the component interactively on the client using Blazor WebAssembly.
        - `@rendermode InteractiveAuto`: Initially uses Blazor Server, then switches to Blazor WebAssembly on subsequent visits after the WASM bundle is downloaded.
        - `@rendermode StaticServer`: Renders the component statically as part of the page from the server. No interactivity.
        - Configure per-component, per-page, or globally.
        ```csharp
        // Example: Applying a render mode to a component instance
         <MyInteractiveComponent @rendermode="InteractiveServer" />

        // Example: Applying a render mode to a component definition
        // MyComponent.razor
         @rendermode InteractiveWebAssembly
        // ... component content ...
        ```

  - State Management:
    - Component State: For simple, localized state, use component parameters and fields.
    - App-Level State:
        - Cascading Parameters: Suitable for simple global state.
        - Scoped Services: Register services (e.g., `AddScoped`) to manage state within a user's session (Blazor Server) or for the lifetime of the app (Blazor WASM).
            ```csharp
            // Good: Scoped service for managing shopping cart state
            // Program.cs (or relevant startup)
             builder.Services.AddScoped<ShoppingCartState>();

            // ShoppingCartState.cs
             public class ShoppingCartState
             {
               public List<CartItem> Items { get; private set; } = new();
               public event Action OnChange;
               public void AddItem(CartItem item) { /* ... */ NotifyStateChanged(); }
               private void NotifyStateChanged() => OnChange?.Invoke();
             }
            ```
        - Flux/Redux Patterns: For complex applications, consider libraries like Fluxor or implement a similar pattern for predictable state changes.
    - URL/Query Parameters: For state that should be bookmarkable or shareable.
    - Local Storage/Session Storage: For persisting state on the client-side (primarily Blazor WASM or Blazor Server with JS interop).
    - Persistent Component State (.NET 9): Use `PersistentComponentState` service to preserve component state across prerendering and full rendering, avoiding loss of state during initial load for interactive components.
        ```csharp
        // Good: Using PersistentComponentState
        // MyComponent.razor
         @inject PersistentComponentState ApplicationState
         @implements IDisposable
        // ...
         @code {
           private PersistingComponentStateSubscription persistingSubscription;
           private WeatherForecast[]? forecasts;
        
           protected override async Task OnInitializedAsync()
           {
             persistingSubscription = ApplicationState.RegisterOnPersisting(PersistForecasts);
        
             if (!ApplicationState.TryTakeFromJson<WeatherForecast[]>("weatherforecasts", out forecasts))
             {
               // forecasts = await Http.GetFromJsonAsync<WeatherForecast>("WeatherForecast"); // Fetch if not persisted
             }
           }
        
           private Task PersistForecasts()
           {
             ApplicationState.PersistAsJson("weatherforecasts", forecasts);
             return Task.CompletedTask;
           }
        
           void IDisposable.Dispose() => persistingSubscription.Dispose();
         }
        ```

  - Performance Optimization:
    - `ShouldRender`: Implement `ShouldRender` to prevent unnecessary re-renders of components or subtrees.
        ```csharp
        // Good: Preventing re-render if parameters haven't changed
         @code {
           private int _previousValue;
           [Parameter] public int Value { get; set; }
        
           protected override bool ShouldRender()
           {
             if (Value == _previousValue) return false;
             _previousValue = Value;
             return true;
           }
         }
        ```
    - `@key` Directive: Use `@key` when rendering lists of components to help Blazor efficiently track and update elements.
        ```csharp
        // Good: Using @key for dynamic lists
         @foreach (var item in Items)
         {
           <MyItemComponent @key="item.Id" Item="item" />
         }
        ```
    - Virtualization: Use the `<Virtualize>` component for rendering large lists efficiently.
    - Async Operations: Use `async/await` correctly. Avoid `async void`.
    - JS Interop: Minimize JS interop calls. Batch calls where possible.
    - Code Splitting/Lazy Loading (Blazor WASM): Lazy load assemblies for routes or features not immediately needed.
        ```csharp
        // Good: Lazy loading an assembly for a specific route
        // App.razor
         <Router AppAssembly="@typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(MyLazyLoadedComponent).Assembly }">
        // ...
         </Router>
        ```
    - AOT Compilation (Blazor WASM): Ahead-of-Time compilation can improve runtime performance at the cost of larger download sizes. Evaluate for .NET 9.
    - Server-Side Rendering (SSR) with Streaming Rendering (.NET 9):
        - Leverage enhanced SSR for faster initial page loads.
        - Use streaming rendering (`@attribute [StreamRendering(true)]` on a routable component or `Html.RenderComponentAsync` with `renderMode: ServerStaticStreaming`) to progressively render UI parts as data becomes available, improving perceived performance.
        ```csharp
        // MyPage.razor
         @page "/streaming-data"
         @attribute [StreamRendering(true)]
         @inject MyDataService DataService
        
         <h1>Streaming Data Example</h1>
         <p>Content before await.</p>
         @await Task.Delay(1000) <!-- Simulate async work -->
         <p>Content after first await.</p>
        
         @foreach (var item in await DataService.GetItemsSlowlyAsync())
         {
           <p>@item.Name</p>
           @await Task.Delay(500) <!-- Simulate more async work per item -->
         }
         <p>All items loaded.</p>
        ```
    - Sections API: Use `@rendermode` with sections (`<SectionOutlet>` and `<SectionContent>`) to define content placeholders that can be filled by other components, allowing for more flexible layouts with different render modes.

  - Forms and Validation:
    - Use `EditForm` component for handling forms.
    - Implement validation using Data Annotations (`[Required]`, `[StringLength]`, etc.) on your model.
    - Use built-in validation components like `DataAnnotationsValidator` and `ValidationSummary` or `ValidationMessage`.
    - Custom Validation: Create custom `ValidationAttribute` or implement custom validation logic.
    - Consider new form handling features or improvements in .NET 9 if available.

  - Routing:
    - Use `@page` directive for defining routes.
    - Route Parameters: Define route parameters (e.g., `@page "/product/{Id:int}"`).
    - `NavLink` Component: Use for navigation links to get active class styling.
    - Programmatic Navigation: Use `NavigationManager` service.
    - Route Constraints: Utilize route constraints for type checking and matching.

  - JavaScript Interop:
    - Use `IJSRuntime` to call JavaScript functions from .NET and .NET methods from JavaScript.
    - Isolate JS interop calls in dedicated services or helper classes.
    - Prefer unmarshalled JS interop for performance-critical scenarios in Blazor WebAssembly. .NET 9 continues to refine this.
    - Ensure JS code is placed in `wwwroot` and referenced correctly.
    - Use JS initializers for setting up JS libraries or performing setup tasks when the Blazor app starts.

  - Security:
    - Authentication & Authorization: Use ASP.NET Core Identity or other authentication providers (e.g., Azure AD B2C, OpenID Connect).
    - Use `[Authorize]` attribute on components or `@attribute [Authorize]` in `.razor` files.
    - `AuthorizeView` Component: Conditionally render UI based on authorization status.
    - Protect against XSS: Blazor automatically encodes HTML, but be cautious with `MarkupString` and JS interop.
    - Protect against CSRF: Blazor Server apps are generally protected by default. For Blazor WASM with custom backend APIs, ensure CSRF protection is implemented on the API. Antiforgery support is enhanced in .NET 8+ for form submissions.
    - Content Security Policy (CSP): Implement a strong CSP.
    - Secrets Management: Use user secrets in development and Azure Key Vault or similar in production.

  - Error Handling:
    - Error Boundaries: Use the `<ErrorBoundary>` component to catch exceptions within a part of the UI and display a fallback UI.
    - Global Exception Handling: Implement custom error handling for unhandled exceptions.
    - Logging: Use structured logging (e.g., Serilog, NLog) integrated with ASP.NET Core logging.

  - Testing:
    - bUnit: The recommended library for unit testing Blazor components.
        ```csharp
        // Good: bUnit test example
         using Bunit;
         using Xunit;
        
         public class CounterTests : TestContext
         {
           [Fact]
           public void CounterShouldIncrementWhenClicked()
           {
             // Arrange
             var cut = RenderComponent<Counter>();
        
             // Act
             cut.Find("button").Click();
        
             // Assert
             cut.Find("p").MarkupMatches("<p>Current count: 1</p>");
           }
         }
        ```
    - Test Services: Mock or provide test implementations for services injected into components.
    - Integration Testing: For Blazor Server, use `WebApplicationFactory` similar to ASP.NET Core. For Blazor WASM, testing often focuses on component interactions and service calls.

  - .NET 9 Specific Features to Leverage:
    - **Render Modes**: Master the use of `InteractiveServer`, `InteractiveWebAssembly`, `InteractiveAuto`, and `StaticServer` render modes for fine-grained control over component behavior and performance.
    - **Streaming Rendering**: Apply `@attribute [StreamRendering(true)]` or use `Html.RenderComponentAsync` with streaming for routable server-side components to improve perceived load times by rendering content as it becomes available.
    - **Persistent Component State**: Utilize `PersistentComponentState` to preserve state during prerendering, ensuring a smoother user experience for interactive components.
    - **WebAssembly Performance Enhancements**: Benefit from ongoing JIT/AOT compiler improvements, potentially smaller download sizes, and faster execution.
    - **Enhanced Form Handling**: Look for improvements in form binding, validation, or new form-related components/utilities. (Verify specific .NET 9 features).
    - **Improved JS Interop**: Leverage any new capabilities or performance gains in JS interop, especially unmarshalled calls.
    - **Tooling Improvements**: Stay updated with Visual Studio and .NET CLI enhancements for Blazor development, debugging, and deployment.
    - **Sections API Enhancements**: Explore how sections can be used with different render modes for more dynamic page layouts.
    - **Blazor Hybrid Improvements**: If developing for Blazor Hybrid, check for specific enhancements in .NET 9.

Best Practices Summary:
  - Keep components small and focused.
  - Choose the appropriate **render mode** for each component or page.
  - Prefer strong typing with `EventCallback<T>`.
  - Manage state appropriately, using **Persistent Component State** for prerendered interactive components.
  - Optimize rendering with `ShouldRender`, `@key`, and **Streaming Rendering**.
  - Use `<Virtualize>` for large lists.
  - Minimize and batch JS interop calls, using unmarshalled interop where beneficial.
  - Implement robust authentication, authorization, and error handling.
  - Write comprehensive tests using bUnit.
  - Stay informed about and leverage new .NET 9 Blazor features.
  - Follow official Microsoft Blazor documentation and community best practices.

Troubleshooting:
  - Browser Developer Tools: Inspect console errors, network requests, and component DOM.
  - Debugging: Use Visual Studio debugger for Blazor Server, Blazor WebAssembly, and Blazor Hybrid.
  - Logging: Check application logs for server-side errors or detailed client-side information.
  - `.NET Aspire Dashboard` (if used): Can provide insights into Blazor app behavior within a distributed system.
  - Understand the implications of different render modes on debugging and state.
---
