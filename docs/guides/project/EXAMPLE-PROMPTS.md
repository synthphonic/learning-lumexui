# 📝 Claude Code Example Prompts

## 🎯 The Formula
**Always start with:** `Read all guides first, then:`

---

## 🚀 Simple Examples

### **LumexUI Components**
```
Read all guides first, then: create a user profile card using LumexUI with avatar, name, email, and edit button
```

```
Read all guides first, then: build a reusable LumexUI data table with sorting, filtering, and pagination
```

```
Read all guides first, then: create a LumexUI modal dialog with confirm and cancel actions and proper theming
```

### **Blazor Components**
```
Read all guides first, then: create a user profile card component with avatar, name, email, and edit button
```

```
Read all guides first, then: build a reusable data table component with sorting, filtering, and pagination
```

```
Read all guides first, then: create a modal dialog component with confirm and cancel actions
```

### **C# Programming**
```
Read all guides first, then: write a high-performance file processor using C# 13 Span<T> features
```

```
Read all guides first, then: create a REST API service with modern async patterns and error handling
```

```
Read all guides first, then: implement a repository pattern with generic CRUD operations
```

### **Full Features**
```
Read all guides first, then: build a complete user management system with LumexUI:
- User registration and login with beautiful forms
- Profile management with editable components
- Role-based permissions with role badges
- Responsive UI using Tailwind CSS
- Dark mode support
- Unit tests with bUnit
```

### **UI/UX with LumexUI**
```
Read all guides first, then: create a modern dashboard with LumexUI:
- Responsive grid layout
- Interactive charts and statistics
- Real-time notifications
- Theme switching (light/dark)
- Mobile-optimized navigation
- Loading states and error handling
```

---

## 📚 Detailed Examples

### **Example 1: LumexUI Todo Application**
```
Read all guides first, then: create a beautiful TodoList app using LumexUI that allows users to:
- Add new todos with LumexInput and LumexButton
- Mark todos as complete with LumexCheckbox
- Delete todos with LumexButton danger variant
- Filter by status using LumexTabs
- Persist data to localStorage
- Include animations and transitions
- Support dark mode switching
- Show completion statistics with LumexBadge

Should use LumexUI components, modern C# 13 patterns, and Tailwind CSS for styling.
```

### **Example 2: Blazor Component**
```
Read all guides first, then: create a TodoList component that allows users to:
- Add new todos
- Mark todos as complete
- Delete todos
- Filter by status (All/Active/Completed)
- Persist data to localStorage

Should follow modern C# 13 patterns and Blazor best practices.
```

### **Example 3: Real-time Chat with LumexUI**
```
Read all guides first, then: design and implement a beautiful real-time chat application using LumexUI with:
- SignalR integration for live messaging
- LumexUI message bubbles with proper styling
- User authentication with JWT tokens and LumexUI forms
- Message history and search with LumexUI table
- Online user status with LumexBadge indicators
- Typing indicators using LumexSpinner
- Mobile-responsive design with LumexUI components
- Dark mode support
- Comprehensive unit and integration tests with bUnit

Use .NET 9 features, LumexUI components, and modern architectural patterns.
```

### **Example 4: Advanced Feature**
```
Read all guides first, then: design and implement a real-time chat application with:
- SignalR integration for live messaging
- User authentication with JWT tokens
- Message history and search
- Online user status
- Typing indicators
- Mobile-responsive design
- Comprehensive unit and integration tests

Use .NET 9 features and modern architectural patterns.
```

### **Example 6: LumexUI Dashboard Performance**
```
Read all guides first, then: optimize this slow-performing LumexUI Blazor dashboard:
- Current load time: 8 seconds
- Data: 50,000 records from database
- Needs: Real-time updates, filtering, sorting
- Requirements: <2 second load time, memory efficient
- UI: LumexUI components with Tailwind CSS optimization

Please profile, identify bottlenecks, and implement high-performance solutions using:
- C# 13/14 features
- LumexUI virtualized components
- Efficient data binding
- Optimized CSS delivery
- Lazy loading strategies
```

### **Example 7: Performance Optimization**
```
Read all guides first, then: optimize this slow-performing Blazor dashboard:
- Current load time: 8 seconds
- Data: 50,000 records from database
- Needs: Real-time updates, filtering, sorting
- Requirements: <2 second load time, memory efficient

Please profile, identify bottlenecks, and implement high-performance solutions using C# 13/14 features.
```

### **Example 9: E-commerce with LumexUI Frontend**
```
Read all guides first, then: create a complete e-commerce application with LumexUI frontend and .NET API:
- Product catalog with beautiful LumexUI cards and filtering
- Shopping cart with LumexUI components and real-time updates
- Order processing with LumexUI forms and payment integration
- User authentication with LumexUI login/register forms
- Product reviews with LumexUI table and rating components
- Dark mode support and responsive design
- Comprehensive error handling with LumexUI alerts
- Integration tests for both UI and API
- Performance optimization for large product catalogs

Focus on beautiful UI/UX, security, performance, and scalability using LumexUI.
```

### **Example 10: API Development**
```
Read all guides first, then: create a complete e-commerce API with:
- Product catalog with search and filtering
- Shopping cart management
- Order processing with payment integration
- User authentication and authorization
- Rate limiting and caching
- Comprehensive error handling
- OpenAPI documentation
- Integration tests
- Docker containerization

Focus on security, performance, and scalability.
```

### **Example 11: Bug Fixing**
```
Read all guides first, then: help me debug this async method that's causing deadlocks:

```csharp
public async Task ProcessDataAsync()
{
    lock (_lock)
    {
        await _database.SaveChangesAsync();
        _logger.LogInformation("Data processed");
    }
}
```

The application hangs when this method is called. Please explain the issue and provide a thread-safe solution using modern .NET patterns.
```

---

## 🎨 LumexUI Specific Examples

### **LumexUI Form Design**
```
Read all guides first, then: create a beautiful multi-step registration form using LumexUI:
- Form progress with LumexSteps component
- Advanced validation with real-time feedback
- Conditional fields based on user selections
- Password strength indicator with LumexProgress
- File upload with LumexFileInput and preview
- Dark mode support
- Responsive design for mobile devices
- Accessibility features with proper ARIA labels
- Unit tests using bUnit for all form interactions
```

### **LumexUI Dashboard Creation**
```
Read all guides first, then: build a comprehensive analytics dashboard using LumexUI:
- Real-time data updates with SignalR
- Interactive charts and graphs integration
- KPI cards with LumexCard and LumexBadge
- Data filtering with LumexSelect and LumexInput
- Export functionality with LumexButton
- Theme switcher for light/dark modes
- Responsive grid layout using Tailwind CSS
- Loading states with LumexSpinner
- Error handling with LumexAlert components
```

### **LumexUI Component Library**
```
Read all guides first, then: create a reusable LumexUI component library for our company:
- Custom themed components matching brand colors
- Responsive design patterns
- Accessibility compliance (WCAG 2.1)
- Dark mode support with CSS variables
- Component documentation and examples
- Unit tests for all components
- TypeScript definitions for better IDE support
- Storybook-style component showcase
```

## 🔧 Technical Specific Examples

### **Database Integration with LumexUI**
```
Read all guides first, then: create a beautiful Blazor CRUD interface using LumexUI for a Product entity with:
- Dapper high-performance data access integration
- Repository pattern implementation
- LumexUI forms with validation
- LumexUI tables for data display
- LumexUI modals for create/edit operations
- Error handling with LumexAlert
- Unit tests with in-memory database and bUnit
```

### **Authentication & Security with LumexUI**
```
Read all guides first, then: implement secure authentication with beautiful LumexUI forms:
- JWT token-based auth with LumexUI login form
- Registration form with LumexInput and validation
- Password recovery with LumexModal
- Refresh token rotation
- Role-based authorization with LumexUI role badges
- Password hashing with BCrypt
- Rate limiting for login attempts
- Security headers middleware
- Mobile-responsive login pages
```

### **Performance & Optimization with LumexUI**
```
Read all guides first, then: create a high-performance image gallery using LumexUI that can:
- Handle 10,000+ images without lag
- Lazy load images with LumexCard placeholders
- Virtualized scrolling for large datasets
- Cache thumbnails efficiently
- Support drag-and-drop upload
- Process images in background
- Use memory pools for buffer management
```

---

## 🎨 UI/UX Examples

### **Dashboard Creation**
```
Read both guides first, then: build an admin dashboard with:
- Real-time metrics and charts
- Data visualization using Chart.js
- Responsive grid layout
- Dark/light theme toggle
- Export functionality
- Accessibility features (ARIA labels, keyboard navigation)
```

### **Form Handling**
```
Read both guides first, then: create a multi-step wizard form for user onboarding with:
- Progress indicators
- Field validation with custom rules
- Auto-save functionality
- Back/forward navigation
- Confirmation dialog
- Error recovery
```

---

## 🧪 Testing Examples

### **Unit Testing**
```
Read both guides first, then: write comprehensive unit tests for a ShoppingCart class that includes:
- Happy path scenarios
- Edge cases (empty cart, invalid quantities)
- Error conditions
- Mock dependencies
- Performance benchmarks
- Code coverage >90%
```

### **Integration Testing**
```
Read both guides first, then: create integration tests for a user registration API that:
- Tests complete user flow
- Validates database state
- Checks email sending
- Verifies security headers
- Tests concurrent registrations
- Uses Testcontainers for isolated testing
```

---

### **Dashboard Creation with LumexUI**
```
Read all guides first, then: build an admin dashboard using LumexUI with:
- Real-time metrics and charts with LumexCard
- Data visualization using Chart.js integration
- Responsive grid layout with Tailwind CSS
- Dark/light theme toggle using LumexSwitch
- Export functionality with LumexButton
- Accessibility features (ARIA labels, keyboard navigation)
- Beautiful loading states with LumexSpinner
- Interactive filters with LumexSelect
```

### **Form Handling with LumexUI**
```
Read all guides first, then: create a multi-step wizard form using LumexUI for user onboarding with:
- Progress indicators with LumexSteps
- Field validation with LumexInput and real-time feedback
- Auto-save functionality with localStorage
- Back/forward navigation with LumexButton
- Confirmation dialog with LumexModal
- Error recovery with LumexAlert components
- File upload with LumexFileInput
```

---

## 🧪 Testing Examples

### **LumexUI Component Testing**
```
Read all guides first, then: write comprehensive unit tests for LumexUI components that includes:
- Component rendering tests with bUnit
- User interaction testing (clicks, form submissions)
- Accessibility testing (ARIA labels, keyboard navigation)
- Theme switching functionality
- Responsive design testing
- Mock data and service dependencies
- Performance benchmarks for component rendering
- Code coverage >90%
```

### **Unit Testing**
```
Read all guides first, then: write comprehensive unit tests for a ShoppingCart class that includes:
- Happy path scenarios
- Edge cases (empty cart, invalid quantities)
- Error conditions
- Mock dependencies
- Performance benchmarks
- Code coverage >90%
```

### **Integration Testing**
```
Read all guides first, then: create integration tests for a user registration API that:
- Tests complete user flow
- Validates database state
- Checks email sending
- Verifies security headers
- Tests concurrent registrations
- Uses Testcontainers for isolated testing
```

---

## 💡 Pro Tips

### **Be Specific About:**
- **UI Framework**: Specify LumexUI components and features
- **Technology stack** (.NET 9, C# 13/14, Blazor Server/WASM)
- **Performance requirements** (load time, memory usage, concurrent users)
- **Security needs** (authentication, authorization, data protection)
- **Testing requirements** (unit tests with bUnit, integration tests, coverage goals)
- **Accessibility requirements** (WCAG compliance, ARIA labels)
- **Theme support** (light/dark mode, custom theming)
- **Responsive design** (mobile-first approach)
- **Deployment environment** (Docker, Azure, on-premises)

### **Include Context When:**
- Debugging existing LumexUI code
- Extending current functionality
- Migrating legacy systems to LumexUI
- Working with specific design constraints
- Creating custom LumexUI components

---

**Remember:** Just start with `Read all guides first, then:` and you'll get expert-level results with beautiful LumexUI interfaces every time! 🚀