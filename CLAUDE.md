# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a comprehensive learning repository for **LumexUI** - a modern Blazor UI library built with Tailwind CSS. The repository contains documentation, guides, and examples for learning LumexUI component usage and integration patterns.

## Development Commands

### Essential Commands

**Development Setup:**
```bash
# Create new projects following the LumexUI integration pattern
# Refer to documentation for setup instructions
```


**Build Solution:**
```bash
dotnet build learning-lumexui.sln
```

**Testing:**
```bash
# Manual testing: Access your applications via localhost ports
# Automated testing: Create test scripts based on your specific needs
```

## LumexUI Integration Pattern

All applications follow the same LumexUI integration pattern:

1. **Package Reference**: `LumexUI` version 2.0.1
2. **Service Registration**: In `Program.cs` add `builder.Services.AddLumexServices()`
3. **🚨 GLOBAL USINGS**: Create `_GlobalUsings.cs` with LumexUI namespaces (MANDATORY for all projects)
4. **Import Statement**: In `_Imports.razor` add `@using LumexUI`
5. **JavaScript**: In `App.razor` include `<script src="_content/LumexUI/js/LumexUI.js" type="module"></script>`
6. **CSS Integration**: In `wwwroot/app.css` include `@import "../bin/lumexui/theme"` and `@source "../bin/lumexui/*.cs"`

### 🚨 Critical AI Agent Requirement: Global Usings

**ALL projects MUST include `_GlobalUsings.cs` with the following mandatory entries:**

```csharp
// Global using statements for [ProjectName]
// MANDATORY: This file is required for ALL projects

// ===================================================================
// SYSTEM CORE NAMESPACES
// ===================================================================
global using System;
global using System.Collections.Generic;
global using System.Linq;
global using System.Threading;
global using System.Threading.Tasks;

// ===================================================================
// ASP.NET CORE NAMESPACES
// ===================================================================
global using Microsoft.AspNetCore.Builder;
global using Microsoft.AspNetCore.Components;
global using Microsoft.AspNetCore.Components.Web;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Hosting;

// ===================================================================
// LUMEXUI NAMESPACES (MANDATORY)
// ===================================================================
global using LumexUI;
global using LumexUI.Extensions;
global using LumexUI.Components;
```

**IMPORTANT**: `.cs` files must NOT contain any using statements. All namespaces must be centralized in `_GlobalUsings.cs`.



### Component Usage Patterns

**Standard Component Pattern:**
```razor
<LumexButton Variant="Variant.Primary" Size="Size.Medium">
    Click Me
</LumexButton>
```

**Form Integration:**
```razor
<EditForm Model="@model" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    <LumexTextField @bind-Value="model.Name" Label="Name" />
    <ValidationMessage For="@(() => model.Name)" />
</EditForm>
```

**Component with Start/End Content:**
```razor
<LumexTextField StartContent="@searchIcon" EndContent="@clearIcon" />
```

## Technology Stack

- **.NET 10**: Target framework for development
- **Blazor**: Interactive Server Components (Blazor Server)
- **LumexUI**: Version 2.0.1 for UI components
- **Tailwind CSS**: Utility-first CSS framework integrated via LumexUI
- **Razor Components**: Modern Blazor component model

## Development Workflow

1. **Documentation Study**: Review comprehensive guides and examples
2. **Pattern Learning**: Study LumexUI integration patterns and best practices
3. **Reference Implementation**: Use provided examples for learning component usage
4. **Testing**: Implement comprehensive testing for your applications

## Project Structure and Quality Assurance

### Learning Resources
This repository provides comprehensive learning resources including:
- **Integration Guides**: Step-by-step LumexUI setup instructions
- **Component References**: Detailed usage examples for all LumexUI components
- **Best Practices**: Modern C# and Blazor development patterns
- **Architecture Patterns**: Clean Architecture and DDD examples

### Common Issues and Solutions
1. **Build Errors**: Ensure .NET 10 SDK is installed (`dotnet --version` should show 10.x.x)
2. **Missing Packages**: Run `dotnet restore` in solution directory
3. **Documentation Updates**: Ensure examples reflect current LumexUI version
4. **Project Structure**: Ensure projects follow established patterns

### Development Best Practices
- **Study Documentation**: Review all learning guides before starting development
- **Follow Patterns**: Use established LumexUI integration patterns
- **Modern C#**: Apply C# 13/14 features and .NET 10 best practices
- **Testing**: Include comprehensive testing for new implementations
- **Accessibility**: Ensure WCAG compliance in all implementations

### Visual Studio Alternative
1. Open `learning-lumexui.sln` in Visual Studio
2. Set specific project as Startup Project (if available)
3. Press F5 to run with debugging

## Important Notes

- **Reference Implementation**: Documentation assumes Blazor Server render mode (`AddInteractiveServerComponents()`)
- **Clean Builds**: Enable `TreatWarningsAsErrors` for quality development
- **JavaScript Integration**: LumexUI JavaScript must be included in root HTML for components to function
- **CSS Integration**: Tailwind CSS integration is handled automatically through LumexUI's theme system
- **Documentation Focus**: Repository serves as comprehensive learning resource

## Required Reading for Development

Before making any changes to this repository, **always read** the following files:

### 📚 Essential Learning Guides
1. **docs/guides/learning/C-SHARP-NET9-NET10-MASTERY-GUIDE-FOR-AI-AGENTS.md** - **🚨 PRIMARY FOUNDATION** - Modern C# 13/14 features and .NET 9/10 patterns
2. **docs/guides/learning/BLAZOR-LEARNING-RESOURCE-GUIDE-FOR-AI-AGENTS.md** - **🏗️ FRAMEWORK MASTERY** - Modern Blazor development patterns and best practices
3. **docs/guides/lumexui/LUMEXUI-SETUP-GUIDE.md** - **🚨 PRIMARY SETUP GUIDE** - Complete integration instructions for adding LumexUI to Blazor applications
4. **docs/guides/lumexui/LUMEXUI-COMPREFERENCE.md** - **📚 COMPONENT REFERENCE** - Comprehensive component examples and quick reference guide
5. **docs/guides/lumexui/LUMEXUI-ADVANCED-GUIDE.md** - **🎓 ADVANCED TOPICS** - Learning progression, testing with bUnit, custom component development, and performance optimization

### 📋 Repository Documentation
6. **docs/guides/standards/NUGET-PACKAGE-STANDARDS.md** - **🚨 MANDATORY PACKAGE STANDARDS** - Official NuGet package standards and integration patterns for AI agents
7. **CLAUDE.md** (this file) - Architecture and development patterns
8. **docs/guides/project/QUICK_START.md** - Current working application status and testing procedures
9. **docs/SETUP_AND_RUN.md** - Comprehensive setup instructions and troubleshooting

## 🛠️ Enhanced Development Standards

### Technology Stack Requirements
- **Framework**: .NET 10+ only (no legacy .NET Framework)
- **Language**: C# 13/14 features preferred
- **UI Framework**: LumexUI version 2.0.1 with Tailwind CSS
- **Data Access**: Dapper 2.1.35+ with multi-database support (SQL Server, PostgreSQL, SQLite)
- **Background Jobs**: TickerQ 1.0.0+ for modern .NET 10 job processing
- **Architecture**: Clean Architecture with DDD patterns
- **Testing**: >80% coverage with modern patterns (bUnit for Blazor components)
- **Package Standards**: **🚨 MANDATORY** - Follow docs/guides/standards/NUGET-PACKAGE-STANDARDS.md

### Code Quality Standards
- **Modern C#**: Use C# 13/14 features (field keyword, params collections, new lock semantics)
- **🚨 GLOBAL USINGS**: MANDATORY - All projects must use `_GlobalUsings.cs` with proper categorization
- **Clean Code**: .cs files must NOT contain any using statements (centralized in _GlobalUsings.cs)
- **Performance**: Use Span<T>, Memory<T>, ValueTask where appropriate
- **Accessibility**: WCAG compliant interfaces with proper ARIA labels
- **Responsive Design**: Mobile-first approach using Tailwind CSS utilities
- **Security**: Enterprise-grade security patterns
- **Documentation**: Comprehensive code comments and documentation

### 🚨 Global Usings Requirements (MANDATORY FOR AI AGENTS)
- **ALWAYS Create**: Every new project MUST have `_GlobalUsings.cs` at project root
- **CENTRALIZE**: All namespace using statements for `.cs` files MUST be in `_GlobalUsings.cs`
- **FORBID**: `.cs` files MUST NOT contain any using statements at the top
- **ORGANIZE**: Namespaces MUST be organized by category with alphabetical sorting
- **MAINTAIN**: Razor components continue using `_Imports.razor` (existing pattern)
- **VALIDATE**: Verify compilation success and namespace coverage

### LumexUI Best Practices
- **Theme Support**: Always include light/dark mode switching
- **Loading States**: Proper loading indicators with LumexSpinner
- **Error Handling**: User-friendly error messages with LumexAlert
- **Form Validation**: Real-time validation with clear feedback
- **Component Consistency**: Follow established LumexUI patterns
- **Accessibility**: Proper ARIA labels and keyboard navigation

## 🚀 Project Enhancement Requirements

### When Modifying Documentation
1. **Read all guides first** - Apply modern .NET 9/10 and C# 13/14 patterns
2. **Update examples** - Use latest LumexUI component patterns and best practices
3. **Verify accuracy** - Ensure all code examples are current and functional
4. **Enhance accessibility** - Include accessibility considerations in examples
5. **Maintain quality** - Review and update content regularly

### When Creating New Applications
1. **🚨 CREATE GLOBAL USINGS**: Create `_GlobalUsings.cs` at project root (MANDATORY FIRST STEP)
2. **Follow LumexUI integration pattern** - Use the established pattern from documentation
3. **Modern architecture** - Apply Clean Architecture with DDD patterns
4. **Clean code principles** - NO using statements in `.cs` files (all in `_GlobalUsings.cs`)
5. **Comprehensive testing** - Include unit tests, integration tests, accessibility tests
6. **Documentation** - Create complete README and component documentation
7. **Examples** - Provide clear usage examples and best practices
8. **Namespace organization** - Organize by category with alphabetical sorting

## 💡 Pro Development Workflow

### Before Starting Any Task
1. **Read all learning guides** first
2. **🚨 READ PACKAGE STANDARDS** - docs/guides/standards/NUGET-PACKAGE-STANDARDS.md (MANDATORY)
3. **Understand documented patterns** and examples
4. **Plan using modern architecture** patterns
5. **Reference LumexUI components** and modern C# features
6. **Use Dapper for data access** and TickerQ for background jobs
7. **Include comprehensive testing** with bUnit when implementing
8. **Ensure accessibility compliance** in all implementations
9. **Verify performance** and responsive design for applications

### Code Review Checklist
- [ ] Uses modern C# 13/14 features
- [ ] Follows LumexUI best practices
- [ ] **🚨 PACKAGE STANDARDS**: Follows docs/guides/standards/NUGET-PACKAGE-STANDARDS.md
- [ ] **🚨 DATA ACCESS**: Uses Dapper (NOT Entity Framework)
- [ ] **🚨 BACKGROUND JOBS**: Uses TickerQ for modern .NET 10 job processing
- [ ] **🚨 GLOBAL USINGS**: `_GlobalUsings.cs` exists at project root
- [ ] **🚨 CLEAN CODE**: No using statements in `.cs` files
- [ ] **🚨 NAMESPACE ORG**: Namespaces organized by category, sorted alphabetically
- [ ] Includes accessibility features
- [ ] Has comprehensive tests
- [ ] Responsive on all devices
- [ ] Proper error handling
- [ ] Performance optimized
- [ ] Well documented
- [ ] **🚨 RAZOR PATTERN**: Razor components use `_Imports.razor` (not global usings)
- [ ] **🚨 COMPILATION**: Project builds successfully with all namespaces available

These files and standards ensure code quality, modern patterns, and provide comprehensive guidance for LumexUI development while staying current with the latest .NET and LumexUI features.