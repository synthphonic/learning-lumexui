# Comprehensive LumexUI Setup Guide for AI Agents

## Overview
This document provides AI agents with a complete, step-by-step guide to successfully integrate LumexUI into Blazor web applications. It covers all 6 essential installation steps (including the mandatory Step 2.5 for global usings) with detailed implementation options, troubleshooting, and best practices.

## Prerequisites
- .NET 8.0 or later project (Blazor Web App, Blazor Server, or Blazor WebAssembly)
- Package Manager (NuGet)
- Basic understanding of Blazor component structure
- (Optional) Node.js and npm for Tailwind CLI approach

---

## Step 1: Install LumexUI Package

### Action
Add the LumexUI NuGet package to your project:

```bash
dotnet add package LumexUI
```

Or via Visual Studio/Package Manager Console:
```
Install-Package LumexUI
```

### Verification
Check that your `.csproj` file contains:
```xml
<PackageReference Include="LumexUI" Version="2.0.1" />
```

### Common Issues
- **Version Conflicts**: Ensure you're using a compatible version (2.0.1+ recommended)
- **Restore Failures**: Run `dotnet restore` if package installation fails

---

## Step 2: Configure CSS Integration (Detailed)

This is the most critical step with multiple valid approaches. Choose ONE based on your project needs.

### Approach A: Tailwind CLI Integration (Recommended for New Projects)

**Best For**: Production applications, full Tailwind utility access, optimal performance

#### Prerequisites
- Node.js installed
- npm available

#### Setup Steps

1. **Initialize Tailwind CSS**:
   ```bash
   npm init -y
   npm install -D tailwindcss
   npx tailwindcss init
   ```

2. **Configure Tailwind** - Create/update `tailwind.config.js`:
   ```javascript
   /** @type {import('tailwindcss').Config} */
   module.exports = {
     content: [
       "./Components/**/*.{razor,cshtml,html}",
       "./Pages/**/*.{razor,cshtml,html}",
       "./wwwroot/**/*.{html,js}",
       "../bin/lumexui/*.cs"  // Critical: Scan LumexUI C# files
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

3. **Create Input CSS** - Create `wwwroot/css/input.css`:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

4. **Build CSS** - Run Tailwind to generate final CSS:
   ```bash
   npx tailwindcss -i ./wwwroot/css/input.css -o ./wwwroot/css/app.css --watch
   ```
   (For production, omit `--watch`)

5. **Add LumexUI Integration** - In your final `wwwroot/css/app.css`:
   ```css
   @import "tailwindcss";
   @import "../bin/lumexui/theme";
   @source "../bin/lumexui/*.cs";
   ```

#### Verification
- Check `bin/lumexui/` directory exists after build
- Verify `app.css` contains utility classes
- Test LumexUI components have proper styling

### Approach B: Pre-built CSS Files (Simpler, Current Repository Approach)

**Best For**: Learning projects, quick setup, minimal dependencies

#### Setup Steps

1. **Build Project** to generate LumexUI resources:
   ```bash
   dotnet build
   ```

2. **Verify Generated Files** - Check `bin/Debug/netX.0/lumexui/` contains:
   ```
   theme.css
   _light.css
   _dark.css
   _layout.css
   ```

3. **Create Main CSS File** - Update `wwwroot/css/app.css`:
   ```css
   /* LumexUI Theme Integration */
   @import url("../lumexui/_light.css");
   @import url("../lumexui/_dark.css");

   /* Essential CSS Variables */
   :root {
     --lumex-background: 0 0% 100%;
     --lumex-foreground: 222.2 84% 4.9%;
     --lumex-primary: 221.2 83.2% 53.3%;
     --lumex-primary-foreground: 210 40% 98%;
   }

   .dark {
     --lumex-background: 222.2 84% 4.9%;
     --lumex-foreground: 210 40% 98%;
     --lumex-primary: 217.2 91.2% 59.8%;
     --lumex-primary-foreground: 222.2 84% 4.9%;
   }

   /* Essential Utility Classes */
   .bg-background { background-color: hsl(var(--lumex-background)); }
   .text-foreground { color: hsl(var(--lumex-foreground)); }
   .bg-primary { background-color: hsl(var(--lumex-primary)); }
   .text-primary-foreground { color: hsl(var(--lumex-primary-foreground)); }

   /* Common Layout Utilities */
   .flex { display: flex; }
   .inline-flex { display: inline-flex; }
   .items-center { align-items: center; }
   .justify-center { justify-content: center; }
   .font-medium { font-weight: 500; }
   .rounded-md { border-radius: 0.375rem; }
   .px-4 { padding-left: 1rem; padding-right: 1rem; }
   .py-2 { padding-top: 0.5rem; padding-bottom: 0.5rem; }
   .text-sm { font-size: 0.875rem; }
   ```

#### Verification
- Components should render with basic LumexUI styling
- Theme switching should work (light/dark mode)
- No build errors or missing CSS files

### Approach C: Hybrid Method (Production-Ready Without Tailwind CLI)

**Best For**: Production apps that want performance without CLI dependency

#### Setup Steps

1. **Install Tailwind CSS as dependency** (but don't run CLI):
   ```bash
   npm install tailwindcss
   ```

2. **Use Tailwind's .NET integration** via `tailwindcss-dotnet` package:
   ```bash
   dotnet add package TailwindCSS.Net
   ```

3. **Configure in `Program.cs`**:
   ```csharp
   // Add after AddServices()
   builder.Services.AddTailwindCSS();
   ```

4. **Create `tailwind.config.js`** (same as Approach A)

5. **CSS file** - Use `wwwroot/css/app.css`:
   ```css
   @import "tailwindcss";
   @import "../bin/lumexui/theme";
   @source "../bin/lumexui/*.cs";
   ```

### Step 2 Troubleshooting Guide

#### Common Issues and Solutions

1. **Missing `bin/lumexui/` Directory**
   - **Cause**: Project not built, or LumexUI package missing
   - **Solution**: Run `dotnet build` and verify package installation
   - **Check**: `.csproj` contains LumexUI reference

2. **Path Resolution Errors**
   - **Cause**: Incorrect relative paths in CSS imports
   - **Solution**: Verify CSS file location relative to `bin/` directory
   - **Typical Path**: From `wwwroot/css/app.css` to `../bin/lumexui/theme`

3. **Tailwind CLI Not Working**
   - **Cause**: Node.js not installed or not in PATH
   - **Solution**: Install Node.js or use Approach B/C
   - **Verification**: Run `node --version` and `npm --version`

4. **Components Appear Unstyled**
   - **Cause**: CSS imports missing or incorrect
   - **Solution**: Check browser DevTools for CSS loading
   - **Debug**: Look for 404 errors on CSS files

5. **Build Performance Issues**
   - **Cause**: Too many file scans or inefficient Tailwind config
   - **Solution**: Optimize content paths in `tailwind.config.js`
   - **Monitor**: Check build times and CSS bundle size

#### Verification Checklist

After completing Step 2, verify:

- [ ] `bin/lumexui/` directory exists after build
- [ ] CSS files load without 404 errors (check DevTools)
- [ ] LumexUI components render with proper styling
- [ ] Light/dark theme switching works
- [ ] No console errors related to CSS
- [ ] Build completes without CSS-related warnings
- [ ] CSS bundle size is reasonable (<500KB for basic apps)

---

## Step 2.5: Create Global Usings File (🚨 MANDATORY FOR AI AGENTS)

### **CRITICAL AI AGENT REQUIREMENT**
Before registering services, you MUST create a `_GlobalUsings.cs` file at your project root to centralize all namespace imports for `.cs` files.

### Action
Create `_GlobalUsings.cs` in your project root with the following content:

```csharp
// Global using statements for [YourProjectName]
// MANDATORY: This file is required for ALL LumexUI projects
// AI AGENTS: DO NOT skip this step - it's essential for clean code

// ===================================================================
// SYSTEM CORE NAMESPACES
// ===================================================================
global using System;
global using System.Collections.Generic;
global using System.ComponentModel.DataAnnotations;
global using System.Linq;
global using System.Threading;
global using System.Threading.Tasks;

// ===================================================================
// ASP.NET CORE NAMESPACES
// ===================================================================
global using Microsoft.AspNetCore.Builder;
global using Microsoft.AspNetCore.Components;
global using Microsoft.AspNetCore.Components.Web;
global using Microsoft.AspNetCore.Hosting;
global using Microsoft.Extensions.DependencyInjection;
global using Microsoft.Extensions.Hosting;
global using Microsoft.Extensions.Logging;

// ===================================================================
// LUMEXUI NAMESPACES (MANDATORY)
// ===================================================================
global using LumexUI;
global using LumexUI.Extensions;
global using LumexUI.Components;

// ===================================================================
// PROJECT-SPECIFIC NAMESPACES
// ===================================================================
global using [YourProjectName].Components;
global using [YourProjectName].Data;
global using [YourProjectName].Models;
global using [YourProjectName].Services;
```

### Verification
- [ ] `_GlobalUsings.cs` exists at project root
- [ ] File contains all LumexUI namespaces
- [ ] Namespaces are organized by category
- [ ] Categories are sorted alphabetically
- [ ] Replace `[YourProjectName]` with your actual project name

### Why This is MANDATORY
- **Clean Code**: Eliminates using statement pollution in `.cs` files
- **Central Management**: Single location for all namespace dependencies
- **AI Agent Consistency**: Standardized approach across all projects
- **Modern .NET**: Follows current best practices for namespace management

---

## Step 3: Register LumexUI Services

### Action
In `Program.cs`, add LumexUI services (NOTE: NO using statements needed at top - they're in `_GlobalUsings.cs`):

```csharp
// Add after other service registrations - no using statements needed!
builder.Services.AddLumexServices();
```

### Complete Example (Clean Version with Global Usings)
```csharp
// NOTE: No using statements needed at the top!
// All namespaces are centralized in _GlobalUsings.cs

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();  // or AddInteractiveWebAssemblyComponents()

builder.Services.AddLumexServices();

var app = builder.Build();

// Configure pipeline
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();  // or AddInteractiveWebAssemblyRenderMode()

app.Run();
```

### Traditional Version (FOR REFERENCE ONLY - DO NOT USE)
```csharp
// ❌ OLD APPROACH - DO NOT USE WITH GLOBAL USINGS
using LumexUI.Extensions;

// The global usings approach above is preferred for AI agents
```

### Key Benefits of Clean Version
- **No Using Statements**: All namespaces centralized in `_GlobalUsings.cs`
- **Cleaner Code**: Focus on application logic rather than namespace imports
- **AI Agent Consistency**: Standardized approach across all projects
- **Better Maintainability**: Single location for namespace management

### Common Issues (Updated for Global Usings)
- **❌ OLD ISSUE - Missing Using Directive**: This should not happen with global usings
- **✅ NEW ISSUE - Missing _GlobalUsings.cs**: Ensure `_GlobalUsings.cs` exists at project root
- **✅ NEW ISSUE - Namespaces Not Found**: Add missing namespaces to appropriate category in `_GlobalUsings.cs`
- **Placement**: Add after other `.AddServices()` calls
- **Global Usings Not Working**: Ensure `_GlobalUsings.cs` is included in project build
- **Render Mode**: Ensure compatible with your Blazor render mode

---

## Step 4: Include LumexUI JavaScript

### Action
Add LumexUI JavaScript to your main layout file:

**For Blazor Web App/Server**: In `Components/App.razor`
```razor
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="css/app.css" />
    <link rel="stylesheet" href="_content/LumexUI/css/LumexUI.css" />
    <component type="typeof(HeadOutlet)" rendermode="ServerPrerendered" />
</head>
<body>
    <Routes @rendermode="InteractiveServer" />

    <!-- LumexUI JavaScript - CRITICAL: Must be in body -->
    <script src="_framework/blazor.server.js"></script>
    <script src="_content/LumexUI/js/LumexUI.js" type="module"></script>
</body>
</html>
```

### Alternative Locations
- **Blazor WebAssembly**: In `wwwroot/index.html`
- **Legacy Blazor Server**: In `Pages/_Host.cshtml`
- **Blazor Web App**: In `Components/App.razor`

### Critical Requirements
- **Location**: Must be in `<body>`, not `<head>`
- **After Blazor Script**: Load after main Blazor JavaScript
- **Type Attribute**: Use `type="module"` for modern ES modules

### Common Issues
- **Components Not Interactive**: JavaScript missing or incorrectly placed
- **404 Errors**: Incorrect path to LumexUI.js file
- **Load Order**: JavaScript loaded before Blazor framework

---

## Step 5: Add Import Statement

### Action
Add LumexUI namespace to make components available throughout your application:

**Recommended**: Add to `Components/_Imports.razor`:
```razor
@using LumexUI
```

**Alternative**: Add to individual component files as needed:
```razor
@page "/counter"
@using LumexUI

<PageTitle>Counter</PageTitle>

<LumexButton Variant="Variant.Primary">Click me</LumexButton>
```

### Complete _Imports.razor Example
```razor
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using YourProjectName
@using YourProjectName.Components
@using LumexUI
```

### Common Issues
- **Components Not Found**: Missing import statement
- **IntelliSense Not Working**: Import not recognized by IDE
- **Partial Import**: Components available in some files but not others

---

## Optional Enhancements (Recommended)

### Theme Management
Add theme switching JavaScript in your main HTML file's `<head>`:
```html
<script>
    const stored = localStorage.getItem("lumexui.theme") || "system";
    const theme = stored === "system"
        ? (window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light")
        : stored;
    document.documentElement.classList.add(theme);
    document.documentElement.style.colorScheme = theme;
    localStorage.setItem("lumexui.theme", theme);
</script>
```

### Theme Provider Component
Add to `MainLayout.razor` or root component:
```razor
<LumexThemeProvider @rendermode="@InteractiveWebAssembly" />
```

### Basic Theme Toggle Component
```razor
<LumexButton Variant="Variant.Outline" Size="Size.Small" OnClick="ToggleTheme">
    <LumexIcon Icon="@IconName.Sun" />
</LumexButton>

@code {
    private void ToggleTheme()
    {
        var html = document.documentElement;
        if (html.classList.contains("dark"))
        {
            html.classList.remove("dark");
            html.classList.add("light");
            localStorage.setItem("lumexui.theme", "light");
        }
        else
        {
            html.classList.remove("light");
            html.classList.add("dark");
            localStorage.setItem("lumexui.theme", "dark");
        }
    }
}
```

---

## Complete Verification Checklist

### Pre-Installation
- [ ] .NET 8.0+ project created
- [ ] Project builds successfully
- [ ] NuGet package manager accessible

### Step 1: Package Installation
- [ ] LumexUI package added to project
- [ ] Package reference visible in `.csproj` file
- [ ] `dotnet restore` completes without errors

### Step 2: CSS Integration
- [ ] CSS approach selected (A, B, or C)
- [ ] Required dependencies installed (npm, packages)
- [ ] CSS file updated with correct imports
- [ ] `bin/lumexui/` directory exists after build
- [ ] CSS files load without 404 errors
- [ ] Components render with proper styling
- [ ] Theme switching works

### Step 3: Service Registration
- [ ] `using LumexUI.Extensions;` added to Program.cs
- [ ] `builder.Services.AddLumexServices();` called
- [ ] Application builds and runs without service errors

### Step 4: JavaScript Integration
- [ ] LumexUI.js script added to correct location
- [ ] Script placed in `<body>` section
- [ ] Script loads after main Blazor JavaScript
- [ ] No 404 errors for JavaScript files

### Step 5: Import Statement
- [ ] `@using LumexUI;` added to _Imports.razor
- [ ] LumexUI components recognized in Razor files
- [ ] IntelliSense working for LumexUI components

### Final Testing
- [ ] Basic LumexUI components render correctly
- [ ] Component interactivity works (buttons, forms, etc.)
- [ ] Light/dark theme switching functional
- [ ] No console errors related to LumexUI
- [ ] Application performs adequately
- [ ] Responsive design works on mobile devices

---

## Troubleshooting Guide

### Build Issues
1. **Package Not Restored**: Run `dotnet restore` and check internet connection
2. **CSS Build Errors**: Verify Tailwind configuration and file paths
3. **Missing Dependencies**: Ensure all required packages are installed

### Runtime Issues
1. **Components Not Styled**: Check CSS imports and file loading
2. **JavaScript Errors**: Verify script paths and load order
3. **Service Errors**: Confirm LumexUI services are properly registered

### Performance Issues
1. **Slow Loading**: Optimize CSS bundle size and enable compression
2. **Large CSS Files**: Use CSS purging and remove unused styles
3. **Memory Issues**: Monitor component lifecycle and dispose resources

### Common Error Messages
- **"Cannot find LumexUI components"**: Missing import statement
- **"LumexUI.js not found"**: Incorrect JavaScript path
- **"CSS variables undefined"**: Theme import missing
- **"Service not registered"**: Missing service registration

---

## Best Practices

### Development Workflow
1. **Use Approach B** for learning and quick prototyping
2. **Migrate to Approach A** for production applications
3. **Test thoroughly** after each step
4. **Use browser DevTools** to debug CSS and JavaScript issues

### Performance Optimization
1. **Enable CSS purging** in production builds
2. **Use lazy loading** for large applications
3. **Monitor bundle sizes** and optimize as needed
4. **Test on mobile devices** for responsive issues

### Code Organization
1. **Keep CSS imports** at the top of your main CSS file
2. **Group related imports** together
3. **Use descriptive comments** for complex CSS configurations
4. **Maintain consistent naming** throughout your application

### Testing Strategy
1. **Test basic functionality** after completing all steps
2. **Verify component interactions** work correctly
3. **Test theme switching** across all components
4. **Validate responsive design** on various screen sizes
5. **Performance test** with realistic data loads

---

## Repository-Specific Notes

### This Learning Repository
The applications in this repository primarily use **Approach B** (Pre-built CSS files) for simplicity and educational purposes. This makes it easier for learners to focus on LumexUI component usage without complex build configurations.

### Migration Paths
- **Learning → Production**: Start with Approach B, migrate to Approach A for production
- **Simple → Advanced**: Begin with pre-built CSS, add Tailwind CLI as needs grow
- **Individual → Team**: Personal projects can use simpler approaches, teams benefit from full Tailwind setup

### Recommended Learning Order
1. **Component Showcase**: Master LumexUI components using existing setup
2. **Task Manager**: Practice building real applications with Approach B
3. **Portfolio/Weather**: Explore advanced features and customization
4. **Custom Projects**: Choose appropriate approach based on requirements

## Next Steps

After completing LumexUI setup and verification, explore these resources to advance your skills:

### 📚 Continue Learning
- [Advanced LumexUI Guide](./LUMEXUI-ADVANCED-GUIDE.md#--learning-path--mastery) - Structured curriculum from beginner to expert
- [Component Reference](./LUMEXUI-COMPREFERENCE.md) - Quick component reference with 569+ code examples

### 🧪 Advanced Development
- [Component Testing Strategies](./LUMEXUI-ADVANCED-GUIDE.md#---component-testing-with-bunit) - bUnit testing strategies for LumexUI components
- [Custom Component Development](./LUMEXUI-ADVANCED-GUIDE.md#--custom-component-development) - Building custom components and extensions

### 📚 Documentation Resources
- **Component Reference** (`./LUMEXUI-COMPREFERENCE.md`) - 569+ practical component examples
- **Advanced Guide** (`./LUMEXUI-ADVANCED-GUIDE.md`) - Testing, custom components, and best practices
- **Learning Guides** (`../learning/`) - C# and Blazor foundational knowledge
- **Project Documentation** (`../project/`) - Prompts and templates for AI-assisted development

### 🛠️ Development Workflow
1. **Study** the learning guides for C# and Blazor fundamentals
2. **Practice** with component examples in the Component Reference
3. **Build** your own applications using the setup patterns shown
4. **Test** your implementations using the Advanced Guide testing strategies
5. **Extend** LumexUI with custom components when needed

This comprehensive guide provides AI agents with everything needed to successfully integrate LumexUI into Blazor applications, with detailed instructions for different project requirements and thorough troubleshooting guidance.