---
title: "Blazor .NET 10 Migrate from .NET 8 to .NET 10"
date: 2026-01-02T00:00:00+00:00
hero: blazor_dotnet10.jpg
description: Blazor .NET 10 Migrate from .NET 8 to .NET 10
menu:
  dotnet:
    name: Blazor .NET 10 Migrate from .NET 8 to .NET 10
    identifier: blazor-dotnet10-migrate-from-NET8-to-NET10
    weight: -20260102
tags: [dotnet10, NET10, Blazor, Migrate from .NET 8 to .NET 10]
categories: [dotnet10, NET10, Blazor, Migrate from .NET 8 to .NET 10]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet10.jpg" alt="blazor_dotnet10" title="blazor_dotnet10" style="border-radius: 20px;"><br>
</p>


#### **Blazor .NET 10 Migrate from .NET 8 to .NET 10**

Blazor: ```Blazor``` is a web framework that allows developers to build interactive web applications using C# instead of JavaScript. It leverages the power of .NET to create rich client-side applications that run in the browser via WebAssembly or server-side through SignalR. Blazor provides a component-based architecture, making it easy to create reusable UI components.  

.NET 10: ```.NET 10``` is the latest version of the .NET platform, which includes numerous enhancements over its predecessor, .NET 8. Key improvements include better performance, enhanced security features, and new APIs that simplify development. Migrating to .NET 10 allows developers to take advantage of these enhancements, ensuring that applications remain modern and efficient.  

Migration: ```Migrating from .NET 8 to .NET 10``` involves several steps, including updating project files, resolving any breaking changes, and testing the application thoroughly. It is essential to review the official Microsoft documentation for any specific migration guidelines and best practices.  


##### **Update .csproj**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10.csproj" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10.csproj" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="NET8.csproj" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/ad99354d3fa20fd4314e2fe9cbd94019c636f1ff/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10.csproj" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

The ```TargetFramework``` element in the .NET 10 project file specifies ```net10.0```, while the .NET 8 project file specifies ```net8.0```.   
The .NET 10 project file includes the property ```<BlazorDisableThrowNavigationException> true </BlazorDisableThrowNavigationException>```. This setting is specific to Blazor applications and controls the behavior of navigation exceptions. By setting this to true, the application will suppress navigation exceptions, which can enhance user experience by preventing abrupt application crashes during navigation errors. This property is absent in the .NET 8 project file, indicating that it may not have this specific feature or behavior.  


##### **Update Routes.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/Routes.razor" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="NET8" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/ad99354d3fa20fd4314e2fe9cbd94019c636f1ff/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/Routes.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

In ```.NET 10```, the ```NotFoundPage``` property is included in the Router component. This allows developers to define a specific page to be displayed when a user navigates to a non-existent route. This feature enhances user experience by providing a clear indication of navigation errors.


##### **Update _Imports.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/_Imports.razor" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="NET8" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/ad99354d3fa20fd4314e2fe9cbd94019c636f1ff/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/_Imports.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

The ```.NET 10``` version includes an additional namespace: ```@using BlazorAppNET10MigratefromNET8toNET10.Components.Layout```.


##### **Update Program.cs**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Program.cs" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="NET8" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/ad99354d3fa20fd4314e2fe9cbd94019c636f1ff/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Program.cs" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

.NET 10: Introduces ```app.UseStatusCodePagesWithReExecute("/not-found", createScopeForStatusCodePages: true);```, which provides a more user-friendly experience by redirecting users to a custom ```"not found"``` page when a status code is encountered.  
.NET 10: Uses ```app.MapStaticAssets();```, which is a more streamlined approach to serving static files.


##### **Update App.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/App.razor" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="NET8" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/ad99354d3fa20fd4314e2fe9cbd94019c636f1ff/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/App.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

The addition of the ```<ResourcePreloader />``` component in .NET 10 is a significant enhancement. This component preloads resources, which can lead to faster rendering times and a smoother user experience. In .NET 10, the use of ```@Assets[]``` allows for a more flexible way to manage assets.  


##### **Add ReconnectModal.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/Layout/ReconnectModal.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

The ```ReconnectModal.razor``` component in ```.NET 10``` is a vital part of enhancing user experience during connectivity issues. By providing clear feedback and actionable options, it helps users navigate potential disruptions in service.  


##### **Add NotFound.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="NET10" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2e97598b9ddca071aa3f860de93488f0ed033761/src/BlazorAppNET10MigratefromNET8toNET10/BlazorAppNET10MigratefromNET8toNET10/Components/Pages/NotFound.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

The ```NotFound.razor``` component in ```.NET 10``` is a simple yet effective way to manage user navigation errors. By providing a clear message and maintaining the application's layout, it ensures that users are not left confused when they encounter a non-existent page.


##### **Blazor .NET 10 Folder Structure**
```
BlazorAppNET10MigratefromNET8toNET10/
├── BlazorAppNET10MigratefromNET8toNET10/
│   ├── Components/
│   │   ├── Layout/
│   │   │   ├── MainLayout.razor
│   │   │   ├── MainLayout.razor.css
│   │   │   ├── NavMenu.razor
│   │   │   ├── NavMenu.razor.css
│   │   │   ├── ReconnectModal.razor
│   │   │   ├── ReconnectModal.razor.css
│   │   │   ├── ReconnectModal.razor.js
│   │   ├── Pages/
│   │   │   ├── Counter.razor
│   │   │   ├── Error.razor
│   │   │   ├── Home.razor
│   │   │   ├── NotFound.razor
│   │   │   ├── Weather.razor
│   │   ├── _Imports.razor
│   │   ├── App.razor
│   │   ├── Routes.razor
│   ├── Properties/
│   │   ├── launchSettings.json
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── BlazorAppNET10MigratefromNET8toNET10.csproj
│   └── Program.cs
└── BlazorAppNET10MigratefromNET8toNET10.slnx
```


#### **Source**
Diff list:  
https://github.com/akifmt/DotNetCoding/commit/2e97598b9ddca071aa3f860de93488f0ed033761?diff=split#diff-92f163fbf651848305f05ee77c6bf2da46ca6cceb1f983c43b7233411a42fd45  
  
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppNET10MigratefromNET8toNET10  
  