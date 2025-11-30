# 🚀 Smart Development Prompt Template

## Standard Prompt Structure

Copy and paste this template for consistent, context-aware development:

```
TASK: [Your specific task here]

Claude automatically reads claude.md and all learning guides first.

REQUIREMENTS:
- Follow modern C# 13/14 patterns from the C# guide
- Use Blazor best practices from the Blazor guide
- Use LumexUI components and best practices
- Target .NET 10+ only
- Include comprehensive testing with bUnit
- Focus on performance, accessibility, and security

CONTEXT:
[Additional context about your specific project/requirements]

EXPECTED OUTPUT:
[What you want Claude to deliver - code, analysis, plan, etc.]
```

## Example Usage

```
TASK: Create a reusable Blazor data table component using LumexUI

REQUIREMENTS:
- Follow modern C# 13/14 patterns from the C# guide
- Use Blazor best practices from the Blazor guide
- Use LumexUI components and best practices
- Target .NET 10+ only
- Include comprehensive testing with bUnit
- Focus on performance, accessibility, and security

CONTEXT:
I need a data table that can handle 10,000+ rows efficiently with sorting, filtering, and pagination. Should be accessible and themeable.

EXPECTED OUTPUT:
1. Complete Blazor component code
2. Unit tests using bUnit
3. Integration test example
4. Performance analysis
5. Usage documentation
```

---

*Use this template to ensure Claude always has the proper context before starting work.*