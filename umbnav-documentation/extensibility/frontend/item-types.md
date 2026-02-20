---
description: >-
  UmbNav supports three built-in item types: Document, Link, and Title. You can
  register additional item types for specialized use cases.
---

# Item Types

### Basic Registration

```typescript
import { UmbNavExtensionRegistry } from '@umbraco-community/umbnav/api';

UmbNavExtensionRegistry.registerItemType({
    type: 'button',
    label: 'Button',
    icon: 'icon-hand-pointer',
    allowChildren: false
});
```

### Interface Definition

```typescript
interface UmbNavItemTypeRegistration {
    /** Unique type identifier */
    type: string;

    /** Display label in the UI */
    label: string;

    /** Icon for this type */
    icon: string;

    /** Whether items of this type can have children */
    allowChildren?: boolean;

    /** Default values to apply when creating an item of this type */
    defaultValues?: Partial<ModelEntryType>;

    /** Custom render function for display */
    render?: (item: ModelEntryType) => TemplateResult;

    /** Modal token to open when editing */
    editModalToken?: unknown;

    /** Modal token to open when creating */
    createModalToken?: unknown;
}
```

### Default Behavior (No Modal)

By default, custom item types are added **directly** when their button is clicked — no modal dialog is opened. The item is created using the `defaultValues` from the registration.

When no `editModalToken` is provided:
- The item's name is displayed as **plain text** (not a clickable link)
- The **Edit button is hidden** from the toolbar
- The item can still be deleted and reordered via drag-and-drop

This is ideal for simple item types like dividers or separators that don't need user input.

To enable editing, provide an `editModalToken` (and optionally a `createModalToken`) — see [Type with Custom Modal](#type-with-custom-modal) below.

### Examples

#### Simple Custom Type

A basic custom type that is added directly without a modal:

```typescript
UmbNavExtensionRegistry.registerItemType({
    type: 'divider',
    label: 'Add Divider',
    icon: 'icon-split-alt',
    allowChildren: false,
    defaultValues: {
        name: '---',
        icon: 'icon-split-alt',
        itemType: 'divider'
    }
});
```

Since no `editModalToken` is provided, clicking "Add Divider" immediately creates the item with the specified `defaultValues`. The item name displays as plain text and has no edit button.

#### Type with Custom Rendering

Custom display in the item list:

```typescript
import { html } from '@umbraco-cms/backoffice/external/lit';

UmbNavExtensionRegistry.registerItemType({
    type: 'highlight',
    label: 'Highlighted Item',
    icon: 'icon-lightbulb',
    allowChildren: true,
    render: (item) => html`
        <div style="
            display: flex;
            align-items: center;
            padding: 4px 8px;
            background: linear-gradient(90deg, #fef3c7, transparent);
            border-radius: 4px;
        ">
            <uui-icon name="icon-lightbulb" style="color: #f59e0b; margin-right: 8px;"></uui-icon>
            <strong>${item.name}</strong>
        </div>
    `
});
```

#### Type with Custom Modal

For types that need special editing:

```typescript
import { UmbModalToken } from '@umbraco-cms/backoffice/modal';

// Define your modal token
const MY_CUSTOM_TYPE_MODAL = new UmbModalToken('my-custom-type-modal', {
    modal: { type: 'sidebar', size: 'small' }
});

UmbNavExtensionRegistry.registerItemType({
    type: 'custom-form',
    label: 'Custom Form',
    icon: 'icon-clipboard',
    allowChildren: false,
    editModalToken: MY_CUSTOM_TYPE_MODAL,
    createModalToken: MY_CUSTOM_TYPE_MODAL
});
```

#### Call-to-Action Button Type

```typescript
UmbNavExtensionRegistry.registerItemType({
    type: 'cta-button',
    label: 'CTA Button',
    icon: 'icon-cursor-finger',
    allowChildren: false,
    render: (item) => html`
        <div style="
            display: inline-flex;
            align-items: center;
            padding: 6px 16px;
            background: var(--uui-color-positive);
            color: white;
            border-radius: 20px;
            font-weight: 600;
        ">
            ${item.name}
            <uui-icon name="icon-arrow-right" style="margin-left: 8px;"></uui-icon>
        </div>
    `
});
```

#### Mega Menu Section Type

```typescript
UmbNavExtensionRegistry.registerItemType({
    type: 'mega-section',
    label: 'Mega Menu Section',
    icon: 'icon-layout',
    allowChildren: true,
    render: (item) => html`
        <div style="
            display: flex;
            flex-direction: column;
            border-left: 3px solid var(--uui-color-current);
            padding-left: 12px;
        ">
            <span style="
                font-weight: 700;
                text-transform: uppercase;
                font-size: 11px;
                letter-spacing: 0.5px;
                color: var(--uui-color-text-alt);
            ">${item.name}</span>
            ${item.description ? html`
                <span style="font-size: 12px; opacity: 0.7;">${item.description}</span>
            ` : ''}
        </div>
    `
});
```

#### Social Link Type

```typescript
UmbNavExtensionRegistry.registerItemType({
    type: 'social-link',
    label: 'Social Media Link',
    icon: 'icon-share-alt',
    allowChildren: false,
    render: (item) => {
        // Map common social networks to colors
        const socialColors: Record<string, string> = {
            'facebook': '#1877f2',
            'twitter': '#1da1f2',
            'instagram': '#e4405f',
            'linkedin': '#0a66c2',
            'youtube': '#ff0000'
        };

        const name = item.name.toLowerCase();
        const color = Object.entries(socialColors).find(([key]) =>
            name.includes(key)
        )?.[1] || '#6b7280';

        return html`
            <div style="
                display: flex;
                align-items: center;
                color: ${color};
            ">
                <uui-icon name="${item.icon}" style="margin-right: 8px;"></uui-icon>
                <span>${item.name}</span>
            </div>
        `;
    }
});
```

### Working with Custom Types in Views

On the frontend, check the item type and render accordingly:

```cshtml
@foreach (var item in menuItems)
{
    @switch (item.ItemType.ToString().ToLower())
    {
        case "divider":
            <hr class="nav-divider" />
            break;

        case "mega-section":
            <div class="mega-section">
                <h4>@item.Name</h4>
                @if (item.Children?.Any() == true)
                {
                    <ul>
                        @foreach (var child in item.Children)
                        {
                            <li><umbnavitem menu-item="@child"></umbnavitem></li>
                        }
                    </ul>
                }
            </div>
            break;

        case "cta-button":
            <a href="@item.Url()" class="btn btn-primary">@item.Name</a>
            break;

        default:
            <umbnavitem menu-item="@item"></umbnavitem>
            break;
    }
}
```

### Backend Support

To fully support custom types in the backend, you may need to extend the enum or handle them as custom strings:

```csharp
// In your views, check for custom types
if (item.ItemType.ToString() == "mega-section")
{
    // Handle mega section
}
```

### Unregistering Item Types

```typescript
UmbNavExtensionRegistry.unregisterItemType('button');
```

### Getting Registered Types

```typescript
// Get all item types
const allTypes = UmbNavExtensionRegistry.getItemTypes();

// Get specific type
const buttonType = UmbNavExtensionRegistry.getItemType('button');
```

### Best Practices

#### 1. Use Meaningful Type Names

```typescript
// Good
type: 'mega-menu-section'

// Avoid
type: 'type1'
```

#### 2. Provide Clear Icons

Choose icons that represent the type's purpose:

```typescript
// Good - represents a button
icon: 'icon-cursor-finger'

// Less clear
icon: 'icon-settings'
```

#### 3. Consider Child Support

Think about whether items of this type should support nesting:

```typescript
// Sections typically have children
allowChildren: true

// Dividers don't have children
allowChildren: false
```

#### 4. Handle Missing Data

Your render function should handle undefined values:

```typescript
render: (item) => html`
    <span>${item.name || 'Unnamed'}</span>
    ${item.description ? html`<small>${item.description}</small>` : ''}
`
```

#### 5. Match UmbNav Styling

Use consistent styling with the rest of UmbNav:

```typescript
render: (item) => html`
    <div style="
        display: flex;
        align-items: center;
        min-height: var(--uui-size-14);
    ">
        <!-- content -->
    </div>
`
```

### Limitations

* Custom types are stored as strings in the item's `itemType` property
* The backend `UmbNavItemType` enum doesn't automatically include custom types
* You may need to handle custom types specially in your rendering code
* Modals for custom types must be registered separately with Umbraco
