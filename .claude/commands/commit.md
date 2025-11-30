# Smart Commit Command

Intelligently stage files and generate commit messages based on repository patterns.

## Usage

```bash
/commit [message]
```

## What it does

1. **Analyzes current changes** - Detects modified, added, and deleted files
2. **Identifies affected applications** - Maps files to specific learning applications
3. **Generates intelligent commit messages** - Based on file patterns and repository context
4. **Stages appropriate files** - Smart staging based on change patterns
5. **Creates high-quality commits** - Following repository standards with Claude Code footer

## Examples

```bash
/commit                                    # Auto-generate message and commit all changes
/commit "Add user authentication feature"  # Custom message with smart staging
/commit "Fix styling issues"               # Custom message for CSS/UI fixes
```

## Message Generation Logic

### Pattern Detection

The command analyzes file changes to determine the most appropriate commit message:

**LumexUI Integration Files:**
- `Program.cs`, `_Imports.razor`, `App.razor`, `*.csproj`, `wwwroot/app.css`
- Generated message: `"Update LumexUI integration in [app]"`

**Component Files:**
- `*.razor` (new or modified components)
- Generated message: `"Add [component_name] functionality to [app]"`

**Documentation Files:**
- `*.md`, `docs/**`, `README.md`
- Generated message: `"Update [section] documentation"`

**Styling Files:**
- `app.css`, `*.css`, `tailwind.config.js`
- Generated message: `"Enhance styling in [app]"`

**Configuration Files:**
- `*.csproj`, `package.json`, `appsettings.json`
- Generated message: `"Update configuration for [app]"`

### Application Detection

Files are automatically mapped to their respective applications:

```
src/basics/component-showcase/    → component-showcase
src/basics/task-manager/         → task-manager
src/intermediate/portfolio-blog/ → portfolio-blog
src/intermediate/weather-dashboard/ → weather-dashboard
Root level (*.md)                → repository-level
```

## Commit Message Format

All commits follow the repository standard:

```markdown
<Descriptive Title Case Message>

- <Detailed bullet point description>
- <Additional context about changes>
- <Impact on functionality>

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Safety Features

- **Smart staging** - Groups related files together
- **Context awareness** - Avoids committing unrelated files together
- **Repository standards** - Maintains existing commit quality
- **Clear feedback** - Shows what will be committed before execution

## Implementation Details

The command performs these steps:

1. **File Analysis**: Run `git status --porcelain` to detect changes
2. **Pattern Matching**: Apply pattern detection rules to identify change types
3. **App Detection**: Map files to their respective applications
4. **Message Generation**: Create intelligent commit message or use custom message
5. **Smart Staging**: Group and stage appropriate files
6. **Commit Creation**: Execute commit with standard footer

---

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>