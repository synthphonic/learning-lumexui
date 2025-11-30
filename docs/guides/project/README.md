# 🚀 Learning LumexUI

A comprehensive learning repository for mastering **LumexUI** - the modern Blazor UI library built with Tailwind CSS.

## 📋 Table of Contents

- [About LumexUI](#what-is-lumexui)
- [Sample Applications](#sample-applications)
- [Quick Start](#quick-start)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Applications](#running-the-applications)
- [Learning Path](#learning-path)
- [Documentation](#documentation)
- [Contributing](#contributing)

## ✨ What is LumexUI?

LumexUI is a versatile **Blazor UI library** built with **Tailwind CSS** for creating modern, responsive user interfaces in .NET applications.

### Key Features

- 🎨 **Modern Components**: 30+ pre-built UI components for Blazor
- 🎭 **Tailwind CSS Integration**: Built on top of Tailwind CSS for styling
- 🎛️ **Highly Customizable**: Extensive theming and configuration options
- 🌙 **Dark Mode Support**: Built-in dark mode capabilities
- 📱 **Responsive Design**: Mobile-first approach with responsive components
- ⚡ **Performance Optimized**: Lightweight and fast rendering
- 🔧 **TypeScript Support**: Full TypeScript definitions included
- 🎯 **Blazor Native**: Designed specifically for Blazor applications

### Available Components

- **Interactive Elements**: Accordion, Alert, Avatar, Badge, Button, Card, Modal
- **Form Controls**: TextField, Select, Checkbox, RadioButton, DatePicker, Form Validation
- **Navigation**: Navbar, Breadcrumb, Pagination, Tabs, Menu
- **Feedback**: Spinner, Progress, Toast, Alert, Skeleton
- **Layout**: Grid, Container, Divider, Space

## 🏗️ Sample Applications

This repository includes **progressive learning applications** that build from basic to advanced concepts:

### 🌱 Basics

| Application | Description | Skills Learned |
|-------------|-------------|---------------|
| **Component Showcase** | Interactive demonstration of all LumexUI components | Component usage, styling, theming |
| **Task Manager** | Full-featured task management system | Form handling, state management, data binding |

### 🚀 Intermediate

| Application | Description | Skills Learned |
|-------------|-------------|---------------|
| **Portfolio & Blog** | Professional portfolio website with blog functionality | Routing, navigation, content management |
| **Weather Dashboard** | Real-time weather application with API integration | HTTP requests, data visualization, responsive design |

### 🔮 Advanced (Coming Soon)

- **E-commerce Platform** - Shopping cart, payment integration, product catalog
- **Admin Dashboard** - Analytics, user management, system configuration
- **Chat Application** - Real-time messaging, SignalR integration

## ⚡ Quick Start

### 1. Clone and Setup

```bash
git clone <repository-url>
cd learning-lumexui
```

### 2. Install LumexUI CLI

```bash
dotnet tool install --global LumexUI.CLI
lumex --version
```

### 3. Development Setup

```bash
# Create new applications following the LumexUI integration pattern
# Refer to documentation for detailed setup instructions
```

### 4. Access Applications

- **Component Showcase**: http://localhost:5139
- **Task Manager**: http://localhost:5114
- **Portfolio/Blog**: http://localhost:5123
- **Weather Dashboard**: http://localhost:5147

## 📁 Repository Structure

```
learning-lumexui/
├── 📂 src/
│   ├── 📂 basics/                    # Basic applications
│   │   ├── 📂 component-showcase/    # App 0: Component demonstration
│   │   └── 📂 task-manager/          # App 1: Task management system
│   ├── 📂 intermediate/              # Intermediate applications
│   │   ├── 📂 portfolio-blog/        # App 2: Portfolio & blog website
│   │   └── 📂 weather-dashboard/     # App 3: Weather dashboard
│   └── 📂 advanced/                  # Advanced applications (coming soon)
├── 📂 docs/                         # Documentation
│   └── 📄 SETUP_AND_RUN.md          # Setup and run guide
└── 📄 README.md                     # This file
```

## 🔧 Prerequisites

### Required Software

- **.NET 10 SDK** - Download from [dotnet.microsoft.com](https://dotnet.microsoft.com/download)
- **Visual Studio 2022** or **VS Code** - Recommended IDEs
- **Git** - For cloning and managing the repository

### System Requirements

- **Windows**: Windows 10/11 with Visual Studio 2022
- **macOS**: macOS 10.15+ with VS Code and .NET SDK
- **Linux**: Any modern distribution with .NET SDK support

## 📦 Installation

### Create Your Own LumexUI Project

1. **Create a new Blazor application**
   ```bash
   dotnet new blazor -o MyLumexUIApp
   cd MyLumexUIApp
   ```

2. **Install LumexUI package**
   ```bash
   dotnet add package LumexUI
   ```

3. **Configure services** (in `Program.cs`)
   ```csharp
   using LumexUI.Extensions;

   builder.Services.AddRazorComponents()
       .AddInteractiveServerComponents();

   builder.Services.AddLumexServices();
   ```

4. **Add JavaScript** (in `Components/App.razor`)
   ```html
   <script src="_content/LumexUI/lumex.js"></script>
   ```

5. **Add using directives** (in `Components/_Imports.razor`)
   ```csharp
   @using LumexUI
   ```

6. **Run your application**
   ```bash
   dotnet run
   ```

## 📚 Learning Resources

### Documentation-Based Learning

This repository focuses on comprehensive documentation rather than runnable applications. Access the learning resources through:

```bash
# Navigate to documentation
cd docs/guides/

# Start with the learning path
# 1. C# Mastery Guide: ./learning/C-SHARP-NET9-NET10-MASTERY-GUIDE-FOR-AI-AGENTS.md
# 2. Blazor Learning Guide: ./learning/BLAZOR-LEARNING-RESOURCE-GUIDE-FOR-AI-AGENTS.md
# 3. LumexUI Setup Guide: ./lumexui/LUMEXUI-SETUP-GUIDE.md
# 4. Component Reference: ./lumexui/LUMEXUI-COMPREFERENCE.md
# 5. Advanced Topics: ./lumexui/LUMEXUI-ADVANCED-GUIDE.md
```

### Development Practice

1. **Study** the documentation guides in the recommended sequence
2. **Create** your own Blazor projects following the setup instructions
3. **Practice** with component examples from the reference guide
4. **Apply** advanced patterns from the advanced topics guide

### Testing

```bash
# Create your own test scripts based on application needs
# Refer to Playwright documentation for Blazor testing
```

## 🎓 Learning Path

### 🌱 Beginner Level

1. **Start with Component Showcase**
   - Learn all available LumexUI components
   - Understand component variants and sizing
   - Practice basic styling and theming

2. **Move to Task Manager**
   - Learn form handling and validation
   - Understand state management in Blazor
   - Practice CRUD operations

### 🚀 Intermediate Level

3. **Build Portfolio/Blog**
   - Master routing and navigation
   - Learn content management patterns
   - Practice responsive design

4. **Create Weather Dashboard**
   - Learn API integration
   - Practice data visualization
   - Master asynchronous operations

### 🔮 Advanced Level

5. **E-commerce Platform** (Coming Soon)
   - Learn complex state management
   - Practice payment integration
   - Master advanced Blazor patterns

## 📚 Documentation

### Core Documentation

- **[Setup and Run Guide](../../SETUP_AND_RUN.md)** - Complete setup instructions
- **[LumexUI Official Docs](https://lumexui.org)** - Official documentation and API reference
- **[Blazor Documentation](https://docs.microsoft.com/aspnet/core/blazor)** - Microsoft Blazor documentation

### Component Examples

```razor
<!-- Basic Button -->
<LumexButton Variant="Variant.Primary">Click Me</LumexButton>

<!-- Text Input with Validation -->
<LumexTextField @bind-Value="searchQuery"
                 Placeholder="Search..."
                 Variant="Variant.Outline"
                 Label="Search" />

<!-- Card Component -->
<LumexCard>
    <LumexCardBody>
        <h3>Card Title</h3>
        <p>Card content goes here</p>
    </LumexCardBody>
</LumexCard>

<!-- Alert Component -->
<LumexAlert Variant="Variant.Success">
    Operation completed successfully!
</LumexAlert>
```

### Advanced Patterns

```csharp
// Custom theming
builder.Services.AddLumexServices(options =>
{
    options.Theme.PrimaryColor = "#3B82F6";
    options.Theme.SecondaryColor = "#6B7280";
});

// Component validation
<EditForm Model="@model">
    <DataAnnotationsValidator />
    <LumexTextField @bind-Value="model.Name" />
    <ValidationMessage For="@(() => model.Name)" />
</EditForm>
```

## 🧪 Testing

### Manual Testing

Each application includes interactive features for manual testing:
- Component interactions
- Form validation
- Responsive design
- Error handling

### Automated Testing with Playwright

The repository includes comprehensive Playwright tests:

```bash
# Install Playwright
npm install @playwright/test
npx playwright install

# Run all tests
npx playwright test

# Run specific test
npx playwright test component-showcase.spec.js
```

## 🤝 Contributing

We welcome contributions! Here's how to get started:

### Development Workflow

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes
4. Test thoroughly
5. Submit a pull request with clear description

### Contribution Areas

- **Bug fixes** and issue resolution
- **New sample applications**
- **Documentation improvements**
- **Component examples**
- **Performance optimizations**

### Guidelines

- Follow existing code style and patterns
- Add comments for complex logic
- Include tests for new features
- Update documentation as needed

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **LumexUI Team** - For creating this amazing UI library
- **Blazor Community** - For inspiration and support
- **Tailwind CSS** - For the excellent CSS framework

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/your-repo/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-repo/discussions)
- **LumexUI**: [Official Documentation](https://lumexui.org)

---

🎉 **Happy coding with LumexUI!**

Start your learning journey today and build amazing Blazor applications with modern UI components.
