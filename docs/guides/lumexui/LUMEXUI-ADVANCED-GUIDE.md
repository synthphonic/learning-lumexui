# LumexUI Advanced Guide

## Overview

This comprehensive guide covers advanced LumexUI development topics including learning progression, component testing with bUnit, custom component development, performance optimization, and best practices. It serves as the definitive resource for mastering LumexUI beyond basic component usage.

For component examples and quick reference, see the [LUMEXUI-COMPREFERENCE.md](./LUMEXUI-COMPREFERENCE.md).

For installation and setup instructions, see the [LUMEXUI-SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md).

## Quick Navigation

### Guide Sections
- [🎓 Learning Path & Mastery](#-learning-path--mastery) - Structured learning progression from beginner to expert
- [🧪 Component Testing with bUnit](#-component-testing-with-bunit) - Comprehensive testing strategies and patterns
- [🔧 Custom Component Development](#-custom-component-development) - Building and extending LumexUI components
- [⚡ Performance & Optimization](#-performance--optimization) - Advanced performance techniques and best practices
- [🎨 Advanced Theming & Customization](#-advanced-theming--customization) - Deep customization strategies

### Quick Reference
- **Need component examples?** → [COMPREFERENCE.md](./LUMEXUI-COMPREFERENCE.md)
- **Need to install LumexUI?** → [SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md)
- **Looking for specific testing patterns?** → See [Component Testing section](#-component-testing-with-bunit)

---

## 🎓 Learning Path & Mastery

### LumexUI Learning Philosophy

**Design-First**: Beautiful UI/UX without requiring extensive design skills
**Tailwind-Powered**: Leverage modern CSS utility framework for consistent styling
**Performance-Optimized**: Minimal JavaScript, fast rendering, efficient state management
**Highly Customizable**: Extensive theming and styling options
**Responsive-Ready**: Mobile-first design principles built into every component

### Prerequisites for Advanced Development

Before diving into advanced topics, ensure you have:

- **Solid Blazor Foundation**: Understanding of components, lifecycle, and state management
- **C# Proficiency**: Modern C# features, async/await patterns, LINQ
- **Tailwind CSS Knowledge**: Utility-first CSS approach and responsive design
- **Component Architecture**: Understanding of component composition and reusability
- **Testing Fundamentals**: Basic unit testing concepts and patterns

### Structured Learning Progression

#### Level 1: Fundamentals Mastery (Weeks 1-4)
**Focus**: Core component usage and basic integration patterns

**Week 1-2: Core Component Mastery**
- Form components with validation
- Navigation and routing patterns
- Feedback and status components
- Basic layout and data display

**Week 3-4: Component Composition**
- Building composite components
- State management patterns
- Event handling and data flow
- Responsive design implementation

#### Level 2: Advanced Integration (Weeks 5-12)
**Focus**: Complex component interactions and enterprise patterns

**Week 5-8: Advanced Form Development**
- Dynamic and conditional forms
- Complex validation scenarios
- Multi-step forms and wizards
- File upload and media handling

**Week 9-12: Data-Driven Applications**
- Virtualization for large datasets
- Advanced table and grid patterns
- Real-time data synchronization
- Pagination and filtering strategies

#### Level 3: Architecture & Performance (Weeks 13-20)
**Focus**: Application architecture and optimization

**Week 13-16: Component Architecture**
- Custom component libraries
- Design system implementation
- Component testing strategies
- Documentation and maintenance

**Week 17-20: Performance Optimization**
- Bundle optimization strategies
- Memory management patterns
- Rendering performance tuning
- Accessibility compliance

#### Level 4: Expert Development (Weeks 21-28)
**Focus**: Advanced patterns and specialized applications

**Week 21-24: Enterprise Patterns**
- Multi-tenant applications
- Theming and branding systems
- Internationalization and localization
- Advanced state management

**Week 25-28: Specialized Applications**
- Dashboard and analytics interfaces
- Content management systems
- E-commerce patterns
- Real-time collaboration features

### Advanced Learning Projects

#### Progressive Project Complexity

**Project 1: Advanced Dashboard (Weeks 5-8)**
```razor
// Features to implement:
- Real-time data updates
- Interactive charts and visualizations
- User preference management
- Responsive layout optimization
- Advanced filtering and search

// Key Learning Objectives:
- Complex state management
- Component composition patterns
- Performance optimization
- User experience design
```

**Project 2: Dynamic Form Builder (Weeks 9-12)**
```razor
// Features to implement:
- Drag-and-drop form builder
- Dynamic field validation
- Conditional field logic
- Form template management
- Export/import functionality

// Key Learning Objectives:
- Advanced component composition
- Dynamic rendering patterns
- Complex validation logic
- Template-based design
```

**Project 3: Component Library (Weeks 13-16)**
```razor
// Features to implement:
- Custom component creation
- Comprehensive documentation
- Design token system
- Automated testing suite
- Package distribution

// Key Learning Objectives:
- Component architecture design
- Documentation best practices
- Testing strategies
- Package management
```

### Mastery Assessment Criteria

**Technical Proficiency**
- Component Composition: Can build complex components from smaller pieces
- State Management: Implements efficient data flow patterns
- Performance: Optimizes rendering and memory usage
- Accessibility: Ensures WCAG compliance
- Testing: Writes comprehensive test suites

**Design Excellence**
- Responsive Design: Mobile-first approach with consistent breakpoints
- Theming: Implements flexible, maintainable design systems
- UX Patterns: Applies established interaction patterns appropriately
- Visual Consistency: Maintains cohesive design language

**Professional Practices**
- Code Organization: Follows clean architecture principles
- Documentation: Provides clear, maintainable documentation
- Collaboration: Writes code that others can easily understand and modify
- Problem Solving: Approaches complex challenges with structured thinking

---

## 🧪 Component Testing with bUnit

### Testing Setup and Configuration

#### Required Packages

```bash
# Install bUnit for Blazor testing
dotnet add package bunit

# For Blazor Server/Interactive Server
dotnet add package bunit.web

# For Blazor WebAssembly/Interactive WebAssembly
dotnet add package bunit.webassembly

# For testing Microsoft.Extensions.DependencyInjection
dotnet add package Moq
dotnet add package Microsoft.Extensions.DependencyInjection
```

#### Test Project Structure

```
MyLumexUITests/
├── UnitTests/
│   ├── Components/
│   │   ├── ButtonTests.cs
│   │   ├── FormTests.cs
│   │   └── ModalTests.cs
│   └── Services/
│       └── UserServiceTests.cs
└── IntegrationTests/
    ├── ComponentInteractionTests.cs
    └── FormSubmissionTests.cs
```

### Basic Testing Patterns

#### Button Component Tests

```csharp
using Bunit;
using Xunit;
using LumexUI;

namespace MyLumexUITests.UnitTests.Components
{
    public class ButtonTests : TestContext
    {
        [Fact]
        public void Button_RendersCorrectly()
        {
            // Arrange & Act
            var cut = RenderComponent<LumexButton>(parameters => parameters
                .Add(p => p.Variant, Variant.Primary)
                .Add(p => p.ChildContent, "Click Me"));

            // Assert
            cut.MarkupMatches(@"<button[^>]*>Click Me</button>");
            Assert.Equal("Click Me", cut.Find("button").TextContent.Trim());
        }

        [Fact]
        public void Button_ClickEvent_TriggersCorrectly()
        {
            // Arrange
            var clicked = false;
            var cut = RenderComponent<LumexButton>(parameters => parameters
                .Add(p => p.Variant, Variant.Secondary)
                .Add(p => p.OnClick, () => clicked = true));

            // Act
            cut.Find("button").Click();

            // Assert
            Assert.True(clicked);
        }

        [Theory]
        [InlineData(Variant.Primary, "primary")]
        [InlineData(Variant.Secondary, "secondary")]
        [InlineData(Variant.Danger, "danger")]
        public void Button_Variant_AppliesCorrectClass(Variant variant, string expectedClass)
        {
            // Arrange & Act
            var cut = RenderComponent<LumexButton>(parameters => parameters
                .Add(p => p.Variant, variant)
                .Add(p => p.ChildContent, "Test"));

            // Assert
            cut.Find("button").ClassList.Contains(expectedClass);
        }

        [Fact]
        public void Button_LoadingState_DisablesCorrectly()
        {
            // Arrange & Act
            var cut = RenderComponent<LumexButton>(parameters => parameters
                .Add(p => p.Loading, true)
                .Add(p => p.ChildContent, "Loading Button"));

            // Assert
            var button = cut.Find("button");
            Assert.True(button.HasAttribute("disabled"));
            Assert.True(button.ClassList.Contains("loading"));
        }
    }
}
```

#### Input Component Tests

```csharp
using Bunit;
using Xunit;
using LumexUI;
using Microsoft.Extensions.DependencyInjection;

namespace MyLumexUITests.UnitTests.Components
{
    public class InputTests : TestContext
    {
        [Fact]
        public void Input_Binding_WorksCorrectly()
        {
            // Arrange
            var cut = RenderComponent<TextBoxTest>();

            // Act
            var inputElement = cut.Find("input");
            inputElement.Change("Hello World");

            // Assert
            Assert.Equal("Hello World", cut.Instance.Value);
        }

        [Fact]
        public void Input_Placeholder_DisplaysCorrectly()
        {
            // Arrange & Act
            var cut = RenderComponent<LumexInput>(parameters => parameters
                .Add(p => p.Placeholder, "Enter your name")
                .Add(p => p.Type, InputType.Text));

            // Assert
            var inputElement = cut.Find("input");
            Assert.Equal("Enter your name", inputElement.GetAttribute("placeholder"));
        }

        [Fact]
        public void Input_Validation_DisplaysErrorMessage()
        {
            // Arrange
            var cut = RenderComponent<ValidationTest>();

            // Act
            var inputElement = cut.Find("input");
            inputElement.Change(""); // Trigger validation

            // Simulate form submission to trigger validation
            cut.Find("form").Submit();

            // Assert
            cut.MarkupMatches(@"<div[^>]*class=""validation-message""[^>]*>Name is required</div>");
        }
    }

    // Helper component for testing
    public class TextBoxTest : ComponentBase
    {
        [Parameter]
        public string Value { get; set; } = "";

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenComponent<LumexInput>(0);
            builder.AddAttribute(1, "Value", Value);
            builder.AddAttribute(2, "ValueChanged", EventCallback.Factory<string>(this, value => Value = value));
            builder.CloseComponent();
        }
    }

    public class ValidationTest : ComponentBase
    {
        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "form");
            builder.OpenComponent<LumexInput>(1);
            builder.AddAttribute(2, "Type", InputType.Text);
            builder.AddAttribute(3, "Required", true);
            builder.CloseComponent();
            builder.CloseElement();
        }
    }
}
```

### Advanced Testing Scenarios

#### Modal Component Testing

```csharp
using Bunit;
using Xunit;
using LumexUI;
using System.Threading.Tasks;

namespace MyLumexUITests.UnitTests.Components
{
    public class ModalTests : TestContext
    {
        [Fact]
        public void Modal_IsOpen_ShowsContent()
        {
            // Arrange & Act
            var cut = RenderComponent<ModalTest>(parameters => parameters
                .Add(p => p.IsOpen, true));

            // Assert
            cut.MarkupMatches(@"<div[^>]*class=""modal[^""]*show[^""]*""[^>]*>");
            cut.Find(".modal-content");
        }

        [Fact]
        public void Modal_CloseButton_ClosesModal()
        {
            // Arrange
            var cut = RenderComponent<ModalTest>(parameters => parameters
                .Add(p => p.IsOpen, true));

            // Act
            var closeButton = cut.Find(".modal-close-button");
            closeButton.Click();

            // Assert
            Assert.False(cut.Instance.IsOpen);
            cut.MarkupDoesNotContain(".modal-content");
        }

        [Fact]
        public async Task Modal_OnClose_EventFires()
        {
            // Arrange
            var eventTriggered = false;
            var cut = RenderComponent<ModalTest>(parameters => parameters
                .Add(p => p.IsOpen, true)
                .Add(p => p.OnClose, () => eventTriggered = true));

            // Act
            var closeButton = cut.Find(".modal-close-button");
            await closeButton.ClickAsync(new MouseEventArgs());

            // Wait for async operation
            await Task.Delay(100);

            // Assert
            Assert.True(eventTriggered);
        }

        [Fact]
        public void Modal_Content_RendersCorrectlyWhenOpen()
        {
            // Arrange & Act
            var cut = RenderComponent<ModalTest>(parameters => parameters
                .Add(p => p.IsOpen, true)
                .Add(p => p.Title, "Test Modal")
                .Add(p => p.Content, "Test Content"));

            // Assert
            Assert.Contains("Test Modal", cut.Markup);
            Assert.Contains("Test Content", cut.Markup);
        }
    }

    public class ModalTest : ComponentBase
    {
        [Parameter]
        public bool IsOpen { get; set; } = false;

        [Parameter]
        public string Title { get; set; } = "Default Title";

        [Parameter]
        public string Content { get; set; } = "Default Content";

        [Parameter]
        public EventCallback OnClose { get; set; }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            if (IsOpen)
            {
                builder.OpenComponent<LumexModal>(0);
                builder.AddAttribute(1, "IsOpen", IsOpen);
                builder.AddAttribute(2, "OnClose", OnClose);
                builder.OpenComponent<LumexModalContent>(3);
                builder.OpenComponent<LumexModalHeader>(4);
                builder.OpenComponent<LumexModalTitle>(5);
                builder.AddContent(6, Title);
                builder.CloseComponent();
                builder.OpenComponent<LumexModalCloseButton>(6);
                builder.AddAttribute(7, "class", "modal-close-button");
                builder.CloseComponent();
                builder.CloseComponent();
                builder.OpenComponent<LumexModalBody>(7);
                builder.AddContent(8, Content);
                builder.CloseComponent();
                builder.CloseComponent();
                builder.CloseComponent();
            }
        }
    }
}
```

#### Complex Form Testing

```csharp
using Bunit;
using Xunit;
using LumexUI;
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

namespace MyLumexUITests.UnitTests.Components
{
    public class FormTests : TestContext
    {
        public FormTests()
        {
            // Configure services for validation
            Services.AddDataAnnotations();
        }

        [Fact]
        public void Form_ValidData_SubmitsSuccessfully()
        {
            // Arrange
            var formSubmitted = false;
            var cut = RenderComponent<UserRegistrationForm>(parameters => parameters
                .Add(p => p.OnValidSubmit, (user) => formSubmitted = true));

            // Act
            FillValidFormData(cut);
            cut.Find("form").Submit();

            // Assert
            Assert.True(formSubmitted);
        }

        [Fact]
        public void Form_InvalidData_ShowsValidationErrors()
        {
            // Arrange
            var cut = RenderComponent<UserRegistrationForm>();

            // Act
            cut.Find("#name").Change(""); // Required field empty
            cut.Find("#email").Change("invalid-email"); // Invalid email
            cut.Find("form").Submit();

            // Assert
            cut.MarkupMatches(@"<div[^>]*class=""validation-message""[^>]*>Name is required</div>");
            cut.MarkupMatches(@"<div[^>]*class=""validation-message""[^>]*>Invalid email format</div>");
        }

        [Fact]
        public void Form_LoadingState_DisablesSubmitButton()
        {
            // Arrange
            var cut = RenderComponent<UserRegistrationForm>(parameters => parameters
                .Add(p => p.IsSubmitting, true));

            // Act & Assert
            var submitButton = cut.Find("button[type='submit']");
            Assert.True(submitButton.HasAttribute("disabled"));
        }

        private void FillValidFormData(IRenderedComponent cut)
        {
            cut.Find("#name").Change("John Doe");
            cut.Find("#email").Change("john@example.com");
            cut.Find("#password").Change("password123");
            cut.Find("#confirmPassword").Change("password123");
        }
    }

    public class UserRegistrationForm : ComponentBase
    {
        private RegistrationModel model = new();
        private bool isSubmitting = false;

        [Parameter]
        public EventCallback<RegistrationModel> OnValidSubmit { get; set; }

        public bool IsSubmitting
        {
            get => isSubmitting;
            set => isSubmitting = value;
        }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "form");
            builder.OpenComponent<EditForm>(1);
            builder.AddAttribute(2, "Model", model);
            builder.AddAttribute(3, "OnValidSubmit", HandleValidSubmit);

            builder.OpenComponent<DataAnnotationsValidator>(4);
            builder.CloseComponent();

            // Name field
            builder.OpenElement(5, "div");
            builder.OpenComponent<LumexLabel>(6);
            builder.AddAttribute(7, "For", "name");
            builder.AddContent(8, "Name");
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(9);
            builder.AddAttribute(10, "id", "name");
            builder.AddAttribute(11, "@bind-Value", model.Name);
            builder.CloseComponent();
            builder.OpenComponent<ValidationMessage>(12);
            builder.AddAttribute(13, "For", () => model.Name);
            builder.CloseComponent();
            builder.CloseElement();

            // Email field
            builder.OpenElement(14, "div");
            builder.OpenComponent<LumexLabel>(15);
            builder.AddAttribute(16, "For", "email");
            builder.AddContent(17, "Email");
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(18);
            builder.AddAttribute(19, "id", "email");
            builder.AddAttribute(20, "Type", InputType.Email);
            builder.AddAttribute(21, "@bind-Value", model.Email);
            builder.CloseComponent();
            builder.OpenComponent<ValidationMessage>(22);
            builder.AddAttribute(23, "For", () => model.Email);
            builder.CloseComponent();
            builder.CloseElement();

            // Password field
            builder.OpenElement(24, "div");
            builder.OpenComponent<LumexLabel>(25);
            builder.AddAttribute(26, "For", "password");
            builder.AddContent(27, "Password");
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(27);
            builder.AddAttribute(28, "id", "password");
            builder.AddAttribute(29, "Type", InputType.Password);
            builder.AddAttribute(30, "@bind-Value", model.Password);
            builder.CloseComponent();
            builder.OpenComponent<ValidationMessage>(31);
            builder.AddAttribute(32, "For", () => model.Password);
            builder.CloseComponent();
            builder.CloseElement();

            // Confirm password field
            builder.OpenElement(33, "div");
            builder.OpenComponent<LumexLabel>(34);
            builder.AddAttribute(35, "For", "confirmPassword");
            builder.AddContent(36, "Confirm Password");
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(37);
            builder.AddAttribute(38, "id", "confirmPassword");
            builder.AddAttribute(39, "Type", InputType.Password);
            builder.AddAttribute(40, "@bind-Value", model.ConfirmPassword);
            builder.CloseComponent();
            builder.OpenComponent<ValidationMessage>(41);
            builder.AddAttribute(42, "For", () => model.ConfirmPassword);
            builder.CloseComponent();
            builder.CloseElement();

            // Submit button
            builder.OpenComponent<LumexButton>(42);
            builder.AddAttribute(43, "Type", ButtonType.Submit);
            builder.AddAttribute(44, "Variant", Variant.Primary);
            builder.AddAttribute(45, "Loading", isSubmitting);
            builder.AddAttribute(46, "Disabled", isSubmitting);
            builder.AddContent(47, isSubmitting ? "Submitting..." : "Register");
            builder.CloseComponent();

            builder.CloseComponent();
            builder.CloseElement();
        }

        private async Task HandleValidSubmit()
        {
            await OnValidSubmit.InvokeAsync(model);
        }
    }

    public class RegistrationModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; } = "";

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        public string Email { get; set; } = "";

        [Required(ErrorMessage = "Password is required")]
        [MinLength(6, ErrorMessage = "Password must be at least 6 characters")]
        public string Password { get; set; } = "";

        [Compare("Password", ErrorMessage = "Passwords do not match")]
        public string ConfirmPassword { get; set; } = "";
    }
}
```

### Mocking and Dependency Injection

#### Service Mocking Tests

```csharp
using Bunit;
using Xunit;
using LumexUI;
using Moq;
using Microsoft.Extensions.DependencyInjection;

namespace MyLumexUITests.UnitTests.Components
{
    public class ServiceMockingTests : TestContext
    {
        private Mock<IUserService> _userServiceMock;

        public ServiceMockingTests()
        {
            _userServiceMock = new Mock<IUserService>();
            Services.AddSingleton(_userServiceMock.Object);
        }

        [Fact]
        public void Component_WithService_CallsServiceCorrectly()
        {
            // Arrange
            var users = new[]
            {
                new User { Id = 1, Name = "John Doe", Email = "john@example.com" },
                new User { Id = 2, Name = "Jane Smith", Email = "jane@example.com" }
            };
            _userServiceMock.Setup(x => x.GetUsers()).ReturnsAsync(users);

            // Act
            var cut = RenderComponent<UserListComponent>();

            // Assert
            _userServiceMock.Verify(x => x.GetUsers(), Times.Once);
            cut.MarkupMatches(@"<div[^>]*class=""user-item""[^>]*>John Doe</div>");
            cut.MarkupMatches(@"<div[^>]*class=""user-item""[^>]*>Jane Smith</div>");
        }

        [Fact]
        public async Task Component_UserAction_CallsServiceMethod()
        {
            // Arrange
            var newUser = new User { Name = "New User", Email = "new@example.com" };
            _userServiceMock.Setup(x => x.AddUser(It.IsAny<User>())).ReturnsAsync(newUser);

            // Act
            var cut = RenderComponent<UserFormComponent>();
            await cut.Instance.AddUserAsync(newUser);

            // Assert
            _userServiceMock.Verify(x => x.AddUser(It.Is<User>(u => u.Name == "New User")), Times.Once);
        }

        [Fact]
        public void Component_ServiceError_HandlesGracefully()
        {
            // Arrange
            _userServiceMock.Setup(x => x.GetUsers())
                .ThrowsAsync(new Exception("Service unavailable"));

            // Act
            var cut = RenderComponent<UserListComponent>();

            // Assert
            cut.Find(".error-message");
            cut.Find(".retry-button");
        }
    }

    public interface IUserService
    {
        Task<User[]> GetUsers();
        Task<User> AddUser(User user);
        Task<bool> DeleteUser(int id);
    }

    public class User
    {
        public int Id { get; set; }
        public string Name { get; set; } = "";
        public string Email { get; set; } = "";
    }

    public class UserListComponent : ComponentBase
    {
        [Inject]
        private IUserService UserService { get; set; } = null!;

        private User[] users = Array.Empty<User>();
        private string errorMessage = "";
        private bool isLoading = true;

        protected override async Task OnInitializedAsync()
        {
            await LoadUsers();
        }

        private async Task LoadUsers()
        {
            try
            {
                isLoading = true;
                users = await UserService.GetUsers();
                errorMessage = "";
            }
            catch (Exception ex)
            {
                errorMessage = ex.Message;
                users = Array.Empty<User>();
            }
            finally
            {
                isLoading = false;
            }
        }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            if (isLoading)
            {
                builder.OpenComponent<LumexSpinner>(0);
                builder.CloseComponent();
                return;
            }

            if (!string.IsNullOrEmpty(errorMessage))
            {
                builder.OpenElement(1, "div");
                builder.AddAttribute(2, "class", "error-message");
                builder.AddContent(3, $"Error: {errorMessage}");
                builder.OpenComponent<LumexButton>(4);
                builder.AddAttribute(5, "class", "retry-button");
                builder.AddAttribute(6, "OnClick", EventCallback.Factory.Create(this, LoadUsers));
                builder.AddContent(7, "Retry");
                builder.CloseComponent();
                builder.CloseElement();
                return;
            }

            builder.OpenElement(8, "div");
            builder.AddAttribute(9, "class", "user-list");

            foreach (var user in users)
            {
                builder.OpenElement(10, "div");
                builder.AddAttribute(11, "class", "user-item");
                builder.AddContent(12, user.Name);
                builder.CloseElement();
            }

            builder.CloseElement();
        }
    }

    public class UserFormComponent : ComponentBase
    {
        [Inject]
        private IUserService UserService { get; set; } = null!;

        public async Task<User> AddUserAsync(User user)
        {
            return await UserService.AddUser(user);
        }
    }
}
```

### Integration Testing

#### Complex Component Integration Tests

```csharp
using Bunit;
using Xunit;
using LumexUI;
using System.Threading.Tasks;

namespace MyLumexUITests.IntegrationTests
{
    public class ComponentInteractionTests : TestContext
    {
        [Fact]
        public void MultiStepForm_WorksCorrectly()
        {
            // Arrange
            var cut = RenderComponent<MultiStepForm>();

            // Act - Step 1: Personal Information
            Assert.True(cut.Instance.CurrentStep == 1);
            cut.Find("#firstName").Change("John");
            cut.Find("#lastName").Change("Doe");
            cut.Find(".next-button").Click();

            // Assert - Step 2
            Assert.True(cut.Instance.CurrentStep == 2);
            cut.Find("#email").Change("john@example.com");
            cut.Find("#phone").Change("555-123-4567");
            cut.Find(".next-button").Click();

            // Assert - Step 3
            Assert.True(cut.Instance.CurrentStep == 3);
            cut.Find("#address").Change("123 Main St");
            cut.Find(".submit-button").Click();

            // Assert - Form completed
            Assert.True(cut.Instance.IsCompleted);
            Assert.Equal("John Doe", cut.Instance.FormData.FirstName);
            Assert.Equal("john@example.com", cut.Instance.FormData.Email);
        }

        [Fact]
        public void ModalWithForm_SubmissionWorksCorrectly()
        {
            // Arrange
            var cut = RenderComponent<ModalWithForm>();

            // Act
            cut.Find(".open-modal-button").Click();
            cut.Find("#modalInput").Change("Test Value");
            cut.Find(".submit-button").Click();

            // Assert
            Assert.Equal("Test Value", cut.Instance.SubmittedValue);
            Assert.False(cut.Instance.IsModalOpen);
        }

        [Fact]
        public async Task DataTableWithPagination_HandlesLargeDataSets()
        {
            // Arrange
            var largeDataSet = GenerateLargeDataSet(1000);
            var cut = RenderComponent<DataTableWithPagination>(parameters => parameters
                .Add(p => p.Data, largeDataSet));

            // Act
            // Test pagination controls
            cut.Find(".next-page-button").Click();
            cut.Find(".previous-page-button").Click();

            // Test page size changes
            cut.Find(".page-size-select").Change("50");

            // Test filtering
            cut.Find(".search-input").Change("test");

            // Assert
            var tableRows = cut.FindAll("table tbody tr");
            Assert.True(tableRows.Count <= 50); // Should respect page size
        }

        private object[] GenerateLargeDataSet(int count)
        {
            var data = new object[count];
            for (int i = 0; i < count; i++)
            {
                data[i] = new { Id = i, Name = $"Item {i}", Value = $"Value {i}" };
            }
            return data;
        }
    }

    public class MultiStepForm : ComponentBase
    {
        private int currentStep = 1;
        private readonly int totalSteps = 3;
        private FormData formData = new();
        private bool isCompleted = false;

        public int CurrentStep => currentStep;
        public FormData FormData => formData;
        public bool IsCompleted => isCompleted;

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", "multi-step-form");

            // Progress indicator
            for (int i = 1; i <= totalSteps; i++)
            {
                builder.OpenElement(2, "div");
                builder.AddAttribute(3, "class", $"step {(i <= currentStep ? "active" : "")}");
                builder.AddContent(4, $"Step {i}");
                builder.CloseElement();
            }

            // Form content based on current step
            if (currentStep == 1)
            {
                RenderPersonalInfoStep(builder);
            }
            else if (currentStep == 2)
            {
                RenderContactStep(builder);
            }
            else if (currentStep == 3)
            {
                RenderAddressStep(builder);
            }

            builder.CloseElement();
        }

        private void RenderPersonalInfoStep(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", "form-step");
            builder.OpenComponent<LumexInput>(2);
            builder.AddAttribute(3, "id", "firstName");
            builder.AddAttribute(4, "Placeholder", "First Name");
            builder.AddAttribute(5, "@bind-Value", formData.FirstName);
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(6);
            builder.AddAttribute(7, "id", "lastName");
            builder.AddAttribute(8, "Placeholder", "Last Name");
            builder.AddAttribute(9, "@bind-Value", formData.LastName);
            builder.CloseComponent();
            builder.OpenComponent<LumexButton>(10);
            builder.AddAttribute(11, "class", "next-button");
            builder.AddAttribute(12, "OnClick", () => currentStep = 2);
            builder.AddContent(13, "Next");
            builder.CloseComponent();
            builder.CloseElement();
        }

        private void RenderContactStep(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", "form-step");
            builder.OpenComponent<LumexInput>(2);
            builder.AddAttribute(3, "id", "email");
            builder.AddAttribute(4, "Placeholder", "Email");
            builder.AddAttribute(5, "@bind-Value", formData.Email);
            builder.CloseComponent();
            builder.OpenComponent<LumexInput>(6);
            builder.AddAttribute(7, "id", "phone");
            builder.AddAttribute(8, "Placeholder", "Phone");
            builder.AddAttribute(9, "@bind-Value", formData.Phone);
            builder.CloseComponent();
            builder.OpenComponent<LumexButton>(10);
            builder.AddAttribute(11, "class", "previous-button");
            builder.AddAttribute(12, "OnClick", () => currentStep = 1);
            builder.AddContent(13, "Previous");
            builder.CloseComponent();
            builder.OpenComponent<LumexButton>(14);
            builder.AddAttribute(15, "class", "next-button");
            builder.AddAttribute(16, "OnClick", () => currentStep = 3);
            builder.AddContent(17, "Next");
            builder.CloseComponent();
            builder.CloseElement();
        }

        private void RenderAddressStep(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", "form-step");
            builder.OpenComponent<LumexInput>(2);
            builder.AddAttribute(3, "id", "address");
            builder.AddAttribute(4, "Placeholder", "Address");
            builder.AddAttribute(5, "@bind-Value", formData.Address);
            builder.CloseComponent();
            builder.OpenComponent<LumexButton>(16);
            builder.AddAttribute(17, "class", "previous-button");
            builder.AddAttribute(18, "OnClick", () => currentStep = 2);
            builder.AddContent(19, "Previous");
            builder.CloseComponent();
            builder.OpenComponent<LumexButton>(10);
            builder.AddAttribute(11, "class", "submit-button");
            builder.AddAttribute(12, "OnClick", SubmitForm);
            builder.AddContent(13, "Submit");
            builder.CloseComponent();
            builder.CloseElement();
        }

        private void SubmitForm()
        {
            isCompleted = true;
            StateHasChanged();
        }
    }

    public class FormData
    {
        public string FirstName { get; set; } = "";
        public string LastName { get; set; } = "";
        public string Email { get; set; } = "";
        public string Phone { get; set; } = "";
        public string Address { get; set; } = "";
    }

    public class ModalWithForm : ComponentBase
    {
        private bool isModalOpen = false;
        private string inputValue = "";
        private string submittedValue = "";

        public bool IsModalOpen => isModalOpen;
        public string SubmittedValue => submittedValue;

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenComponent<LumexButton>(0);
            builder.AddAttribute(1, "class", "open-modal-button");
            builder.AddAttribute(2, "OnClick", () => isModalOpen = true);
            builder.AddContent(3, "Open Modal");
            builder.CloseComponent();

            if (isModalOpen)
            {
                builder.OpenComponent<LumexModal>(4);
                builder.AddAttribute(5, "IsOpen", isModalOpen);
                builder.OpenComponent<LumexModalContent>(6);

                builder.OpenComponent<LumexInput>(7);
                builder.AddAttribute(8, "id", "modalInput");
                builder.AddAttribute(9, "Placeholder", "Enter value");
                builder.AddAttribute(10, "@bind-Value", inputValue);
                builder.CloseComponent();

                builder.OpenComponent<LumexButton>(11);
                builder.AddAttribute(12, "class", "submit-button");
                builder.AddAttribute(13, "OnClick", () => { submittedValue = inputValue; isModalOpen = false; });
                builder.AddContent(14, "Submit");
                builder.CloseComponent();

                builder.CloseComponent();
                builder.CloseComponent();
            }
        }
    }

    public class DataTableWithPagination : ComponentBase
    {
        [Parameter]
        public object[] Data { get; set; } = Array.Empty<object>();

        private int currentPage = 1;
        private int pageSize = 10;
        private string searchTerm = "";
    }
}
```

### Accessibility Testing

```csharp
using Bunit;
using Xunit;
using LumexUI;

namespace MyLumexUITests.UnitTests.Components
{
    public class AccessibilityTests : TestContext
    {
        [Fact]
        public void Button_HasCorrectARIALabels()
        {
            // Arrange & Act
            var cut = RenderComponent<LumexButton>(parameters => parameters
                .Add(p => p.AriaLabel, "Submit Form")
                .Add(p => p.AriaDescribedBy, "submit-help")
                .Add(p => p.ChildContent, "Submit"));

            // Assert
            var button = cut.Find("button");
            Assert.Equal("Submit Form", button.GetAttribute("aria-label"));
            Assert.Equal("submit-help", button.GetAttribute("aria-describedby"));
        }

        [Fact]
        public void Input_HasCorrectAccessibilityAttributes()
        {
            // Arrange & Act
            var cut = RenderComponent<LumexInput>(parameters => parameters
                .Add(p => p.Id, "username")
                .Add(p => p.AriaRequired, true)
                .Add(p => p.AriaInvalid, false));

            // Assert
            var input = cut.Find("input");
            Assert.Equal("username", input.GetAttribute("id"));
            Assert.Equal("true", input.GetAttribute("aria-required"));
            Assert.Equal("false", input.GetAttribute("aria-invalid"));
        }

        [Fact]
        public void Modal_HasCorrectAccessibilityRoles()
        {
            // Arrange & Act
            var cut = RenderComponent<AccessibleModal>(parameters => parameters
                .Add(p => p.IsOpen, true));

            // Assert
            var modal = cut.Find("[role='dialog']");
            Assert.NotNull(modal);

            var modalTitle = cut.Find("[aria-labelledby]");
            Assert.NotNull(modalTitle);
        }

        [Fact]
        public void Form_KeyboardNavigation_WorksCorrectly()
        {
            // Arrange
            var cut = RenderComponent<AccessibleForm>();

            // Act & Assert - Test tab order
            var inputs = cut.FindAll("input, button, select, textarea");

            // Verify all form elements are focusable
            foreach (var element in inputs)
            {
                Assert.True(element.HasAttribute("tabindex") ||
                         element.TagName.Equals("BUTTON") ||
                         element.TagName.Equals("INPUT") ||
                         element.TagName.Equals("SELECT") ||
                         element.TagName.Equals("TEXTAREA"));
            }
        }
    }

    public class AccessibleModal : ComponentBase
    {
        [Parameter]
        public bool IsOpen { get; set; }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            if (IsOpen)
            {
                builder.OpenComponent<LumexModal>(0);
                builder.AddAttribute(1, "IsOpen", IsOpen);
                builder.OpenComponent<LumexModalContent>(2);
                builder.OpenComponent<LumexModalHeader>(3);
                builder.OpenComponent<LumexModalTitle>(4);
                builder.AddAttribute(5, "id", "modal-title");
                builder.AddContent(6, "Modal Title");
                builder.CloseComponent();
                builder.CloseComponent();
                builder.CloseComponent();
                builder.CloseComponent();
            }
        }
    }

    public class AccessibleForm : ComponentBase
    {
        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "form");

            // Accessible form field with proper labeling
            builder.OpenElement(1, "label");
            builder.AddAttribute(2, "for", "accessible-input");
            builder.AddContent(3, "Accessible Input");
            builder.CloseElement();

            builder.OpenElement(4, "input");
            builder.AddAttribute(5, "id", "accessible-input");
            builder.AddAttribute(6, "type", "text");
            builder.AddAttribute(7, "aria-required", "true");
            builder.CloseElement();

            // Accessible button
            builder.OpenElement(8, "button");
            builder.AddAttribute(9, "type", "submit");
            builder.AddAttribute(10, "aria-label", "Submit accessible form");
            builder.AddContent(11, "Submit");
            builder.CloseElement();

            builder.CloseElement();
        }
    }
}
```

---

## 🔧 Custom Component Development

### Understanding LumexUI Architecture

#### Component Base Classes

LumexUI components inherit from specific base classes that provide core functionality:

```csharp
// Basic LumexUI component structure
public class LumexCustomButton : LumexComponentBase
{
    protected override void BuildRenderTree(RenderTreeBuilder builder)
    {
        // Component rendering logic
    }
}

// Component with parameter handling
public class LumexAdvancedCard : LumexComponentBase
{
    [Parameter]
    public Variant? Variant { get; set; }

    [Parameter]
    public Size? Size { get; set; }

    [Parameter]
    public RenderFragment? ChildContent { get; set; }

    protected override void BuildRenderTree(RenderTreeBuilder builder)
    {
        // Enhanced component with parameters
    }
}
```

#### Theming Integration

LumexUI uses CSS custom properties for theming:

```csharp
// Access theme values in components
protected string GetThemeColor(Variant variant)
{
    return variant switch
    {
        Variant.Primary => "var(--lumex-primary)",
        Variant.Secondary => "var(--lumex-secondary)",
        Variant.Success => "var(--lumex-success)",
        Variant.Danger => "var(--lumex-danger)",
        _ => "var(--lumex-default)"
    };
}

// Apply theme classes
protected void ApplyThemeClasses(RenderTreeBuilder builder, string baseClasses)
{
    builder.AddAttribute(0, "class", baseClasses);
}
```

#### CSS Variable Usage

LumexUI exposes CSS variables that you can use in custom components:

```css
/* Available LumexUI CSS Variables */
:root {
    --lumex-background: 0 0% 100%;
    --lumex-foreground: 222.2 84% 4.9%;
    --lumex-primary: 221.2 83.2% 53.3%;
    --lumex-primary-foreground: 210 40% 98%;
    --lumex-secondary: 210 40% 96.1%;
    --lumex-secondary-foreground: 222.2 84% 4.9%;
    --lumex-success: 142.1 76.2% 36.3%;
    --lumex-success-foreground: 355.7 100% 94.1%;
    --lumex-danger: 0 84.2% 60.2%;
    --lumex-danger-foreground: 210 40% 98%;
    /* ... many more */
}
```

### Basic Custom Component Development

#### Creating a Simple Custom Button

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;

namespace MyLumexUI.Components
{
    public class CustomButton : LumexComponentBase
    {
        [Parameter]
        public Variant Variant { get; set; } = Variant.Default;

        [Parameter]
        public Size Size { get; set; } = Size.Medium;

        [Parameter]
        public bool IsDisabled { get; set; }

        [Parameter]
        public EventCallback OnClick { get; set; }

        [Parameter]
        public RenderFragment? ChildContent { get; set; }

        private string GetButtonClasses()
        {
            var classes = new List<string>
            {
                "custom-button",
                $"variant-{Variant.ToString().ToLower()}",
                $"size-{Size.ToString().ToLower()}"
            };

            if (IsDisabled)
            {
                classes.Add("disabled");
            }

            return string.Join(" ", classes);
        }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            var cssClasses = GetButtonClasses();

            builder.OpenElement(0, "button");
            builder.AddAttribute(1, "class", cssClasses);
            builder.AddAttribute(2, "disabled", IsDisabled);
            builder.AddAttribute(3, "onclick", EventCallback.Factory.Create(this, HandleClick));

            if (ChildContent != null)
            {
                builder.AddContent(4, ChildContent);
            }
            else
            {
                builder.AddContent(4, "Click me");
            }

            builder.CloseElement();
        }

        private async Task HandleClick()
        {
            if (!IsDisabled)
            {
                await OnClick.InvokeAsync();
            }
        }
    }

    public enum Variant
    {
        Default,
        Primary,
        Secondary,
        Success,
        Danger,
        Warning,
        Info
    }

    public enum Size
    {
        Small,
        Medium,
        Large
    }
}
```

#### CSS Styling for Custom Button

```css
/* CustomButton.css */
.custom-button {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border-radius: 0.375rem;
    font-weight: 500;
    transition: all 0.2s ease-in-out;
    cursor: pointer;
    padding: 0.5rem 1rem;
    border: 1px solid transparent;
    background-color: hsl(var(--lumex-background));
    color: hsl(var(--lumex-foreground));
}

.custom-button:hover:not(.disabled) {
    transform: translateY(-1px);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.custom-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    transform: none;
}

/* Variants */
.custom-button.variant-primary {
    background-color: hsl(var(--lumex-primary));
    color: hsl(var(--lumex-primary-foreground));
    border-color: hsl(var(--lumex-primary));
}

.custom-button.variant-secondary {
    background-color: hsl(var(--lumex-secondary));
    color: hsl(var(--lumex-secondary-foreground));
    border-color: hsl(var(--lumex-secondary));
}

.custom-button.variant-success {
    background-color: hsl(var(--lumex-success));
    color: hsl(var(--lumex-success-foreground));
    border-color: hsl(var(--lumex-success));
}

.custom-button.variant-danger {
    background-color: hsl(var(--lumex-danger));
    color: hsl(var(--lumex-danger-foreground));
    border-color: hsl(var(--lumex-danger));
}

/* Sizes */
.custom-button.size-small {
    padding: 0.375rem 0.75rem;
    font-size: 0.875rem;
}

.custom-button.size-medium {
    padding: 0.5rem 1rem;
    font-size: 1rem;
}

.custom-button.size-large {
    padding: 0.75rem 1.5rem;
    font-size: 1.125rem;
}
```

#### Usage Example

```razor
@using MyLumexUI.Components

<CustomButton Variant="Variant.Primary" Size="Size.Large" OnClick="HandleClick">
    Custom Button
</CustomButton>

@code {
    private void HandleClick()
    {
        Console.WriteLine("Custom button clicked!");
    }
}
```

### Building Composite Components

#### Creating a Custom Card Component

```csharp
using Microsoft.AspNetCore.Components;

namespace MyLumexUI.Components
{
    public class CustomCard : LumexComponentBase
    {
        [Parameter]
        public string Title { get; set; } = "";

        [Parameter]
        public string Description { get; set; } = "";

        [Parameter]
        public Variant Variant { get; set; } = Variant.Default;

        [Parameter]
        public RenderFragment? HeaderContent { get; set; }

        [Parameter]
        public RenderFragment? BodyContent { get; set; }

        [Parameter]
        public RenderFragment? FooterContent { get; set; }

        [Parameter]
        public bool ShowHeader { get; set; } = true;

        [Parameter]
        public bool ShowFooter { get; set; } = false;

        private string GetCardClasses()
        {
            var classes = new List<string>
            {
                "custom-card",
                $"variant-{Variant.ToString().ToLower()}"
            };

            return string.Join(" ", classes);
        }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            var cardClasses = GetCardClasses();

            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", cardClasses);

            // Header
            if (ShowHeader && (HeaderContent != null || !string.IsNullOrEmpty(Title)))
            {
                builder.OpenElement(2, "div");
                builder.AddAttribute(3, "class", "custom-card-header");

                if (HeaderContent != null)
                {
                    builder.AddContent(4, HeaderContent);
                }
                else
                {
                    builder.OpenElement(5, "h3");
                    builder.AddAttribute(6, "class", "custom-card-title");
                    builder.AddContent(7, Title);
                    builder.CloseElement();
                }

                if (!string.IsNullOrEmpty(Description))
                {
                    builder.OpenElement(8, "p");
                    builder.AddAttribute(9, "class", "custom-card-description");
                    builder.AddContent(10, Description);
                    builder.CloseElement();
                }

                builder.CloseElement();
            }

            // Body
            if (BodyContent != null)
            {
                builder.OpenElement(11, "div");
                builder.AddAttribute(12, "class", "custom-card-body");
                builder.AddContent(13, BodyContent);
                builder.CloseElement();
            }

            // Footer
            if (ShowFooter && FooterContent != null)
            {
                builder.OpenElement(14, "div");
                builder.AddAttribute(15, "class", "custom-card-footer");
                builder.AddContent(16, FooterContent);
                builder.CloseElement();
            }

            builder.CloseElement();
        }
    }
}
```

#### CSS for Custom Card

```css
/* CustomCard.css */
.custom-card {
    background-color: hsl(var(--lumex-background));
    border: 1px solid hsl(var(--lumex-border));
    border-radius: 0.5rem;
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
    overflow: hidden;
    transition: box-shadow 0.2s ease-in-out;
}

.custom-card:hover {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

.custom-card-header {
    padding: 1rem 1.5rem 0;
    border-bottom: 1px solid hsl(var(--lumex-border));
}

.custom-card-title {
    font-size: 1.25rem;
    font-weight: 600;
    color: hsl(var(--lumex-foreground));
    margin: 0;
}

.custom-card-description {
    font-size: 0.875rem;
    color: hsl(var(--lumex-muted-foreground));
    margin: 0.5rem 0 0 0;
}

.custom-card-body {
    padding: 1.5rem;
}

.custom-card-footer {
    padding: 0 1.5rem 1rem;
    border-top: 1px solid hsl(var(--lumex-border));
}

/* Variants */
.custom-card.variant-primary {
    border-color: hsl(var(--lumex-primary));
}

.custom-card.variant-primary .custom-card-header {
    background-color: hsl(var(--lumex-primary));
    color: hsl(var(--lumex-primary-foreground));
}

.custom-card.variant-primary .custom-card-title {
    color: hsl(var(--lumex-primary-foreground));
}
```

#### Usage Example

```razor
<CustomCard Variant="Variant.Primary" Title="Welcome"
           Description="This is a custom card component">
    <HeaderContent>
        <LumexIcon Icon="Heart" />
    </HeaderContent>
    <BodyContent>
        <p>Card content goes here.</p>
    </BodyContent>
    <FooterContent>
        <CustomButton Size="Size.Small">Action</CustomButton>
    </FooterContent>
</CustomCard>
```

### Extending Existing Components

#### Creating an Enhanced Input Component

```csharp
using Microsoft.AspNetCore.Components;
using LumexUI;

namespace MyLumexUI.Components
{
    public class EnhancedInput : LumexComponentBase
    {
        [Parameter]
        public string Value { get; set; } = "";

        [Parameter]
        public EventCallback<string> ValueChanged { get; set; }

        [Parameter]
        public string Label { get; set; } = "";

        [Parameter]
        public string Placeholder { get; set; } = "";

        [Parameter]
        public InputType Type { get; set; } = InputType.Text;

        [Parameter]
        public bool HasError { get; set; }

        [Parameter]
        public string ErrorMessage { get; set; } = "";

        [Parameter]
        public bool ShowPasswordToggle { get; set; }

        [Parameter]
        public bool ShowClearButton { get; set; } = true;

        [Parameter]
        public RenderFragment? LeftIcon { get; set; }

        [Parameter]
        public RenderFragment? RightIcon { get; set; }

        private bool showPassword = false;
        private bool hasValue => !string.IsNullOrEmpty(Value);

        private void TogglePassword()
        {
            showPassword = !showPassword;
        }

        private void ClearValue()
        {
            Value = "";
            ValueChanged.InvokeAsync(Value);
        }

        private async Task HandleValueChanged(ChangeEventArgs e)
        {
            Value = e.Value?.ToString() ?? "";
            await ValueChanged.InvokeAsync(Value);
        }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", "enhanced-input-container");

            // Label
            if (!string.IsNullOrEmpty(Label))
            {
                builder.OpenElement(2, "label");
                builder.AddAttribute(3, "class", "enhanced-input-label");
                builder.AddContent(4, Label);
                builder.CloseElement();
            }

            builder.OpenElement(5, "div");
            builder.AddAttribute(6, "class", GetInputContainerClasses());

            // Left icon
            if (LeftIcon != null)
            {
                builder.OpenElement(7, "div");
                builder.AddAttribute(8, "class", "input-left-icon");
                builder.AddContent(9, LeftIcon);
                builder.CloseElement();
            }

            // Input
            builder.OpenElement(10, "input");
            builder.AddAttribute(11, "type", GetInputType());
            builder.AddAttribute(12, "class", GetInputClasses());
            builder.AddAttribute(13, "placeholder", Placeholder);
            builder.AddAttribute(14, "value", Value);
            builder.AddAttribute(15, "onchange", EventCallback.Factory.Create<ChangeEventArgs>(this, HandleValueChanged));
            builder.CloseElement();

            // Right side content
            if (RightIcon != null || (ShowPasswordToggle && Type == InputType.Password) || (ShowClearButton && hasValue))
            {
                builder.OpenElement(16, "div");
                builder.AddAttribute(17, "class", "input-right-content");

                // Clear button
                if (ShowClearButton && hasValue)
                {
                    builder.OpenComponent<LumexButton>(18);
                    builder.AddAttribute(19, "class", "clear-button");
                    builder.AddAttribute(20, "Variant", Variant.Ghost);
                    builder.AddAttribute(21, "Size", Size.Small);
                    builder.AddAttribute(22, "OnClick", EventCallback.Factory.Create(this, (MouseEventArgs e) => ClearValue()));
                    builder.OpenComponent<LumexIcon>(23);
                    builder.AddAttribute(24, "Icon", IconName.X);
                    builder.CloseComponent();
                    builder.CloseComponent();
                }

                // Password toggle
                if (ShowPasswordToggle && Type == InputType.Password)
                {
                    builder.OpenComponent<LumexButton>(25);
                    builder.AddAttribute(26, "class", "password-toggle");
                    builder.AddAttribute(27, "Variant", Variant.Ghost);
                    builder.AddAttribute(28, "Size", Size.Small);
                    builder.AddAttribute(29, "OnClick", EventCallback.Factory.Create(this, (MouseEventArgs e) => TogglePassword()));
                    builder.OpenComponent<LumexIcon>(30);
                    builder.AddAttribute(31, "Icon", showPassword ? IconName.Eye : IconName.EyeOff);
                    builder.CloseComponent();
                    builder.CloseComponent();
                }

                // Right icon
                if (RightIcon != null)
                {
                    builder.OpenElement(32, "div");
                    builder.AddAttribute(33, "class", "input-right-icon");
                    builder.AddContent(34, RightIcon);
                    builder.CloseElement();
                }

                builder.CloseElement();
            }

            builder.CloseElement();

            // Error message
            if (HasError && !string.IsNullOrEmpty(ErrorMessage))
            {
                builder.OpenElement(35, "div");
                builder.AddAttribute(36, "class", "enhanced-input-error");
                builder.AddContent(37, ErrorMessage);
                builder.CloseElement();
            }

            builder.CloseElement();
        }

        private string GetInputContainerClasses()
        {
            var classes = new List<string>
            {
                "input-container"
            };

            if (HasError)
            {
                classes.Add("error");
            }

            if (LeftIcon != null)
            {
                classes.Add("has-left-icon");
            }

            if (RightIcon != null || ShowPasswordToggle || ShowClearButton)
            {
                classes.Add("has-right-content");
            }

            return string.Join(" ", classes);
        }

        private string GetInputClasses()
        {
            var classes = new List<string>
            {
                "enhanced-input"
            };

            if (HasError)
            {
                classes.Add("error");
            }

            return string.Join(" ", classes);
        }

        private string GetInputType()
        {
            if (Type == InputType.Password && !showPassword)
            {
                return "password";
            }

            return Type switch
            {
                InputType.Email => "email",
                InputType.Number => "number",
                InputType.Tel => "tel",
                InputType.Url => "url",
                InputType.Text => "text",
                _ => "text"
            };
        }
    }
}
```

### Advanced Customization

#### Creating Custom Design Tokens

```csharp
using Microsoft.AspNetCore.Components;

namespace MyLumexUI.Components
{
    public class ThemeProvider : LumexComponentBase
    {
        [Parameter]
        public ThemeMode Mode { get; set; } = ThemeMode.System;

        [Parameter]
        public RenderFragment? ChildContent { get; set; }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            var themeClasses = GetThemeClasses();

            builder.OpenElement(0, "div");
            builder.AddAttribute(1, "class", themeClasses);

            builder.AddContent(2, ChildContent);

            builder.CloseElement();
        }

        private string GetThemeClasses()
        {
            return Mode switch
            {
                ThemeMode.Light => "theme-light",
                ThemeMode.Dark => "theme-dark",
                ThemeMode.System => "theme-system",
                _ => "theme-system"
            };
        }
    }

    public enum ThemeMode
    {
        Light,
        Dark,
        System
    }
}
```

#### Custom Theme Variables

```css
/* Custom theme extensions */
:root {
    /* Custom brand colors */
    --lumex-brand-primary: 210 40% 98%;
    --lumex-brand-secondary: 217.2 91.2% 59.8%;
    --lumex-brand-accent: 278.6 100% 69.8%;

    /* Custom neutral colors */
    --lumex-neutral-50: 210 40% 98%;
    --lumex-neutral-100: 210 40% 96.1%;
    --lumex-neutral-200: 213 31% 97%;
    --lumex-neutral-300: 213 31% 91%;
    --lumex-neutral-400: 215 20.2% 65.1%;
    --lumex-neutral-500: 217.9 10.4% 34.9%;
    --lumex-neutral-600: 215.4 16.3% 15.3%;
    --lumex-neutral-700: 215.4 15.3% 7.1%;
    --lumex-neutral-800: 217.9 10.2% 3.1%;
    --lumex-neutral-900: 222.2 84.4% 4.9%;

    /* Custom spacing */
    --lumex-spacing-xs: 0.25rem;
    --lumex-spacing-sm: 0.5rem;
    --lumex-spacing-md: 1rem;
    --lumex-spacing-lg: 1.5rem;
    --lumex-spacing-xl: 2rem;
    --lumex-spacing-2xl: 3rem;

    /* Custom border radius */
    --lumex-radius-sm: 0.25rem;
    --lumex-radius-md: 0.375rem;
    --lumex-radius-lg: 0.5rem;
    --lumex-radius-xl: 0.75rem;
    --lumex-radius-full: 9999px;
}

.theme-light {
    /* Light mode overrides */
    --lumex-background: 0 0% 100%;
    --lumex-foreground: 222.2 84% 4.9%;
}

.theme-dark {
    /* Dark mode overrides */
    --lumex-background: 222.2 84% 4.9%;
    --lumex-foreground: 210 40% 98%;
}
```

#### Component Library Creation

```csharp
// ComponentLibrary.cs
namespace MyLumexUI.Components
{
    public static class ComponentLibrary
    {
        public static RenderFragment Alert(string message, Variant variant = Variant.Info)
        {
            return builder =>
            {
                builder.OpenComponent<CustomAlert>(0);
                builder.AddAttribute(1, "Message", message);
                builder.AddAttribute(2, "Variant", variant);
                builder.CloseComponent();
            };
        }

        public static RenderFragment Badge(string text, Variant variant = Variant.Primary)
        {
            return builder =>
            {
                builder.OpenComponent<CustomBadge>(0);
                builder.AddAttribute(1, "Text", text);
                builder.AddAttribute(2, "Variant", variant);
                builder.CloseComponent();
            };
        }

        public static RenderFragment Button(string text,
            Variant variant = Variant.Default,
            Size size = Size.Medium,
            EventCallback? onClick = null)
        {
            return builder =>
            {
                builder.OpenComponent<CustomButton>(0);
                builder.AddAttribute(1, "ChildContent", text);
                builder.AddAttribute(2, "Variant", variant);
                builder.AddAttribute(3, "Size", size);
                if (onClick != null)
                {
                    builder.AddAttribute(4, "OnClick", onClick);
                }
                builder.CloseComponent();
            };
        }
    }
}
```

#### Usage of Component Library

```razor
@using MyLumexUI.Components

@{
    ComponentLibrary.Alert("Success message!", Variant.Success)
    ComponentLibrary.Badge("New", Variant.Danger)
    ComponentLibrary.Button("Click me", Variant.Primary, Size.Large, HandleClick)
}
```

### Publishing & Distribution

#### Creating a NuGet Package

```xml
<!-- MyLumexUI.csproj -->
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <LangVersion>latest</LangVersion>
    <PackageId>MyLumexUI</PackageId>
    <Version>1.0.0</Version>
    <Authors>Your Name</Authors>
    <Description>Custom LumexUI components</Description>
    <PackageTags>blazor;lumexui;ui;components</PackageTags>
    <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
  </PropertyGroup>

  <ItemGroup>
    <ProjectReference Include="LumexUI" Version="2.0.1" />
  </ItemGroup>

  <ItemGroup>
    <Content Include="Components\**\*.razor" />
    <Content Include="wwwroot\css\*.css" />
  </ItemGroup>

</Project>
```

#### Package Structure

```
MyLumexUI/
├── Components/
│   ├── CustomButton.razor
│   ├── CustomCard.razor
│   ├── EnhancedInput.razor
│   └── ComponentLibrary.cs
├── wwwroot/
│   └── css/
│       ├── custom-button.css
│       ├── custom-card.css
│       └── enhanced-input.css
├── MyLumexUI.csproj
└── README.md
```

---

## ⚡ Performance & Optimization

### Component Performance Testing

```csharp
using Bunit;
using Xunit;
using System.Diagnostics;
using System.Threading.Tasks;

namespace MyLumexUITests.PerformanceTests
{
    public class PerformanceTests : TestContext
    {
        [Fact]
        public void LargeDataGrid_RendersQuickly()
        {
            // Arrange
            var largeDataSet = GenerateLargeDataSet(1000);
            var stopwatch = Stopwatch.StartNew();

            // Act
            var cut = RenderComponent<DataGrid>(parameters => parameters
                .Add(p => p.Data, largeDataSet));

            stopwatch.Stop();

            // Assert
            Assert.True(stopwatch.ElapsedMilliseconds < 1000,
                $"Data grid took {stopwatch.ElapsedMilliseconds}ms to render 1000 items");

            // Verify all items are rendered
            var rows = cut.FindAll("tr[data-row]");
            Assert.Equal(1000, rows.Count);
        }

        [Fact]
        public void Component_Rerender_PerformsWell()
        {
            // Arrange
            var cut = RenderComponent<PerformanceTestComponent>();
            var stopwatch = Stopwatch.StartNew();

            // Act - Trigger multiple rapid re-renders
            for (int i = 0; i < 100; i++)
            {
                cut.SetParametersAndRender(parameters => parameters
                    .Add(p => p.Counter, i));
            }

            stopwatch.Stop();

            // Assert
            Assert.True(stopwatch.ElapsedMilliseconds < 2000,
                $"100 re-renders took {stopwatch.ElapsedMilliseconds}ms");
        }

        private object[] GenerateLargeDataSet(int count)
        {
            var data = new object[count];
            for (int i = 0; i < count; i++)
            {
                data[i] = new { Id = i, Name = $"Item {i}", Value = $"Value {i}" };
            }
            return data;
        }
    }

    public class PerformanceTestComponent : ComponentBase
    {
        [Parameter]
        public int Counter { get; set; }

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "div");
            builder.AddContent(1, $"Counter: {Counter}");

            for (int i = 0; i < 10; i++)
            {
                builder.OpenElement(2, "div");
                builder.AddAttribute(3, "class", "nested-item");
                builder.AddContent(4, $"Item {i}");
                builder.CloseElement();
            }

            builder.CloseElement();
        }
    }

    public class DataGrid : ComponentBase
    {
        [Parameter]
        public object[] Data { get; set; } = Array.Empty<object>();

        protected override void BuildRenderTree(RenderTreeBuilder builder)
        {
            builder.OpenElement(0, "table");
            builder.OpenElement(1, "thead");
            builder.OpenElement(2, "tr");
            builder.OpenElement(3, "th");
            builder.AddContent(4, "ID");
            builder.CloseElement();
            builder.OpenElement(5, "th");
            builder.AddContent(6, "Name");
            builder.CloseElement();
            builder.OpenElement(7, "th");
            builder.AddContent(8, "Value");
            builder.CloseElement();
            builder.CloseElement();
            builder.CloseElement();

            builder.OpenElement(9, "tbody");

            foreach (var item in Data)
            {
                builder.OpenElement(10, "tr");
                builder.AddAttribute(11, "data-row", "");

                builder.OpenElement(12, "td");
                builder.AddContent(13, item.GetType().GetProperty("Id")?.GetValue(item)?.ToString());
                builder.CloseElement();

                builder.OpenElement(14, "td");
                builder.AddContent(15, item.GetType().GetProperty("Name")?.GetValue(item)?.ToString());
                builder.CloseElement();

                builder.OpenElement(16, "td");
                builder.AddContent(17, item.GetType().GetProperty("Value")?.GetValue(item)?.ToString());
                builder.CloseElement();

                builder.CloseElement();
            }

            builder.CloseElement();
            builder.CloseElement();
        }
    }
}
```

### Memory Management

```csharp
using Microsoft.AspNetCore.Components;

namespace MyLumexUI.Components
{
    public class MemoryEfficientComponent : ComponentBase, IDisposable
    {
        private Timer? _timer;
        private List<string> _data = new();

        protected override void OnInitialized()
        {
            // Initialize timer
            _timer = new Timer(UpdateData, null, TimeSpan.Zero, TimeSpan.FromSeconds(1));
        }

        protected override void OnParametersSet()
        {
            // Clean up old data when parameters change
            if (_data.Count > 1000)
            {
                _data.RemoveRange(0, _data.Count - 1000);
            }
        }

        private void UpdateData(object? state)
        {
            // Add new data
            _data.Add($"Data point {_data.Count}");

            // Notify component to re-render
            StateHasChanged();
        }

        public void Dispose()
        {
            _timer?.Dispose();
        }
    }
}
```

### Bundle Optimization

#### Lazy Loading Components

```razor
@* LazyLoadedComponent.razor *@
@if (_isLoaded)
{
    <ExpensiveComponent Data="@_data" />
}
else
{
    <LumexSpinner />
    <button @onclick="LoadComponent">Load Component</button>
}

@code {
    private bool _isLoaded = false;
    private object? _data;

    private async Task LoadComponent()
    {
        // Simulate loading expensive data
        await Task.Delay(1000);
        _data = await LoadExpensiveData();
        _isLoaded = true;
    }

    private async Task<object> LoadExpensiveData()
    {
        // Load expensive data here
        return new { /* large data object */ };
    }
}
```

#### Virtualization for Large Lists

```razor
@* VirtualizedList.razor *@
<LumexContainer>
    <Virtualize Items="@_items" OverscanCount="10" Context="item">
        <div class="list-item">
            <LumexCard>
                <LumexCardBody>
                    <h4>@item.Name</h4>
                    <p>@item.Description</p>
                </LumexCardBody>
            </LumexCard>
        </div>
    </Virtualize>
</LumexContainer>

@code {
    private readonly List<ListItem> _items = Enumerable.Range(1, 10000)
        .Select(i => new ListItem($"Item {i}", $"Description for item {i}"))
        .ToList();

    public class ListItem
    {
        public string Name { get; set; }
        public string Description { get; set; }

        public ListItem(string name, string description)
        {
            Name = name;
            Description = description;
        }
    }
}
```

### Best Practices Summary

#### Component Performance

1. **Use Virtualization**: For lists with 100+ items
2. **Lazy Loading**: Load expensive components on demand
3. **Avoid Excessive Re-renders**: Use ShouldRender override
4. **Memory Management**: Implement IDisposable for resources
5. **Efficient Data Binding**: Use appropriate binding strategies

#### Testing Performance

1. **Render Time Testing**: Measure component rendering performance
2. **Memory Testing**: Monitor memory usage over time
3. **Interaction Testing**: Test with realistic user interactions
4. **Load Testing**: Test with large datasets
5. **Regression Testing**: Monitor performance over releases

---

## 🎨 Advanced Theming & Customization

### Design Token System

#### CSS Custom Properties Architecture

```css
/* Design tokens base */
:root {
  /* Color system */
  --color-primary-h: 221.2;
  --color-primary-s: 83.2%;
  --color-primary-l: 53.3%;

  --color-secondary-h: 210;
  --color-secondary-s: 40%;
  --color-secondary-l: 96.1%;

  --color-success-h: 142.1;
  --color-success-s: 76.2%;
  --color-success-l: 36.3%;

  --color-warning-h: 45.1;
  --color-warning-s: 91.2%;
  --color-warning-l: 59.3%;

  --color-error-h: 0;
  --color-error-s: 84.2%;
  --color-error-l: 60.2%;

  /* Semantic mapping */
  --lumex-primary: var(--color-primary-h) var(--color-primary-s) var(--color-primary-l);
  --lumex-primary-foreground: 210 40% 98%;
  --lumex-secondary: var(--color-secondary-h) var(--color-secondary-s) var(--color-secondary-l);
  --lumex-secondary-foreground: 222.2 84% 4.9%;
  --lumex-success: var(--color-success-h) var(--color-success-s) var(--color-success-l);
  --lumex-success-foreground: 355.7 100% 94.1%;
  --lumex-warning: var(--color-warning-h) var(--color-warning-s) var(--color-warning-l);
  --lumex-warning-foreground: 15.6 86.7% 60.8%;
  --lumex-danger: var(--color-error-h) var(--color-error-s) var(--color-error-l);
  --lumex-danger-foreground: 210 40% 98%;
}
```

#### Dark Mode Implementation

```css
/* Dark mode overrides */
.dark {
  --color-primary-l: 58.8%;
  --color-secondary-l: 14.9%;
  --color-success-l: 59.6%;
  --lumex-background: 222.2 84% 4.9%;
  --lumex-foreground: 210 40% 98%;
}

/* Automatic dark mode detection */
@media (prefers-color-scheme: dark) {
  :root:not(.light) {
    --color-primary-l: 58.8%;
    --color-secondary-l: 14.9%;
    --color-success-l: 59.6%;
    --lumex-background: 222.2 84% 4.9%;
    --lumex-foreground: 210 40% 98%;
  }
}
```

### Component Theming

#### Theme Provider Implementation

```razor
@* ThemeProvider.razor *@
<CascadingValue Value="@currentTheme">
    @ChildContent
</CascadingValue>

@code {
    [Parameter]
    public RenderFragment? ChildContent { get; set; }

    [Parameter]
    public ThemeMode Mode { get; set; } = ThemeMode.System;

    private Theme currentTheme = new();

    protected override void OnInitialized()
    {
        currentTheme = CreateTheme(Mode);
    }

    protected override void OnParametersSet()
    {
        if (Mode != currentTheme.Mode)
        {
            currentTheme = CreateTheme(Mode);
        }
    }

    private Theme CreateTheme(ThemeMode mode)
    {
        return mode switch
        {
            ThemeMode.Dark => new Theme { Mode = ThemeMode.Dark, IsDark = true },
            ThemeMode.Light => new Theme { Mode = ThemeMode.Light, IsDark = false },
            ThemeMode.System => new Theme { Mode = ThemeMode.System, IsDark = SystemDarkMode },
            _ => new Theme { Mode = ThemeMode.System, IsDark = SystemDarkMode }
        };
    }

    private bool SystemDarkMode =>
        OperatingSystem.IsWindows() &&
        SystemParameters.WindowScheme == WindowScheme.Dark;
}

public class Theme
{
    public ThemeMode Mode { get; set; }
    public bool IsDark { get; set; }
    public string BackgroundColor => IsDark ? "rgb(17, 24, 39)" : "rgb(255, 255, 255)";
    public string TextColor => IsDark ? "rgb(243, 244, 246)" : "rgb(17, 24, 39)";
    public string PrimaryColor => IsDark ? "rgb(59, 130, 246)" : "rgb(37, 99, 235)";
}

public enum ThemeMode
{
    Light,
    Dark,
    System
}
```

#### Theme-Aware Components

```razor
@* ThemeAwareCard.razor *@
<div class="theme-aware-card @GetThemeClasses()">
    <div class="card-header">
        <h3 class="card-title">@Title</h3>
        @if (ShowThemeToggle)
        {
            <ThemeToggle />
        }
    </div>
    <div class="card-content">
        @ChildContent
    </div>
</div>

@code {
    [Parameter]
    public string Title { get; set; } = "";

    [Parameter]
    public bool ShowThemeToggle { get; set; }

    [Parameter]
    public RenderFragment? ChildContent { get; set; }

    [CascadingParameter]
    private Theme? CurrentTheme { get; set; }

    private string GetThemeClasses()
    {
        var classes = new List<string>();

        if (CurrentTheme?.IsDark == true)
        {
            classes.Add("dark");
        }

        return string.Join(" ", classes);
    }
}
```

### Advanced Customization

#### Dynamic Theming

```razor
@* DynamicThemeProvider.razor *@
<style>
    :root {
        @foreach (var token in customTokens)
        {
            @($"--custom-{token.Key}: {token.Value};")
        }
    }
</style>

@code {
    [Parameter]
    public Dictionary<string, string> CustomTokens { get; set; } = new();

    private Dictionary<string, string> customTokens = new();

    protected override void OnParametersSet()
    {
        customTokens = CustomTokens;
    }

    public void UpdateToken(string name, string value)
    {
        customTokens[name] = value;
        StateHasChanged();
    }
}
```

#### Brand Customization

```css
/* Brand-specific customization */
.brand-microsoft {
  --lumex-primary: 0 84.2% 60.2%;      /* Microsoft blue */
  --lumex-secondary: 210 40% 96.1%;     /* Light gray */
  --lumex-success: 142.1 76.2% 36.3%;    /* Green */
  --lumex-brand-gradient: linear-gradient(135deg, #0078D4, #00BCF2);
}

.brand-github {
  --lumex-primary: 220 13.5% 46.1%;      /* GitHub black */
  --lumex-secondary: 210 40% 96.1%;     /* Light gray */
  --lumex-success: 142.1 76.2% 36.3%;    /* Green */
  --lumex-brand-gradient: linear-gradient(135deg, #24292e, #58a6ff);
}
```

---

## Related Topics

- **Component Reference**: See [LUMEXUI-COMPREFERENCE.md](./LUMEXUI-COMPREFERENCE.md) for all component examples
- **Installation & Setup**: See [LUMEXUI-SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md) for getting started
- **Real-world Examples**: Check the repository's sample applications
- **Community Resources**: LumexUI documentation and community forums

---

**Document Created**: December 2025
**Last Updated**: December 2025
**Target Framework**: .NET 8+
**UI Library**: LumexUI with Tailwind CSS
**Version**: 1.0