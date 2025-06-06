---
applyTo: '*.cs'
---
Role Definition:
 - C# Language Expert
 - Software Architect
 - Code Quality Specialist

General:
  Description: >
    C# code should be written to maximize readability, maintainability, and correctness
    while minimizing complexity and coupling. Prefer functional patterns and immutable
    data where appropriate, and keep abstractions simple and focused.
  Requirements:
    - Write clear, self-documenting code
    - Keep abstractions simple and focused
    - Minimize dependencies and coupling
    - Use modern C# features appropriately

Type Definitions:
  - Prefer records for data types:
      ```csharp
      // Good: Immutable data type with value semantics
      public sealed record CustomerDto(string Name, Email Email);
      
      // Avoid: Class with mutable properties
      public class Customer
      {
          public string Name { get; set; }
          public string Email { get; set; }
      }
      ```
  - Make classes sealed by default:
      ```csharp
      // Good: Sealed by default
      public sealed class OrderProcessor
      {
          // Implementation
      }
      
      // Only unsealed when inheritance is specifically designed for
      public abstract class Repository<T>
      {
          // Base implementation
      }
      ```
  - Use value objects to avoid primitive obsession:
      ```csharp
      // Good: Strong typing with value objects
      public sealed record OrderId(Guid Value)
      {
          public static OrderId New() => new(Guid.NewGuid());
          public static OrderId From(string value) => new(Guid.Parse(value));
      }
      
      // Avoid: Primitive types for identifiers
      public class Order
      {
          public Guid Id { get; set; }  // Primitive obsession
      }
      ```

Functional Patterns:
  - Use pattern matching effectively:
      ```csharp
      // Good: Clear pattern matching
      public decimal CalculateDiscount(Customer customer) =>
          customer switch
          {
              { Tier: CustomerTier.Premium } => 0.2m,
              { OrderCount: > 10 } => 0.1m,
              _ => 0m
          };
      
      // Avoid: Nested if statements
      public decimal CalculateDiscount(Customer customer)
      {
          if (customer.Tier == CustomerTier.Premium)
              return 0.2m;
          if (customer.OrderCount > 10)
              return 0.1m;
          return 0m;
      }
      ```
  - Prefer pure methods:
      ```csharp
      // Good: Pure function
      public static decimal CalculateTotalPrice(
          IEnumerable<OrderLine> lines,
          decimal taxRate) =>
          lines.Sum(line => line.Price * line.Quantity) * (1 + taxRate);
      
      // Avoid: Method with side effects
      public void CalculateAndUpdateTotalPrice()
      {
          this.Total = this.Lines.Sum(l => l.Price * l.Quantity);
          this.UpdateDatabase();
      }
      ```

Code Organization:
  - Separate state from behavior:
      ```csharp
      // Good: Behavior separate from state
      public sealed record Order(OrderId Id, List<OrderLine> Lines);
      
      public static class OrderOperations
      {
          public static decimal CalculateTotal(Order order) =>
              order.Lines.Sum(line => line.Price * line.Quantity);
      }
      ```
  - Use extension methods appropriately:
      ```csharp
      // Good: Extension method for domain-specific operations
      public static class OrderExtensions
      {
          public static bool CanBeFulfilled(this Order order, Inventory inventory) =>
              order.Lines.All(line => inventory.HasStock(line.ProductId, line.Quantity));
      }
      ```

Dependency Management:
  - Minimize constructor injection:
      ```csharp
      // Good: Minimal dependencies
      public sealed class OrderProcessor
      {
          private readonly IOrderRepository _repository;
          
          public OrderProcessor(IOrderRepository repository)
          {
              _repository = repository;
          }
      }
      
      // Avoid: Too many dependencies
      public class OrderProcessor
      {
          public OrderProcessor(
              IOrderRepository repository,
              ILogger logger,
              IEmailService emailService,
              IMetrics metrics,
              IValidator validator)
          {
              // Too many dependencies indicates possible design issues
          }
      }
      ```
  - Prefer composition with interfaces:
      ```csharp
      // Good: Composition with interfaces
      public sealed class EnhancedLogger : ILogger
      {
          private readonly ILogger _baseLogger;
          private readonly IMetrics _metrics;
          
          public EnhancedLogger(ILogger baseLogger, IMetrics metrics)
          {
              _baseLogger = baseLogger;
              _metrics = metrics;
          }
      }
      ```

Code Clarity:
    - Prefer range indexers over LINQ:
      ```csharp
      // Good: Using range indexers with clear comments
      var lastItem = items[^1];  // ^1 means "1 from the end"
      var firstThree = items[..3];  // ..3 means "take first 3 items"
      var slice = items[2..5];  // take items from index 2 to 4 (5 exclusive)
      
      // Avoid: Using LINQ when range indexers are clearer
      var lastItem = items.LastOrDefault();
      var firstThree = items.Take(3).ToList();
      var slice = items.Skip(2).Take(3).ToList();
      ```
  - Use meaningful names:
      ```csharp
      // Good: Clear intent
      public async Task<Result<Order>> ProcessOrderAsync(
          OrderRequest request,
          CancellationToken cancellationToken)
      
      // Avoid: Unclear abbreviations
      public async Task<Result<T>> ProcAsync<T>(ReqDto r, CancellationToken ct)
      ```

Error Handling:
  - Use Result types for expected failures:
      ```csharp
      // Good: Explicit error handling
      public sealed record Result<T>
      {
          public T? Value { get; }
          public Error? Error { get; }
          
          private Result(T value) => Value = value;
          private Result(Error error) => Error = error;
          
          public static Result<T> Success(T value) => new(value);
          public static Result<T> Failure(Error error) => new(error);
      }
      ```
  - Prefer exceptions for exceptional cases:
      ```csharp
      // Good: Exception for truly exceptional case
      public static OrderId From(string value)
      {
          if (!Guid.TryParse(value, out var guid))
              throw new ArgumentException("Invalid OrderId format", nameof(value));
          
          return new OrderId(guid);
      }
      ```

Testing Considerations:
  - Design for testability:
      ```csharp
      // Good: Easy to test pure functions
      public static class PriceCalculator
      {
          public static decimal CalculateDiscount(
              decimal price,
              int quantity,
              CustomerTier tier) =>
              // Pure calculation
      }
      
      // Avoid: Hard to test due to hidden dependencies
      public decimal CalculateDiscount()
      {
          var user = _userService.GetCurrentUser();  // Hidden dependency
          var settings = _configService.GetSettings(); // Hidden dependency
          // Calculation
      }
      ```

Immutable Collections:
  - Use System.Collections.Immutable with records:
      ```csharp
      // Good: Immutable collections in records
      public sealed record Order(
          OrderId Id, 
          ImmutableList<OrderLine> Lines,
          ImmutableDictionary<string, string> Metadata);
      
      // Avoid: Mutable collections in records
      public record Order(
          OrderId Id,
          List<OrderLine> Lines,  // Can be modified after creation
          Dictionary<string, string> Metadata);
      ```
  - Initialize immutable collections efficiently:
      ```csharp
      // Good: Using builder pattern
      var builder = ImmutableList.CreateBuilder<OrderLine>();
      foreach (var line in lines)
      {
          builder.Add(line);
      }
      return new Order(id, builder.ToImmutable());
      
      // Also Good: Using collection initializer
      return new Order(
          id,
          lines.ToImmutableList(),
          metadata.ToImmutableDictionary());
      ```

// ... existing code ...

Error Handling:
  - Use Result types for expected failures:
      ```csharp
      // Good: Explicit error handling
      public sealed record Result<T>
      {
          public T? Value { get; }
          public Error? Error { get; }
          
          private Result(T value) => Value = value;
          private Result(Error error) => Error = error;
          
          public static Result<T> Success(T value) => new(value);
          public static Result<T> Failure(Error error) => new(error);
      }
      ```
  - Prefer exceptions for exceptional cases:
      ```csharp
      // Good: Exception for truly exceptional case
      public static OrderId From(string value)
      {
          if (!Guid.TryParse(value, out var guid))
              throw new ArgumentException("Invalid OrderId format", nameof(value));
          
          return new OrderId(guid);
      }
      ```

Safe Operations:
  - Use Try methods for safer operations:
      ```csharp
      // Good: Using TryGetValue for dictionary access
      if (dictionary.TryGetValue(key, out var value))
      {
          // Use value safely here
      }
      else
      {
          // Handle missing key case
      }

      // Avoid: Direct indexing which can throw
      var value = dictionary[key];  // Throws if key doesn't exist

      // Good: Using Uri.TryCreate for URL parsing
      if (Uri.TryCreate(urlString, UriKind.Absolute, out var uri))
      {
          // Use uri safely here
      }
      else
      {
          // Handle invalid URL case
      }

      // Avoid: Direct Uri creation which can throw
      var uri = new Uri(urlString);  // Throws on invalid URL

      // Good: Using int.TryParse for number parsing
      if (int.TryParse(input, out var number))
      {
          // Use number safely here
      }
      else
      {
          // Handle invalid number case
      }

      // Good: Combining Try methods with null coalescing
      var value = dictionary.TryGetValue(key, out var result)
          ? result
          : defaultValue;

      // Good: Using Try methods in LINQ with pattern matching
      var validNumbers = strings
          .Select(s => (Success: int.TryParse(s, out var num), Value: num))
          .Where(x => x.Success)
          .Select(x => x.Value);
      ```
      
  - Prefer Try methods over exception handling:
      ```csharp
      // Good: Using Try method
      if (decimal.TryParse(priceString, out var price))
      {
          // Process price
      }

      // Avoid: Exception handling for expected cases
      try
      {
          var price = decimal.Parse(priceString);
          // Process price
      }
      catch (FormatException)
      {
          // Handle invalid format
      }
      ```

Asynchronous Programming:
  - Avoid async void:
      ```csharp
      // Good: Async method returns Task
      public async Task ProcessOrderAsync(Order order)
      {
          await _repository.SaveAsync(order);
      }
      
      // Avoid: Async void can crash your application
      public async void ProcessOrder(Order order)
      {
          await _repository.SaveAsync(order);
      }
      ```
  - Use Task.FromResult for pre-computed values:
      ```csharp
      // Good: Return pre-computed value
      public Task<int> GetDefaultQuantityAsync() =>
          Task.FromResult(1);
      
      // Better: Use ValueTask for zero allocations
      public ValueTask<int> GetDefaultQuantityAsync() =>
          new ValueTask<int>(1);
      
      // Avoid: Unnecessary thread pool usage
      public Task<int> GetDefaultQuantityAsync() =>
          Task.Run(() => 1);
      ```
  - Always flow CancellationToken:
      ```csharp
      // Good: Propagate cancellation
      public async Task<Order> ProcessOrderAsync(
          OrderRequest request,
          CancellationToken cancellationToken)
      {
          var order = await _repository.GetAsync(
              request.OrderId, 
              cancellationToken);
              
          await _processor.ProcessAsync(
              order, 
              cancellationToken);
              
          return order;
      }
      ```
  - Prefer await over ContinueWith:
      ```csharp
      // Good: Using await
      public async Task<Order> ProcessOrderAsync(OrderId id)
      {
          var order = await _repository.GetAsync(id);
          await _validator.ValidateAsync(order);
          return order;
      }
      
      // Avoid: Using ContinueWith
      public Task<Order> ProcessOrderAsync(OrderId id)
      {
          return _repository.GetAsync(id)
              .ContinueWith(t => 
              {
                  var order = t.Result; // Can deadlock
                  return _validator.ValidateAsync(order);
              });
      }
      ```
  - Never use Task.Result or Task.Wait:
      ```csharp
      // Good: Async all the way
      public async Task<Order> GetOrderAsync(OrderId id)
      {
          return await _repository.GetAsync(id);
      }
      
      // Avoid: Blocking on async code
      public Order GetOrder(OrderId id)
      {
          return _repository.GetAsync(id).Result; // Can deadlock
      }
      ```
  - Use TaskCompletionSource correctly:
      ```csharp
      // Good: Using RunContinuationsAsynchronously
      private readonly TaskCompletionSource<Order> _tcs = 
          new(TaskCreationOptions.RunContinuationsAsynchronously);
      
      // Avoid: Default TaskCompletionSource can cause deadlocks
      private readonly TaskCompletionSource<Order> _tcs = new();
      ```
  - Always dispose CancellationTokenSources:
      ```csharp
      // Good: Proper disposal of CancellationTokenSource
      public async Task<Order> GetOrderWithTimeout(OrderId id)
      {
          using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
          return await _repository.GetAsync(id, cts.Token);
      }
      ```
  - Prefer async/await over direct Task return:
      ```csharp
      // Good: Using async/await
      public async Task<Order> ProcessOrderAsync(OrderRequest request)
      {
          await _validator.ValidateAsync(request);
          var order = await _factory.CreateAsync(request);
          return order;
      }
      
      // Avoid: Manual task composition
      public Task<Order> ProcessOrderAsync(OrderRequest request)
      {
          return _validator.ValidateAsync(request)
              .ContinueWith(t => _factory.CreateAsync(request))
              .Unwrap();
      }
      ```

Nullability:
  - Enable nullable reference types:
      ```xml
      <PropertyGroup>
        <Nullable>enable</Nullable>
        <WarningsAsErrors>nullable</WarningsAsErrors>
      </PropertyGroup>
      ```
  - Mark nullable fields explicitly:
      ```csharp
      // Good: Explicit nullability
      public class OrderProcessor
      {
          private readonly ILogger<OrderProcessor>? _logger;
          private string? _lastError;
          
          public OrderProcessor(ILogger<OrderProcessor>? logger = null)
          {
              _logger = logger;
          }
      }
      
      // Avoid: Implicit nullability
      public class OrderProcessor
      {
          private readonly ILogger<OrderProcessor> _logger; // Warning: Could be null
          private string _lastError; // Warning: Could be null
      }
      ```
  - Use null checks appropriately:
      ```csharp
      // Good: Proper null checking
      public void ProcessOrder(Order? order)
      {
          if (order is null)
              throw new ArgumentNullException(nameof(order));
              
          _logger?.LogInformation("Processing order {Id}", order.Id);
      }
      
      // Good: Using pattern matching for null checks
      public decimal CalculateTotal(Order? order) =>
          order switch
          {
              null => throw new ArgumentNullException(nameof(order)),
              { Lines: null } => throw new ArgumentException("Order lines cannot be null", nameof(order)),
              _ => order.Lines.Sum(l => l.Total)
          };
      ```
  - Use null-forgiving operator when appropriate:
      ```csharp
      public class OrderValidator
      {
          private readonly IValidator<Order> _validator;
          
          public OrderValidator(IValidator<Order> validator)
          {
              _validator = validator ?? throw new ArgumentNullException(nameof(validator));
          }
          
          public ValidationResult Validate(Order order)
          {
              // We know _validator can't be null due to constructor check
              return _validator!.Validate(order);
          }
      }
      ```
  - Use nullability attributes:
      ```csharp
      public class StringUtilities
      {
          // Output is non-null if input is non-null
          [return: NotNullIfNotNull(nameof(input))]
          public static string? ToUpperCase(string? input) =>
              input?.ToUpperInvariant();
          
          // Method never returns null
          [return: NotNull]
          public static string EnsureNotNull(string? input) =>
              input ?? string.Empty;
          
          // Parameter must not be null when method returns true
          public static bool TryParse(string? input, [NotNullWhen(true)] out string? result)
          {
              result = null;
              if (string.IsNullOrEmpty(input))
                  return false;
                  
              result = input;
              return true;
          }
      }
      ```
  - Use init-only properties with non-null validation:
      ```csharp
      // Good: Non-null validation in constructor
      public sealed record Order
      {
          public required OrderId Id { get; init; }
          public required ImmutableList<OrderLine> Lines { get; init; }
          
          public Order()
          {
              Id = null!; // Will be set by required property
              Lines = null!; // Will be set by required property
          }
          
          private Order(OrderId id, ImmutableList<OrderLine> lines)
          {
              Id = id;
              Lines = lines;
          }
          
          public static Order Create(OrderId id, IEnumerable<OrderLine> lines) =>
              new(id, lines.ToImmutableList());
      }
      ```
  - Document nullability in interfaces:
      ```csharp
      public interface IOrderRepository
      {
          // Explicitly shows that null is a valid return value
          Task<Order?> FindByIdAsync(OrderId id, CancellationToken ct = default);
          
          // Method will never return null
          [return: NotNull]
          Task<IReadOnlyList<Order>> GetAllAsync(CancellationToken ct = default);
          
          // Parameter cannot be null
          Task SaveAsync([NotNull] Order order, CancellationToken ct = default);
      }
      ```

Symbol References:
  - Always use nameof operator:
      ```csharp
      // Good: Using nameof for parameter names
      public void ProcessOrder(Order order)
      {
          if (order is null)
              throw new ArgumentNullException(nameof(order));
      }
      
      // Good: Using nameof for property names
      public class Customer
      {
          private string _email;
          
          public string Email
          {
              get => _email;
              set => _email = value ?? throw new ArgumentNullException(nameof(value));
          }
          
          public void UpdateEmail(string newEmail)
          {
              if (string.IsNullOrEmpty(newEmail))
                  throw new ArgumentException("Email cannot be empty", nameof(newEmail));
              
              Email = newEmail;
          }
      }
      
      // Good: Using nameof in attributes
      public class OrderProcessor
      {
          [Required(ErrorMessage = "The {0} field is required")]
          [Display(Name = nameof(OrderId))]
          public string OrderId { get; init; }
          
          [MemberNotNull(nameof(_repository))]
          private void InitializeRepository()
          {
              _repository = new OrderRepository();
          }
          
          [NotifyPropertyChangedFor(nameof(FullName))]
          public string FirstName
          {
              get => _firstName;
              set => SetProperty(ref _firstName, value);
          }
      }
      
      // Avoid: Hard-coded string references
      public void ProcessOrder(Order order)
      {
          if (order is null)
              throw new ArgumentNullException("order"); // Breaks refactoring
      }
      ```
  - Use nameof with exceptions:
      ```csharp
      public class OrderService
      {
          public async Task<Order> GetOrderAsync(OrderId id, CancellationToken ct)
          {
              var order = await _repository.FindAsync(id, ct);
              
              if (order is null)
                  throw new OrderNotFoundException(
                      $"Order with {nameof(id)} '{id}' not found");
                      
              if (!order.Lines.Any())
                  throw new InvalidOperationException(
                      $"{nameof(order.Lines)} cannot be empty");
                      
              return order;
          }
          
          public void ValidateOrder(Order order)
          {
              ArgumentNullException.ThrowIfNull(order, nameof(order));
              
              if (order.Lines.Count == 0)
                  throw new ArgumentException(
                      "Order must have at least one line",
                      nameof(order));
          }
      }
      ```
  - Use nameof in logging:
      ```csharp
      public class OrderProcessor
      {
          private readonly ILogger<OrderProcessor> _logger;
          
          public async Task ProcessAsync(Order order)
          {
              _logger.LogInformation(
                  "Starting {Method} for order {OrderId}",
                  nameof(ProcessAsync),
                  order.Id);
                  
              try
              {
                  await ProcessInternalAsync(order);
              }
              catch (Exception ex)
              {
                  _logger.LogError(
                      ex,
                      "Error in {Method} for {Property} {Value}",
                      nameof(ProcessAsync),
                      nameof(order.Id),
                      order.Id);
                  throw;
              }
          }
      }
      ```