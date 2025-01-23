# Installation

Depending on your Umbraco version you will need to use a different installation command / package from NuGet.

### Umbraco 8, 9, 10 -> 13

#### Our.Umbraco.UmbNav.Web

To install the full package including the backoffice UI:\
[https://www.nuget.org/packages/Our.Umbraco.UmbNav.Web](https://www.nuget.org/packages/Our.Umbraco.UmbNav.Web)

{% code title=".NET CLI" %}
```csharp
dotnet add package Our.Umbraco.UmbNav.Web
```
{% endcode %}

{% code title="Package Manager" %}
```powershell
NuGet\Install-Package Our.Umbraco.UmbNav.Web
```
{% endcode %}

{% hint style="info" %}
The Package Manager command is intended to be used within the Package Manager Console in Visual Studio, as it uses the NuGet module's version of [Install-Package](https://docs.microsoft.com/nuget/reference/ps-reference/ps-ref-install-package).
{% endhint %}

#### Our.Umbraco.UmbNav.Core

To install the core:\
[https://www.nuget.org/packages/Our.Umbraco.UmbNav.Core](https://www.nuget.org/packages/Our.Umbraco.UmbNav.Core)

{% code title=".NET CLI" %}
```csharp
dotnet add package Our.Umbraco.UmbNav.Core
```
{% endcode %}

{% code title="Package Manager" %}
```powershell
NuGet\Install-Package Our.Umbraco.UmbNav.Core
```
{% endcode %}

{% hint style="info" %}
The Package Manager command is intended to be used within the Package Manager Console in Visual Studio, as it uses the NuGet module's version of [Install-Package](https://docs.microsoft.com/nuget/reference/ps-reference/ps-ref-install-package).
{% endhint %}

Our.Umbraco.UmbNav.Api

To install the API for the backoffice UI:\
[https://www.nuget.org/packages/Our.Umbraco.UmbNav.Api](https://www.nuget.org/packages/Our.Umbraco.UmbNav.Api)

{% code title=".NET CLI" %}
```csharp
dotnet add package Our.Umbraco.UmbNav.Api
```
{% endcode %}

{% code title="Package Manager" %}
```powershell
NuGet\Install-Package Our.Umbraco.UmbNav.Api
```
{% endcode %}

{% hint style="info" %}
The Package Manager command is intended to be used within the Package Manager Console in Visual Studio, as it uses the NuGet module's version of [Install-Package](https://docs.microsoft.com/nuget/reference/ps-reference/ps-ref-install-package).
{% endhint %}

### Umbraco 15

#### Umbraco.Community.UmbNav

To install the full package:\
[https://www.nuget.org/packages/Umbraco.Community.UmbNav](https://www.nuget.org/packages/Umbraco.Community.UmbNav)

{% code title=".NET CLI" %}
```csharp
dotnet add package Umbraco.Community.UmbNav
```
{% endcode %}

{% code title="Package Manager" %}
```
NuGet\Install-Package Umbraco.Community.UmbNav
```
{% endcode %}

{% hint style="info" %}
The Package Manager command is intended to be used within the Package Manager Console in Visual Studio, as it uses the NuGet module's version of Install-Package.
{% endhint %}
