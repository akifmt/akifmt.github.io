---
title: "Blazor .NET 10 with Minimal API"
date: 2026-01-30T00:00:00+00:00
hero: blazor_dotnet10.jpg
description: Blazor .NET 10 with Minimal API
menu:
  dotnet:
    name: Blazor .NET 10 with Minimal API
    identifier: blazor-dotnet10-minimal-api
    weight: -20260130
tags: [dotnet10, NET10, Blazor, Minimal API]
categories: [dotnet10, NET10, Blazor, Minimal API]

author:
  # name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet10.jpg" alt="blazor_dotnet10" title="blazor_dotnet10" style="border-radius: 20px;"><br>
</p>



#### **Blazor .NET 10 with Minimal API**

<p class="d-flex justify-content-center">
<img src="blazor_dotnet10.jpg" alt="blazor_dotnet10" title="blazor_dotnet10" style="border-radius: 20px;"><br>
</p>

.NET 10: ```.NET 10``` is the latest version of the .NET framework, which provides a robust platform for building web applications, services, and more.  

Blazor: ```Blazor``` is a web framework that enables developers to create client-side web applications using C#. It leverages WebAssembly to run C# code directly in the browser, allowing for a rich interactive experience without relying on JavaScript. Blazor can be used in two modes: Blazor Server and Blazor WebAssembly. 

Minimal API: ```Minimal APIs``` in .NET 10 provide a streamlined approach to building HTTP APIs. They allow developers to define routes and handle requests with minimal boilerplate code, making it easier to create lightweight services. This is particularly beneficial for microservices and small applications where simplicity and speed are paramount.



##### **Program.cs**

{{% custom_code_tabs %}}
{{% custom_code_tab name="Program.cs" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/8d800e5e7d613fe6fbfee3480c04fead67bdca68/src/BlazorAppNET10withMinimalAPI/WebApplicationNET10WebAPI/Program.cs" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

```WeatherForecast``` record defines the structure of the weather data, including a computed property to convert Celsius to Fahrenheit.  
GET endpoint at ```/weatherforecast```, it generates an array of five weather forecasts, each with a date, a random temperature, and a random summary. The WeatherForecast record is instantiated for each forecast.



##### **Weather.razor**

{{% custom_code_tabs %}}
{{% custom_code_tab name="Weather.razor" rawlink="https://raw.githubusercontent.com/akifmt/DotNetCoding/8d800e5e7d613fe6fbfee3480c04fead67bdca68/src/BlazorAppNET10withMinimalAPI/BlazorAppNET10withMinimalAPI/Components/Pages/Weather.razor" %}}{{% /custom_code_tab %}}
{{% /custom_code_tabs %}}

Page Directive: The ```@page "/weather"``` directive specifies that this component will be accessible at the ```/weather``` URL.  
Dependency Injection: The ```@inject HttpClient Client``` line allows the component to use the ```HttpClient``` service for making HTTP requests.  
Data Table: Once the ```data``` is loaded, a table is rendered displaying the date, temperature in Celsius and Fahrenheit, and a summary of the weather.  
WeatherForecast Class: This ```class``` defines the structure of the ```weather data```, including properties for ```date```, ```temperature in Celsius```, ```summary```, and a calculated property for temperature in Fahrenheit.  



#### **Source**

Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppNET10withMinimalAPI  
  