---
title: "Blazor .NET 8 Migrating app from .NET 6 or .NET 7 to .NET 8"
date: 2024-01-20T00:00:00+00:00
hero: blazor_dotnet8.jpg
description: Blazor .NET 8 Migrating app from .NET 6 or .NET 7 to .NET 8
menu:
  dotnet:
    name: Blazor .NET 8 Migrating app from .NET 6 or .NET 7 to .NET 8
    identifier: blazor-dotnet8-migrating-app-from-dotnet6-or-dotnet7-to-dotnet8
    weight: -20240120
tags: [dotnet8, .NET8, Blazor, Migrating app from .NET 6 or .NET 7 to .NET 8]
categories: [dotnet8, .NET8, Migrating app from .NET 6 or .NET 7 to .NET 8]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet8.jpg" alt="blazor_dotnet8" title="blazor_dotnet8" style="border-radius: 20px;"><br>
<p>

#### **Blazor .NET 8 Migrating app from .NET 6 or .NET 7 to .NET 8**
Migrating your Blazor app from .NET 6 or .NET 7 to .NET 8 can bring several benefits, including improved performance, enhanced features, and bug fixes. Here are the steps you can follow to migrate your app successfully.

```(Old) Request Flow ``` 
<kbd>Request</kbd>
→
<kbd>_Host</kbd>
→
<kbd>App</kbd>
→
<kbd>Component</kbd>

```(New) Request Flow ``` 
<kbd>Request</kbd>
→
<kbd>App</kbd>
→
<kbd>Routes</kbd>
→
<kbd>Component</kbd>


##### **Update your project file .csproj**
Open your Blazor app's project file (.csproj) and change the ```TargetFramework``` to .NET 8.0. This ensures that your app targets the latest version of .NET.

<kbd>(OLD) .csproj</kbd>
```
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
	<TargetFramework>net6.0</TargetFramework>
	<Nullable>enable</Nullable>
	<ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```
<kbd>(NEW) .csproj</kbd>
```
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
	<TargetFramework>net8.0</TargetFramework>
	<Nullable>enable</Nullable>
	<ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```


##### **(Optional) Update NuGet packages**
Use the ```NuGet Package Manager``` or the ```.NET CLI``` to update the NuGet packages used in your project to their latest versions compatible with .NET 8. This ensures that you have the most up-to-date dependencies.

<kbd>(OLD) .csproj</kbd>
```
<ItemGroup>
	<PackageReference Include="Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore" Version="6.0.22" />
	<PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="6.0.22" />
	<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="6.0.22" />
	<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="6.0.22">
		<PrivateAssets>all</PrivateAssets>
		<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
	</PackageReference>
	<PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="6.0.16" />
</ItemGroup>
```

<kbd>(NEW) .csproj</kbd>
```
<ItemGroup>
	<PackageReference Include="Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore" Version="8.0.0" />
	<PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="8.0.0" />
	<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.0" />
	<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="8.0.0">
		<PrivateAssets>all</PrivateAssets>
		<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
	</PackageReference>
	<PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="8.0.0" />
</ItemGroup>
```


##### **Update Program.cs**
To migrate your Blazor app to .NET 8, you need to update the Program.cs file. Specifically, you need to make changes to the services configuration and the app routing.

<kbd>(OLD) Program.cs</kbd>
```
...
// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
...
app.UseRouting();
app.MapBlazorHub();
app.MapFallbackToPage("/_Host");
...
```

<kbd>(NEW) Program.cs</kbd>
```
...
// Add services to the container.
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();
...
app.UseAntiforgery();
app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();
...
```
In the old code, the services were added using the ```AddRazorPages()``` and ```AddServerSideBlazor()``` methods. However, in the new code, we use the ```AddRazorComponents()``` method to add the services required for ```Interactive Server``` Components.

The ```UseRouting()``` method is replaced with the ```UseAntiforgery()``` method, which adds anti-forgery protection to the app. This is necessary to prevent cross-site request forgery attacks.

The ```MapBlazorHub()``` method is replaced with the ```MapRazorComponents<App>()``` method, which maps the Blazor components to the app and sets the root component to App. Additionally, the ```AddInteractiveServerRenderMode()``` method is used to enable interactive server rendering.


##### **from _Host.cshtml to App.razor**
By migrating the ```App.razor``` file to the new ```Routes.razor``` file, you can ensure compatibility with the latest version of .NET and take advantage of any new features or improvements. 
Remember to make the necessary changes to the ```AppAssembly``` and ```DefaultLayout``` parameters, and handle not found routes if required.

In the old ```App.razor``` file, the routing configuration was defined. In the new ```Routes.razor``` file, the routing configuration is moved. Here's an example of the old and new code

<kbd>(OLD) App.razor</kbd>
```
<Router AppAssembly="@typeof(App).Assembly">
	<Found Context="routeData">
		<RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
		<FocusOnNavigate RouteData="@routeData" Selector="h1" />
	</Found>
	<NotFound>
		<PageTitle>Not found</PageTitle>
		<LayoutView Layout="@typeof(MainLayout)">
			<p role="alert">Sorry, there's nothing at this address.</p>
		</LayoutView>
	</NotFound>
</Router>
```

<kbd>(NEW) Routes.razor</kbd>
```
<Router AppAssembly="typeof(Program).Assembly">
	<Found Context="routeData">
		<RouteView RouteData="routeData" DefaultLayout="typeof(Layout.MainLayout)" />
		<FocusOnNavigate RouteData="routeData" Selector="h1" />
	</Found>
</Router>
```
			
In the old ```_Host.cshtml``` file, the Blazor component was rendered. In the new ```App.razor``` file, the rendering logic is moved. Here's an example of the old and new code:

<kbd>(OLD) _Host.cshtml</kbd>
```
				@page "/"
				@namespace BlazorApp.Pages
				@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
				@{
					Layout = "_Layout";
				}

				<component type="typeof(App)" render-mode="ServerPrerendered" />
```

<kbd>(NEW) App.razor</kbd>
```
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0" />
	<base href="/" />
	<link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
	<link rel="stylesheet" href="app.css" />
	<link rel="stylesheet" href="BlazorApp4.styles.css" />
	<link rel="icon" type="image/png" href="favicon.png" />
	<HeadOutlet @rendermode="InteractiveServer" />
</head>

<body>
	<Routes @rendermode="InteractiveServer" />
	<script src="_framework/blazor.web.js"></script>
</body>

</html>
```



{{< alert type="info" >}}
ℹ️ Information
- Remove ```@page``` directives
- Update base links: from ```"~/"``` to ```"/"```
- Update render-mode to ```"<HeadOutlet/>"```
- Update script name from ```"<script src="_framework/blazor.server.js"></script>"``` to ```"<script src="_framework/blazor.web.js"></script>"```
- Add Routes ```"<Routes/>"```
- (Optional) Update render-mode (global render-mode)
```
<HeadOutlet @rendermode="InteractiveServer" />
<Routes @rendermode="InteractiveServer" />
```
- (Optional) Update forms in the project
{{< /alert >}}

{{< alert type="info" >}}
ℹ️ Project Structure
- <kbd>OLD</kbd>
	- Data
	- Pages
	- Properties
	- Shared
	- wwwroot
- <kbd>NEW</kbd>
	- Components
		- Layout
		- Pages
	- Properties
	- wwwroot
{{< /alert >}}

Remember to back up your code and project files before starting the migration process. This allows you to revert to the previous version if any issues arise during the migration.

By following these steps, you can successfully migrate your Blazor app from .NET 6 or .NET 7 to .NET 8 and take advantage of the latest features and improvements offered by the framework.