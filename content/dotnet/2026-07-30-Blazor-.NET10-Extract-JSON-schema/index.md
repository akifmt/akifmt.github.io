---
title: "Blazor .NET 10 Extract JSON schema"
date: 2026-07-30T00:00:00+00:00
hero: blazor_dotnet10.jpg
description: Blazor .NET 10 Extract JSON schema
menu:
  dotnet:
    name: Blazor .NET 10 Extract JSON schema
    identifier: blazor-dotnet10-extract-json-schema
    weight: -20260730
tags: [dotnet10, NET10, Blazor, Extract JSON schema]
categories: [dotnet10, NET10, Blazor, Extract JSON schema]

author:
  # name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet10.jpg" alt="blazor_dotnet10" title="blazor_dotnet10" style="border-radius: 20px;"><br>
</p>



#### **Blazor .NET 10 Extract JSON schema**

<p class="d-flex justify-content-center">
<img src="extractjsonschema.jpg" alt="extractjsonschema" title="extractjsonschema" style="border-radius: 20px;"><br>
</p>

.NET 10: ```.NET 10``` introduces several enhancements, including performance improvements, new APIs, and better support for cloud-native applications. The integration of JSON schema extraction in .NET 10 is a significant feature that aids in data validation and API documentation.

Blazor: ```Blazor``` is a web framework that allows developers to build interactive web applications using C# instead of JavaScript. It leverages the power of .NET to create rich client-side applications that can run in the browser via WebAssembly or on the server.

JSON Schema: ```JSON Schema``` is a powerful tool for validating the structure of JSON data. It defines the expected format, types, and constraints of JSON objects, ensuring that the data adheres to a specified structure. This is particularly useful in API development, where consistent data formats are crucial for interoperability.


##### **Models**

{{% custom_code_tabs %}}
{{% custom_code_tab name="WeatherForecast.cs" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2675731860b39860ae435cd0e97e391c06e26269/src/BlazorAppExtractJSONschema/BlazorAppExtractJSONschema/Models/WeatherForecast.cs" %}}{{% /custom_code_tab %}}
{{% custom_code_tab name="BlogPost.cs" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2675731860b39860ae435cd0e97e391c06e26269/src/BlazorAppExtractJSONschema/BlazorAppExtractJSONschema/Models/BlogPost.cs" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}
  
```WeatherForecast``` and ```BlogPost``` classes exemplify how to create structured data models in a Blazor .NET 10 application. By defining ```properties``` and utilizing calculated ```fields```, these models not only facilitate data management but also enhance the application's ability to interact with ```JSON data```.


##### **Extract.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="Program.cs" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/2675731860b39860ae435cd0e97e391c06e26269/src/BlazorAppExtractJSONschema/BlazorAppExtractJSONschema/Components/Pages/Extract.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}
  
```OnInitializedAsync```, retrieves all types from the application's assembly that belong to a specific namespace and stores them in the ```definedTypes``` array. ```ButtonExtractClick```, checks if a type has been selected and then calls either ```SimpleExtraction``` or ```CustomExtraction``` to generate the JSON schema. ```SimpleExtraction```, uses the default ```JSON serializer``` options to extract the schema from the selected type. ```CustomExtraction```, allows for more control over the JSON serialization process. It customizes the serializer options, such as naming policies and handling of unmapped members, providing a tailored schema output.

By utilizing both ```SimpleExtraction``` and ```CustomExtraction```, developers can choose the method that best fits their needs. This flexibility, combined with the ease of use provided by the Blazor framework, makes it an excellent choice for building interactive web applications that require ```JSON schema``` validation.


#### **Source**

Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppExtractJSONschema  
  