---
icon: code
---

# Examples

### Example Categories

#### Navigation Patterns

* Basic Navigation - Simple top-level navigation
* Multi-Level Dropdown - Dropdown menus with nested items
* Mega Menu - Complex mega menu implementations
* Breadcrumbs - Breadcrumb navigation from menu structure
* Footer Navigation - Footer link groups
* Mobile Navigation - Responsive mobile menus

#### Framework Integration

* Bootstrap 5 - Bootstrap navbar integration
* Tailwind CSS - Tailwind CSS styling

### Quick Reference

#### Minimal Implementation

```cshtml
@using Umbraco.Community.UmbNav.Core.Models
@{
    var menu = Model.Value<IEnumerable<UmbNavItem>>("navigation");
}

<nav>
    <ul>
        @foreach (var item in menu)
        {
            <li>
                <umbnavitem menu-item="@item"
                            active-class="active"
                            current-page="@Model">
                </umbnavitem>
            </li>
        }
    </ul>
</nav>
```

#### With Service Processing

```cshtml
@using Umbraco.Community.UmbNav.Core.Models
@using Umbraco.Community.UmbNav.Core.Abstractions
@inject IUmbNavMenuBuilderService MenuBuilder

@{
    var rawItems = Model.Value<IEnumerable<UmbNavItem>>("navigation");
    var options = new UmbNavBuildOptions { MaxDepth = 3 };
    var menu = MenuBuilder.BuildMenu(rawItems, options);
}

<nav>
    @await Html.PartialAsync("_NavigationPartial", menu)
</nav>
```

#### Common Patterns

| Pattern              | Best For                     |
| -------------------- | ---------------------------- |
| TagHelper            | Simple, declarative markup   |
| Extension Methods    | Custom rendering logic       |
| Menu Builder Service | Advanced processing, caching |
| Custom TagHelper     | Reusable custom behavior     |

### Next Steps

* [Basic Navigation](standard.md) - Start with a simple implementation
* TagHelper Reference - Full TagHelper documentation
* Extensibility - Customization options
