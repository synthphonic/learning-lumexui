# NuGet Package Standards for LumexUI Learning Repository

## 🚨 Critical Package Standards

This document defines the official NuGet package standards for AI agents working on the LumexUI learning repository. **All AI agents MUST follow these standards when creating or modifying applications.**

## Package Philosophy

### Core Principles
- **Performance-First**: Prioritize high-performance libraries that enhance LumexUI responsiveness
- **.NET 10 Compatible**: All packages must support .NET 10 with modern C# 13/14 features
- **Learning-Focused**: Libraries should support educational objectives without unnecessary complexity
- **Production-Ready**: Demonstrate real-world patterns and best practices
- **Minimal Overhead**: Avoid heavy dependencies that distract from LumexUI learning

### Forbidden Packages
- ❌ **Entity Framework Core** - Replaced by Dapper for performance and learning clarity
- ❌ **Ardalis.Result** - Removed to maintain simplicity for learning scenarios
- ❌ **Legacy .NET Framework packages** - Only .NET 10+ compatible packages allowed
- ❌ **Heavy ORM alternatives** - Dapper is the standard for data access

## 📦 Required Package Stack

### Data Access Layer (Dapper Ecosystem)

```xml
<!-- Core Dapper Stack -->
<PackageReference Include="Dapper" Version="2.1.35" />
<PackageReference Include="Dapper.Contrib" Version="2.0.78" />

<!-- Database Providers -->
<PackageReference Include="System.Data.SqlClient" Version="5.0.1" />
<PackageReference Include="Npgsql" Version="8.0.3" />
<PackageReference Include="Microsoft.Data.Sqlite" Version="8.0.3" />
```

**Rationale**: Dapper provides maximum performance (5-10x faster than EF Core) while teaching SQL fundamentals alongside LumexUI UI development.

### Background Jobs (TickerQ)

```xml
<!-- Modern .NET 10 Background Jobs -->
<PackageReference Include="TickerQ" Version="1.0.0" />
```

**Rationale**: TickerQ is built specifically for .NET 10 with source generators, reflection-free execution, and seamless Blazor integration.

### Essential Utilities

```xml
<!-- Object Mapping -->
<PackageReference Include="AutoMapper" Version="13.0.1" />

<!-- Validation -->
<PackageReference Include="FluentValidation" Version="11.10.0" />
<PackageReference Include="FluentValidation.DependencyInjectionExtensions" Version="11.10.0" />

<!-- Testing -->
<PackageReference Include="bunit" Version="1.29.0" />
<PackageReference Include="xunit" Version="2.6.6" />
<PackageReference Include="Moq" Version="4.20.69" />

<!-- Logging -->
<PackageReference Include="Serilog" Version="4.0.1" />
<PackageReference Include="Serilog.Extensions.Hosting" Version="8.0.1" />
```

## 🔧 Integration Patterns

### Dapper + LumexUI Integration

#### Repository Pattern Base Class
```csharp
public abstract class BaseRepository<T> where T : class
{
    protected readonly IDbConnection _connection;
    protected readonly string _tableName;

    protected BaseRepository IDbConnection connection, string tableName)
    {
        _connection = connection;
        _tableName = tableName;
    }

    public virtual async Task<IEnumerable<T>> GetAllAsync()
    {
        var sql = $"SELECT * FROM {_tableName}";
        return await _connection.QueryAsync<T>(sql);
    }

    public virtual async Task<T?> GetByIdAsync(int id)
    {
        var sql = $"SELECT * FROM {_tableName} WHERE Id = @Id";
        return await _connection.QuerySingleOrDefaultAsync<T>(sql, new { Id = id });
    }

    public virtual async Task<int> CreateAsync(T entity)
    {
        var sql = $@"INSERT INTO {_tableName} ({GetColumns()})
                    VALUES ({GetParameters()});
                    SELECT CAST(SCOPE_IDENTITY() as int)";
        return await _connection.QuerySingleAsync<int>(sql, entity);
    }

    public virtual async Task<bool> UpdateAsync(T entity)
    {
        var sql = $@"UPDATE {_tableName}
                    SET {GetSetColumns()}
                    WHERE Id = @Id";
        return await _connection.ExecuteAsync(sql, entity) > 0;
    }

    public virtual async Task<bool> DeleteAsync(int id)
    {
        var sql = $"DELETE FROM {_tableName} WHERE Id = @Id";
        return await _connection.ExecuteAsync(sql, new { Id = id }) > 0;
    }
}
```

#### LumexUI Form Integration
```razor
@inject IProductRepository ProductRepository

<EditForm Model="@product" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />

    <LumexTextField @bind-Value="product.Name" Label="Product Name">
        <ValidationMessage For="@(() => product.Name)" />
    </LumexTextField>

    <LumexTextField @bind-Value="product.Price" Label="Price" Type="number">
        <ValidationMessage For="@(() => product.Price)" />
    </LumexTextField>

    <LumexButton Variant="Variant.Primary" Type="ButtonType.Submit"
                 IsDisabled="isSaving">
        @if (isSaving) {
            <LumexSpinner Size="Size.Small" />
        }
        Save Product
    </LumexButton>
</EditForm>

@code {
    private Product product = new();
    private bool isSaving = false;

    private async Task HandleValidSubmit()
    {
        isSaving = true;
        try
        {
            if (product.Id == 0)
                await ProductRepository.CreateAsync(product);
            else
                await ProductRepository.UpdateAsync(product);

            // Show success message with LumexUI Alert
        }
        finally
        {
            isSaving = false;
        }
    }
}
```

### TickerQ + LumexUI Integration

#### Job Registration in Program.cs
```csharp
builder.Services.AddTickerQ(options =>
{
    options.ConfigureScheduler(scheduler =>
    {
        scheduler.NodeIdentifier = "lumexui-app";
        scheduler.MaxConcurrency = Environment.ProcessorCount * 2;
    });

    // Add database persistence
    options.AddOperationalStore(efOptions =>
    {
        efOptions.UseTickerQDbContext<TickerQDbContext>(dbOptions =>
        {
            dbOptions.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"));
        });
    });
});

builder.Services.AddHostedService<TickerQBackgroundService>();
```

#### Background Job with LumexUI Progress Updates
```csharp
public class EmailNotificationJob : IJob<EmailNotificationJobData>
{
    private readonly ILogger<EmailNotificationJob> _logger;

    public EmailNotificationJob(ILogger<EmailNotificationJob> logger)
    {
        _logger = logger;
    }

    public async Task ExecuteAsync(IJobContext<EmailNotificationJobData> context)
    {
        var data = context.Data;

        try
        {
            // Report progress (can be shown in LumexUI)
            context.ReportProgress(0, "Starting email notification...");

            // Simulate email sending with progress updates
            for (int i = 0; i <= 100; i += 20)
            {
                await Task.Delay(100); // Simulate work
                context.ReportProgress(i, $"Sending email... {i}%");
            }

            _logger.LogInformation($"Email sent to {data.ToAddress}");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, $"Failed to send email to {data.ToAddress}");
            throw;
        }
    }
}

public record EmailNotificationJobData(string ToAddress, string Subject, string Body);
```

#### LumexUI Progress Monitoring
```razor
@inject IJobMonitoringService JobMonitoringService

@if (activeJobs.Any())
{
    <LumexCard>
        <LumexCardContent>
            <LumexCardHeader>Active Background Jobs</LumexCardHeader>
            <LumexCardBody>
                @foreach (var job in activeJobs)
                {
                    <LumexProgress Value="@job.Progress" Max="100" />
                    <LumexText>@job.Description - @job.Progress%</LumexText>
                }
            </LumexCardBody>
        </LumexCardContent>
    </LumexCard>
}
```

## 🔗 Database Connection Examples

### Connection String Configuration
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=LumexUILearning;Trusted_Connection=true;",
    "PostgresConnection": "Host=localhost;Database=LumexUILearning;Username=postgres;Password=password;",
    "SQLiteConnection": "Data Source=lumexui_learning.db"
  }
}
```

### Database-Specific Connection Factories
```csharp
public interface IDbConnectionFactory
{
    IDbConnection CreateConnection();
}

public class SqlServerConnectionFactory : IDbConnectionFactory
{
    private readonly string _connectionString;

    public SqlServerConnectionFactory(IConfiguration configuration)
    {
        _connectionString = configuration.GetConnectionString("DefaultConnection");
    }

    public IDbConnection CreateConnection()
        => new SqlConnection(_connectionString);
}

public class PostgreSqlConnectionFactory : IDbConnectionFactory
{
    private readonly string _connectionString;

    public PostgreSqlConnectionFactory(IConfiguration configuration)
    {
        _connectionString = configuration.GetConnectionString("PostgresConnection");
    }

    public IDbConnection CreateConnection()
        => new NpgsqlConnection(_connectionString);
}

public class SQLiteConnectionFactory : IDbConnectionFactory
{
    private readonly string _connectionString;

    public SQLiteConnectionFactory(IConfiguration configuration)
    {
        _connectionString = configuration.GetConnectionString("SQLiteConnection");
    }

    public IDbConnection CreateConnection()
        => new SqliteConnection(_connectionString);
}
```

## 📋 Package Management Guidelines

### Version Strategy
- **Dapper**: Always use latest stable (2.1.35+)
- **TickerQ**: Use latest .NET 10 compatible version (1.0.0+)
- **Database Providers**: Use latest stable versions
- **Utilities**: Prefer latest stable versions with .NET 10 support

### Dependency Rules
- **No EF Core Dependencies**: Any package requiring EF Core is forbidden
- **Minimal Transitive Dependencies**: Prefer packages with clean dependency trees
- **.NET 10 Targeting**: All packages must target .NET 8+ (preferably .NET 10)
- **MIT/Apache Licensing**: Prefer permissive licenses for learning scenarios

### Package Validation Checklist
- [ ] Package is .NET 10 compatible
- [ ] Package has active maintenance (updates within last 6 months)
- [ ] Package has comprehensive documentation
- [ ] Package integrates well with LumexUI patterns
- [ ] Package supports Blazor Server hosting model
- [ ] Package has minimal performance overhead

## 🎓 Educational Usage Guidelines

### When to Use Each Package

**Dapper**:
- ✅ Teaching SQL fundamentals
- ✅ Performance-critical scenarios
- ✅ Simple CRUD operations
- ✅ When you want to understand database interactions

**TickerQ**:
- ✅ Background processing examples
- ✅ Email notifications
- ✅ Data import/export jobs
- ✅ Real-time UI updates from background tasks

**AutoMapper**:
- ✅ DTO mapping examples
- ✅ Complex object transformations
- ✅ API response formatting

**FluentValidation**:
- ✅ Form validation with LumexUI components
- ✅ Complex business rule validation
- ✅ User input sanitization

### Performance Monitoring Examples
```csharp
// Use LumexUI to show performance metrics
public class PerformanceMonitorComponent : ComponentBase
{
    [Inject] public IDbConnectionFactory DbFactory { get; set; }

    private List<PerformanceMetric> metrics = new();

    protected override async Task OnInitializedAsync()
    {
        // Measure Dapper query performance
        var stopwatch = Stopwatch.StartNew();

        using var connection = DbFactory.CreateConnection();
        var result = await connection.QueryAsync<Product>("SELECT * FROM Products");

        stopwatch.Stop();

        metrics.Add(new PerformanceMetric
        {
            Operation = "Dapper Query",
            ExecutionTime = stopwatch.ElapsedMilliseconds,
            RecordCount = result.Count()
        });
    }
}
```

## ⚠️ Migration Guidelines

### From Entity Framework to Dapper
1. **Remove EF Core packages** and references
2. **Create repository classes** inheriting from BaseRepository
3. **Write explicit SQL** for complex queries
4. **Use parameterized queries** to prevent SQL injection
5. **Implement transactions** using Dapper transaction methods

### Example EF to Dapper Conversion

**Before (EF Core)**:
```csharp
public async Task<Product> GetProductWithCategoryAsync(int id)
{
    return await _context.Products
        .Include(p => p.Category)
        .FirstOrDefaultAsync(p => p.Id == id);
}
```

**After (Dapper)**:
```csharp
public async Task<Product> GetProductWithCategoryAsync(int id)
{
    var sql = @"
        SELECT p.*, c.*
        FROM Products p
        LEFT JOIN Categories c ON p.CategoryId = c.Id
        WHERE p.Id = @Id";

    var productDict = new Dictionary<int, Product>();

    await _connection.QueryAsync<Product, Category, Product>(
        sql,
        (product, category) =>
        {
            if (!productDict.TryGetValue(product.Id, out var productEntry))
            {
                productEntry = product;
                productEntry.Category = category;
                productDict.Add(productEntry.Id, productEntry);
            }

            return productEntry;
        },
        new { Id = id },
        splitOn: "Id"
    );

    return productDict.Values.FirstOrDefault();
}
```

## 🔍 Quality Assurance

### Testing Requirements
- **Unit Tests**: Test all repository methods with in-memory databases
- **Integration Tests**: Test Dapper queries against real databases
- **Performance Tests**: Validate query performance meets expectations
- **LumexUI Integration**: Test UI components work correctly with Dapper backends

### Code Review Checklist
- [ ] All database connections use proper connection factories
- [ ] SQL queries are parameterized to prevent injection
- [ ] Repository pattern is followed consistently
- [ ] Error handling is appropriate for database operations
- [ ] LumexUI components display loading states properly
- [ ] Background jobs have proper progress reporting

## 📚 Reference Implementation

### Complete Example: Product Management with LumexUI + Dapper

```csharp
// Models/Product.cs
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
    public int CategoryId { get; set; }
    public Category? Category { get; set; }
}

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
}

// Repositories/IProductRepository.cs
public interface IProductRepository : IBaseRepository<Product>
{
    Task<IEnumerable<Product>> GetWithCategoryAsync();
    Task<IEnumerable<Product>> GetByCategoryAsync(int categoryId);
}

// Repositories/ProductRepository.cs
public class ProductRepository : BaseRepository<Product>, IProductRepository
{
    public ProductRepository(IDbConnectionFactory connectionFactory)
        : base(connectionFactory.CreateConnection(), "Products")
    {
    }

    public async Task<IEnumerable<Product>> GetWithCategoryAsync()
    {
        var sql = @"
            SELECT p.*, c.*
            FROM Products p
            LEFT JOIN Categories c ON p.CategoryId = c.Id";

        return await _connection.QueryAsync<Product, Category, Product>(
            sql,
            (product, category) =>
            {
                product.Category = category;
                return product;
            },
            splitOn: "Id"
        );
    }

    public async Task<IEnumerable<Product>> GetByCategoryAsync(int categoryId)
    {
        var sql = "SELECT * FROM Products WHERE CategoryId = @CategoryId";
        return await _connection.QueryAsync<Product>(sql, new { CategoryId = categoryId });
    }
}
```

This comprehensive standard ensures that all AI agents working on this repository will create consistent, high-performance applications that demonstrate modern .NET 10 patterns while maintaining the educational focus on LumexUI component integration.