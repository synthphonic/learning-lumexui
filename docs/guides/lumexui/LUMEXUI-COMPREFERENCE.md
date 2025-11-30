# LumexUI Component Reference & Examples

## Overview

This document provides comprehensive examples and reference information for all LumexUI components. It serves as the definitive resource for component usage patterns, parameters, and practical implementations.

For installation and setup instructions, see the [LUMEXUI-SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md).

For advanced topics including testing, custom components, and learning progression, see the [LUMEXUI-ADVANCED-GUIDE.md](./LUMEXUI-ADVANCED-GUIDE.md).

## Quick Navigation

### Component Categories
- [📝 Form Components](#form-components) - Input, Select, Checkbox, Radio, etc.
- [🎯 Interactive Components](#interactive-components) - Button, Dropdown, Modal, etc.
- [📊 Data Display Components](#data-display-components) - Table, List, Badge, etc.
- [🏗️ Layout Components](#layout-components) - Card, Container, Divider, etc.
- [🧭 Navigation Components](#navigation-components) - Tabs, Accordion, Breadcrumb, etc.
- [💬 Feedback Components](#feedback-components) - Alert, Toast, Spinner, etc.
- [🎨 Styling & Theming](#styling--theming) - Color variants, sizes, themes

### Quick Reference
- **Need to install LumexUI?** → [SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md)
- **Looking for learning progression?** → [ADVANCED-GUIDE.md](./LUMEXUI-ADVANCED-GUIDE.md)
- **Component parameter reference** → See individual component sections below

---

## 📝 Form Components

### LumexButton

**Basic Button Component**
```razor
@using LumexUI

<LumexButton>Click me</LumexButton>
```

**Button Color Variants**
```razor
<LumexButton Variant="Variant.Default">Default</LumexButton>
<LumexButton Variant="Variant.Primary">Primary</LumexButton>
<LumexButton Variant="Variant.Secondary">Secondary</LumexButton>
<LumexButton Variant="Variant.Success">Success</LumexButton>
<LumexButton Variant="Variant.Warning">Warning</LumexButton>
<LumexButton Variant="Variant.Danger">Danger</LumexButton>
<LumexButton Variant="Variant.Info">Info</LumexButton>
```

**Button with Icons**
```razor
<LumexButton Variant="Variant.Primary" OnClick="HandleClick">
    <LumexIcon Icon="Plus" />
    Add Item
</LumexButton>
```

**Button Sizes**
```razor
<LumexButton Size="Size.Small">Small</LumexButton>
<LumexButton Size="Size.Medium">Medium</LumexButton>
<LumexButton Size="Size.Large">Large</LumexButton>
```

**Loading State**
```razor
<LumexButton Variant="Variant.Primary" Loading="@isLoading" Disabled="@isLoading">
    @isLoading ? "Processing..." : "Submit"
</LumexButton>

@code {
    private bool isLoading = false;

    private async Task HandleClick()
    {
        isLoading = true;
        await Task.Delay(2000);
        isLoading = false;
    }
}
```

### LumexInput / LumexTextField

**Basic Text Input**
```razor
<div class="w-full flex flex-wrap gap-4 md:flex-nowrap">
    <LumexInput Type="InputType.Email" Label="Email" />
    <LumexInput Type="InputType.Email" Label="Email" Placeholder="Enter your email" />
</div>
```

**Input with Variants**
```razor
<div class="w-full grid grid-cols-1 gap-4">
    @foreach( var variant in _variants )
    {
        <div>
            <div class="w-full flex flex-wrap gap-4 md:flex-nowrap">
                <LumexInput Variant="@variant" Type="InputType.Email" Label="Email" />
                <LumexInput Variant="@variant" Type="InputType.Email" Label="Email" Placeholder="Enter your email" />
            </div>
            <small class="text-default-400 mt-1">@variant</small>
        </div>
    }
</div>

@code {
    private InputVariant[] _variants = [
        InputVariant.Flat,
        InputVariant.Outlined,
        InputVariant.Underlined
    ];
}
```

**Advanced Input with Start/End Content**
```razor
@using LumexUI.Shared.Icons

<div class="grid gap-4">
    <div class="w-full grid grid-cols-1 gap-4">
        <div>
            <div class="w-full flex flex-wrap gap-4 md:flex-nowrap">
                <LumexInput Type="InputType.Url"
                             Label="Website"
                             Placeholder="lumexui"
                             LabelPlacement="LabelPlacement.Outside"
                             StartContent="@_protocolContent"
                             EndContent="@_domainContent" />

                <LumexInput Type="InputType.Search"
                             Label="Search"
                             Placeholder="Type to search..."
                             LabelPlacement="LabelPlacement.Outside"
                             StartContent="@_searchIcon"
                             EndContent="@_micIcon" />

                <LumexInput Type="InputType.Email"
                             Label="Email"
                             Placeholder="john"
                             LabelPlacement="LabelPlacement.Outside"
                             StartContent="@_mailIcon"
                             EndContent="@_emailDomainContent" />
            </div>
            <small class="text-default-500 mt-1">Both</small>
        </div>
    </div>
</div>

@code {
    private RenderFragment _mailIcon = @<MailIcon Size="20" class="text-default-400 shrink-0" />;
    private RenderFragment _searchIcon = @<SearchIcon Size="20" class="text-default-400 shrink-0" />;
    private RenderFragment _micIcon = @<MicIcon Size="20" class="text-default-400 shrink-0" />;

    private RenderFragment _protocolContent =
        @<div class="pointer-events-none flex items-center">
            <span class="text-default-400 text-small">https://</span>
        </div>;

    private RenderFragment _domainContent =
        @<div class="pointer-events-none flex items-center">
            <span class="text-default-400 text-small">.org</span>
        </div>;

    private RenderFragment _emailDomainContent =
        @<div class="pointer-events-none flex items-center">
            <span class="text-default-400 text-small">@doe.com</span>
        </div>;
}
```

**Form Integration with Validation**
```razor
<EditForm Model="@userModel" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />

    <div class="space-y-4">
        <div>
            <LumexLabel For="name">Full Name</LumexLabel>
            <LumexInput @bind-Value="userModel.Name" id="name" />
            <ValidationMessage For="@(() => userModel.Name)" />
        </div>

        <div>
            <LumexLabel For="email">Email Address</LumexLabel>
            <LumexInput @bind-Value="userModel.Email" id="email" Type="InputType.Email" />
            <ValidationMessage For="@(() => userModel.Email)" />
        </div>

        <LumexButton Type="ButtonType.Submit" Variant="Variant.Primary">Submit</LumexButton>
    </div>
</EditForm>

@code {
    private UserRegistrationModel userModel = new();

    private void HandleSubmit()
    {
        Console.WriteLine($"Form submitted: {userModel.Name} - {userModel.Email}");
    }

    public class UserRegistrationModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; } = "";

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        public string Email { get; set; } = "";
    }
}
```

### LumexSelect

**Basic Select Component**
```razor
<LumexSelect TValue="string" Label="Favorite Animal" Placeholder="Select an animal">
    @foreach (var animal in _animals)
    {
        <LumexSelectItem @key="@animal" Value="@animal.Key">@animal.Value</LumexSelectItem>
    }
</LumexSelect>

@code {
    private Dictionary<string, string> _animals = new()
    {
        ["cat"] = "Cat",
        ["dog"] = "Dog",
        ["elephant"] = "Elephant",
        ["lion"] = "Lion",
        ["tiger"] = "Tiger",
        ["giraffe"] = "Giraffe",
        ["dolphin"] = "Dolphin",
        ["penguin"] = "Penguin",
        ["zebra"] = "Zebra",
        ["shark"] = "Shark",
        ["whale"] = "Whale",
        ["otter"] = "Otter",
        ["crocodile"] = "Crocodile"
    };
}
```

**Select with Description**
```razor
<LumexSelect TValue="string"
             Label="Favorite Animal"
             Placeholder="Select an animal"
             Description="Select your favorite animal"
             Class="max-w-xs">
    @foreach (var animal in _animals)
    {
        <LumexSelectItem @key="@animal" Value="@animal.Key">@animal.Value</LumexSelectItem>
    }
</LumexSelect>
```

### LumexCheckbox

**Basic Checkbox**
```razor
<LumexCheckbox @bind-Checked="acceptTerms">
    I accept the terms and conditions
</LumexCheckbox>

@code {
    private bool acceptTerms = false;
}
```

**Multiple Checkboxes**
```razor
<div class="space-y-3">
    <LumexCheckbox @bind-Checked="emailNotifications">
        Email notifications for account updates
    </LumexCheckbox>

    <LumexCheckbox @bind-Checked="marketingEmails">
        Marketing emails and newsletters
    </LumexCheckbox>

    <LumexCheckbox @bind-Checked="pushNotifications">
        Push notifications for important updates
    </LumexCheckbox>
</div>

@code {
    private bool emailNotifications = true;
    private bool marketingEmails = false;
    private bool pushNotifications = true;
}
```

### LumexRadio

**Radio Button Group**
```razor
<LumexLabel>Notification Preferences</LumexLabel>
<div class="space-y-2">
    <LumexRadio @bind-Value="notificationType" Value="email">Email Notifications</LumexRadio>
    <LumexRadio @bind-Value="notificationType" Value="sms">SMS Notifications</LumexRadio>
    <LumexRadio @bind-Value="notificationType" Value="push">Push Notifications</LumexRadio>
</div>

@code {
    private string notificationType = "email";
}
```

### LumexSwitch

**Basic Switch**
```razor
<LumexSwitch @bind-Checked="darkMode">
    Enable Dark Mode
</LumexSwitch>

@code {
    private bool darkMode = false;
}
```

**Switch with Icons**
```razor
@using LumexUI.Shared.Icons

<LumexSwitch Value="@true">
    <StartContent>
        <SunIcon />
    </StartContent>
    <ChildContent>
        Dark Mode
    </ChildContent>
    <EndContent>
        <MoonIcon />
    </EndContent>
</LumexSwitch>
```

### LumexTextarea

**Basic Textarea**
```razor
<LumexTextarea @bind-Value="message"
                Label="Message"
                Placeholder="Enter your message here..."
                Rows="4" />

@code {
    private string message = "";
}
```

---

## 🎯 Interactive Components

### LumexDropdown

**Basic Dropdown**
```razor
<LumexDropdown>
    <LumexDropdownTrigger>
        <LumexButton Variant="Variant.Outlined">
            Open Dropdown
        </LumexButton>
    </LumexDropdownTrigger>
    <LumexDropdownMenu>
        <LumexDropdownItem>New file</LumexDropdownItem>
        <LumexDropdownItem>Edit file</LumexDropdownItem>
        <LumexDropdownItem>Share file</LumexDropdownItem>
        <LumexDropdownItem Color="ThemeColor.Danger" Class="text-danger">
            Delete file
        </LumexDropdownItem>
    </LumexDropdownMenu>
</LumexDropdown>
```

### LumexModal

**Basic Modal**
```razor
<LumexButton Variant="Variant.Primary" @onclick="() => showModal = true">
    Open Modal
</LumexButton>

<LumexModal IsOpen="@showModal" OnClose="() => showModal = false">
    <LumexModalContent>
        <LumexModalHeader>
            <LumexModalTitle>Modal Title</LumexModalTitle>
            <LumexModalCloseButton />
        </LumexModalHeader>
        <LumexModalBody>
            <p>This is the modal content. You can put any content here.</p>
        </LumexModalBody>
        <LumexModalFooter>
            <LumexButton Variant="Variant.Secondary" @onclick="() => showModal = false">
                Cancel
            </LumexButton>
            <LumexButton Variant="Variant.Primary" @onclick="HandleConfirm">
                Confirm
            </LumexButton>
        </LumexModalFooter>
    </LumexModalContent>
</LumexModal>

@code {
    private bool showModal = false;

    private void HandleConfirm()
    {
        Console.WriteLine("Confirmed!");
        showModal = false;
    }
}
```

### LumexPopover

**Basic Popover**
```razor
<LumexPopover>
    <LumexPopoverTrigger>
        <LumexButton>Open Popover</LumexButton>
    </LumexPopoverTrigger>
    <LumexPopoverContent>
        <div class="px-1 py-2">
            <div class="text-small font-bold">Oh, hi there!</div>
            <div class="text-tiny">I am the popover content</div>
        </div>
    </LumexPopoverContent>
</LumexPopover>
```

### LumexTabs

**Basic Tabs**
```razor
<LumexTabs>
    <LumexTabsList>
        <LumexTabsTrigger Value="profile">Profile</LumexTabsTrigger>
        <LumexTabsTrigger Value="settings">Settings</LumexTabsTrigger>
        <LumexTabsTrigger Value="activity">Activity</LumexTabsTrigger>
    </LumexTabsList>

    <LumexTabsContent Value="profile">
        <div class="p-4">
            <h3>Profile Content</h3>
            <p>User profile information goes here.</p>
        </div>
    </LumexTabsContent>

    <LumexTabsContent Value="settings">
        <div class="p-4">
            <h3>Settings Content</h3>
            <p>Application settings go here.</p>
        </div>
    </LumexTabsContent>

    <LumexTabsContent Value="activity">
        <div class="p-4">
            <h3>Activity Content</h3>
            <p>Recent activity goes here.</p>
        </div>
    </LumexTabsContent>
</LumexTabs>
```

**Advanced Tabs with Icons and Badges**
```razor
@using LumexUI.Shared.Icons

<div class="w-full flex flex-col">
    <LumexTabs Variant="TabVariant.Underlined" Classes="@_classes">
        <LumexTab Id="photos">
            <TitleContent>
                <div class="flex items-center space-x-2">
                    <ImagesIcon Size="20" />
                    <span>Photos</span>
                    <LumexChip Size="Size.Small" Variant="ChipVariant.Flat">
                        9
                    </LumexChip>
                </div>
            </TitleContent>
        </LumexTab>
        <LumexTab Id="music">
            <TitleContent>
                <div class="flex items-center space-x-2">
                    <MusicIcon Size="20" />
                    <span>Music</span>
                    <LumexChip Size="Size.Small" Variant="ChipVariant.Flat">
                        2
                    </LumexChip>
                </div>
            </TitleContent>
        </LumexTab>
        <LumexTab Id="videos">
            <TitleContent>
                <div class="flex items-center space-x-2">
                    <ClapperboardIcon Size="20" />
                    <span>Videos</span>
                    <LumexChip Size="Size.Small" Variant="ChipVariant.Flat">
                        1
                    </LumexChip>
                </div>
            </TitleContent>
        </LumexTab>
    </LumexTabs>
</div>

@code {
    private TabsSlots _classes = new()
    {
        TabList = "gap-6 w-full rounded-none p-0 border-b border-divider",
        Tab = "max-w-fit px-0 h-12",
        TabContent = "group-data-[selected=true]:text-secondary-500",
        Cursor = "w-full bg-secondary-500"
    };
}
```

---

## 📊 Data Display Components

### LumexTable

**Basic Table**
```razor
<LumexTable>
    <LumexTableHead>
        <LumexTableRow>
            <LumexTableHeader>Name</LumexTableHeader>
            <LumexTableHeader>Email</LumexTableHeader>
            <LumexTableHeader>Role</LumexTableHeader>
            <LumexTableHeader>Actions</LumexTableHeader>
        </LumexTableRow>
    </LumexTableHead>
    <LumexTableBody>
        @foreach (var user in users)
        {
            <LumexTableRow>
                <LumexTableCell>@user.Name</LumexTableCell>
                <LumexTableCell>@user.Email</LumexTableCell>
                <LumexTableCell>@user.Role</LumexTableCell>
                <LumexTableCell>
                    <div class="flex space-x-2">
                        <LumexButton Size="Size.Small" Variant="Variant.Primary">
                            Edit
                        </LumexButton>
                        <LumexButton Size="Size.Small" Variant="Variant.Danger">
                            Delete
                        </LumexButton>
                    </div>
                </LumexTableCell>
            </LumexTableRow>
        }
    </LumexTableBody>
</LumexTable>

@code {
    private readonly User[] users = new[]
    {
        new User("John Doe", "john@example.com", "Admin"),
        new User("Jane Smith", "jane@example.com", "User"),
        new User("Bob Johnson", "bob@example.com", "Guest")
    };

    public record User(string Name, string Email, string Role);
}
```

### LumexBadge

**Basic Badge**
```razor
<LumexBadge Text="5" Color="ThemeColor.Danger" />
<LumexBadge Text="New" Color="ThemeColor.Success" />
<LumexBadge Text="Updated" Color="ThemeColor.Info" />
```

**Badge with Avatar**
```razor
@using LumexUI.Shared.Icons

<div class="flex gap-6 items-center">
    <LumexBadge Text="5" Color="ThemeColor.Danger">
        <LumexAvatar Radius="Radius.Medium" Src="https://i.pravatar.cc/150?img=2" />
    </LumexBadge>

    <LumexBadge Color="ThemeColor.Success" Placement="BadgePlacement.BottomEnd">
        <LumexAvatar Radius="Radius.Full" Src="https://i.pravatar.cc/150?img=6" />
    </LumexBadge>

    <LumexBadge Text="new" Color="ThemeColor.Danger" Size="Size.Small">
        <LumexAvatar Bordered="@true"
                     Radius="Radius.Medium"
                     Color="ThemeColor.Danger"
                     Src="https://i.pravatar.cc/150?img=9" />
    </LumexBadge>
</div>
```

### LumexAvatar

**Basic Avatar**
```razor
<LumexAvatar Name="John Doe" />
<LumexAvatar Name="Jane Smith" Size="AvatarSize.Large" />
<LumexAvatar Radius="Radius.Full" Size="AvatarSize.Small" />
```

**Avatar with Image**
```razor
<LumexAvatar Src="https://i.pravatar.cc/150?img=1" Size="AvatarSize.Medium" />
<LumexAvatar Src="https://i.pravatar.cc/150?img=2" Radius="Radius.Full" />
```

### LumexList

**Basic List**
```razor
<LumexList>
    <LumexListItem>
        <LumexListItemIcon>
            <LumexIcon Icon="Home" />
        </LumexListItemIcon>
        Dashboard
    </LumexListItem>
    <LumexListItem>
        <LumexListItemIcon>
            <LumexIcon Icon="Users" />
        </LumexListItemIcon>
        Users
    </LumexListItem>
    <LumexListItem>
        <LumexListItemIcon>
            <LumexIcon Icon="Settings" />
        </LumexListItemIcon>
        Settings
    </LumexListItem>
</LumexList>
```

### LumexChip

**Basic Chip**
```razor
<LumexChip>Default Chip</LumexChip>
<LumexChip Color="ThemeColor.Primary">Primary Chip</LumexChip>
<LumexChip Color="ThemeColor.Success">Success Chip</LumexChip>
<LumexChip Color="ThemeColor.Danger">Danger Chip</LumexChip>
```

**Chip with Icons**
```razor
@using LumexUI.Shared.Icons

<div class="flex gap-4">
    <LumexChip Color="ThemeColor.Success" Variant="ChipVariant.Flat">
        <StartContent>
            <CircleCheckFilledIcon Size="18" />
        </StartContent>
        <ChildContent>Chip</ChildContent>
    </LumexChip>

    <LumexChip Color="ThemeColor.Secondary" Variant="ChipVariant.Flat">
        <ChildContent>Chip</ChildContent>
        <EndContent>
            <BellFilledIcon Size="18" />
        </EndContent>
    </LumexChip>
</div>
```

---

## 🏗️ Layout Components

### LumexCard

**Basic Card**
```razor
<LumexCard>
    <LumexCardHeader>
        <LumexCardTitle>Card Title</LumexCardTitle>
        <LumexCardDescription>Card description goes here</LumexCardDescription>
    </LumexCardHeader>
    <LumexCardBody>
        <p>This is the main content of the card.</p>
    </LumexCardBody>
    <LumexCardFooter>
        <LumexButton Variant="Variant.Primary">Action</LumexButton>
    </LumexCardFooter>
</LumexCard>
```

**Card with Custom Content**
```razor
<LumexCard>
    <LumexCardBody>
        <div class="space-y-4">
            <div class="flex items-center space-x-4">
                <LumexAvatar Size="AvatarSize.Lg">
                    <LumexAvatarFallback>JD</LumexAvatarFallback>
                </LumexAvatar>
                <div>
                    <h3 class="text-lg font-semibold">John Doe</h3>
                    <p class="text-gray-600">john@example.com</p>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <div>
                    <LumexLabel>Role</LumexLabel>
                    <LumexBadge Color="ThemeColor.Primary">Admin</LumexBadge>
                </div>
                <div>
                    <LumexLabel>Status</LumexLabel>
                    <LumexBadge Color="ThemeColor.Success">Active</LumexBadge>
                </div>
            </div>
        </div>
    </LumexCardBody>
</LumexCard>
```

### LumexContainer

**Basic Container**
```razor
<LumexContainer>
    <p>This content is wrapped in a responsive container.</p>
</LumexContainer>

<LumexContainer MaxWidth="MaxWidth.Medium">
    <p>This container has a maximum width of medium.</p>
</LumexContainer>
```

### LumexDivider

**Basic Divider**
```razor
<div class="max-w-sm grid gap-4 p-6 rounded-large bg-background ring ring-default-900/10">
    <div class="flex gap-4">
        <LumexAvatar Name="Rachel Green" />
        <div class="space-y-1">
            <h4 class="font-semibold text-small">Coffee Catch-up?</h4>
            <p class="text-small text-foreground-500 line-clamp-2">
                Hi Rachel, Hope you're doing well! Are you free for a quick coffee catch-up later this week?
            </p>
        </div>
    </div>
    <LumexDivider />
    <div class="flex gap-4">
        <LumexAvatar Name="Sarah Salvatore" />
        <div class="space-y-1">
            <h4 class="font-semibold text-small">Welcome to the Team!</h4>
            <p class="text-small text-foreground-500 line-clamp-2">
                Hey Sarah, Welcome aboard! We're excited to have you with us.
            </p>
        </div>
    </div>
</div>
```

---

## 🧭 Navigation Components

### LumexAccordion

**Basic Accordion**
```razor
<LumexAccordion>
    <LumexAccordionItem Value="1" Title="Section 1">
        <ChildContent>
            <p>Content for section 1 goes here.</p>
        </ChildContent>
    </LumexAccordionItem>
    <LumexAccordionItem Value="2" Title="Section 2">
        <ChildContent>
            <p>Content for section 2 goes here.</p>
        </ChildContent>
    </LumexAccordionItem>
</LumexAccordion>
```

**Advanced Accordion with Icons**
```razor
<LumexAccordion Variant="AccordionVariant.Shadow"
                ShowDividers="@false"
                Class="p-2 flex flex-col gap-1 max-w-sm"
                ItemClasses="@_itemClasses">
    <LumexAccordionItem Id="1" Title="Connected Devices">
        <StartContent>
            <MonitorSmartphoneIcon class="text-primary" />
        </StartContent>
        <SubtitleContent>
            2 issues to <span class="text-primary">fix now</span>
        </SubtitleContent>
        <ChildContent>@_content</ChildContent>
    </LumexAccordionItem>
    <LumexAccordionItem Id="2" Title="Apps Permissions" Subtitle="3 apps have read permissions">
        <StartContent>
            <ShieldAlertIcon />
        </StartContent>
        <ChildContent>@_content</ChildContent>
    </LumexAccordionItem>
</LumexAccordion>

@code {
    private RenderFragment _content = @<text>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</text>;

    private AccordionItemSlots _itemClasses = new()
    {
        Title = "font-medium text-medium",
        Trigger = "px-2 py-0 hover:bg-default-100 rounded-lg h-14",
        Content = "text-small p-2"
    };
}
```

### LumexListbox

**Basic Listbox**
```razor
<div class="w-full max-w-60 px-1 py-2 border border-default-900/10 rounded-small">
    <LumexListbox TValue="string">
        <LumexListboxItem>New file</LumexListboxItem>
        <LumexListboxItem>Edit file</LumexListboxItem>
        <LumexListboxItem>Share file</LumexListboxItem>
        <LumexListboxItem Color="ThemeColor.Danger" Class="text-danger">
            Delete file
        </LumexListboxItem>
    </LumexListbox>
</div>
```

---

## 💬 Feedback Components

### LumexAlert

**Basic Alerts**
```razor
<div class="space-y-4">
    <!-- Success Alert -->
    <LumexAlert Variant="Variant.Success" Show="@showSuccess">
        <LumexAlertIcon>
            <LumexIcon Icon="CheckCircle" />
        </LumexAlertIcon>
        <LumexAlertTitle>Success!</LumexAlertTitle>
        <LumexAlertDescription>Your changes have been saved successfully.</LumexAlertDescription>
        <LumexAlertCloseButton />
    </LumexAlert>

    <!-- Warning Alert -->
    <LumexAlert Variant="Variant.Warning" Show="@showWarning">
        <LumexAlertIcon>
            <LumexIcon Icon="AlertTriangle" />
        </LumexAlertIcon>
        <LumexAlertTitle>Warning</LumexAlertTitle>
        <LumexAlertDescription>Please review your changes before proceeding.</LumexAlertDescription>
    </LumexAlert>

    <!-- Error Alert -->
    <LumexAlert Variant="Variant.Danger" Show="@showError">
        <LumexAlertIcon>
            <LumexIcon Icon="XCircle" />
        </LumexAlertIcon>
        <LumexAlertTitle>Error</LumexAlertTitle>
        <LumexAlertDescription>Failed to save changes. Please try again.</LumexAlertDescription>
        <LumexAlertCloseButton />
    </LumexAlert>
</div>

@code {
    private bool showSuccess = true;
    private bool showWarning = false;
    private bool showError = false;
}
```

### LumexSpinner

**Loading Spinners**
```razor
<div class="space-y-4">
    <LumexSpinner Size="SpinnerSize.Small" />
    <LumexSpinner Size="SpinnerSize.Medium" />
    <LumexSpinner Size="SpinnerSize.Large" />

    <!-- Colored Spinners -->
    <LumexSpinner Color="ThemeColor.Primary" Size="SpinnerSize.Large" />
    <LumexSpinner Color="ThemeColor.Success" Size="SpinnerSize.Large" />
</div>
```

### LumexProgress

**Progress Bars**
```razor
<div class="space-y-4">
    <LumexProgress Value="@progressValue" Max="100" />
    <p>Upload Progress: @progressValue%</p>

    <!-- Success Progress -->
    <LumexProgress Value="100" Max="100" Color="ThemeColor.Success" />
    <LumexProgress Value="60" Max="100" Color="ThemeColor.Warning" />
    <LumexProgress Value="30" Max="100" Color="ThemeColor.Danger" />
</div>

@code {
    private int progressValue = 75;
}
```

### LumexSkeleton

**Skeleton Loading States**
```razor
<div class="max-w-[300px] w-full flex items-center gap-3">
    <div>
        <LumexSkeleton Class="flex rounded-full w-12 h-12" />
    </div>
    <div class="w-full flex flex-col gap-2">
        <LumexSkeleton Class="h-3 w-3/5 rounded-lg" />
        <LumexSkeleton Class="h-3 w-4/5 rounded-lg" />
    </div>
</div>
```

---

## 🎨 Styling & Theming

### Color Variants

LumexUI components support consistent color variants across the library:

```razor
<!-- Available color variants -->
Variant.Default    <!-- Gray/neutral color -->
Variant.Primary    <!-- Primary brand color (blue) -->
Variant.Secondary  <!-- Secondary color (gray) -->
Variant.Success    <!-- Success state (green) -->
Variant.Warning    <!-- Warning state (yellow) -->
Variant.Danger     <!-- Error/danger state (red) -->
Variant.Info       <!-- Information state (cyan) -->
```

### Size Variants

Most components support multiple size options:

```razor
<!-- Common size variants -->
Size.Small    <!-- Compact version -->
Size.Medium   <!-- Default size -->
Size.Large    <!-- Larger version -->
```

### Theme Integration

LumexUI uses CSS custom properties for theming:

```css
:root {
    --lumex-primary: 221.2 83.2% 53.3%;
    --lumex-primary-foreground: 210 40% 98%;
    --lumex-secondary: 210 40% 96.1%;
    --lumex-secondary-foreground: 222.2 84% 4.9%;
    --lumex-success: 142.1 76.2% 36.3%;
    --lumex-success-foreground: 355.7 100% 94.1%;
    --lumex-danger: 0 84.2% 60.2%;
    --lumex-danger-foreground: 210 40% 98%;
    --lumex-warning: 45.1 91.2% 59.3%;
    --lumex-warning-foreground: 15.6 86.7% 60.8%;
    --lumex-info: 217.2 91.2% 59.8%;
    --lumex-info-foreground: 210 40% 98%;
}
```

### Dark Mode Support

Components automatically adapt to dark/light theme switching:

```razor
<!-- Theme toggle example -->
<LumexButton Variant="Variant.Outline" Size="Size.Small" @onclick="ToggleTheme">
    <LumexIcon Icon="@isDarkMode ? IconName.Sun : IconName.Moon" />
</LumexButton>

@code {
    private bool isDarkMode = false;

    private void ToggleTheme()
    {
        isDarkMode = !isDarkMode;
        // Theme switching logic would go here
    }
}
```

---

## Related Topics

- **Installation & Setup**: See [LUMEXUI-SETUP-GUIDE.md](./LUMEXUI-SETUP-GUIDE.md) for complete setup instructions
- **Advanced Topics**: See [LUMEXUI-ADVANCED-GUIDE.md](./LUMEXUI-ADVANCED-GUIDE.md) for testing, custom components, and learning progression
- **Component Testing**: Comprehensive bUnit testing strategies
- **Custom Component Development**: Building your own LumexUI-compatible components

---

**Document Created**: December 2025
**Last Updated**: December 2025
**Target Framework**: .NET 8+
**UI Library**: LumexUI with Tailwind CSS
**Version**: 1.0