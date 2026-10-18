---
title: "Blazor Fluent UI .NET 10 Highlighter"
date: 2026-10-18T00:00:00+00:00
hero: blazor_fluentui_dotnet10.jpg
description: "Blazor Fluent UI .NET 10 Highlighter"
menu:
  dotnet:
    name: "Blazor Fluent UI .NET 10 Highlighter"
    identifier: blazor-fluentui-dotnet10-highlighter
    weight: -20261018
tags: [dotnet10, .NET10, Blazor, fluentui, highlighter]
categories: [dotnet10, .NET10, Blazor, fluentui, highlighter]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_fluentui_dotnet10.jpg" alt="blazor_fluentui_dotnet10" title="blazor_fluentui_dotnet10" style="border-radius: 20px;"><br>
</p>



#### **Blazor Fluent UI .NET 10 Highlighter**

.NET 10: ```.NET 10``` introduces several enhancements, including performance improvements, new APIs, and better support for cloud-native applications. The integration of JSON schema extraction in .NET 10 is a significant feature that aids in data validation and API documentation.

Blazor: ```Blazor``` is a web framework that allows developers to build interactive web applications using C# instead of JavaScript. It leverages the power of .NET to create rich client-side applications that can run in the browser via WebAssembly or on the server.

Fluent UI Blazor: ```Fluent UI Blazor``` is a set of components that implement Microsoft's Fluent Design System for Blazor applications. It provides a consistent and modern user interface, enhancing the overall user experience.

Highlighter: ```Highlighter``` that highlights specific text within a larger body of text based on user input. This is particularly useful for emphasizing keywords or phrases.


##### **Home.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="Home.razor" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/bf76c68d94f187c54765e119c0f7332c8091f43a/src/BlazorAppFluentUINet10Highlighter/BlazorAppFluentUINet10Highlighter/Components/Pages/Home.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

When the user types into the ```FluentTextInput```, the input variable is updated in real-time due to the ```@bind-Value``` directive. The ```FluentHighlighter``` then takes this input and highlights the corresponding text within the content. This interaction creates a dynamic and responsive user experience, allowing users to see the effects of their ```input``` immediately. 

By leveraging the capabilities of Blazor and .NET 10, developers can build responsive and user-friendly applications that enhance the user experience. The combination of ```real-time input``` binding and dynamic content highlighting not only improves usability but also encourages user engagement with the application.


#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppFluentUINet10Highlighter  
