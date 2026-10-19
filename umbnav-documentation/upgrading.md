---
description: >-
  This guide covers upgrading UmbNav between major versions, including the
  automatic data migration process.
icon: up
---

# Upgrading

### Upgrading to UmbNav 4.x

#### Prerequisites

1. **Umbraco 17+** - Upgrade Umbraco first
2. **Backup** - Always backup your database before upgrading
3. **.NET 10** - Ensure your project targets .NET 10

#### Upgrade Steps

1. **Remove old package references**
2.  **Add new package**:

    ```bash
    dotnet add package Umbraco.Community.UmbNav
    ```
3.  **Build and run**:

    ```bash
    dotnet build
    dotnet run
    ```
4. **Automatic migration runs** on first startup

#### Automatic Data Migration

UmbNav 4.x includes an automatic migration system that runs on first startup after upgrading. This migration:

1. **Detects legacy data types** using the old `AaronSadler.UmbNav` property editor alias, or one of the Meganav aliases (`Our.Umbraco.Meganav`, `AaronSadler.MegaNavV8`, `Meganav`, `Cogworks.Meganav`)
2. **Transforms content data** from the old format to the new format, including values inside Block List, Block Grid and Rich Text blocks (nested to any depth)
3. **Updates data type definitions** to use the new `Umbraco.Community.UmbNav` alias
4. **Preserves all menu data** including nested children, images, and settings

Values are recognised by their shape, so legacy or Meganav data stored on a property that already uses `Umbraco.Community.UmbNav` is converted too. Items already in the 4.x format are left untouched, which makes the migration safe to run again.

**What Gets Migrated**

| Old Property         | New Property          | Notes                            |
| -------------------- | --------------------- | -------------------------------- |
| `udi`                | `contentKey`          | Converted from UDI to GUID       |
| `title`              | `name`                | Title takes precedence over name |
| `name`               | `name`                | Used if title is empty           |
| `url`                | `url`                 | Preserved                        |
| `itemType` (Link)    | `itemType` (External) | Renamed                          |
| `itemType` (Content) | `itemType` (Document) | Renamed                          |
| `itemType` (Label)   | `itemType` (Title)    | Renamed                          |
| `anchor`             | `anchor`              | Preserved                        |
| `target`             | `target`              | Preserved                        |
| `noopener` (bool)    | `noopener` (string)   | Converted to string              |
| `noreferrer` (bool)  | `noreferrer` (string) | Converted to string              |
| `children`           | `children`            | Recursively migrated             |
| `image`              | `imageArray`          | Preserved                        |
| `customClasses`      | `customClasses`       | Preserved                        |
| `hideLoggedIn`       | `hideLoggedIn`        | Preserved                        |
| `hideLoggedOut`      | `hideLoggedOut`       | Preserved                        |
| `includeChildNodes`  | `includeChildNodes`   | Preserved                        |
| `description`        | `description`         | Preserved                        |
| `icon`               | `icon`                | Preserved                        |

**Meganav Data**

| Meganav Property        | New Property                        | Notes                                                     |
| ----------------------- | ----------------------------------- | --------------------------------------------------------- |
| `title`                 | `name`                              | When empty, the picked node's name is used                |
| `udi` / `id`            | `contentKey`                        | `id` (MeganavV8) is looked up to find the node's key      |
| `url`                   | `url`                               | Items without a node become `External`, or `Title` if no url |
| `target`, `anchor`      | `target`, `anchor`                  | Preserved                                                 |
| `visible: false`        | `hideLoggedIn` + `hideLoggedOut`    | Kept, but hidden from everyone                            |
| `itemTypeId`, `settings`| —                                   | No UmbNav equivalent, dropped                             |
| `children`              | `children`                          | Recursively migrated                                      |

Meganav data type configuration is not carried over; the switched data types start with UmbNav's default configuration.

**Item Type Mapping**

| Old Type      | New Type   |
| ------------- | ---------- |
| `Link` (0)    | `External` |
| `Content` (1) | `Document` |
| `Label` (2)   | `Title`    |

**Migration Process**

The migration runs automatically via Umbraco's package migration system:

```
UmbNavPackageMigrationPlan
├── UmbNavLegacyModelMigration (786E9C82-8621-4B0E-8E3A-7A7AAD61B820)
└── UmbNavLegacyModelMigration (5D0C6E1A-3F7B-4E2B-9C1D-8A4F2B6E7C30)
    ├── Find all data types with a legacy UmbNav or Meganav alias
    ├── Find document types with a nav property, or with block properties
    │   when a nav property lives on an element type
    ├── For each content node:
    │   ├── Read the nav value, or walk the block value to find nested nav values
    │   ├── Transform items still in a legacy shape
    │   ├── Save updated content
    │   └── Republish it if it was published with no pending changes
    └── Update data types to the new alias
```

The second step runs the same migration again for sites that upgraded before it could reach block content or Meganav data.

**Verifying Migration**

After upgrade, verify the migration completed:

1.  **Check Umbraco logs** for migration messages:

    ```
    Starting UmbNav legacy model migration.
    Converting legacy UmbNav data format for instances of {ContentType}
    Completed UmbNav legacy model migration.
    ```
2. **Check data types** - Should now use `Umbraco.Community.UmbNav` alias
3. **Test navigation** - Verify menus render correctly on frontend

**Troubleshooting Migration**

**Migration didn't run:**

* Check if `umbracoKeyValue` table has the migration key
* Set the `Umbraco.Core.Upgrader.State+UmbNav` value back to `786E9C82-8621-4B0E-8E3A-7A7AAD61B820` to re-run the second step, or delete it to re-run both

**Content not migrated:**

* Check logs for errors
* Verify content isn't trashed (trashed content is skipped)
* Manual migration may be needed for edge cases

**Data type not updated:**

* Check logs for "Unable to update data type" errors
* Manually change the property editor alias if needed

#### Breaking Changes in 4.x

**Frontend (TypeScript)**

1. **New extension system** - Old AngularJS extensions won't work
2. **Lit components** - UI rebuilt with Lit web components
3. **New API imports** - Use `@umbraco-community/umbnav/api`

**Backend (C#)**

1. **Virtual methods** - Service methods are now virtual for extension
2. **UmbNavBuildOptions** - New class for menu processing options
3. **Item type enum** - `UmbNavItemType` values renamed

**Data Model**

1. **noopener/noreferrer** - Changed from `bool` to `string`
2. **contentKey** - Now uses `Guid` instead of UDI string
3. **itemType** - Enum values renamed (see table above)

***

### Upgrading from 2.x to 3.x

This upgrade path is for moving from Umbraco 10-13 to Umbraco 14+.

#### Key Changes

1. **AngularJS to Lit** - Complete frontend rewrite
2. **New property editor** - Different backoffice UI
3. **Same data format** - Content data compatible

#### Steps

1. Upgrade Umbraco to 14+
2. Update package reference
3. Rebuild and test

***

### Upgrading from 1.x to 2.x

This upgrade path is for moving from Umbraco 8/9 to Umbraco 10+.

#### Key Changes

1. **.NET upgrade** - Framework to modern .NET
2. **Package restructure** - New namespace
3. **Same features** - Backward compatible

#### Steps

1. Upgrade Umbraco to 10+
2. Update package reference
3. Update any custom code namespaces
4. Rebuild and test

***

### Rollback

If you need to rollback after upgrading:

1. **Restore database backup**
2.  **Restore previous package version**:

    ```bash
    dotnet add package Umbraco.Community.UmbNav --version 3.x.x
    ```
3. **Rebuild and deploy**

> **Warning**: Rolling back after migration will lose any content changes made after the upgrade.

***

### Getting Help

* **GitHub Issues**: Report migration problems
* **Logs**: Include Umbraco logs when reporting issues
* **Database backup**: Always have a backup before upgrading
