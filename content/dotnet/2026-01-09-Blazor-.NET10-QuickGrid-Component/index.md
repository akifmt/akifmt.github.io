---
title: "Blazor .NET 10 QuickGrid Component"
date: 2026-01-09T00:00:00+00:00
hero: blazor_dotnet10.jpg
description: Blazor .NET 10 QuickGrid Component
menu:
  dotnet:
    name: Blazor .NET 10 QuickGrid Component
    identifier: blazor-dotnet10-quickgrid-component
    weight: -20260109
tags: [dotnet10, NET10, Blazor, QuickGrid Component]
categories: [dotnet10, NET10, Blazor, QuickGrid Component]

author:
  # name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet10.jpg" alt="blazor_dotnet10" title="blazor_dotnet10" style="border-radius: 20px;"><br>
</p>


#### **Blazor .NET 10 QuickGrid Component**

<p class="d-flex justify-content-center">
<img src="quickgrid.jpg" alt="quickgrid" title="quickgrid" style="border-radius: 20px;"><br>
</p>

.NET 10: ```.NET 10``` is the latest version of the .NET framework, which provides a robust platform for building web applications, services, and more.  

Blazor: ```Blazor``` A framework for building interactive web applications using C# instead of JavaScript. It allows developers to create rich web UIs with C#.  

QuickGrid: ```QuickGrid``` A component that simplifies the process of displaying tabular data in Blazor applications. It offers features like sorting, filtering, and pagination out of the box, making it a powerful tool for developers.  


##### **.csproj**

{{% custom_code_tabs %}}
{{% custom_code_tab name=".csproj" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/5a2d869660f4c2f7fba4fb9aad53017388bc1451/src/BlazorAppQuickGridComponent/BlazorAppQuickGridComponent/BlazorAppQuickGridComponent.csproj" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

```<PackageReference Include="Microsoft.AspNetCore.Components.QuickGrid" Version="10.0.1" />``` adds the ```QuickGrid``` package to the project, enabling the use of its components for displaying data in a grid format.


##### **Weather.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="Weather.razor" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/5a2d869660f4c2f7fba4fb9aad53017388bc1451/src/BlazorAppQuickGridComponent/BlazorAppQuickGridComponent/Components/Pages/Weather.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

Filtering: The ```FilteredWeatherForecasts``` property filters the forecasts based on the user input in the search box. It checks if the ```summaryFilter``` is not empty and applies the filter accordingly.  

Pagination: The ```PaginationState``` object manages the number of items displayed per page, allowing users to select their preference from a dropdown.  

The ```Weather component``` exemplifies how to effectively implement ```sorting```, ```filtering```, and ```paging``` in a ```Blazor``` application using ```.NET 10``` and ```C#```. By understanding the structure and functionality of this component, developers can create ```dynamic``` and ```user-friendly``` interfaces for displaying data.  


#### **Source**

Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppQuickGridComponent  
  