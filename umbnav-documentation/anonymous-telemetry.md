---
icon: binoculars
description: What data do I collect and why?
---

# Anonymous Telemetry

By default, anonymous telemetry is sent to me on a new install, when you update Umbraco or update UmbNav.

**Why do I collect this data?**

It is so that I can concentrate my development on the most used versions of Umbraco, it also lets me have a rough idea of how many sites are using my package

**What data is collected?**

We collect the following data:



| Key             | Value                                                                         |
| --------------- | ----------------------------------------------------------------------------- |
| umbracoId       | The Umbraco Guid for the site                                                 |
| umbracoVersion  | The Umbraco version                                                           |
| umbNavVersion   | The UmbNav version                                                            |
| environmentName | The sites hosting environment name (Development, Staging, Production, etc...) |

_There is a unique constraint on `umbracoId` and `environmentName`_

**Can I disable the telemetry?**

Yes, you can disable the telemetry service by setting the following configuration in your appsettings.json file

```json
{
  "UmbNav": {
    "DisableTelemetry": true
  }
}
```
