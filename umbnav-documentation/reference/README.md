---
description: Complete API reference and technical documentation for UmbNav.
icon: books
---

# Reference

### Contents

* API Reference - Complete API documentation
* Changelog - Version history and changes
* Contributing - How to contribute to UmbNav
* Troubleshooting - Common issues and solutions

### Quick Links

#### C# Types

| Type                        | Description             |
| --------------------------- | ----------------------- |
| `UmbNavItem`                | Core menu item model    |
| `UmbNavBuildOptions`        | Menu processing options |
| `IUmbNavMenuBuilderService` | Menu building interface |
| `UmbnavitemTagHelper`       | Razor TagHelper         |

#### TypeScript Types

| Type                   | Description               |
| ---------------------- | ------------------------- |
| `ModelEntryType`       | Menu item data model      |
| `UmbNavToolbarAction`  | Toolbar action definition |
| `UmbNavItemSlot`       | Item slot definition      |
| `UmbNavItemTypeConfig` | Item type configuration   |

#### Extension Points

| Extension            | Location              |
| -------------------- | --------------------- |
| Toolbar Actions      | Frontend (TypeScript) |
| Item Slots           | Frontend (TypeScript) |
| Item Types           | Frontend (TypeScript) |
| Menu Builder Service | Backend (C#)          |
| TagHelper            | Backend (C#)          |
| Value Converter      | Backend (C#)          |

### Package Information

| Package                         | NuGet                    |
| ------------------------------- | ------------------------ |
| `Umbraco.Community.UmbNav`      | Full package (UI + Core) |
| `Umbraco.Community.UmbNav.Core` | Core only (headless)     |

### Version Compatibility

| Umbraco Version           | UmbNav Version                                                                                |
| ------------------------- | --------------------------------------------------------------------------------------------- |
| Umbraco Version 17        | 4.0.0 - Latest - Active development                                                           |
| Umbraco Version 16        | 4.0.0-beta0030 - (Not all features available) - No further development planned                |
| Umbraco Version 15        | 4.0.0-beta0001 - 4.0.0-beta0021 (Not all features available) - No further development planned |
| Umbraco Version 14        | Not Supported                                                                                 |
| Umbraco Versions 11 -> 13 | 3.x                                                                                           |
| Umbraco Version 10        | 2.x - End-of-Life or 3.x                                                                      |
| Umbraco Version 9         | 1.x - End-of-Life                                                                             |
| Umbraco Version 8         | 1.x - End-of-Life                                                                             |
