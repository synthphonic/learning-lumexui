# 🚀 C# Mastery Guide for AI Agents (.NET 9 & .NET 10 Exclusive)
### Modern C# Programming: From Fundamentals to Expert Level

> **This document is specifically designed for AI agents to master modern C# development using .NET 9 and .NET 10 exclusively. No legacy .NET Framework content included.**

---

## 📚 Table of Contents

1. [🎯 Learning Path Overview](#-learning-path-overview)
2. [🌱 Level 1: C# Fundamentals (.NET 9+ Features) (Weeks 1-4)](#-level-1-c-fundamentals-net-9-features-weeks-1-4)
   - **Week 1**: Modern C# 13 Fundamentals
   - **Week 2**: C# 13 Advanced Features
   - **Week 2.5**: Global Usings and Namespace Management (**🚨 MANDATORY FOR AI AGENTS**)
   - **Week 3**: Collection Expressions & Modern Patterns
   - **Week 4**: Modern Error Handling & Validation
3. [🔧 Level 2: Intermediate C# Programming (Weeks 5-12)](#-level-2-intermediate-c-programming-weeks-5-12)
4. [⚡ Level 3: Advanced C# Patterns (.NET 9/10 Features) (Weeks 13-20)](#-level-3-advanced-c-patterns-net-910-features-weeks-13-20)
5. [🏆 Level 4: Expert C# & Performance Optimization (Weeks 21-28)](#-level-4-expert-c--performance-optimization-weeks-21-28)
6. [🛠️ Essential Tools & Extensions](#️-essential-tools--extensions)
7. [📖 Key Books & Publications (.NET 9/10)](#-key-books--publications-net-910)
8. [🎓 Certification & Validation](#-certification--validation)
9. [🔗 Quick Reference Links](#-quick-reference-links)

---

## 🎯 Learning Path Overview

### **AI Agent Learning Philosophy**
- **Modern-First**: Focus exclusively on .NET 9 and .NET 10 features
- **Performance-Driven**: Leverage latest C# 13/14 syntax for optimal performance
- **Practical Patterns**: Real-world enterprise scenarios
- **Memory-Safe**: Utilize Span<T>, Memory<T> and modern patterns
- **Concurrent-Ready**: Async/await and modern threading patterns

### **Prerequisites for AI Agents**
- Understanding of programming fundamentals
- Basic knowledge of object-oriented concepts
- Familiarity with web development concepts
- Database and API fundamentals

---

## 🌱 Level 1: C# Fundamentals (.NET 9+ Features) (Weeks 1-4)

### **Week 1: Modern C# 13 Fundamentals**

#### **C# 13 Essential Features**
```csharp
// 1. Params Collections (NEW in C# 13)
public void ProcessItems(params ReadOnlySpan<int> items)
{
    foreach (var item in items)
    {
        Console.WriteLine(item);
    }
}

// Usage with collection literals
ProcessItems([1, 2, 3, 4, 5]);
ProcessItems(stackalloc int[] { 6, 7, 8 });

// 2. Field Keyword (Preview in C# 13, Standard in C# 14)
public class Product
{
    public string Name
    {
        get => field ?? "Default";
        set => field = value?.Trim() ?? throw new ArgumentNullException(nameof(value));
    }
}

// 3. New Lock Type and Semantics
private readonly Lock _lock = new();

public void CriticalSection()
{
    using var scope = _lock.EnterScope();
    // Critical section code
    // Scope automatically exits with Dispose()
}

// 4. New Escape Sequence \e
string escapeSequence = "\e[31mRed Text\e[0m"; // ANSI escape codes
Console.WriteLine(escapeSequence);
```

#### **Essential Resources**
- **📖 [C# 13 New Features](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-13)**
  - Complete overview of all C# 13 features
  - Code examples and best practices
  - Migration guide from C# 12

- **🎓 [.NET 9 Documentation](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - Platform-specific improvements
  - Performance enhancements
  - Runtime optimizations

#### **Practice Projects**
1. **Modern Calculator**: Use field keyword and params collections
2. **Collection Processor**: Implement with Span<T> and new syntax
3. **Thread-Safe Logger**: Use new Lock semantics

### **Week 2: C# 13 Advanced Features**

#### **Partial Properties and Indexers**
```csharp
// Partial Class with Partial Properties
public partial class DataService
{
    // Declaration part
    public partial string ConnectionString { get; set; }
    public partial DataRecord this[int id] { get; }
}

public partial class DataService
{
    // Implementation part
    private string _connectionString;
    public partial string ConnectionString
    {
        get => _connectionString;
        set => _connectionString = ValidateConnectionString(value);
    }

    private Dictionary<int, DataRecord> _records = new();
    public partial DataRecord this[int id] => _records.TryGetValue(id, out var record)
        ? record
        : throw new KeyNotFoundException($"Record {id} not found");
}

// Ref Struct Interfaces (NEW in C# 13)
public ref struct HighPerformanceBuffer : IDisposable
{
    private readonly Span<byte> _buffer;
    private bool _disposed;

    public HighPerformanceBuffer(int size)
    {
        _buffer = new byte[size];
    }

    public ReadOnlySpan<byte> Data => _disposed ? default : _buffer;

    public void Dispose()
    {
        if (!_disposed)
        {
            _disposed = true;
        }
    }
}

// Implement interface
public interface IBuffer
{
    ReadOnlySpan<byte> Data { get; }
    void Dispose();
}

public ref struct OptimizedBuffer : IBuffer
{
    private Span<byte> _data;
    private bool _disposed;

    public ReadOnlySpan<byte> Data => _disposed ? default : _data;

    public void Dispose()
    {
        if (!_disposed)
        {
            _data = default;
            _disposed = true;
        }
    }
}
```

#### **Essential Resources**
- **📖 [Partial Members Documentation](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-13)**
  - Partial properties and indexers
  - Code generation scenarios
  - Best practices

- **🎓 [Ref Struct Improvements](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-13)**
  - Interface implementation
  - Performance considerations
  - Memory management

#### **Practice Projects**
1. **Code Generator**: Use partial properties for generated code
2. **High-Performance Cache**: Implement with ref structs
3. **Data Access Layer**: Modern patterns with partial classes

### **Week 2.5: Global Usings and Namespace Management**

> **🚨 CRITICAL AI AGENT REQUIREMENT**: This section establishes mandatory namespace management patterns that ALL AI agents MUST follow when creating or modifying C# projects.

#### **🌐 Global Usings Philosophy**

Global usings represent a fundamental shift in C# namespace management, promoting cleaner code by eliminating repetitive using statements while centralizing namespace imports for better maintainability.

**Core Benefits:**
- **Clean Code**: Eliminates namespace pollution at the top of every `.cs` file
- **Centralized Management**: Single location for all project namespace dependencies
- **Improved Readability**: Focus on business logic rather than namespace imports
- **Consistent Patterns**: Standardized approach across all project files
- **AI Agent Efficiency**: Clear, repeatable workflow for namespace handling

#### **⚙️ AI Agent Requirements (MANDATORY)**

**🔴 NON-NEGOTIABLE RULES FOR ALL AI AGENTS:**

1. **ALWAYS Create `_GlobalUsings.cs`**: Every new project MUST have a `_GlobalUsings.cs` file at the project root
2. **CENTRALIZE All Namespaces**: ALL namespace using statements for `.cs` files MUST be in `_GlobalUsings.cs`
3. **FORBID Using Statements in `.cs` Files**: `.cs` files MUST NOT contain any namespace using statements at the top
4. **ORGANIZE by Category**: Namespaces MUST be organized by category with alphabetical sorting within each category
5. **MAINTAIN Razor Pattern**: Continue using `_Imports.razor` for Razor components (existing pattern)

**Project Structure Standard:**
```
ProjectRoot/
├── _GlobalUsings.cs           # 🎯 CENTRAL: All .cs file namespaces
├── Program.cs                 # ✅ CLEAN: No using statements
├── Components/
│   ├── _Imports.razor        # ✅ MAINTAIN: Razor component namespaces
│   └── [Component files]     # ✅ CLEAN: No @using statements
└── [Other .cs files]         # ✅ CLEAN: No using statements
```

#### **🏗️ Implementation Patterns**

**_GlobalUsings.cs Template Structure:**
```csharp
// Global using statements for [ProjectName]
// Centralizes all namespace imports for .cs files throughout the project
// AI AGENT: This file is MANDATORY for all projects

// ===================================================================
// SYSTEM CORE NAMESPACES
// ===================================================================
global using System;
global using System.Collections.Generic;
global using System.ComponentModel.DataAnnotations;
global using System.Linq;
global using System.Text;
global using System.Threading;
global using System.Threading.Tasks;

// ===================================================================
// ASP.NET CORE NAMESPACES
// ===================================================================
global using Microsoft.AspNetCore.Builder;
global using Microsoft.AspNetCore.Components;
global using Microsoft.AspNetCore.Components.Forms;
global using Microsoft.AspNetCore.Components.Web;
global using Microsoft.AspNetCore.Hosting;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Hosting;
global using Microsoft.Extensions.Logging;

// ===================================================================
// THIRD-PARTY LIBRARY NAMESPACES
// ===================================================================
global using LumexUI;
global using LumexUI.Extensions;
global using LumexUI.Components;

// ===================================================================
// PROJECT-SPECIFIC NAMESPACES
// ===================================================================
global using [ProjectName].Components;
global using [ProjectName].Data;
global using [ProjectName].Models;
global using [ProjectName].Services;
```

**Clean .cs File Example:**
```csharp
// BEFORE (Traditional approach - FORBIDDEN)
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components;
using LumexUI;
using MyProject.Services;

namespace MyProject.Components
{
    public class MyComponent : ComponentBase
    {
        // Component code
    }
}

// AFTER (Global usings approach - REQUIRED)
namespace MyProject.Components
{
    public class MyComponent : ComponentBase
    {
        // Component code - NO using statements needed!
    }
}
```

#### **📁 Namespace Organization Standards**

**Category 1: System Core Namespaces** (Alphabetical)
```csharp
// Core .NET namespaces used throughout the application
global using System;
global using System.Collections.Generic;
global using System.ComponentModel.DataAnnotations;
global using System.Linq;
global using System.Text;
global using System.Threading;
global using System.Threading.Tasks;
```

**Category 2: ASP.NET Core Namespaces** (Alphabetical)
```csharp
// ASP.NET Core and web-related namespaces
global using Microsoft.AspNetCore.Builder;
global using Microsoft.AspNetCore.Components;
global using Microsoft.AspNetCore.Components.Forms;
global using Microsoft.AspNetCore.Components.Web;
global using Microsoft.AspNetCore.Hosting;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Hosting;
global using Microsoft.Extensions.Logging;
```

**Category 3: Third-Party Library Namespaces** (Alphabetical)
```csharp
// External libraries and frameworks
global using LumexUI;
global using LumexUI.Extensions;
global using LumexUI.Components;
global using Dapper;
global using Serilog;
```

**Category 4: Project-Specific Namespaces** (Alphabetical)
```csharp
// Your application's internal namespaces
global using MyProject.Components;
global using MyProject.Data;
global using MyProject.Models;
global using MyProject.Services;
global using MyProject.Utilities;
```

#### **🔧 AI Agent Workflow Integration**

**Mandatory Project Creation Workflow:**

1. **Analysis Phase**: Identify all required namespaces before coding
   - Review project requirements and dependencies
   - List all namespaces that will be needed
   - Categorize namespaces according to standards

2. **Creation Phase**: Create `_GlobalUsings.cs` with proper categorization
   - Create file at project root
   - Add namespaces in correct categories
   - Ensure alphabetical sorting within categories

3. **Coding Phase**: Generate clean `.cs` files without using statements
   - Write all C# code without using statements
   - Verify all required namespaces are in `_GlobalUsings.cs`
   - Test compilation to ensure namespace coverage

4. **Validation Phase**: Verify compilation and compliance
   - Build the project to verify all namespaces are available
   - Check for any missing namespace references
   - Validate compliance with AI agent requirements

**Quality Assurance Checklist:**
```
□ _GlobalUsings.cs exists at project root
□ .cs files contain no using statements
□ Namespaces organized by category
□ Categories sorted alphabetically
□ Project compiles successfully
□ No unused namespaces
□ Razor components still use _Imports.razor
□ All required namespaces are available
```

#### **🔍 Migration Guide for Existing Projects**

**Converting Traditional Projects:**

1. **Audit Current Using Statements:**
   ```bash
   # Find all using statements in the project
   grep -r "^using " . --include="*.cs" | sort | uniq
   ```

2. **Create _GlobalUsings.cs:**
   - Collect all unique namespaces from audit
   - Organize by category using the template structure
   - Add to project root

3. **Remove Using Statements from .cs Files:**
   - Remove all `using` statements from the top of `.cs` files
   - Ensure no namespace aliasing or special cases are lost
   - Test compilation after cleanup

4. **Handle Special Cases:**
   ```csharp
   // Namespace aliases (Keep in .cs file if needed)
   using OldFramework = LegacyLibrary.OldNamespace;

   // Conditional compilation (Keep in .cs file)
   #if DEBUG
   using System.Diagnostics;
   #endif
   ```

#### **🚨 Common Pitfalls and Solutions**

**Pitfall 1: Missing Namespaces**
- **Problem**: Build fails because namespace not in `_GlobalUsings.cs`
- **Solution**: Add missing namespace to appropriate category
- **Prevention**: Comprehensive namespace analysis during creation phase

**Pitfall 2: Namespace Conflicts**
- **Problem**: Multiple namespaces with same class names
- **Solution**: Use explicit namespace qualification or aliasing in specific files
- **Example**: `global using System.Data;` and `global using MyProject.Data;`

**Pitfall 3: Overcomplicated Global Usings**
- **Problem**: Too many namespaces making it hard to track dependencies
- **Solution**: Be selective - only add namespaces actually used throughout the project
- **Best Practice**: Review and clean up unused namespaces regularly

**Pitfall 4: Forgetting Razor Pattern**
- **Problem**: Accidentally using global usings for Razor components
- **Solution**: Remember that `.razor` files continue using `_Imports.razor`
- **Pattern**: `GlobalUsings.cs` for `.cs` files, `_Imports.razor` for `.razor` files

#### **✅ Advanced Global Usings Patterns**

**Conditional Global Usings:**
```csharp
#if DEBUG
global using System.Diagnostics;
global using System.Diagnostics.CodeAnalysis;
#endif

#if RELEASE
global using System.Runtime.CompilerServices;
#endif
```

**Static Global Usings:**
```csharp
// Enable static method access without class name
global using static System.Math;
global using static System.Console;

/// Usage: WriteLn(Sin(PI/2)) instead of Console.WriteLine(Math.Sin(Math.PI/2))
```

**Global Alias Directives:**
```csharp
// Create aliases for frequently used types
global using ProjectComponent = MyProject.Components.BaseComponent;
global using Result = Microsoft.AspNetCore.Mvc.Result;
```

#### **🎯 Integration with Modern .NET Patterns**

**Global Usings with C# 13 Features:**
```csharp
// Modern approach combining global usings with C# 13 syntax
global using System;
global using System.Collections.Generic;

// Field keyword usage in clean files
public class ModernProduct
{
    public string Name
    {
        get => field ?? string.Empty;
        set => field = value?.Trim() ?? throw new ArgumentNullException(nameof(value));
    }
}

// No using statements needed at file level!
```

**Performance Considerations:**
- Global usings have minimal impact on compilation time
- No runtime performance penalty
- Improved developer productivity and code readability
- Better IDE performance due to reduced parsing overhead

#### **📚 Essential Resources**

- **📖 [Global Using Directives (C# Reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-directive#global-using-directive)**
  - Complete syntax and usage patterns
  - Advanced scenarios and best practices
  - Compatibility information

- **🎓 [Organize and Refactor Using Directives](https://learn.microsoft.com/en-us/visualstudio/ide/reference/organize-using-directives)**
  - IDE automation for using statement management
  - Code cleanup and refactoring techniques
  - Integration with build processes

#### **Practice Projects**
1. **Project Converter**: Take an existing project and convert it to use global usings
2. **Namespace Analyzer**: Create a tool to analyze and suggest global usings organization
3. **Template Generator**: Build project templates with pre-configured global usings for different project types

---

### **Week 3: Collection Expressions & Modern Patterns**

#### **C# 12+ Collection Expressions with .NET 9 Enhancements**
```csharp
// Modern Collection Initialization
// NOTE: This example assumes global usings are configured in _GlobalUsings.cs
// Required namespaces: System, System.Collections.Generic
public class ModernCollections
{
    // Basic collection expressions
    public int[] ArrayData = [1, 2, 3, 4, 5];
    public List<string> StringList = ["first", "second", "third"];
    public Span<char> CharSpan = ['a', 'b', 'c', 'd', 'e'];

    // Spread operator
    public int[] Combined = [.. ArrayData, 6, 7, 8];

    // 2D arrays with collection expressions
    public int[][] Matrix =
    [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
    ];

    // Performance-optimized collections
    public ReadOnlyMemory<int> ReadOnlyData = [10, 20, 30];

    // Modern method with params collections
    public void ProcessData(params ReadOnlySpan<int> values)
    {
        foreach (var value in values)
        {
            Console.WriteLine($"Processing: {value}");
        }
    }

    // Usage examples
    public void DemonstrateUsage()
    {
        ProcessData([1, 2, 3, 4, 5]);

        // Implicit indexer access in object initializers
        var countdown = new TimerData
        {
            Values =
            {
                [^1] = 0,  // Last element
                [^2] = 1,  // Second to last
                [^3] = 2   // Third to last
            }
        };
    }
}

public class TimerData
{
    public int[] Values { get; set; } = new int[10];
}
```

#### **Essential Resources**
- **📖 [Collection Expressions](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12)**
  - Syntax and usage patterns
  - Performance considerations
  - Comparison with traditional methods

- **🎓 [.NET 9 Performance Guide](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - Runtime optimizations
  - Memory management improvements
  - Native AOT enhancements

#### **Practice Projects**
1. **Data Processing Pipeline**: Use collection expressions and Span<T>
2. **Configuration Manager**: Modern initialization patterns
3. **Math Library**: Performance-optimized with new syntax

### **Week 4: Modern Error Handling & Validation**

#### **Result<T> Pattern with Modern C#**
```csharp
// Modern Error Handling with Result Pattern
public record Error(string Code, string Message, ErrorType Type);

public enum ErrorType
{
    Validation,
    Business,
    Security,
    System
}

public readonly record struct Result<T>(T? Value, Error? Error, bool IsSuccess)
{
    public static Result<T> Success(T value) => new(value, null, true);
    public static Result<T> Failure(Error error) => new(default, error, false);

    public TResult Match<TResult>(
        Func<T, TResult> onSuccess,
        Func<Error, TResult> onFailure) =>
        IsSuccess ? onSuccess(Value!) : onFailure(Error!);
}

// Usage in modern C#
public class UserService
{
    private readonly IUserRepository _repository;

    public Result<User> CreateUser(CreateUserCommand command)
    {
        // Validation with collection expressions
        var errors = ValidateUser(command);
        if (errors.Count > 0)
        {
            return Result<User>.Failure(new Error(
                "ValidationFailed",
                string.Join(", ", errors),
                ErrorType.Validation));
        }

        // Modern LINQ with pattern matching
        var existingUser = _repository.FindByEmail(command.Email);
        if (existingUser is not null)
        {
            return Result<User>.Failure(new Error(
                "UserExists",
                "User with this email already exists",
                ErrorType.Business));
        }

        var user = new User(command.Name, command.Email);
        _repository.Add(user);

        return Result<User>.Success(user);
    }

    private List<string> ValidateUser(CreateUserCommand command)
    {
        var errors = new List<string>();

        if (string.IsNullOrWhiteSpace(command.Name))
            errors.Add("Name is required");

        if (command.Name?.Length > 100)
            errors.Add("Name cannot exceed 100 characters");

        if (command.Email is null || !command.Email.Contains('@'))
            errors.Add("Valid email is required");

        return errors;
    }
}

// Record with primary constructor (C# 12)
public record User(string Name, string Email, Guid Id = default)
{
    public Guid Id { get; } = Id == default ? Guid.NewGuid() : Id;
}

public record CreateUserCommand(string Name, string Email);
```

#### **Essential Resources**
- **📖 [Modern Error Handling Patterns](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12)**
  - Result pattern implementation
  - Functional error handling
  - Comparison with exceptions

- **🎓 [.NET 9 Validation Guide](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - Built-in validation improvements
  - Performance-optimized validation
  - Integration patterns

#### **Practice Projects**
1. **API Service Layer**: Implement Result<T> pattern
2. **Validation Framework**: Modern validation with collection expressions
3. **Error Reporting System**: Advanced error handling and logging

---

## 🔧 Level 2: Intermediate C# Programming (Weeks 5-12)

### **Week 5-6: Async/Await & Modern Concurrency**

#### **Modern Async Patterns with .NET 9**
```csharp
// Modern Async/Await with ValueTask and Channel
// NOTE: This example assumes global usings are configured in _GlobalUsings.cs
// Required namespaces: System, System.Threading, System.Threading.Channels, System.Threading.Tasks
public class ModernAsyncService
{
    private readonly Channel<WorkItem> _workChannel;
    private readonly SemaphoreSlim _semaphore;

    public ModernAsyncService()
    {
        var options = new BoundedChannelOptions(1000)
        {
            FullMode = BoundedChannelFullMode.Wait,
            SingleReader = false,
            SingleWriter = false
        };
        _workChannel = Channel.CreateBounded<WorkItem>(options);
        _semaphore = new SemaphoreSlim(Environment.ProcessorCount);
    }

    public async ValueTask EnqueueWorkAsync(WorkItem item)
    {
        await _workChannel.Writer.WriteAsync(item);
    }

    public async Task ProcessWorkItemsAsync(CancellationToken cancellationToken = default)
    {
        await foreach (var workItem in _workChannel.Reader.ReadAllAsync(cancellationToken))
        {
            await ProcessSingleItemAsync(workItem, cancellationToken);
        }
    }

    private async Task ProcessSingleItemAsync(WorkItem item, CancellationToken cancellationToken)
    {
        await _semaphore.WaitAsync(cancellationToken);
        try
        {
            // Modern async with ConfigureAwait
            await ProcessWithRetryAsync(item, cancellationToken).ConfigureAwait(false);
        }
        finally
        {
            _semaphore.Release();
        }
    }

    private async Task ProcessWithRetryAsync(WorkItem item, CancellationToken cancellationToken)
    {
        var maxRetries = 3;
        var delay = TimeSpan.FromSeconds(1);

        for (int attempt = 1; attempt <= maxRetries; attempt++)
        {
            try
            {
                await item.ProcessAsync(cancellationToken);
                return; // Success
            }
            catch (Exception ex) when (attempt < maxRetries && IsRetriable(ex))
            {
                await Task.Delay(delay * attempt, cancellationToken);
            }
        }

        throw new InvalidOperationException($"Failed after {maxRetries} attempts");
    }

    private static bool IsRetriable(Exception exception) =>
        exception is HttpRequestException or TimeoutException;
}

// Modern async enumerable pattern
public async IAsyncEnumerable<ProcessedItem> ProcessItemsAsync(
    IEnumerable<WorkItem> items,
    [EnumeratorCancellation] CancellationToken cancellationToken = default)
{
    foreach (var item in items)
    {
        var result = await ProcessItemAsync(item, cancellationToken);
        yield return result;
    }
}

// Usage with modern C#
public class AsyncExample
{
    public async Task DemonstrateAsync()
    {
        var service = new ModernAsyncService();

        // Parallel processing with limited concurrency
        var workItems = Enumerable.Range(1, 100)
            .Select(i => new WorkItem($"Item-{i}"));

        await Parallel.ForEachAsync(workItems, async (item, ct) =>
        {
            await service.EnqueueWorkAsync(item);
        });

        // Process with cancellation
        using var cts = new CancellationTokenSource(TimeSpan.FromMinutes(5));
        await service.ProcessWorkItemsAsync(cts.Token);
    }
}

public record WorkItem(string Id)
{
    public async Task ProcessAsync(CancellationToken cancellationToken)
    {
        // Simulate work
        await Task.Delay(Random.Shared.Next(100, 1000), cancellationToken);
        Console.WriteLine($"Processed {Id}");
    }
}

public record ProcessedItem(string Id, DateTime ProcessedAt);
```

#### **Essential Resources**
- **📖 [Async/Await Best Practices](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/)**
  - ConfigureAwait guidance
  - ValueTask usage patterns
  - Async enumerable patterns

- **🎓 [.NET 9 Concurrency Improvements](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - Channel improvements
  - Performance optimizations
  - Memory safety enhancements

#### **Practice Projects**
1. **Background Job Processor**: Use Channel and ValueTask
2. **Real-time Data Stream**: Async enumerable patterns
3. **API Client**: Modern async with retry logic

### **Week 7-8: LINQ & Functional Programming**

#### **Modern LINQ with C# 12+ Features**
```csharp
// Advanced LINQ with modern C# patterns
// NOTE: This example assumes global usings are configured in _GlobalUsings.cs
// Required namespaces: System, System.Collections.Generic, System.Linq
public class ModernLinqExamples
{
    private readonly List<Order> _orders;

    public ModernLinqExamples()
    {
        _orders = GenerateSampleOrders();
    }

    // Modern LINQ with pattern matching and switch expressions
    public Dictionary<string, decimal> GetCustomerSpendingByCategory()
    {
        return _orders
            .GroupBy(order => order.CustomerId)
            .ToDictionary(
                group => group.Key,
                group => group
                    .SelectMany(order => order.Items)
                    .GroupBy(item => item.Category)
                    .Select(categoryGroup => categoryGroup.Sum(item => item.Price * item.Quantity))
                    .Sum()
            );
    }

    // Advanced query with collection expressions
    public Report GenerateSalesReport()
    {
        var report = new Report
        {
            TotalOrders = _orders.Count,
            TotalRevenue = _orders.Sum(o => o.TotalAmount),
            AverageOrderValue = _orders.Average(o => o.TotalAmount),
            TopSellingItems = _orders
                .SelectMany(o => o.Items)
                .GroupBy(i => i.ProductId)
                .Select(g => new ItemSales
                {
                    ProductId = g.Key,
                    TotalSold = g.Sum(i => i.Quantity),
                    Revenue = g.Sum(i => i.Price * i.Quantity)
                })
                .OrderByDescending(x => x.TotalSold)
                .Take(10)
                .ToList()
        };

        return report;
    }

    // Functional style data processing
    public IEnumerable<AnalyzedOrder> AnalyzeOrders()
    {
        return _orders
            .Select(order => new AnalyzedOrder
            {
                Order = order,
                RiskScore = CalculateRiskScore(order),
                Category = ClassifyOrder(order),
                Recommendations = GenerateRecommendations(order)
            })
            .Where(analysis => analysis.RiskScore < 0.7)
            .OrderBy(analysis => analysis.Order.OrderDate);
    }

    private double CalculateRiskScore(Order order)
    {
        return order switch
        {
            { TotalAmount: > 1000, CustomerId: var id } when IsNewCustomer(id) => 0.8,
            { TotalAmount: > 500, Items.Count: > 10 } => 0.6,
            { TotalAmount: < 100 } => 0.2,
            _ => 0.4
        };
    }

    private OrderCategory ClassifyOrder(Order order)
    {
        return order.Items.Sum(item => item.Quantity) switch
        {
            > 50 => OrderCategory.Wholesale,
            > 10 => OrderCategory.Business,
            _ => OrderCategory.Retail
        };
    }

    private List<string> GenerateRecommendations(Order order)
    {
        var recommendations = new List<string>();

        if (order.TotalAmount > 500)
            recommendations.Add("Consider loyalty program enrollment");

        if (order.Items.Any(item => item.Category == "Electronics"))
            recommendations.Add("Add warranty options");

        return recommendations;
    }

    private bool IsNewCustomer(string customerId) =>
        !_orders.Any(o => o.CustomerId == customerId && o.OrderDate < DateTime.Now.AddDays(-30));

    private List<Order> GenerateSampleOrders()
    {
        return
        [
            new Order("CUST001", DateTime.Now.AddDays(-5))
            {
                Items =
                [
                    new OrderItem("PROD001", "Laptop", 999.99m, 1, "Electronics"),
                    new OrderItem("PROD002", "Mouse", 29.99m, 2, "Electronics")
                ]
            },
            new Order("CUST002", DateTime.Now.AddDays(-3))
            {
                Items =
                [
                    new OrderItem("PROD003", "Book", 19.99m, 5, "Books"),
                    new OrderItem("PROD004", "Pen", 1.99m, 10, "Stationery")
                ]
            }
        ];
    }
}

// Supporting types with modern C# features
public record Order(string CustomerId, DateTime OrderDate)
{
    public List<OrderItem> Items { get; init; } = [];
    public decimal TotalAmount => Items.Sum(item => item.Price * item.Quantity);
}

public record OrderItem(string ProductId, string ProductName, decimal Price, int Quantity, string Category);

public record Report
{
    public int TotalOrders { get; init; }
    public decimal TotalRevenue { get; init; }
    public decimal AverageOrderValue { get; init; }
    public List<ItemSales> TopSellingItems { get; init; } = [];
}

public record ItemSales(string ProductId, int TotalSold, decimal Revenue);

public record AnalyzedOrder(Order Order, double RiskScore, OrderCategory Category, List<string> Recommendations);

public enum OrderCategory
{
    Retail,
    Business,
    Wholesale
}
```

#### **Essential Resources**
- **📖 [LINQ Documentation](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)**
  - Query syntax patterns
  - Performance considerations
  - Custom operators

- **🎓 [Functional Programming in C#](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/)**
  - Higher-order functions
  - Immutable data structures
  - Pattern matching

#### **Practice Projects**
1. **Data Analysis Tool**: Advanced LINQ queries with modern syntax
2. **Report Generator**: Functional programming patterns
3. **ETL Pipeline**: Modern data processing with LINQ

### **Week 9-10: Dependency Injection & Modern Architecture**

#### **Modern DI with .NET 9**
```csharp
// Modern Dependency Injection with .NET 9 enhancements
public class ModernApplication
{
    public static IServiceCollection ConfigureServices(IServiceCollection services)
    {
        // Modern service registration
        services.AddHttpClient<IWeatherService, WeatherService>(client =>
        {
            client.BaseAddress = new Uri("https://api.weather.com");
            client.DefaultRequestHeaders.Add("User-Agent", "ModernApp/1.0");
        });

        // Options pattern with validation
        services.AddOptions<ApiSettings>()
            .BindConfiguration("ApiSettings")
            .ValidateDataAnnotations()
            .ValidateOnStart();

        // Modern health checks
        services.AddHealthChecks()
            .AddCheck<DatabaseHealthCheck>("database")
            .AddCheck<ExternalApiHealthCheck>("external_api");

        // Scoped services with lifetime validation
        services.AddScoped<IUserRepository, UserRepository>();
        services.AddScoped<IUserService, UserService>();
        services.AddScoped<IUserValidator, UserValidator>();

        // Singleton with factory
        services.AddSingleton<ICacheService>(provider =>
        {
            var options = provider.GetRequiredService<IOptions<CacheOptions>>().Value;
            return new RedisCacheService(options.ConnectionString);
        });

        // Modern configuration
        services.Configure<CachingOptions>(configuration.GetSection("Caching"));

        return services;
    }
}

// Modern service with minimal APIs integration
public class UserEndpoints : IEndpointDefinition
{
    public void DefineEndpoints(IEndpointRouteBuilder app)
    {
        var group = app.MapGroup("/api/users")
            .RequireAuthorization()
            .WithTags("Users")
            .WithOpenApi();

        group.MapGet("/", GetUsersAsync)
            .Produces<UserDto[]>(StatusCodes.Status200OK);

        group.MapGet("/{id:guid}", GetUserAsync)
            .Produces<UserDto>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status404NotFound);

        group.MapPost("/", CreateUserAsync)
            .Accepts<CreateUserCommand>("application/json")
            .Produces<UserDto>(StatusCodes.Status201Created)
            .Produces(StatusCodes.Status400BadRequest);

        group.MapPut("/{id:guid}", UpdateUserAsync)
            .Accepts<UpdateUserCommand>("application/json")
            .Produces<UserDto>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status404NotFound);

        group.MapDelete("/{id:guid}", DeleteUserAsync)
            .Produces(StatusCodes.Status204NoContent)
            .Produces(StatusCodes.Status404NotFound);
    }

    private static async Task<IResult> GetUsersAsync(
        IUserService userService,
        [FromQuery] int page = 1,
        [FromQuery] int pageSize = 10,
        [FromQuery] string? search = null)
    {
        var users = await userService.GetUsersAsync(page, pageSize, search);
        return Results.Ok(users);
    }

    private static async Task<IResult> GetUserAsync(
        Guid id,
        IUserService userService)
    {
        var user = await userService.GetUserAsync(id);
        return user is not null ? Results.Ok(user) : Results.NotFound();
    }

    private static async Task<IResult> CreateUserAsync(
        CreateUserCommand command,
        IUserService userService,
        IValidator<CreateUserCommand> validator)
    {
        var validationResult = await validator.ValidateAsync(command);
        if (!validationResult.IsValid)
        {
            return Results.ValidationProblem(validationResult.ToDictionary());
        }

        var user = await userService.CreateUserAsync(command);
        return Results.Created($"/api/users/{user.Id}", user);
    }

    private static async Task<IResult> UpdateUserAsync(
        Guid id,
        UpdateUserCommand command,
        IUserService userService,
        IValidator<UpdateUserCommand> validator)
    {
        var validationResult = await validator.ValidateAsync(command);
        if (!validationResult.IsValid)
        {
            return Results.ValidationProblem(validationResult.ToDictionary());
        }

        var user = await userService.UpdateUserAsync(id, command);
        return user is not null ? Results.Ok(user) : Results.NotFound();
    }

    private static async Task<IResult> DeleteUserAsync(
        Guid id,
        IUserService userService)
    {
        var deleted = await userService.DeleteUserAsync(id);
        return deleted ? Results.NoContent() : Results.NotFound();
    }
}

// Modern service with proper error handling
public class UserService : IUserService
{
    private readonly IUserRepository _repository;
    private readonly ICacheService _cache;
    private readonly ILogger<UserService> _logger;
    private readonly IMapper _mapper;

    public UserService(
        IUserRepository repository,
        ICacheService cache,
        ILogger<UserService> logger,
        IMapper mapper)
    {
        _repository = repository;
        _cache = cache;
        _logger = logger;
        _mapper = mapper;
    }

    public async Task<UserDto?> GetUserAsync(Guid id)
    {
        var cacheKey = $"user_{id}";

        // Try cache first
        var cachedUser = await _cache.GetAsync<UserDto>(cacheKey);
        if (cachedUser is not null)
        {
            return cachedUser;
        }

        // Get from repository
        var user = await _repository.GetByIdAsync(id);
        if (user is null)
        {
            return null;
        }

        var userDto = _mapper.Map<UserDto>(user);

        // Cache for 5 minutes
        await _cache.SetAsync(cacheKey, userDto, TimeSpan.FromMinutes(5));

        return userDto;
    }

    public async Task<UserDto> CreateUserAsync(CreateUserCommand command)
    {
        var user = _mapper.Map<User>(command);

        var createdUser = await _repository.AddAsync(user);

        _logger.LogInformation("Created new user with ID: {UserId}", createdUser.Id);

        var userDto = _mapper.Map<UserDto>(createdUser);

        // Invalidate cache
        await _cache.RemoveAsync("users_list");

        return userDto;
    }
}

// Configuration with validation
public record ApiSettings
{
    public required string BaseUrl { get; init; }
    public required string ApiKey { get; init; }
    public TimeSpan Timeout { get; init; } = TimeSpan.FromSeconds(30);
}

public record CacheOptions
{
    public string ConnectionString { get; init; } = "localhost:6379";
    public TimeSpan DefaultExpiration { get; init; } = TimeSpan.FromMinutes(5);
}
```

#### **Essential Resources**
- **📖 [.NET 9 Dependency Injection](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - DI container improvements
  - Service lifetime management
  - Performance optimizations

- **🎓 [Minimal APIs Guide](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/)**
  - API endpoint definitions
  - Parameter binding
  - Response handling

#### **Practice Projects**
1. **Microservice Template**: Modern DI and minimal APIs
2. **E-commerce API**: Complete service architecture
3. **Health Monitoring System**: Advanced health checks

### **Week 11-12: Testing & Quality Assurance**

#### **Modern Testing with .NET 9**
```csharp
// Modern testing with xUnit and Moq
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _repositoryMock;
    private readonly Mock<ICacheService> _cacheMock;
    private readonly Mock<ILogger<UserService>> _loggerMock;
    private readonly Mock<IMapper> _mapperMock;
    private readonly UserService _userService;

    public UserServiceTests()
    {
        _repositoryMock = new Mock<IUserRepository>();
        _cacheMock = new Mock<ICacheService>();
        _loggerMock = new Mock<ILogger<UserService>>();
        _mapperMock = new Mock<IMapper>();

        _userService = new UserService(
            _repositoryMock.Object,
            _cacheMock.Object,
            _loggerMock.Object,
            _mapperMock.Object);
    }

    [Fact]
    public async Task GetUserAsync_UserExists_ReturnsUserFromCache()
    {
        // Arrange
        var userId = Guid.NewGuid();
        var expectedUser = new UserDto { Id = userId, Name = "John Doe" };

        _cacheMock
            .Setup(x => x.GetAsync<UserDto>($"user_{userId}"))
            .ReturnsAsync(expectedUser);

        // Act
        var result = await _userService.GetUserAsync(userId);

        // Assert
        result.Should().NotBeNull();
        result!.Id.Should().Be(userId);
        result.Name.Should().Be("John Doe");

        // Verify repository was not called (cache hit)
        _repositoryMock.Verify(x => x.GetByIdAsync(It.IsAny<Guid>()), Times.Never);
    }

    [Fact]
    public async Task CreateUserAsync_ValidCommand_ReturnsCreatedUser()
    {
        // Arrange
        var command = new CreateUserCommand
        {
            Name = "Jane Doe",
            Email = "jane@example.com"
        };

        var user = new User(command.Name, command.Email);
        var expectedUserDto = new UserDto
        {
            Id = user.Id,
            Name = user.Name,
            Email = user.Email
        };

        _mapperMock
            .Setup(x => x.Map<User>(command))
            .Returns(user);

        _repositoryMock
            .Setup(x => x.AddAsync(user))
            .ReturnsAsync(user);

        _mapperMock
            .Setup(x => x.Map<UserDto>(user))
            .Returns(expectedUserDto);

        // Act
        var result = await _userService.CreateUserAsync(command);

        // Assert
        result.Should().NotBeNull();
        result!.Name.Should().Be(command.Name);
        result.Email.Should().Be(command.Email);

        // Verify interactions
        _repositoryMock.Verify(x => x.AddAsync(user), Times.Once);
        _cacheMock.Verify(x => x.RemoveAsync("users_list"), Times.Once);
        _loggerMock.VerifyLog(
            x => x.Log(
                LogLevel.Information,
                It.IsAny<EventId>(),
                It.Is<It.IsAnyType>((v, t) => v.ToString()!.Contains("Created new user")),
                It.IsAny<Exception>(),
                It.IsAny<Func<It.IsAnyType, Exception?, string>>()),
            Times.Once);
    }

    // Theory with member data
    [Theory]
    [MemberData(nameof(InvalidEmailData))]
    public async Task CreateUserAsync_InvalidEmail_ThrowsValidationException(string email)
    {
        // Arrange
        var command = new CreateUserCommand
        {
            Name = "John Doe",
            Email = email
        };

        // Act & Assert
        await Assert.ThrowsAsync<ValidationException>(
            () => _userService.CreateUserAsync(command));
    }

    public static TheoryData<string> InvalidEmailData => new()
    {
        "invalid-email",
        "",
        "not-an-email",
        "@invalid.com"
    };
}

// Integration testing with WebApplicationFactory
public class UserApiIntegrationTests : IClassFixture<TestWebApplicationFactory>
{
    private readonly HttpClient _client;
    private readonly TestWebApplicationFactory _factory;

    public UserApiIntegrationTests(TestWebApplicationFactory factory)
    {
        _factory = factory;
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetUsers_ReturnsOkResponse()
    {
        // Act
        var response = await _client.GetAsync("/api/users");

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.OK);

        var content = await response.Content.ReadAsStringAsync();
        var users = JsonSerializer.Deserialize<UserDto[]>(content);

        users.Should().NotBeNull();
        users!.Should().NotBeEmpty();
    }

    [Fact]
    public async Task CreateUser_ValidCommand_ReturnsCreatedResponse()
    {
        // Arrange
        var command = new
        {
            Name = "Test User",
            Email = "test@example.com"
        };

        var json = JsonSerializer.Serialize(command);
        var content = new StringContent(json, Encoding.UTF8, "application/json");

        // Act
        var response = await _client.PostAsync("/api/users", content);

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.Created);

        var responseContent = await response.Content.ReadAsStringAsync();
        var user = JsonSerializer.Deserialize<UserDto>(responseContent);

        user.Should().NotBeNull();
        user!.Name.Should().Be(command.Name);
        user.Email.Should().Be(command.Email);
    }
}

// Performance testing with BenchmarkDotNet
[MemoryDiagnoser]
[SimpleJob(RuntimeMoniker.Net90)]
public class UserServiceBenchmarks
{
    private UserService _userService = null!;
    private CreateUserCommand _command = null!;

    [GlobalSetup]
    public void Setup()
    {
        var services = new ServiceCollection();
        services.AddLogging();
        services.AddAutoMapper(typeof(Program));

        // Mock dependencies
        services.AddScoped<IUserRepository>(_ => Mock.Of<IUserRepository>());
        services.AddScoped<ICacheService>(_ => Mock.Of<ICacheService>());

        var serviceProvider = services.BuildServiceProvider();

        _userService = new UserService(
            serviceProvider.GetRequiredService<IUserRepository>(),
            serviceProvider.GetRequiredService<ICacheService>(),
            serviceProvider.GetRequiredService<ILogger<UserService>>(),
            serviceProvider.GetRequiredService<IMapper>());

        _command = new CreateUserCommand
        {
            Name = "Performance Test User",
            Email = "perf@test.com"
        };
    }

    [Benchmark]
    public async Task<UserDto> CreateUserAsync() =>
        await _userService.CreateUserAsync(_command);

    [Benchmark]
    public async Task<UserDto?> GetUserAsync() =>
        await _userService.GetUserAsync(Guid.NewGuid());
}

// Test factory for integration tests
public class TestWebApplicationFactory : WebApplicationFactory<Program>
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.ConfigureServices(services =>
        {
            // Replace production services with test services
            services.AddScoped<IUserRepository, TestUserRepository>();
            services.AddScoped<ICacheService, MemoryCacheService>();
        });
    }
}

// Test implementations
public class TestUserRepository : IUserRepository
{
    private readonly List<User> _users = new();

    public async Task<User?> GetByIdAsync(Guid id) =>
        _users.FirstOrDefault(u => u.Id == id);

    public async Task<User> AddAsync(User user)
    {
        _users.Add(user);
        return user;
    }

    // Implement other methods...
}

public class MemoryCacheService : ICacheService
{
    private readonly MemoryCache _cache = new(new MemoryCacheOptions());

    public async Task<T?> GetAsync<T>(string key)
    {
        _cache.TryGetValue(key, out var value);
        return value is T typedValue ? typedValue : default;
    }

    public async Task SetAsync<T>(string key, T value, TimeSpan expiration)
    {
        _cache.Set(key, value, expiration);
    }

    public async Task RemoveAsync(string key)
    {
        _cache.Remove(key);
    }
}
```

#### **Essential Resources**
- **📖 [.NET 9 Testing Guide](https://learn.microsoft.com/en-us/dotnet/core/testing/)**
  - Unit testing patterns
  - Integration testing
  - Performance testing

- **🎓 [xUnit Documentation](https://xunit.net/)**
  - Modern testing framework
  - Theory and data-driven tests
  - Parallel execution

#### **Practice Projects**
1. **Testing Framework**: Custom testing utilities
2. **API Integration Tests**: Complete API test suite
3. **Performance Benchmark Suite**: Performance testing automation

---

## ⚡ Level 3: Advanced C# Patterns (.NET 9/10 Features) (Weeks 13-20)

### **Week 13-14: C# 14 Advanced Features**

#### **Extension Members and Advanced Patterns (C# 14)**
```csharp
// C# 14 Extension Members Syntax
public static class EnumerableExtensions
{
    // Extension block for IEnumerable<T>
    extension<TSource>(IEnumerable<TSource> source)
    {
        // Extension property
        public bool IsEmpty => !source.Any();

        // Extension method
        public IEnumerable<TSource> WhereIf(
            IEnumerable<TSource> source,
            bool condition,
            Func<TSource, bool> predicate) =>
            condition ? source.Where(predicate) : source;

        // Extension property with computation
        public double AverageOrDefault(Func<TSource, int> selector) =>
            source.Any() ? source.Average(selector) : 0;

        // User-defined operator overload
        public static IEnumerable<TSource> operator +(
            IEnumerable<TSource> left,
            IEnumerable<TSource> right) =>
            left.Concat(right);
    }

    // Extension block for static methods
    extension<TSource>(IEnumerable<TSource>)
    {
        // Static extension method
        public static IEnumerable<TSource> Combine(
            IEnumerable<TSource> first,
            IEnumerable<TSource> second) =>
            first.Concat(second);

        // Static extension property
        public static IEnumerable<TSource> Empty => Enumerable.Empty<TSource>();

        // Static user-defined operator
        public static IEnumerable<TSource> operator *(
            IEnumerable<TSource> source,
            int times) =>
            Enumerable.Repeat(source, times).SelectMany(x => x);
    }
}

// Null-Conditional Assignment (C# 14)
public class ModernAssignmentPatterns
{
    public void DemonstratePatterns()
    {
        Customer? customer = null;
        Order? order = null;

        // Null-conditional assignment with member access
        customer?.Order = GetCurrentOrder();
        customer?.Profile?.Settings?.Theme = "Dark";

        // Null-conditional compound assignment
        customer?.Order?.Total += 100;
        customer?.Profile?.Score ??= 0; // Coalescing assignment

        // Complex nested access
        customer?.Order?.Items?[0]?.Quantity ??= 1;
    }

    private Order GetCurrentOrder() => new();
}

// Lambda with Parameter Modifiers (C# 14)
public class ModernLambdaExpressions
{
    public void DemonstrateLambdas()
    {
        // Lambda with ref parameter
        var doubleValue = (ref int x) => x *= 2;
        int number = 5;
        doubleValue(ref number); // number becomes 10

        // Lambda with in parameter
        var isLargeNumber = (in int x) => x > 1000;

        // Lambda with out parameter
        var tryParse = (string text, out int result) =>
            int.TryParse(text, out result);

        // Lambda with scoped parameter
        var processData = scoped Action<int[]> data =>
        {
            // Process data safely
            Array.Sort(data);
        };

        // Async lambda with modifiers
        var processAsync = async (ref readonly CancellationToken token) =>
        {
            await Task.Delay(1000, token);
        };
    }
}

// Nameof with Unbound Generics (C# 14)
public class MetadataAnalysis
{
    public void AnalyzeTypes()
    {
        // Get unbound generic type names
        string listName = nameof(List<>); // "List`1"
        string dictionaryName = nameof(Dictionary<,>); // "Dictionary`2"
        string tupleName = nameof(ValueTuple<,,>); // "ValueTuple`3"

        // Use in diagnostics and logging
        Console.WriteLine($"Analyzing generic type: {listName}");

        // Reflection with unbound types
        var listType = typeof(List<>);
        var constructedList = listType.MakeGenericType(typeof(int));

        Console.WriteLine($"Constructed type: {constructedList.Name}");
    }
}

// User-defined Increment/Decrement Operators (C# 14)
public class Counter
{
    public int Value { get; private set; }

    public Counter(int initialValue = 0) => Value = initialValue;

    public static Counter operator ++(Counter c) => new(c.Value + 1);
    public static Counter operator --(Counter c) => new(c.Value - 1);

    public override string ToString() => Value.ToString();
}

// User-defined Compound Assignment Operators (C# 14)
public struct Vector2D
{
    public double X { get; set; }
    public double Y { get; set; }

    public Vector2D(double x, double y) => (X, Y) = (x, y);

    public static Vector2D operator +(Vector2D left, Vector2D right) =>
        new(left.X + right.X, left.Y + right.Y);

    public static Vector2D operator -(Vector2D left, Vector2D right) =>
        new(left.X - right.X, left.Y - right.Y);

    // Compound assignment operators
    public static Vector2D operator +=(Vector2D left, Vector2D right) =>
        left + right;

    public static Vector2D operator -=(Vector2D left, Vector2D right) =>
        left - right;

    public static Vector2D operator *(Vector2D vector, double scalar) =>
        new(vector.X * scalar, vector.Y * scalar);

    public static Vector2D operator *=(Vector2D vector, double scalar) =>
        vector * scalar;

    public override string ToString() => $"({X}, {Y})";
}

// Partial Instance Constructors (C# 14)
public partial class DataService
{
    // Declaration of partial constructor
    partial DataService(string connectionString);

    private string _connectionString;

    // Implementation of partial constructor
    partial DataService(string connectionString)
    {
        _connectionString = ValidateAndNormalizeConnectionString(connectionString);
        InitializeConnection();
    }

    private string ValidateAndNormalizeConnectionString(string connectionString)
    {
        if (string.IsNullOrWhiteSpace(connectionString))
            throw new ArgumentException("Connection string cannot be null or empty");

        return connectionString.Trim();
    }

    private void InitializeConnection()
    {
        // Initialize database connection
        Console.WriteLine($"Initializing connection to: {_connectionString}");
    }
}

// Partial Events (C# 14)
public partial class EventPublisher
{
    // Declaration of partial event
    public partial event EventHandler<string>? MessageReceived;

    private void OnMessageReceived(string message)
    {
        MessageReceived?.Invoke(this, message);
    }
}

public partial class EventPublisher
{
    // Implementation of partial event
    public partial event EventHandler<string>? MessageReceived;

    public void PublishMessage(string message)
    {
        // Process message
        OnMessageReceived(message);
    }
}

// Span<T> Implicit Conversions (C# 14)
public class SpanConversionExamples
{
    public void DemonstrateConversions()
    {
        // String to ReadOnlySpan<char>
        ReadOnlySpan<char> textSpan = "Hello, World!";

        // Array to Span<T>
        int[] numbers = [1, 2, 3, 4, 5];
        Span<int> numberSpan = numbers;

        // Stackalloc to Span<T>
        Span<byte> buffer = stackalloc byte[1024];

        // Collection expressions to Span
        Span<int> values = [10, 20, 30, 40, 50];

        // Working with spans
        ProcessText(textSpan);
        ProcessNumbers(numberSpan);
        ProcessBuffer(buffer);
    }

    private void ProcessText(ReadOnlySpan<char> text)
    {
        // Efficient text processing without allocations
        var wordCount = text.Count(c => c == ' ') + 1;
        Console.WriteLine($"Word count: {wordCount}");
    }

    private void ProcessNumbers(Span<int> numbers)
    {
        // In-place number processing
        for (int i = 0; i < numbers.Length; i++)
        {
            numbers[i] *= 2;
        }
    }

    private void ProcessBuffer(Span<byte> buffer)
    {
        // Zero-initialize buffer
        buffer.Clear();
    }
}
```

#### **Essential Resources**
- **📖 [C# 14 Documentation](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-14)**
  - Complete feature list
  - Migration guide
  - Breaking changes

- **🎓 [.NET 10 Overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)**
  - Platform improvements
  - Performance enhancements
  - New runtime features

#### **Practice Projects**
1. **Extension Library**: Create custom extension members
2. **Mathematics Library**: Implement advanced operators
3. **Text Processing Engine**: Use Span<T> optimizations

### **Week 15-16: High-Performance C#**

#### **Memory Management & Performance Optimization**
```csharp
// Advanced Memory Management with Span<T> and Memory<T>
public class HighPerformanceProcessor
{
    private readonly byte[] _buffer;
    private readonly Memory<byte> _memory;
    private readonly object _lock = new();

    public HighPerformanceProcessor(int bufferSize)
    {
        _buffer = GC.AllocateArray<byte>(bufferSize, pinned: true);
        _memory = _buffer.AsMemory();
    }

    // Process data without allocations
    public void ProcessData(ReadOnlySpan<byte> input)
    {
        lock (_lock)
        {
            var targetSpan = _memory.Span;

            // Copy input to buffer
            if (input.Length > targetSpan.Length)
            {
                throw new ArgumentException("Input too large");
            }

            input.CopyTo(targetSpan);

            // Process in-place
            ProcessInPlace(targetSpan[..input.Length]);
        }
    }

    private void ProcessInPlace(Span<byte> data)
    {
        // Zero-allocation processing
        for (int i = 0; i < data.Length; i++)
        {
            data[i] = (byte)(data[i] ^ 0xFF); // Invert bits
        }
    }

    // Use MemoryPool<T> for temporary buffers
    public void ProcessWithPool(ReadOnlySpan<byte> input)
    {
        using var owner = MemoryPool<byte>.Shared.Rent(input.Length);
        var buffer = owner.Memory.Span[..input.Length];

        input.CopyTo(buffer);
        ProcessInPlace(buffer);
    }
}

// ValueTask and async performance
public class AsyncPerformanceService
{
    private readonly Channel<WorkItem> _channel;
    private readonly SemaphoreSlim _semaphore;

    public AsyncPerformanceService()
    {
        var options = new BoundedChannelOptions(1000)
        {
            FullMode = BoundedChannelFullMode.Wait,
            SingleReader = false,
            SingleWriter = false
        };
        _channel = Channel.CreateBounded<WorkItem>(options);
        _semaphore = new SemaphoreSlim(Environment.ProcessorCount);
    }

    public async ValueTask EnqueueAsync(WorkItem item)
    {
        await _channel.Writer.WriteAsync(item);
    }

    public async ValueTask<ProcessResult> ProcessAsync(WorkItem item, CancellationToken cancellationToken = default)
    {
        await _semaphore.WaitAsync(cancellationToken);
        try
        {
            // Fast path - synchronous if possible
            if (item.IsQuick)
            {
                return ProcessSynchronously(item);
            }

            // Async path for slower operations
            return await ProcessAsynchronously(item, cancellationToken);
        }
        finally
        {
            _semaphore.Release();
        }
    }

    private ProcessResult ProcessSynchronously(WorkItem item)
    {
        // Synchronous processing for quick items
        var result = new ProcessResult
        {
            ItemId = item.Id,
            ProcessedAt = DateTime.UtcNow,
            Duration = TimeSpan.FromMilliseconds(Random.Shared.Next(1, 10))
        };

        return result;
    }

    private async Task<ProcessResult> ProcessAsynchronously(WorkItem item, CancellationToken cancellationToken)
    {
        // Async processing for slower items
        await Task.Delay(Random.Shared.Next(50, 200), cancellationToken);

        return new ProcessResult
        {
            ItemId = item.Id,
            ProcessedAt = DateTime.UtcNow,
            Duration = TimeSpan.FromMilliseconds(Random.Shared.Next(50, 200))
        };
    }
}

// Custom enumerator for high-performance iteration
public ref struct HighPerformanceEnumerator<T>
{
    private readonly ReadOnlySpan<T> _span;
    private int _index;

    public HighPerformanceEnumerator(ReadOnlySpan<T> span)
    {
        _span = span;
        _index = -1;
    }

    public readonly T Current => _span[_index];

    public bool MoveNext()
    {
        _index++;
        return _index < _span.Length;
    }
}

// Generic high-performance collection
public struct FastStack<T>
{
    private T[] _items;
    private int _size;

    public FastStack(int capacity)
    {
        _items = new T[capacity];
        _size = 0;
    }

    public readonly bool IsEmpty => _size == 0;
    public readonly int Count => _size;

    public void Push(T item)
    {
        EnsureCapacity(_size + 1);
        _items[_size++] = item;
    }

    public T Pop()
    {
        if (_size == 0)
            throw new InvalidOperationException("Stack is empty");

        var item = _items[--_size];
        _items[_size] = default!;
        return item;
    }

    public readonly T Peek()
    {
        if (_size == 0)
            throw new InvalidOperationException("Stack is empty");

        return _items[_size - 1];
    }

    private void EnsureCapacity(int min)
    {
        if (_items.Length < min)
        {
            var newCapacity = _items.Length * 2;
            if (newCapacity < min) newCapacity = min;
            Array.Resize(ref _items, newCapacity);
        }
    }
}

// Memory-mapped file access
public class MemoryMappedFileProcessor
{
    public unsafe void ProcessLargeFile(string filePath)
    {
        using var mmf = MemoryMappedFile.CreateFromFile(filePath);
        using var accessor = mmf.CreateViewAccessor();

        var length = (int)accessor.Capacity;
        var pointer = (byte*)0;

        try
        {
            accessor.SafeMemoryMappedViewHandle.AcquirePointer(ref pointer);

            var span = new ReadOnlySpan<byte>(pointer, length);
            ProcessSpan(span);
        }
        finally
        {
            if (pointer != null)
            {
                accessor.SafeMemoryMappedViewHandle.ReleasePointer();
            }
        }
    }

    private void ProcessSpan(ReadOnlySpan<byte> data)
    {
        // Process the data without copying
        var chunkSize = 4096; // 4KB chunks

        for (int i = 0; i < data.Length; i += chunkSize)
        {
            var chunk = data.Slice(i, Math.Min(chunkSize, data.Length - i));
            ProcessChunk(chunk);
        }
    }

    private void ProcessChunk(ReadOnlySpan<byte> chunk)
    {
        // Process individual chunk
        var checksum = ComputeChecksum(chunk);
        Console.WriteLine($"Chunk checksum: {checksum:X8}");
    }

    private static uint ComputeChecksum(ReadOnlySpan<byte> data)
    {
        uint checksum = 0;
        foreach (byte b in data)
        {
            checksum = (checksum << 1) | (checksum >> 31);
            checksum += b;
        }
        return checksum;
    }
}

// Supporting types
public record WorkItem(string Id, bool IsQuick);
public record ProcessResult(string ItemId, DateTime ProcessedAt, TimeSpan Duration);
```

#### **Essential Resources**
- **📖 [Performance Best Practices](https://learn.microsoft.com/en-us/dotnet/core/performance/)**
  - Memory optimization
  - Span<T> and Memory<T>
  - ValueTask usage

- **🎓 [.NET 9 Performance Guide](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)**
  - Runtime improvements
  - Native AOT
  - ReadyToRun images

#### **Practice Projects**
1. **File Processing Engine**: High-performance file operations
2. **Real-time Data Stream**: Low-latency processing
3. **Memory Pool Manager**: Custom memory management

### **Week 17-18: Advanced Patterns & Domain-Driven Design**

#### **Modern DDD with C# 13/14**
```csharp
// Domain Entity with modern C# features
public abstract class Entity
{
    public Guid Id { get; protected set; }
    public DateTime CreatedAt { get; protected set; }
    public DateTime UpdatedAt { get; protected set; }

    protected Entity(Guid id)
    {
        Id = id;
        CreatedAt = DateTime.UtcNow;
        UpdatedAt = DateTime.UtcNow;
    }

    protected void UpdateTimestamp() => UpdatedAt = DateTime.UtcNow;
}

// Value Object with record and primary constructor
public readonly record struct Money(decimal Amount, string Currency)
{
    public static Money Zero(string currency) => new(0, currency);

    public Money Add(Money other) =>
        Currency != other.Currency
            ? throw new ArgumentException("Cannot add different currencies")
            : new(Amount + other.Amount, Currency);

    public Money Subtract(Money other) =>
        Currency != other.Currency
            ? throw new ArgumentException("Cannot subtract different currencies")
            : new(Amount - other.Amount, Currency);

    public static Money operator +(Money left, Money right) => left.Add(right);
    public static Money operator -(Money left, Money right) => left.Subtract(right);

    public override string ToString() => $"{Amount:N2} {Currency}";
}

// Aggregate Root with modern patterns
public class Order : Entity
{
    private readonly List<OrderLine> _orderLines = [];

    public string CustomerId { get; private set; }
    public OrderStatus Status { get; private set; }
    public Money TotalAmount { get; private set; }
    public DateTime OrderDate { get; private set; }

    public IReadOnlyCollection<OrderLine> OrderLines => _orderLines.AsReadOnly();

    private Order(Guid id, string customerId) : base(id)
    {
        CustomerId = customerId;
        Status = OrderStatus.Pending;
        TotalAmount = Money.Zero("USD");
        OrderDate = DateTime.UtcNow;
    }

    // Factory method with validation
    public static Result<Order> Create(string customerId)
    {
        if (string.IsNullOrWhiteSpace(customerId))
            return Result<Order>.Failure(new Error(
                "InvalidCustomer",
                "Customer ID is required",
                ErrorType.Validation));

        return Result<Order>.Success(new Order(Guid.NewGuid(), customerId));
    }

    // Domain event handling
    public Result AddOrderLine(Product product, int quantity, Money unitPrice)
    {
        if (quantity <= 0)
            return Result.Failure(new Error(
                "InvalidQuantity",
                "Quantity must be positive",
                ErrorType.Validation));

        if (product is null)
            return Result.Failure(new Error(
                "InvalidProduct",
                "Product is required",
                ErrorType.Validation));

        var orderLine = new OrderLine(
            Guid.NewGuid(),
            product.Id,
            product.Name,
            quantity,
            unitPrice);

        _orderLines.Add(orderLine);
        UpdateTimestamp();

        // Calculate new total
        var lineTotal = unitPrice * quantity;
        TotalAmount = TotalAmount + lineTotal;

        // Add domain event
        AddDomainEvent(new OrderLineAddedEvent(Id, orderLine.Id, lineTotal));

        return Result.Success();
    }

    public Result MarkAsShipped()
    {
        if (Status != OrderStatus.Confirmed)
            return Result.Failure(new Error(
                "InvalidStatus",
                "Order must be confirmed before shipping",
                ErrorType.Business));

        Status = OrderStatus.Shipped;
        UpdateTimestamp();

        AddDomainEvent(new OrderShippedEvent(Id, CustomerId, TotalAmount));

        return Result.Success();
    }

    private readonly List<IDomainEvent> _domainEvents = [];

    public IReadOnlyCollection<IDomainEvent> GetDomainEvents() => _domainEvents.AsReadOnly();

    public void ClearDomainEvents() => _domainEvents.Clear();

    private void AddDomainEvent(IDomainEvent domainEvent) => _domainEvents.Add(domainEvent);
}

// Order Line value object
public record OrderLine(
    Guid Id,
    string ProductId,
    string ProductName,
    int Quantity,
    Money UnitPrice)
{
    public Money LineTotal => UnitPrice * Quantity;
}

// Product entity
public class Product : Entity
{
    public string Name { get; private set; }
    public string Description { get; private set; }
    public Money Price { get; private set; }
    public int Stock { get; private set; }
    public bool IsActive { get; private set; }

    private Product(Guid id, string name, string description, Money price, int stock)
        : base(id)
    {
        Name = name;
        Description = description;
        Price = price;
        Stock = stock;
        IsActive = true;
    }

    public static Result<Product> Create(string name, string description, Money price, int stock)
    {
        var errors = new List<string>();

        if (string.IsNullOrWhiteSpace(name))
            errors.Add("Product name is required");

        if (price.Amount <= 0)
            errors.Add("Price must be positive");

        if (stock < 0)
            errors.Add("Stock cannot be negative");

        if (errors.Count > 0)
            return Result<Product>.Failure(new Error(
                "ValidationFailed",
                string.Join(", ", errors),
                ErrorType.Validation));

        return Result<Product>.Success(new Product(
            Guid.NewGuid(),
            name.Trim(),
            description?.Trim() ?? string.Empty,
            price,
            stock));
    }

    public Result UpdatePrice(Money newPrice)
    {
        if (newPrice.Amount <= 0)
            return Result.Failure(new Error(
                "InvalidPrice",
                "Price must be positive",
                ErrorType.Validation));

        var oldPrice = Price;
        Price = newPrice;
        UpdateTimestamp();

        AddDomainEvent(new PriceChangedEvent(Id, oldPrice, newPrice));

        return Result.Success();
    }

    public Result ReduceStock(int quantity)
    {
        if (quantity <= 0)
            return Result.Failure(new Error(
                "InvalidQuantity",
                "Quantity must be positive",
                ErrorType.Validation));

        if (Stock < quantity)
            return Result.Failure(new Error(
                "InsufficientStock",
                $"Only {Stock} items available",
                ErrorType.Business));

        Stock -= quantity;
        UpdateTimestamp();

        if (Stock == 0)
        {
            IsActive = false;
            AddDomainEvent(new ProductOutOfStockEvent(Id));
        }

        return Result.Success();
    }
}

// Domain events
public interface IDomainEvent
{
    DateTime OccurredAt { get; }
    Guid EventId { get; }
}

public abstract record DomainEvent : IDomainEvent
{
    public DateTime OccurredAt { get; } = DateTime.UtcNow;
    public Guid EventId { get; } = Guid.NewGuid();
}

public record OrderLineAddedEvent(Guid OrderId, Guid OrderLineId, Money LineTotal) : DomainEvent;
public record OrderShippedEvent(Guid OrderId, string CustomerId, Money TotalAmount) : DomainEvent;
public record PriceChangedEvent(Guid ProductId, Money OldPrice, Money NewPrice) : DomainEvent;
public record ProductOutOfStockEvent(Guid ProductId) : DomainEvent;

// Application service with Result<T> pattern
public class OrderService
{
    private readonly IOrderRepository _orderRepository;
    private readonly IProductRepository _productRepository;
    private readonly IDomainEventDispatcher _eventDispatcher;
    private readonly IUnitOfWork _unitOfWork;

    public OrderService(
        IOrderRepository orderRepository,
        IProductRepository productRepository,
        IDomainEventDispatcher eventDispatcher,
        IUnitOfWork unitOfWork)
    {
        _orderRepository = orderRepository;
        _productRepository = productRepository;
        _eventDispatcher = eventDispatcher;
        _unitOfWork = unitOfWork;
    }

    public async Task<Result<Order>> CreateOrderAsync(CreateOrderCommand command)
    {
        // Validate order creation
        var validationResult = ValidateOrder(command);
        if (validationResult.IsFailure)
            return validationResult;

        // Create order
        var orderResult = Order.Create(command.CustomerId);
        if (orderResult.IsFailure)
            return Result<Order>.Failure(orderResult.Error);

        var order = orderResult.Value;

        // Add order lines
        foreach (var item in command.Items)
        {
            var product = await _productRepository.GetByIdAsync(item.ProductId);
            if (product is null)
                return Result<Order>.Failure(new Error(
                    "ProductNotFound",
                    $"Product {item.ProductId} not found",
                    ErrorType.NotFound));

            var addLineResult = order.AddOrderLine(product, item.Quantity, product.Price);
            if (addLineResult.IsFailure)
                return Result<Order>.Failure(addLineResult.Error);

            // Reduce stock
            var stockResult = product.ReduceStock(item.Quantity);
            if (stockResult.IsFailure)
                return Result<Order>.Failure(stockResult.Error);
        }

        // Save order
        await _orderRepository.AddAsync(order);

        // Commit transaction
        await _unitOfWork.SaveChangesAsync();

        // Dispatch domain events
        await _eventDispatcher.DispatchAsync(order.GetDomainEvents());

        order.ClearDomainEvents();

        return Result<Order>.Success(order);
    }

    private Result ValidateOrder(CreateOrderCommand command)
    {
        if (command is null)
            return Result.Failure(new Error(
                "InvalidCommand",
                "Command is null",
                ErrorType.Validation));

        if (string.IsNullOrWhiteSpace(command.CustomerId))
            return Result.Failure(new Error(
                "InvalidCustomer",
                "Customer ID is required",
                ErrorType.Validation));

        if (command.Items?.Any() != true)
            return Result.Failure(new Error(
                "NoItems",
                "Order must contain at least one item",
                ErrorType.Validation));

        return Result.Success();
    }
}

// Command and DTO types
public record CreateOrderCommand(string CustomerId, List<CreateOrderItemCommand> Items);
public record CreateOrderItemCommand(string ProductId, int Quantity);

// Enums
public enum OrderStatus
{
    Pending,
    Confirmed,
    Shipped,
    Delivered,
    Cancelled
}
```

#### **Essential Resources**
- **📖 [DDD in C# Guide](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/)**
  - Domain-driven design patterns
  - CQRS implementation
  - Event sourcing

- **🎓 [Clean Architecture .NET](https://github.com/JasonGT/CleanArchitecture)**
  - Architecture patterns
  - Dependency injection
  - Testing strategies

#### **Practice Projects**
1. **E-commerce Domain**: Complete DDD implementation
2. **Banking System**: Financial domain modeling
3. **Inventory Management**: Complex business rules

### **Week 19-20: Advanced Testing & Quality Assurance**

#### **Advanced Testing Patterns with Modern C#**
```csharp
// Test Data Builders with modern C#
public class OrderBuilder
{
    private Guid _id = Guid.NewGuid();
    private string _customerId = "CUSTOMER-001";
    private OrderStatus _status = OrderStatus.Pending;
    private readonly List<OrderLine> _orderLines = [];

    public OrderBuilder WithId(Guid id)
    {
        _id = id;
        return this;
    }

    public OrderBuilder WithCustomerId(string customerId)
    {
        _customerId = customerId;
        return this;
    }

    public OrderBuilder WithStatus(OrderStatus status)
    {
        _status = status;
        return this;
    }

    public OrderBuilder WithOrderLine(string productId, string productName, int quantity, Money unitPrice)
    {
        _orderLines.Add(new OrderLine(
            Guid.NewGuid(),
            productId,
            productName,
            quantity,
            unitPrice));
        return this;
    }

    public OrderBuilder WithOrderLines(params (string productId, string productName, int quantity, Money unitPrice)[] lines)
    {
        foreach (var (productId, productName, quantity, unitPrice) in lines)
        {
            WithOrderLine(productId, productName, quantity, unitPrice);
        }
        return this;
    }

    public Order Build()
    {
        var order = new Order(_id, _customerId);

        // Use reflection to set private properties (for testing only)
        typeof(Order).GetProperty(nameof(Order.Status))?.SetValue(order, _status);
        typeof(Order).GetProperty(nameof(Order.OrderLines))?.SetValue(order, _orderLines);

        return order;
    }
}

// Property-Based Testing with FsCheck (ported to C#)
public class OrderProperties
{
    [Property]
    public Property AddingOrderLine_IncreasesTotalAmount(NonEmptyString productId, PositiveInt quantity, PositiveDecimal price)
    {
        // Arrange
        var order = Order.Create("CUSTOMER-001").Value;
        var product = new Product(productId.Item, "Test Product", new Money(price.Item, "USD"), 100);
        var initialTotal = order.TotalAmount;
        var expectedIncrease = new Money(price.Item * quantity.Item, "USD");

        // Act
        var result = order.AddOrderLine(product, quantity.Item, new Money(price.Item, "USD"));

        // Assert
        return (result.IsSuccess &&
                order.TotalAmount == initialTotal + expectedIncrease)
            .ToProperty();
    }

    [Property]
    public Property OrderTotal_EqualsSumOfOrderLines(List<PositiveInt> quantities, List<PositiveDecimal> prices)
    {
        // Arrange
        if (quantities.Count != prices.Count || quantities.Count == 0)
            return Prop.Label("Invalid test data");

        var order = Order.Create("CUSTOMER-001").Value;
        var expectedTotal = new Money(0, "USD");

        // Act
        for (int i = 0; i < quantities.Count; i++)
        {
            var product = new Product($"PROD-{i}", "Test Product", new Money(prices[i].Item, "USD"), 100);
            var lineTotal = new Money(prices[i].Item * quantities[i].Item, "USD");

            order.AddOrderLine(product, quantities[i].Item, new Money(prices[i].Item, "USD"));
            expectedTotal = expectedTotal + lineTotal;
        }

        // Assert
        return (order.TotalAmount == expectedTotal).ToProperty();
    }
}

// Test Helpers for Modern C#
public static class TestExtensions
{
    public static ResultAssertions<T> Should<T>(this Result<T> result) =>
        new(result);

    public static async Task<ResultAssertions<T>> Should<T>(this Task<Result<T>> resultTask) =>
        (await resultTask).Should();
}

public class ResultAssertions<T>
{
    private readonly Result<T> _result;

    public ResultAssertions(Result<T> result)
    {
        _result = result;
    }

    public AndConstraint<ResultAssertions<T>> BeSuccess(string because = "")
    {
        _result.IsSuccess.Should().BeTrue(because);
        return new AndConstraint<ResultAssertions<T>>(this);
    }

    public AndConstraint<ResultAssertions<T>> BeFailure(string because = "")
    {
        _result.IsSuccess.Should().BeFalse(because);
        return new AndConstraint<ResultAssertions<T>>(this);
    }

    public AndConstraint<ResultAssertions<T>> HaveError(string errorCode, string because = "")
    {
        BeFailure(because);
        _result.Error!.Code.Should().Be(errorCode, because);
        return new AndConstraint<ResultAssertions<T>>(this);
    }

    public AndConstraint<ResultAssertions<T>> HaveValue(T expectedValue, string because = "")
    {
        BeSuccess(because);
        _result.Value.Should().Be(expectedValue, because);
        return new AndConstraint<ResultAssertions<T>>(this);
    }

    public AndConstraint<ResultAssertions<T>> HaveValueMatching<TValue>(
        Expression<Func<T, TValue>> predicate,
        TValue expectedValue,
        string because = "")
    {
        BeSuccess(because);
        _result.Value.Should().Match(predicate, because);
        return new AndConstraint<ResultAssertions<T>>(this);
    }
}

// Integration Testing with Docker
public class OrderApiIntegrationTests : IClassFixture<DockerContainer>, IAsyncLifetime
{
    private readonly HttpClient _client;
    private readonly DockerContainer _container;
    private readonly TestDatabase _database;

    public OrderApiIntegrationTests(DockerContainer container)
    {
        _container = container;
        _client = new HttpClient
        {
            BaseAddress = new Uri(container.GetConnectionString())
        };
        _database = new TestDatabase(container.GetConnectionString());
    }

    public async Task InitializeAsync()
    {
        await _database.InitializeAsync();
        await SeedTestDataAsync();
    }

    public async Task DisposeAsync()
    {
        await _database.CleanupAsync();
        _client.Dispose();
    }

    [Fact]
    public async Task PostOrder_ValidOrder_ReturnsCreatedResponse()
    {
        // Arrange
        var command = new
        {
            CustomerId = "CUSTOMER-001",
            Items = new[]
            {
                new { ProductId = "PROD-001", Quantity = 2 },
                new { ProductId = "PROD-002", Quantity = 1 }
            }
        };

        var json = JsonSerializer.Serialize(command);
        var content = new StringContent(json, Encoding.UTF8, "application/json");

        // Act
        var response = await _client.PostAsync("/api/orders", content);

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.Created);

        var responseContent = await response.Content.ReadAsStringAsync();
        var orderResponse = JsonSerializer.Deserialize<OrderResponse>(responseContent);

        orderResponse.Should().NotBeNull();
        orderResponse!.CustomerId.Should().Be(command.CustomerId);
        orderResponse.OrderLines.Should().HaveCount(2);

        // Verify database state
        var orderInDb = await _database.GetOrderAsync(orderResponse.Id);
        orderInDb.Should().NotBeNull();
        orderInDb!.CustomerId.Should().Be(command.CustomerId);
    }

    [Fact]
    public async Task GetOrder_ExistingOrder_ReturnsOrderDetails()
    {
        // Arrange
        var existingOrder = await CreateTestOrderAsync();

        // Act
        var response = await _client.GetAsync($"/api/orders/{existingOrder.Id}");

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.OK);

        var content = await response.Content.ReadAsStringAsync();
        var order = JsonSerializer.Deserialize<OrderResponse>(content);

        order.Should().NotBeNull();
        order!.Id.Should().Be(existingOrder.Id);
        order.CustomerId.Should().Be(existingOrder.CustomerId);
    }

    private async Task<Order> CreateTestOrderAsync()
    {
        var order = Order.Create("CUSTOMER-001").Value;

        var product1 = Product.Create("Test Product 1", "Description 1", new Money(100, "USD"), 50).Value;
        var product2 = Product.Create("Test Product 2", "Description 2", new Money(200, "USD"), 30).Value;

        order.AddOrderLine(product1, 2, new Money(100, "USD"));
        order.AddOrderLine(product2, 1, new Money(200, "USD"));

        await _database.AddOrderAsync(order);
        return order;
    }

    private async Task SeedTestDataAsync()
    {
        var products = new[]
        {
            Product.Create("PROD-001", "Product 1", "Description 1", 100, 100).Value,
            Product.Create("PROD-002", "Product 2", "Description 2", 200, 50).Value,
            Product.Create("PROD-003", "Product 3", "Description 3", 50, 200).Value
        };

        foreach (var product in products)
        {
            await _database.AddProductAsync(product);
        }
    }
}

// Performance Testing
[MemoryDiagnoser]
[SimpleJob(RuntimeMoniker.Net90)]
public class OrderProcessingBenchmarks
{
    private OrderService _orderService = null!;
    private CreateOrderCommand _testCommand = null!;

    [GlobalSetup]
    public void Setup()
    {
        // Setup in-memory database and services
        var services = new ServiceCollection();

        services.AddDbContext<TestDbContext>(options =>
            options.UseInMemoryDatabase("TestDb"));

        services.AddScoped<IOrderRepository, OrderRepository>();
        services.AddScoped<IProductRepository, ProductRepository>();
        services.AddScoped<IDomainEventDispatcher, InMemoryEventDispatcher>();
        services.AddScoped<IUnitOfWork, UnitOfWork>();
        services.AddScoped<OrderService>();

        var serviceProvider = services.BuildServiceProvider();

        _orderService = serviceProvider.GetRequiredService<OrderService>();

        // Setup test data
        _testCommand = new CreateOrderCommand(
            "CUSTOMER-001",
            [
                new("PROD-001", 5),
                new("PROD-002", 3),
                new("PROD-003", 10)
            ]);
    }

    [Benchmark]
    public async Task<Result<Order>> CreateOrder() =>
        await _orderService.CreateOrderAsync(_testCommand);

    [Benchmark]
    public async Task<List<Order>> CreateMultipleOrders()
    {
        var tasks = Enumerable.Range(0, 100)
            .Select(_ => _orderService.CreateOrderAsync(_testCommand))
            .ToArray();

        var results = await Task.WhenAll(tasks);

        return results.Where(r => r.IsSuccess)
                     .Select(r => r.Value)
                     .ToList();
    }
}

// Supporting types
public record OrderResponse(Guid Id, string CustomerId, List<OrderLineResponse> OrderLines);
public record OrderLineResponse(Guid ProductId, string ProductName, int Quantity, Money UnitPrice);
```

#### **Essential Resources**
- **📖 [Advanced Testing Patterns](https://learn.microsoft.com/en-us/dotnet/core/testing/)**
  - Property-based testing
  - Integration testing
  - Performance testing

- **🎓 [Testcontainers for .NET](https://dotnet.testcontainers.org/)**
  - Docker integration testing
  - Database testing
  - Microservice testing

#### **Practice Projects**
1. **Testing Framework**: Custom testing utilities
2. **Test Automation Pipeline**: CI/CD testing integration
3. **Performance Testing Suite**: Automated performance validation

---

## 🏆 Level 4: Expert C# & Performance Optimization (Weeks 21-28)

### **Week 21-22: Advanced Performance & Memory Management**

#### **Expert-Level Performance Optimization**
```csharp
// Advanced Memory Pool Management
public sealed class HighPerformanceBufferPool : IDisposable
{
    private readonly ConcurrentQueue<byte[]> _buffers = new();
    private readonly int _bufferSize;
    private int _totalBuffers;
    private int _availableBuffers;
    private bool _disposed;

    public HighPerformanceBufferPool(int bufferSize, int initialBuffers = 10)
    {
        _bufferSize = bufferSize;
        _totalBuffers = initialBuffers;
        _availableBuffers = initialBuffers;

        // Pre-allocate initial buffers
        for (int i = 0; i < initialBuffers; i++)
        {
            _buffers.Enqueue(GC.AllocateArray<byte>(bufferSize, pinned: true));
        }
    }

    public BufferHandle Rent()
    {
        ObjectDisposedException.ThrowIf(_disposed, this);

        if (_buffers.TryDequeue(out var buffer))
        {
            Interlocked.Decrement(ref _availableBuffers);
            return new BufferHandle(buffer, this);
        }

        // Create new buffer if none available (up to limit)
        if (_totalBuffers < Environment.ProcessorCount * 4)
        {
            Interlocked.Increment(ref _totalBuffers);
            return new BufferHandle(GC.AllocateArray<byte>(_bufferSize, pinned: true), this);
        }

        // Fallback: allocate temporary buffer
        return new BufferHandle(GC.AllocateArray<byte>(_bufferSize), null);
    }

    internal void Return(byte[] buffer)
    {
        if (_disposed || buffer.Length != _bufferSize)
            return;

        if (Interlocked.Increment(ref _availableBuffers) <= _totalBuffers)
        {
            // Clear buffer for security
            Array.Clear(buffer, 0, buffer.Length);
            _buffers.Enqueue(buffer);
        }
        else
        {
            Interlocked.Decrement(ref _availableBuffers);
            Interlocked.Decrement(ref _totalBuffers);
        }
    }

    public void Dispose()
    {
        if (_disposed)
            return;

        _disposed = true;

        while (_buffers.TryDequeue(out var buffer))
        {
            // No explicit disposal needed for byte arrays
        }
    }

    public readonly struct BufferHandle : IDisposable
    {
        private readonly byte[] _buffer;
        private readonly HighPerformanceBufferPool? _pool;

        public BufferHandle(byte[] buffer, HighPerformanceBufferPool? pool)
        {
            _buffer = buffer;
            _pool = pool;
        }

        public Span<byte> Span => _buffer.AsSpan();
        public Memory<byte> Memory => _buffer.AsMemory();
        public int Length => _buffer.Length;

        public void Dispose()
        {
            _pool?.Return(_buffer);
        }
    }
}

// Lock-Free Data Structures
public class LockFreeQueue<T> where T : class
{
    private readonly ConcurrentQueue<T> _queue = new();
    private volatile bool _disposed;

    public void Enqueue(T item)
    {
        ObjectDisposedException.ThrowIf(_disposed, this);
        _queue.Enqueue(item);
    }

    public bool TryDequeue([MaybeNullWhen(false)] out T item)
    {
        ObjectDisposedException.ThrowIf(_disposed, this);
        return _queue.TryDequeue(out item);
    }

    public void Dispose()
    {
        _disposed = true;
        while (_queue.TryDequeue(out var item))
        {
            if (item is IDisposable disposable)
            {
                disposable.Dispose();
            }
        }
    }
}

// High-Performance JSON Parsing with Span<T>
public static class HighPerformanceJsonParser
{
    public static bool TryParseBool(ReadOnlySpan<byte> json, string propertyName, out bool value)
    {
        value = default;

        var propertySpan = "\""u8 + propertyName.AsSpan() + "\""u8;
        var propertyIndex = json.IndexOf(propertySpan);

        if (propertyIndex == -1)
            return false;

        var colonIndex = json.Slice(propertyIndex + propertySpan.Length)
                            .IndexOf(":"u8);

        if (colonIndex == -1)
            return false;

        var valueStart = propertyIndex + propertySpan.Length + colonIndex + 1;
        var valueSpan = json.Slice(valueStart);

        // Skip whitespace
        valueSpan = valueSpan.TrimStart();

        if (valueSpan.StartsWith("true"u8))
        {
            value = true;
            return true;
        }

        if (valueSpan.StartsWith("false"u8))
        {
            value = false;
            return true;
        }

        return false;
    }

    public static bool TryParseInt32(ReadOnlySpan<byte> json, string propertyName, out int value)
    {
        value = default;

        var propertySpan = "\""u8 + propertyName.AsSpan() + "\""u8;
        var propertyIndex = json.IndexOf(propertySpan);

        if (propertyIndex == -1)
            return false;

        var colonIndex = json.Slice(propertyIndex + propertySpan.Length)
                            .IndexOf(":"u8);

        if (colonIndex == -1)
            return false;

        var valueStart = propertyIndex + propertySpan.Length + colonIndex + 1;
        var valueSpan = json.Slice(valueStart);

        // Skip whitespace
        valueSpan = valueSpan.TrimStart();

        // Find end of number
        var numberEnd = 0;
        for (int i = 0; i < valueSpan.Length; i++)
        {
            var c = valueSpan[i];
            if (!char.IsDigit((char)c) && c != (byte)'-')
                break;
            numberEnd++;
        }

        var numberSpan = valueSpan.Slice(0, numberEnd);
        return int.TryParse(numberSpan, out value);
    }
}

// SIMD-Optimized Processing
public class SimdProcessor
{
    public static void ProcessVectorized(float[] data, float multiplier)
    {
        if (Vector.IsHardwareAccelerated && data.Length >= Vector<float>.Count)
        {
            var vectorMultiplier = new Vector<float>(multiplier);
            var vectorSize = Vector<float>.Count;

            int i = 0;
            for (; i <= data.Length - vectorSize; i += vectorSize)
            {
                var vector = new Vector<float>(data, i);
                vector *= vectorMultiplier;
                vector.CopyTo(data, i);
            }

            // Process remaining elements
            for (; i < data.Length; i++)
            {
                data[i] *= multiplier;
            }
        }
        else
        {
            // Fallback to scalar processing
            for (int i = 0; i < data.Length; i++)
            {
                data[i] *= multiplier;
            }
        }
    }

    public static unsafe float SumVectorized(ReadOnlySpan<float> data)
    {
        if (data.Length == 0)
            return 0f;

        if (Vector.IsHardwareAccelerated && data.Length >= Vector<float>.Count)
        {
            var vectorSize = Vector<float>.Count;
            var vectorSum = Vector<float>.Zero;

            int i = 0;
            fixed (float* ptr = data)
            {
                for (; i <= data.Length - vectorSize; i += vectorSize)
                {
                    var vector = Unsafe.Read<Vector<float>>(ptr + i);
                    vectorSum += vector;
                }
            }

            // Extract scalar sum from vector
            var sum = 0f;
            for (int j = 0; j < vectorSize; j++)
            {
                sum += vectorSum[j];
            }

            // Add remaining elements
            for (; i < data.Length; i++)
            {
                sum += data[i];
            }

            return sum;
        }
        else
        {
            // Fallback
            float sum = 0f;
            foreach (var value in data)
            {
                sum += value;
            }
            return sum;
        }
    }
}

// Native Memory Management
public unsafe class NativeMemoryManager<T> : MemoryManager<T>
    where T : unmanaged
{
    private T* _pointer;
    private int _length;
    private bool _disposed;

    public NativeMemoryManager(int length)
    {
        if (length <= 0)
            throw new ArgumentOutOfRangeException(nameof(length));

        _length = length;
        _pointer = (T*)NativeMemory.Alloc((nuint)(length * sizeof(T)));
    }

    public override Span<T> GetSpan() => new(_pointer, _length);

    public override MemoryHandle Pin(int elementIndex = 0)
    {
        if ((uint)elementIndex >= (uint)_length)
            throw new ArgumentOutOfRangeException(nameof(elementIndex));

        return new MemoryHandle(_pointer + elementIndex);
    }

    public override void Unpin() { }

    protected override void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            NativeMemory.Free(_pointer);
            _pointer = null;
            _disposed = true;
        }
        base.Dispose(disposing);
    }

    public static implicit operator Memory<T>(NativeMemoryManager<T> manager) =>
        manager.Memory;
}

// Advanced Caching with MemoryCache and Eviction Policies
public class AdvancedCache<TKey, TValue> where TKey : notnull
{
    private readonly MemoryCache _cache;
    private readonly EvictionPolicy _evictionPolicy;
    private readonly Func<TKey, Task<TValue>> _valueFactory;
    private readonly SemaphoreSlim _semaphore;
    private readonly ConcurrentDictionary<TKey, Task<TValue>> _pendingOperations = new();

    public AdvancedCache(
        Func<TKey, Task<TValue>> valueFactory,
        EvictionPolicy evictionPolicy,
        int sizeLimit = 1000,
        TimeSpan? expiration = null)
    {
        _valueFactory = valueFactory;
        _evictionPolicy = evictionPolicy;
        _semaphore = new SemaphoreSlim(1, 1);

        var options = new MemoryCacheOptions
        {
            SizeLimit = sizeLimit,
            CompactionPercentage = 0.05
        };

        if (expiration.HasValue)
        {
            options.ExpirationScanFrequency = expiration.Value;
        }

        _cache = new MemoryCache(options);
    }

    public async Task<TValue> GetAsync(TKey key)
    {
        // Try get from cache first
        if (_cache.TryGetValue(key, out TValue? cachedValue))
        {
            _evictionPolicy.RecordAccess(key);
            return cachedValue;
        }

        // Ensure only one operation loads the value for a given key
        var operation = _pendingOperations.GetOrAdd(key, async k =>
        {
            await _semaphore.WaitAsync();
            try
            {
                // Double-check after acquiring semaphore
                if (_cache.TryGetValue(k, out TValue? doubleCheckValue))
                {
                    return doubleCheckValue;
                }

                var value = await _valueFactory(k);

                var options = new MemoryCacheEntryOptions
                {
                    Size = 1,
                    PostEvictionCallbacks = { new PostEvictionCallbackRegistration
                    {
                        EvictionCallback = (k, v, r, s) => _evictionPolicy.RecordEviction((TKey)k!),
                        State = this
                    }}
                };

                _cache.Set(k, value, options);
                _evictionPolicy.RecordAccess(k);

                return value;
            }
            finally
            {
                _semaphore.Release();
            }
        });

        try
        {
            return await operation;
        }
        finally
        {
            _pendingOperations.TryRemove(key, out _);
        }
    }

    public void Remove(TKey key)
    {
        _cache.Remove(key);
        _evictionPolicy.RecordEviction(key);
    }

    public void Clear()
    {
        _cache.Compact(1.0);
        _evictionPolicy.Clear();
    }

    public void Dispose()
    {
        _cache.Dispose();
        _semaphore.Dispose();
    }
}

public enum EvictionPolicyType
{
    LRU,
    LFU,
    FIFO
}

public class EvictionPolicy
{
    private readonly EvictionPolicyType _policyType;
    private readonly ConcurrentDictionary<TKey, CacheItemInfo> _accessInfo = new();

    public EvictionPolicy(EvictionPolicyType policyType)
    {
        _policyType = policyType;
    }

    public void RecordAccess(TKey key)
    {
        _accessInfo.AddOrUpdate(key,
            new CacheItemInfo(DateTime.UtcNow, 1),
            (k, existing) => existing with
            {
                LastAccess = DateTime.UtcNow,
                AccessCount = existing.AccessCount + 1
            });
    }

    public void RecordEviction(TKey key)
    {
        _accessInfo.TryRemove(key, out _);
    }

    public void Clear()
    {
        _accessInfo.Clear();
    }

    public IEnumerable<TKey> GetEvictionCandidates(int count)
    {
        return _policyType switch
        {
            EvictionPolicyType.LRU => _accessInfo
                .OrderBy(kvp => kvp.Value.LastAccess)
                .Take(count)
                .Select(kvp => kvp.Key),

            EvictionPolicyType.LFU => _accessInfo
                .OrderBy(kvp => kvp.Value.AccessCount)
                .ThenBy(kvp => kvp.Value.LastAccess)
                .Take(count)
                .Select(kvp => kvp.Key),

            EvictionPolicyType.FIFO => _accessInfo
                .OrderBy(kvp => kvp.Value.CreatedAt)
                .Take(count)
                .Select(kvp => kvp.Key),

            _ => Enumerable.Empty<TKey>()
        };
    }

    private record CacheItemInfo(DateTime CreatedAt, DateTime LastAccess, long AccessCount);
}
```

#### **Essential Resources**
- **📖 [.NET 9 Performance Guide](https://learn.microsoft.com/en-us/dotnet/core/performance/)**
  - Memory management
  - Native AOT
  - SIMD programming

- **🎓 [High-Performance C#](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/)**
  - GC tuning
  - Memory allocation patterns
  - Performance profiling

#### **Practice Projects**
1. **High-Frequency Trading Engine**: Ultra-low latency processing
2. **Real-time Analytics System**: Stream processing optimization
3. **Memory Pool Framework**: Custom memory management

### **Week 23-24: Cloud-Native & Microservices**

#### **Modern Cloud Patterns with .NET 9**
```csharp
// Modern Microservice with Health Checks and Observability
public class OrderService : BackgroundService
{
    private readonly ILogger<OrderService> _logger;
    private readonly IOrderProcessor _processor;
    private readonly IMetrics _metrics;
    private readonly IHealthCheckPublisher _healthPublisher;
    private readonly ActivitySource _activitySource;

    public OrderService(
        ILogger<OrderService> logger,
        IOrderProcessor processor,
        IMetrics metrics,
        IHealthCheckPublisher healthPublisher)
    {
        _logger = logger;
        _processor = processor;
        _metrics = metrics;
        _healthPublisher = healthPublisher;
        _activitySource = new ActivitySource("OrderService");
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        using var listener = new ActivityListener
        {
            ShouldListenTo = (activitySource) => activitySource.Name == "OrderService",
            Sample = (ref ActivityCreationOptions<ActivityContext> options) => ActivitySamplingResult.AllData,
        };

        ActivitySource.AddActivityListener(listener);

        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                await ProcessNextBatchAsync(stoppingToken);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error processing order batch");
                await _healthPublisher.PublishAsync(
                    new HealthReportEntry(
                        "OrderProcessing",
                        "Error in batch processing",
                        HealthStatus.Unhealthy,
                        TimeSpan.Zero,
                        ex,
                        data: null),
                    CancellationToken.None);
            }
        }
    }

    private async Task ProcessNextBatchAsync(CancellationToken cancellationToken)
    {
        using var activity = _activitySource.StartActivity("ProcessOrderBatch");
        activity?.SetTag("batch.size", 100);

        var stopwatch = Stopwatch.StartNew();
        var processedCount = 0;

        try
        {
            var orders = await _processor.GetNextBatchAsync(100, cancellationToken);

            foreach (var order in orders)
            {
                await ProcessOrderAsync(order, cancellationToken);
                processedCount++;
            }

            _metrics.Counter("orders.processed").Add(processedCount);
            activity?.SetTag("orders.processed", processedCount);
            activity?.SetStatus(ActivityStatusCode.Ok);
        }
        finally
        {
            stopwatch.Stop();
            _metrics.Histogram("order.batch.duration").Record(stopwatch.ElapsedMilliseconds);
            activity?.SetTag("batch.duration_ms", stopwatch.ElapsedMilliseconds);
        }
    }

    private async Task ProcessOrderAsync(Order order, CancellationToken cancellationToken)
    {
        using var activity = _activitySource.StartActivity("ProcessOrder");
        activity?.SetTag("order.id", order.Id);
        activity?.SetTag("customer.id", order.CustomerId);

        try
        {
            await _processor.ProcessOrderAsync(order, cancellationToken);
            _metrics.Counter("orders.processed.success").Add(1, new("customer", order.CustomerId));
            activity?.SetStatus(ActivityStatusCode.Ok);
        }
        catch (Exception ex)
        {
            _metrics.Counter("orders.processed.error").Add(1, new("error_type", ex.GetType().Name));
            activity?.SetStatus(ActivityStatusCode.Error, ex.Message);
            activity?.SetTag("error.message", ex.Message);
            throw;
        }
    }
}

// Circuit Breaker Pattern
public class CircuitBreaker<T>
{
    private readonly TimeSpan _durationOfBreak;
    private readonly int _exceptionsAllowedBeforeBreaking;
    private readonly Func<T> _action;
    private readonly object _stateLock = new();

    private CircuitBreakerState _state = CircuitBreakerState.Closed;
    private int _exceptionsThrown;
    private DateTime _lastStateChange = DateTime.UtcNow;

    public CircuitBreaker(Func<T> action, int exceptionsAllowedBeforeBreaking, TimeSpan durationOfBreak)
    {
        _action = action;
        _exceptionsAllowedBeforeBreaking = exceptionsAllowedBeforeBreaking;
        _durationOfBreak = durationOfBreak;
    }

    public async Task<T> ExecuteAsync()
    {
        lock (_stateLock)
        {
            if (_state == CircuitBreakerState.Open)
            {
                if (DateTime.UtcNow - _lastStateChange > _durationOfBreak)
                {
                    _state = CircuitBreakerState.HalfOpen;
                }
                else
                {
                    throw new CircuitBreakerOpenException("Circuit breaker is open");
                }
            }
        }

        try
        {
            var result = await Task.Run(_action);

            lock (_stateLock)
            {
                if (_state == CircuitBreakerState.HalfOpen)
                {
                    _state = CircuitBreakerState.Closed;
                    _exceptionsThrown = 0;
                    _lastStateChange = DateTime.UtcNow;
                }
            }

            return result;
        }
        catch (Exception ex)
        {
            lock (_stateLock)
            {
                _exceptionsThrown++;

                if (_exceptionsThrown >= _exceptionsAllowedBeforeBreaking)
                {
                    _state = CircuitBreakerState.Open;
                    _lastStateChange = DateTime.UtcNow;
                }
            }

            throw;
        }
    }
}

public enum CircuitBreakerState
{
    Closed,
    Open,
    HalfOpen
}

public class CircuitBreakerOpenException : Exception
{
    public CircuitBreakerOpenException(string message) : base(message) { }
}

// Resilient HTTP Client with Polly
public class ResilientHttpClient
{
    private readonly HttpClient _httpClient;
    private readonly ILogger<ResilientHttpClient> _logger;
    private readonly AsyncPolicy<HttpResponseMessage> _policy;

    public ResilientHttpClient(
        HttpClient httpClient,
        ILogger<ResilientHttpClient> logger,
        IConfiguration configuration)
    {
        _httpClient = httpClient;
        _logger = logger;

        var retryOptions = configuration.GetSection("HttpClient:Retry").Get<RetryOptions>() ?? new RetryOptions();
        var circuitBreakerOptions = configuration.GetSection("HttpClient:CircuitBreaker").Get<CircuitBreakerOptions>() ?? new CircuitBreakerOptions();

        _policy = Policy<HttpResponseMessage>
            .Handle<HttpRequestException>()
            .OrResult<HttpResponseMessage>(response =>
                response.StatusCode >= HttpStatusCode.InternalServerError ||
                response.StatusCode == HttpStatusCode.RequestTimeout)
            .WaitAndRetryAsync(
                retryOptions.MaxRetries,
                retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)),
                onRetry: (outcome, timespan, retryAttempt, context) =>
                {
                    _logger.LogWarning(
                        "Request failed with {StatusCode}. Waiting {TimeSpan} before next retry. Retry attempt {RetryAttempt}",
                        outcome.Result?.StatusCode ?? (HttpStatusCode)0,
                        timespan,
                        retryAttempt);
                })
            .WrapAsync(Policy<HttpResponseMessage>
                .Handle<HttpRequestException>()
                .OrResult<HttpResponseMessage>(response =>
                    response.StatusCode >= HttpStatusCode.InternalServerError)
                .CircuitBreakerAsync(
                    exceptionsAllowedBeforeBreaking: circuitBreakerOptions.ExceptionsAllowedBeforeBreaking,
                    durationOfBreak: circuitBreakerOptions.DurationOfBreak,
                    onBreak: (exception, breakDelay) =>
                    {
                        _logger.LogError(
                            "Circuit breaker opened due to exception: {Exception}. Break duration: {BreakDuration}",
                            exception?.Message,
                            breakDelay);
                    },
                    onReset: () =>
                    {
                        _logger.LogInformation("Circuit breaker reset");
                    }));
    }

    public async Task<T> GetAsync<T>(string requestUri, CancellationToken cancellationToken = default)
    {
        var response = await _policy.ExecuteAsync(
            () => _httpClient.GetAsync(requestUri, cancellationToken));

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<T>(cancellationToken: cancellationToken);
    }

    public async Task<TResponse> PostAsync<TRequest, TResponse>(
        string requestUri,
        TRequest content,
        CancellationToken cancellationToken = default)
    {
        var response = await _policy.ExecuteAsync(
            () => _httpClient.PostAsJsonAsync(requestUri, content, cancellationToken));

        response.EnsureSuccessStatusCode();

        return await response.Content.ReadFromJsonAsync<TResponse>(cancellationToken: cancellationToken);
    }
}

public record RetryOptions(int MaxRetries = 3);
public record CircuitBreakerOptions(int ExceptionsAllowedBeforeBreaking = 5, TimeSpan DurationOfBreak = TimeSpan.FromMinutes(1));

// Distributed Caching with Redis
public class DistributedCacheService : ICacheService
{
    private readonly IDistributedCache _distributedCache;
    private readonly ILogger<DistributedCacheService> _logger;
    private readonly JsonSerializerOptions _jsonOptions;

    public DistributedCacheService(
        IDistributedCache distributedCache,
        ILogger<DistributedCacheService> logger)
    {
        _distributedCache = distributedCache;
        _logger = logger;
        _jsonOptions = new JsonSerializerOptions
        {
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
            WriteIndented = false
        };
    }

    public async Task<T?> GetAsync<T>(string key, CancellationToken cancellationToken = default)
    {
        var cachedBytes = await _distributedCache.GetAsync(key, cancellationToken);
        if (cachedBytes == null)
            return default;

        try
        {
            return JsonSerializer.Deserialize<T>(cachedBytes, _jsonOptions);
        }
        catch (JsonException ex)
        {
            _logger.LogError(ex, "Failed to deserialize cached value for key: {Key}", key);
            await _distributedCache.RemoveAsync(key, cancellationToken);
            return default;
        }
    }

    public async Task SetAsync<T>(
        string key,
        T value,
        TimeSpan? absoluteExpiration = null,
        TimeSpan? slidingExpiration = null,
        CancellationToken cancellationToken = default)
    {
        var options = new DistributedCacheEntryOptions();

        if (absoluteExpiration.HasValue)
            options.AbsoluteExpirationRelativeToNow = absoluteExpiration.Value;

        if (slidingExpiration.HasValue)
            options.SlidingExpiration = slidingExpiration.Value;

        var serializedValue = JsonSerializer.SerializeToUtf8Bytes(value, _jsonOptions);

        await _distributedCache.SetAsync(key, serializedValue, options, cancellationToken);
    }

    public async Task RemoveAsync(string key, CancellationToken cancellationToken = default)
    {
        await _distributedCache.RemoveAsync(key, cancellationToken);
    }

    public async Task<bool> ExistsAsync(string key, CancellationToken cancellationToken = default)
    {
        var cachedBytes = await _distributedCache.GetAsync(key, cancellationToken);
        return cachedBytes != null;
    }
}
```

#### **Essential Resources**
- **📖 [.NET 9 Cloud Development](https://learn.microsoft.com/en-us/dotnet/core/cloud-native/)**
  - Microservices patterns
  - Health checks
  - Observability

- **🎓 [Azure .NET Development](https://learn.microsoft.com/en-us/azure/dotnet/)**
  - Azure integration
  - Scalable architectures
  - DevOps patterns

#### **Practice Projects**
1. **Microservice Template**: Complete cloud-native service
2. **API Gateway**: Advanced routing and resilience
3. **Event-Driven Architecture**: Message broker integration

### **Week 25-26: Security & Authentication**

#### **Modern Security Patterns with .NET 9**
```csharp
// Advanced Authentication with JWT and Refresh Tokens
public class AuthenticationService
{
    private readonly UserManager<ApplicationUser> _userManager;
    private readonly SignInManager<ApplicationUser> _signInManager;
    private readonly IJwtTokenService _jwtTokenService;
    private readonly IRefreshTokenService _refreshTokenService;
    private readonly ILogger<AuthenticationService> _logger;

    public AuthenticationService(
        UserManager<ApplicationUser> userManager,
        SignInManager<ApplicationUser> signInManager,
        IJwtTokenService jwtTokenService,
        IRefreshTokenService refreshTokenService,
        ILogger<AuthenticationService> logger)
    {
        _userManager = userManager;
        _signInManager = signInManager;
        _jwtTokenService = jwtTokenService;
        _refreshTokenService = refreshTokenService;
        _logger = logger;
    }

    public async Task<Result<AuthenticationResponse>> AuthenticateAsync(LoginRequest request, string ipAddress)
    {
        var user = await _userManager.FindByEmailAsync(request.Email);
        if (user == null)
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "InvalidCredentials",
                "Invalid email or password",
                ErrorType.Security));
        }

        if (!user.EmailConfirmed)
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "EmailNotConfirmed",
                "Email address not confirmed",
                ErrorType.Security));
        }

        if (await _userManager.IsLockedOutAsync(user))
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "AccountLocked",
                "Account is locked out",
                ErrorType.Security));
        }

        var result = await _signInManager.PasswordSignInAsync(
            user, request.Password, request.RememberMe, lockoutOnFailure: true);

        if (!result.Succeeded)
        {
            await _userManager.AccessFailedAsync(user);
            return Result<AuthenticationResponse>.Failure(new Error(
                "InvalidCredentials",
                "Invalid email or password",
                ErrorType.Security));
        }

        // Reset lockout counter on successful login
        await _userManager.ResetAccessFailedCountAsync(user);

        // Generate tokens
        var jwtToken = await _jwtTokenService.GenerateTokenAsync(user);
        var refreshToken = await _refreshTokenService.GenerateTokenAsync(user, ipAddress);

        // Update user info
        user.LastLoginAt = DateTime.UtcNow;
        user.LastLoginIpAddress = ipAddress;
        await _userManager.UpdateAsync(user);

        _logger.LogInformation("User {UserId} logged in successfully from {IpAddress}", user.Id, ipAddress);

        return Result<AuthenticationResponse>.Success(new AuthenticationResponse
        {
            AccessToken = jwtToken.AccessToken,
            RefreshToken = refreshToken.Token,
            ExpiresIn = jwtToken.ExpiresIn,
            TokenType = "Bearer",
            User = new UserDto
            {
                Id = user.Id,
                Email = user.Email!,
                FirstName = user.FirstName,
                LastName = user.LastName,
                Roles = await _userManager.GetRolesAsync(user)
            }
        });
    }

    public async Task<Result<AuthenticationResponse>> RefreshTokenAsync(RefreshTokenRequest request, string ipAddress)
    {
        var storedToken = await _refreshTokenService.GetTokenAsync(request.RefreshToken);
        if (storedToken == null)
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "InvalidRefreshToken",
                "Invalid refresh token",
                ErrorType.Security));
        }

        if (storedToken.IsUsed || storedToken.IsRevoked || storedToken.ExpiresAt < DateTime.UtcNow)
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "ExpiredRefreshToken",
                "Refresh token has expired or been used",
                ErrorType.Security));
        }

        var user = await _userManager.FindByIdAsync(storedToken.UserId.ToString());
        if (user == null)
        {
            return Result<AuthenticationResponse>.Failure(new Error(
                "UserNotFound",
                "User not found",
                ErrorType.NotFound));
        }

        // Mark old token as used
        storedToken.IsUsed = true;
        storedToken.UsedAt = DateTime.UtcNow;
        storedToken.UsedIpAddress = ipAddress;
        await _refreshTokenService.UpdateTokenAsync(storedToken);

        // Generate new tokens
        var jwtToken = await _jwtTokenService.GenerateTokenAsync(user);
        var newRefreshToken = await _refreshTokenService.GenerateTokenAsync(user, ipAddress);

        return Result<AuthenticationResponse>.Success(new AuthenticationResponse
        {
            AccessToken = jwtToken.AccessToken,
            RefreshToken = newRefreshToken.Token,
            ExpiresIn = jwtToken.ExpiresIn,
            TokenType = "Bearer",
            User = new UserDto
            {
                Id = user.Id,
                Email = user.Email!,
                FirstName = user.FirstName,
                LastName = user.LastName,
                Roles = await _userManager.GetRolesAsync(user)
            }
        });
    }

    public async Task<Result> LogoutAsync(string refreshToken, string ipAddress)
    {
        var storedToken = await _refreshTokenService.GetTokenAsync(refreshToken);
        if (storedToken != null)
        {
            storedToken.IsRevoked = true;
            storedToken.RevokedAt = DateTime.UtcNow;
            storedToken.RevokedIpAddress = ipAddress;
            await _refreshTokenService.UpdateTokenAsync(storedToken);
        }

        await _signInManager.SignOutAsync();

        return Result.Success();
    }
}

// JWT Token Service with Modern Security
public class JwtTokenService : IJwtTokenService
{
    private readonly IConfiguration _configuration;
    private readonly UserManager<ApplicationUser> _userManager;
    private readonly ILogger<JwtTokenService> _logger;

    public JwtTokenService(
        IConfiguration configuration,
        UserManager<ApplicationUser> userManager,
        ILogger<JwtTokenService> logger)
    {
        _configuration = configuration;
        _userManager = userManager;
        _logger = logger;
    }

    public async Task<TokenResponse> GenerateTokenAsync(ApplicationUser user)
    {
        var jwtSettings = _configuration.GetSection("JwtSettings");
        var secretKey = jwtSettings["SecretKey"]
            ?? throw new InvalidOperationException("JWT SecretKey not configured");
        var issuer = jwtSettings["Issuer"] ?? "MyApp";
        var audience = jwtSettings["Audience"] ?? "MyAppUsers";
        var expiryMinutes = int.Parse(jwtSettings["ExpiryMinutes"] ?? "60");

        var securityKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(secretKey));
        var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);

        var claims = new List<Claim>
        {
            new(JwtRegisteredClaimNames.Sub, user.Id.ToString()),
            new(JwtRegisteredClaimNames.Email, user.Email!),
            new(JwtRegisteredClaimNames.GivenName, user.FirstName),
            new(JwtRegisteredClaimNames.FamilyName, user.LastName),
            new(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
            new(JwtRegisteredClaimNames.Iat, DateTimeOffset.UtcNow.ToUnixTimeSeconds().ToString(), ClaimValueTypes.Integer64)
        };

        // Add roles
        var roles = await _userManager.GetRolesAsync(user);
        claims.AddRange(roles.Select(role => new Claim(ClaimTypes.Role, role)));

        // Add custom claims
        claims.Add(new Claim("user_name", $"{user.FirstName} {user.LastName}"));
        claims.Add(new Claim("last_login", user.LastLoginAt?.ToString("o") ?? string.Empty));

        var token = new JwtSecurityToken(
            issuer: issuer,
            audience: audience,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(expiryMinutes),
            signingCredentials: credentials);

        var tokenString = new JwtSecurityTokenHandler().WriteToken(token);

        return new TokenResponse
        {
            AccessToken = tokenString,
            ExpiresIn = expiryMinutes * 60,
            TokenType = "Bearer"
        };
    }
}

// API Key Authentication
public class ApiKeyAuthenticationHandler : AuthenticationHandler<ApiKeyAuthenticationSchemeOptions>
{
    private readonly IApiKeyService _apiKeyService;

    public ApiKeyAuthenticationHandler(
        IOptionsMonitor<ApiKeyAuthenticationSchemeOptions> options,
        ILoggerFactory logger,
        UrlEncoder encoder,
        ISystemClock clock,
        IApiKeyService apiKeyService)
        : base(options, logger, encoder, clock)
    {
        _apiKeyService = apiKeyService;
    }

    protected override async Task<AuthenticateResult> HandleAuthenticateAsync()
    {
        if (!Request.Headers.TryGetValue("X-API-Key", out var apiKeyValue))
        {
            return AuthenticateResult.Fail("Missing API Key");
        }

        var apiKey = apiKeyValue.FirstOrDefault();
        if (string.IsNullOrEmpty(apiKey))
        {
            return AuthenticateResult.Fail("Missing API Key");
        }

        var validationResult = await _apiKeyService.ValidateApiKeyAsync(apiKey);
        if (!validationResult.IsValid)
        {
            return AuthenticateResult.Fail("Invalid API Key");
        }

        var claims = new[]
        {
            new Claim(ClaimTypes.Name, validationResult.ApiKey.Name),
            new Claim(ClaimTypes.NameIdentifier, validationResult.ApiKey.Id.ToString()),
            new Claim("permissions", string.Join(",", validationResult.ApiKey.Permissions))
        };

        var identity = new ClaimsIdentity(claims, Scheme.Name);
        var principal = new ClaimsPrincipal(identity);
        var ticket = new AuthenticationTicket(principal, Scheme.Name);

        return AuthenticateResult.Success(ticket);
    }
}

// Rate Limiting Middleware
public class RateLimitingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IRateLimitService _rateLimitService;
    private readonly ILogger<RateLimitingMiddleware> _logger;

    public RateLimitingMiddleware(
        RequestDelegate next,
        IRateLimitService rateLimitService,
        ILogger<RateLimitingMiddleware> logger)
    {
        _next = next;
        _rateLimitService = rateLimitService;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var endpoint = context.GetEndpoint();
        var rateLimitAttribute = endpoint?.Metadata.GetMetadata<RateLimitAttribute>();

        if (rateLimitAttribute == null)
        {
            await _next(context);
            return;
        }

        var clientId = GetClientId(context);
        var requestKey = $"{clientId}:{context.Request.Path}";

        var isAllowed = await _rateLimitService.IsRequestAllowedAsync(
            requestKey,
            rateLimitAttribute.Requests,
            rateLimitAttribute.WindowSeconds);

        if (!isAllowed)
        {
            _logger.LogWarning("Rate limit exceeded for client {ClientId} on {Path}", clientId, context.Request.Path);

            context.Response.StatusCode = StatusCodes.Status429TooManyRequests;
            context.Response.Headers["Retry-After"] = rateLimitAttribute.WindowSeconds.ToString();

            await context.Response.WriteAsync("Rate limit exceeded");
            return;
        }

        // Add rate limit headers
        var remainingRequests = await _rateLimitService.GetRemainingRequestsAsync(requestKey);
        context.Response.Headers["X-RateLimit-Limit"] = rateLimitAttribute.Requests.ToString();
        context.Response.Headers["X-RateLimit-Remaining"] = remainingRequests.ToString();
        context.Response.Headers["X-RateLimit-Reset"] =
            DateTimeOffset.UtcNow.AddSeconds(rateLimitAttribute.WindowSeconds).ToUnixTimeSeconds().ToString();

        await _next(context);
    }

    private string GetClientId(HttpContext context)
    {
        // Try to get authenticated user ID first
        var userId = context.User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        if (!string.IsNullOrEmpty(userId))
            return $"user:{userId}";

        // Fall back to IP address
        return $"ip:{context.Connection.RemoteIpAddress?.ToString() ?? "unknown"}";
    }
}

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class RateLimitAttribute : Attribute
{
    public int Requests { get; }
    public int WindowSeconds { get; }

    public RateLimitAttribute(int requests, int windowSeconds)
    {
        Requests = requests;
        WindowSeconds = windowSeconds;
    }
}
```

#### **Essential Resources**
- **📖 [ASP.NET Core Security](https://learn.microsoft.com/en-us/aspnet/core/security/)**
  - Authentication patterns
  - Authorization policies
  - Security best practices

- **🎓 [OWASP Security Guide](https://owasp.org/)**
  - Security vulnerabilities
  - Prevention techniques
  - Security testing

#### **Practice Projects**
1. **Secure API**: Complete authentication and authorization
2. **OAuth2 Provider**: Custom identity provider
3. **Security Scanner**: Automated security validation

### **Week 27-28: Expert-Level Architecture & Best Practices**

#### **Enterprise Architecture with Modern C#**
```csharp
// CQRS Implementation with MediatR and Event Sourcing
public class OrderCommandHandler :
    IRequestHandler<CreateOrderCommand, Result<OrderDto>>,
    IRequestHandler<UpdateOrderCommand, Result<OrderDto>>,
    IRequestHandler<CancelOrderCommand, Result>
{
    private readonly IOrderAggregateRepository _repository;
    private readonly IEventStore _eventStore;
    private readonly IDomainEventDispatcher _dispatcher;
    private readonly ILogger<OrderCommandHandler> _logger;

    public OrderCommandHandler(
        IOrderAggregateRepository repository,
        IEventStore eventStore,
        IDomainEventDispatcher dispatcher,
        ILogger<OrderCommandHandler> logger)
    {
        _repository = repository;
        _eventStore = eventStore;
        _dispatcher = dispatcher;
        _logger = logger;
    }

    public async Task<Result<OrderDto>> Handle(CreateOrderCommand request, CancellationToken cancellationToken)
    {
        try
        {
            // Load or create aggregate
            var order = Order.Create(request.CustomerId, request.Items);
            if (order.IsFailure)
                return Result<OrderDto>.Failure(order.Error);

            var orderAggregate = order.Value;

            // Save to event store
            var events = orderAggregate.GetUncommittedEvents();
            await _eventStore.SaveEventsAsync(
                orderAggregate.Id,
                events,
                orderAggregate.ExpectedVersion);

            // Dispatch domain events
            await _dispatcher.DispatchAsync(events);

            // Commit aggregate
            orderAggregate.MarkChangesAsCommitted();

            _logger.LogInformation("Order {OrderId} created successfully", orderAggregate.Id);

            return Result<OrderDto>.Success(MapToDto(orderAggregate));
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error creating order for customer {CustomerId}", request.CustomerId);
            return Result<OrderDto>.Failure(new Error(
                "CreateOrderFailed",
                "Failed to create order",
                ErrorType.System));
        }
    }

    private OrderDto MapToDto(Order order) =>
        new(order.Id, order.CustomerId, order.Status, order.TotalAmount, order.OrderDate);
}

// Event Store Implementation
public class EventStore : IEventStore
{
    private readonly IEventRepository _repository;
    private readonly ILogger<EventStore> _logger;

    public EventStore(IEventRepository repository, ILogger<EventStore> logger)
    {
        _repository = repository;
        _logger = logger;
    }

    public async Task<IReadOnlyList<IDomainEvent>> GetEventsAsync(Guid aggregateId, int fromVersion = 0)
    {
        var events = await _repository.GetEventsAsync(aggregateId, fromVersion);

        _logger.LogDebug("Retrieved {EventCount} events for aggregate {AggregateId} from version {FromVersion}",
            events.Count, aggregateId, fromVersion);

        return events.Select(e => DeserializeEvent(e)).ToList();
    }

    public async Task SaveEventsAsync(Guid aggregateId, IReadOnlyList<IDomainEvent> events, int expectedVersion)
    {
        var eventDescriptors = events.Select(e => new EventDescriptor
        {
            Id = Guid.NewGuid(),
            AggregateId = aggregateId,
            EventType = e.GetType().Name,
            EventData = SerializeEvent(e),
            Version = expectedVersion + 1,
            Timestamp = DateTime.UtcNow
        }).ToList();

        await _repository.SaveEventsAsync(eventDescriptors, expectedVersion);

        _logger.LogInformation("Saved {EventCount} events for aggregate {AggregateId}",
            events.Count, aggregateId);
    }

    private string SerializeEvent(IDomainEvent @event)
    {
        return JsonSerializer.Serialize(@event, new JsonSerializerOptions
        {
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
            Converters = { new JsonStringEnumConverter() }
        });
    }

    private IDomainEvent DeserializeEvent(EventDescriptor descriptor)
    {
        var eventType = Type.GetType($"MyApp.Domain.Events.{descriptor.EventType}")
            ?? throw new InvalidOperationException($"Unknown event type: {descriptor.EventType}");

        var @event = (IDomainEvent)JsonSerializer.Deserialize(
            descriptor.EventData,
            eventType,
            new JsonSerializerOptions
            {
                PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
                Converters = { new JsonStringEnumConverter() }
            })!;

        return @event;
    }
}

// Repository with Unit of Work Pattern
public interface IUnitOfWork : IDisposable
{
    IRepository<T> Repository<T>() where T : class;
    Task<int> SaveChangesAsync(CancellationToken cancellationToken = default);
    Task BeginTransactionAsync(CancellationToken cancellationToken = default);
    Task CommitTransactionAsync(CancellationToken cancellationToken = default);
    Task RollbackTransactionAsync(CancellationToken cancellationToken = default);
}

public class UnitOfWork : IUnitOfWork
{
    private readonly DbContext _context;
    private readonly Dictionary<Type, object> _repositories = new();
    private IDbContextTransaction? _transaction;

    public UnitOfWork(DbContext context)
    {
        _context = context;
    }

    public IRepository<T> Repository<T>() where T : class
    {
        if (_repositories.TryGetValue(typeof(T), out var repository))
        {
            return (IRepository<T>)repository;
        }

        var repo = new Repository<T>(_context);
        _repositories[typeof(T)] = repo;
        return repo;
    }

    public async Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
    {
        try
        {
            return await _context.SaveChangesAsync(cancellationToken);
        }
        catch (DbUpdateConcurrencyException ex)
        {
            throw new ConcurrencyException("Concurrency conflict detected", ex);
        }
        catch (DbUpdateException ex)
        {
            throw new DataException("Database update failed", ex);
        }
    }

    public async Task BeginTransactionAsync(CancellationToken cancellationToken = default)
    {
        if (_transaction != null)
            throw new InvalidOperationException("Transaction already started");

        _transaction = await _context.Database.BeginTransactionAsync(cancellationToken);
    }

    public async Task CommitTransactionAsync(CancellationToken cancellationToken = default)
    {
        if (_transaction == null)
            throw new InvalidOperationException("No transaction in progress");

        try
        {
            await _context.SaveChangesAsync(cancellationToken);
            await _transaction.CommitAsync(cancellationToken);
        }
        catch
        {
            await _transaction.RollbackAsync(cancellationToken);
            throw;
        }
        finally
        {
            await _transaction.DisposeAsync();
            _transaction = null;
        }
    }

    public async Task RollbackTransactionAsync(CancellationToken cancellationToken = default)
    {
        if (_transaction != null)
        {
            await _transaction.RollbackAsync(cancellationToken);
            await _transaction.DisposeAsync();
            _transaction = null;
        }
    }

    public void Dispose()
    {
        _transaction?.Dispose();
        _context.Dispose();
    }
}

// Configuration Management with Validation
public class AppConfiguration
{
    public DatabaseSettings Database { get; init; } = new();
    public JwtSettings Jwt { get; init; } = new();
    public RedisSettings Redis { get; init; } = new();
    public LoggingSettings Logging { get; init; } = new();
    public CorsSettings Cors { get; init; } = new();
}

public record DatabaseSettings
{
    public string ConnectionString { get; init; } = string.Empty;
    public int MaxPoolSize { get; init; } = 100;
    public int CommandTimeout { get; init; } = 30;
    public bool EnableRetryOnFailure { get; init; } = true;
    public int MaxRetryCount { get; init; } = 3;
}

public record JwtSettings
{
    public string SecretKey { get; init; } = string.Empty;
    public string Issuer { get; init; } = string.Empty;
    public string Audience { get; init; } = string.Empty;
    public int ExpiryMinutes { get; init; } = 60;
    public int RefreshTokenExpiryDays { get; init; } = 7;
}

public record RedisSettings
{
    public string ConnectionString { get; init; } = "localhost:6379";
    public int Database { get; init; } = 0;
    public TimeSpan DefaultExpiration { get; init; } = TimeSpan.FromMinutes(5);
}

public record LoggingSettings
{
    public LogLevel LogLevel { get; init; } = LogLevel.Information;
    public bool EnableConsole { get; init; } = true;
    public bool EnableFile { get; init; } = false;
    public string? LogFilePath { get; init; }
    public string? SeqUrl { get; init; }
}

public record CorsSettings
{
    public string[] AllowedOrigins { get; init; } = Array.Empty<string>();
    public string[] AllowedMethods { get; init; } = ["GET", "POST", "PUT", "DELETE", "OPTIONS"];
    public string[] AllowedHeaders { get; init; } = ["*"];
    public bool AllowCredentials { get; init; } = false;
}

// Health Checks with Custom Implementations
public class DatabaseHealthCheck : IHealthCheck
{
    private readonly DbContext _context;
    private readonly ILogger<DatabaseHealthCheck> _logger;

    public DatabaseHealthCheck(DbContext context, ILogger<DatabaseHealthCheck> logger)
    {
        _context = context;
        _logger = logger;
    }

    public async Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var canConnect = await _context.Database.CanConnectAsync(cancellationToken);

            if (!canConnect)
            {
                return HealthCheckResult.Unhealthy("Cannot connect to database");
            }

            var stopwatch = Stopwatch.StartNew();
            await _context.Database.ExecuteSqlRawAsync("SELECT 1", cancellationToken);
            stopwatch.Stop();

            var data = new Dictionary<string, object>
            {
                ["response_time_ms"] = stopwatch.ElapsedMilliseconds,
                ["connection_string"] = MaskConnectionString(_context.Database.GetConnectionString()!)
            };

            return stopwatch.ElapsedMilliseconds > 1000
                ? HealthCheckResult.Degraded("Slow database response", data: data)
                : HealthCheckResult.Healthy("Database is healthy", data: data);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Database health check failed");
            return HealthCheckResult.Unhealthy("Database health check failed", ex);
        }
    }

    private string MaskConnectionString(string connectionString)
    {
        return Regex.Replace(connectionString, @"Password=.*?;", "Password=***;", RegexOptions.IgnoreCase);
    }
}

// Metrics Collection
public class ApplicationMetrics
{
    private readonly Counter<int> _requestCounter;
    private readonly Histogram<double> _requestDuration;
    private readonly Counter<int> _errorCounter;
    private readonly Gauge<int> _activeConnections;

    public ApplicationMetrics(IMeterFactory meterFactory)
    {
        var meter = meterFactory.Create("MyApp.Application");

        _requestCounter = meter.CreateCounter<int>("requests_total", "Total number of requests");
        _requestDuration = meter.CreateHistogram<double>("request_duration_seconds", "Request duration in seconds");
        _errorCounter = meter.CreateCounter<int>("errors_total", "Total number of errors");
        _activeConnections = meter.CreateGauge<int>("active_connections", "Number of active connections");
    }

    public void RecordRequest(string endpoint, string method, int statusCode, double durationSeconds)
    {
        _requestCounter.Add(1, new("endpoint", endpoint), new("method", method), new("status_code", statusCode.ToString()));
        _requestDuration.Record(durationSeconds, new("endpoint", endpoint), new("method", method));

        if (statusCode >= 400)
        {
            _errorCounter.Add(1, new("endpoint", endpoint), new("method", method), new("status_code", statusCode.ToString()));
        }
    }

    public void SetActiveConnections(int count) => _activeConnections.Record(count);
}
```

#### **Essential Resources**
- **📖 [Enterprise Architecture Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/)**
  - CQRS implementation
  - Event sourcing
  - Microservices patterns

- **🎓 [Clean Architecture .NET](https://github.com/JasonGT/CleanArchitecture)**
  - Architecture guidelines
  - Best practices
  - Sample implementations

#### **Practice Projects**
1. **Complete Microservice**: CQRS with event sourcing
2. **Event Store Implementation**: Custom event storage
3. **Health Monitoring System**: Advanced observability

---

## 🛠️ Essential Tools & Extensions

### **Development Tools (.NET 9/10 Compatible)**
```json
{
  "visualStudioExtensions": [
    "IntelliCode for Visual Studio 2022",
    ".NET Multi-platform App UI development",
    "Azure Development",
    "C# Dev Kit"
  ],
  "vsCodeExtensions": [
    "ms-dotnettools.csharp",
    "ms-dotnettools.blazorwasm-companion",
    "ms-dotnettools.vscode-dotnet-runtime",
    "ms-azuretools.vscode-docker"
  ],
  "cliTools": [
    "dotnet ef",
    "dotnet watch",
    "dotnet build-server",
    "dotnet-format"
  ],
  "profilingTools": [
    "dotMemory Unit",
    "PerfView",
    "dotnet-trace",
    "dotnet-counters"
  ]
}
```

### **Essential NuGet Packages (.NET 9/10)**
```xml
<ItemGroup>
  <!-- Core Framework -->
  <PackageReference Include="Microsoft.Extensions.Hosting" Version="9.0.0" />
  <PackageReference Include="Microsoft.Extensions.Configuration" Version="9.0.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="9.0.0" />
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="9.0.0" />
  <PackageReference Include="Microsoft.Extensions.Caching.StackExchangeRedis" Version="9.0.0" />

  <!-- Web API -->
  <PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="9.0.0" />
  <PackageReference Include="Swashbuckle.AspNetCore" Version="7.0.0" />
  <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="9.0.0" />

  <!-- Data Access - Dapper High Performance Stack -->
  <PackageReference Include="Dapper" Version="2.1.35" />
  <PackageReference Include="Dapper.Contrib" Version="2.0.78" />
  <PackageReference Include="System.Data.SqlClient" Version="5.0.1" />
  <PackageReference Include="Npgsql" Version="8.0.3" />
  <PackageReference Include="Microsoft.Data.Sqlite" Version="8.0.3" />

  <!-- Testing -->
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.9.0" />
  <PackageReference Include="xunit" Version="2.7.0" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.7.0" />
  <PackageReference Include="Moq" Version="4.21.0" />
  <PackageReference Include="FluentAssertions" Version="7.0.0" />
  <PackageReference Include="Microsoft.AspNetCore.Mvc.Testing" Version="9.0.0" />
  <PackageReference Include="Testcontainers" Version="4.0.0" />

  <!-- Performance -->
  <PackageReference Include="BenchmarkDotNet" Version="0.14.0" />
  <PackageReference Include="System.IO.Pipelines" Version="9.0.0" />
  <PackageReference Include="System.Threading.Channels" Version="9.0.0" />

  <!-- Utilities -->
  <PackageReference Include="AutoMapper.Extensions.Microsoft.DependencyInjection" Version="13.0.0" />
  <PackageReference Include="FluentValidation.AspNetCore" Version="12.0.0" />
  <PackageReference Include="Serilog.AspNetCore" Version="9.0.0" />
  <PackageReference Include="MediatR" Version="13.0.0" />
  <PackageReference Include="Polly" Version="8.4.0" />

  <!-- Security -->
  <PackageReference Include="Microsoft.AspNetCore.Identity" Version="9.0.0" />
  <PackageReference Include="Microsoft.AspNetCore.DataProtection.StackExchangeRedis" Version="9.0.0" />

<!-- Background Jobs - Modern .NET 10 -->
  <PackageReference Include="TickerQ" Version="1.0.0" />

  <!-- Observability -->
  <PackageReference Include="OpenTelemetry" Version="1.9.0" />
  <PackageReference Include="OpenTelemetry.Exporter.Jaeger" Version="1.5.1" />
  <PackageReference Include="OpenTelemetry.Exporter.Prometheus" Version="1.9.0" />
</ItemGroup>
```

---

## 📖 Key Books & Publications (.NET 9/10)

### **Essential Reading List**
1. **"C# 13 and .NET 9 – Modern Cross-Platform Development" by Mark J. Price** - Comprehensive .NET 9 guide
2. **"Architecting Modern Web Applications with ASP.NET Core and Azure" by Steve Gordon** - Cloud patterns
3. **"Domain-Driven Design with .NET Core" by Julio Casal** - DDD patterns
4. **"Clean Architecture with .NET" by Jason Taylor** - Clean Architecture
5. **"Microservices in .NET" by Christian Horsdal** - Microservices patterns

### **Online Documentation**
- [Microsoft Learn - .NET 9](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9) - Latest features
- [C# 13 Reference](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-13) - Language features
- [.NET 10 Preview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10) - Next generation

---

## 🎓 Certification & Validation

### **Skill Validation Checklist**

#### **Beginner Level (.NET 9 Features)**
- [ ] Implement C# 13 features (field keyword, params collections)
- [ ] Create modern C# applications with primary constructors
- [ ] Use collection expressions effectively
- [ ] Implement Result<T> pattern for error handling
- [ ] Basic async/await patterns with ValueTask

#### **Intermediate Level (Modern C#)**
- [ ] Advanced async patterns with Channel<T>
- [ ] LINQ with functional programming
- [ ] Dependency injection and minimal APIs
- [ ] Comprehensive testing strategies
- [ ] Performance optimization with Span<T>

#### **Advanced Level (.NET 9/10 Expert)**
- [ ] C# 14 extension members and new features
- [ ] High-performance memory management
- [ ] Domain-driven design implementation
- [ ] Advanced testing and benchmarking
- [ ] Cloud-native microservices patterns

#### **Expert Level (Enterprise Architecture)**
- [ ] CQRS with event sourcing
- [ ] Advanced security patterns
- [ ] Performance engineering
- [ ] System architecture design
- [ ] Leadership and mentoring capabilities

### **Project Portfolio Examples (.NET 9/10)**
1. **Real-time Analytics Platform**: High-performance streaming
2. **E-commerce Microservices**: Complete distributed system
3. **Financial Trading System**: Ultra-low latency
4. **Healthcare Data Pipeline**: HIPAA compliant system
5. **IoT Data Processor**: Edge computing solution

---

## 🔗 Quick Reference Links

### **Official .NET 9/10 Resources**
- [.NET 9 Documentation](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)
- [C# 13 Reference](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-13)
- [.NET 10 Overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)
- [C# 14 Features](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-14)

### **Performance & Best Practices**
- [.NET Performance Blog](https://devblogs.microsoft.com/dotnet/category/performance/)
- [Memory Management Guide](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/)
- [Async Best Practices](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/)
- [Performance Tips](https://learn.microsoft.com/en-us/dotnet/core/performance/)

### **Community Resources**
- [.NET Blog](https://devblogs.microsoft.com/dotnet/)
- [C# GitHub Repository](https://github.com/dotnet/csharplang)
- [.NET Community Standup](https://www.youtube.com/c/dotnet)
- [.NET Conf](https://www.dotnetconf.net/)

---

## 🎯 AI Agent Success Metrics

### **Weekly Milestones**
- **Code Quality**: >85% test coverage for business logic
- **Performance**: <500ms API response times, <100MB memory usage
- **Security**: Zero OWASP vulnerabilities, proper authentication
- **Documentation**: Complete API documentation with OpenAPI
- **Best Practices**: Modern C# 13/14 features, clean architecture

### **Final Evaluation Criteria**
- **Architecture**: Scalable, maintainable, performant systems
- **Modern Features**: Expert use of .NET 9/10 and C# 13/14
- **Performance**: Optimized for production workloads
- **Security**: Enterprise-grade security implementation
- **Leadership**: Ability to architect complex systems

---

> **🚀 This guide provides the most comprehensive C# learning path focused exclusively on .NET 9 and .NET 10, preparing AI agents for mastery of modern C# development with the latest language features and platform capabilities.**

---

*Last Updated: November 2024*
*Target Platform: .NET 9 and .NET 10 Only*
*Language Version: C# 13 and C# 14*
*Version: 1.0*