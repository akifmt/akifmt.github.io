---
title: "Blazor .NET 8 and Minimal APIs Native AOT CRUD"
date: 2024-02-12T00:00:00+00:00
hero: blazor_dotnet8.jpg
description: Blazor .NET 8 and Minimal APIs Native AOT CRUD
menu:
  dotnet:
    name: Blazor .NET 8 and Minimal APIs Native AOT CRUD
    identifier: blazor-dotnet8-and-minimal-APIs-native-AOT-CRUD
    weight: -20240212
tags: [dotnet8, .NET8, Blazor, Minimal APIs Native AOT CRUD]
categories: [dotnet8, .NET8, Minimal APIs Native AOT CRUD]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet8.jpg" alt="blazor_dotnet8" title="blazor_dotnet8" style="border-radius: 20px;"><br>
<p>

#### **Blazor .NET 8 and Minimal APIs Native AOT CRUD**

Let's briefly discuss the key concepts involved in this ```Blazor app and Minimal APIs Native AOT CRUD``` implementation:

Blazor: ```Blazor``` is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables the development of single-page applications (SPAs) with the power of .NET.

Minimal APIs: ```Minimal APIs``` is a new feature introduced in .NET 6 that allows developers to create lightweight and minimalistic APIs without the need for traditional controllers and routes. It simplifies the API development process and reduces the amount of boilerplate code.

Native AOT (Ahead-of-Time) Compilation: ```Native AOT``` compilation is a feature in .NET that allows the application to be compiled to machine code ahead of time, resulting in faster startup times and improved performance.

CRUD Operations: ```CRUD``` stands for ```Create```, ```Read```, ```Update```, and ```Delete```. These are the basic operations performed on data in a database or storage system.

##### **BlazorAppandMinimalAPIsNativeAOTCRUD.Core Class Library**

<kbd>Todo.cs</kbd>
```
namespace BlazorAppandMinimalAPIsNativeAOTCRUD.Core.Models;

public class Todo
{
    public int Id { get; set; }
    public string? Title { get; set; } = string.Empty;
    public DateOnly? DueBy { get; set; } = null;
    public bool IsComplete { get; set; } = false;

    public override string ToString() => $"{this.Id} {this.Title} {this.DueBy} {this.IsComplete}";

}
```
This file contains the logic for our ```CRUD``` operations. It includes methods for creating a new todo item, retrieving all todo items, updating a todo item, and deleting a todo item.

<kbd>TodoDataModel.cs</kbd>
```
namespace BlazorAppandMinimalAPIsNativeAOTCRUD.Core.DataModels;

public record TodoDataModelRecord(int Id = 0, string? Title = "", DateOnly? DueBy = null, bool IsComplete = false);

public class TodoDataModel
{
    public int Id { get; set; }
    public string? Title { get; set; } = string.Empty;
    public DateOnly? DueBy { get; set; } = null;
    public bool IsComplete { get; set; } = false;
}
```
In the ```TodoDataModel.cs``` file defines a ```data model``` for a ```todo``` item in the Blazor app and ```Minimal APIs Native AOT CRUD``` project. This file contains the data model for our application. It defines the structure of a ```todo``` item, including properties like ```Id, Title, DueBy, and IsCompleted```.


##### **WebAppAPINativeAOT Minimal APIs Native AOT Project**

<kbd>TodoService.cs</kbd>
```
using BlazorAppandMinimalAPIsNativeAOTCRUD.Core.Models;
using WebAppAPINativeAOT.Data;

namespace WebAppAPINativeAOT.Services;

public class TodoService(ILogger<TodoService> logger)
{
    public Todo? GetById(int id)
    {
        logger.LogInformation("Called GetById: {id}", id);
        return SampleData.ToDos.FirstOrDefault(x => x.Id == id);
    }

    public Todo[]? GetCompleted()
    {
        logger.LogInformation("Called GetCompleted");
        return SampleData.ToDos.Where(x => x.IsComplete == true).ToArray();
    }

    public Todo[]? GetAll()
    {
        logger.LogInformation("Called GetAll");
        return [.. SampleData.ToDos];
    }

    public bool Add(Todo todo)
    {
        logger.LogInformation("Called Add {todo}", todo);

        try
        {
            var last = SampleData.ToDos.LastOrDefault();
            todo.Id = last is not null ? last.Id + 1 : 1;
            SampleData.ToDos.Add(todo);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Called Add Error {todo}", todo);
            return false;
        }
        return true;
    }

    public bool Update(int id, Todo todo)
    {
        logger.LogInformation("Called Update {todo}", todo);
        try
        {
            var oTodo = SampleData.ToDos.FirstOrDefault(x => x.Id == id);
            if (oTodo == null) return false;

            oTodo.Title = todo.Title;
            oTodo.DueBy = todo.DueBy;
            oTodo.IsComplete = todo.IsComplete;
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Called Update Error {todo}", todo);
            return false;
        }
        return true;
    }

    public bool DeleteById(int id)
    {
        logger.LogInformation("Called DeleteById {id}", id);
        var oTodo = SampleData.ToDos.FirstOrDefault(x => x.Id == id);
        if (oTodo == null)
            return false;

        SampleData.ToDos.Remove(oTodo);

        return true;
    }
}
```
The ```TodoService.cs``` file contains a class named ```TodoService``` that implements various methods for ```CRUD``` operations on the ```Todo model```. Let's take a closer look at each method:

GetById(int id): This method retrieves a ```Todo``` object by its ```ID```. It logs the ```ID``` of the requested ```Todo``` and returns the corresponding ```Todo``` object if found.

GetCompleted(): This method retrieves an array of completed ```Todo``` objects. It logs the request and returns an array of ```Todo``` objects where the ```IsComplete``` property is set to ```true```.

GetAll(): This method retrieves an array of all ```Todo``` objects. It logs the request and returns an array containing all the ```Todo``` objects.

Add(Todo todo): This method adds a new ```Todo``` object to the collection. It logs the request and attempts to add the ```Todo``` object to the ```SampleData.ToDos``` collection. If successful, it assigns a ```unique ID``` to the ```Todo``` object and adds it to the collection. If an error occurs, it logs the error and returns ```false```.

Update(int id, Todo todo): This method updates an existing ```Todo``` object with the provided ```ID```. It logs the request and attempts to find the ```Todo``` object with the specified ```ID```. If found, it updates the ```Title, DueBy, and IsComplete``` properties of the ```Todo``` object. If an error occurs, it logs the error and returns ```false```.

DeleteById(int id): This method deletes a ```Todo``` object with the specified ```ID```. It logs the request and attempts to find the ```Todo``` object with the specified ```ID```. If found, it removes the ```Todo``` object from the ```SampleData.ToDos``` collection. If an error occurs, it returns ```false```.



<kbd>Program.cs</kbd>
```
using BlazorAppandMinimalAPIsNativeAOTCRUD.Core.DataModels;
using BlazorAppandMinimalAPIsNativeAOTCRUD.Core.Models;
using System.Text.Json.Serialization;
using WebAppAPINativeAOT.Data;
using WebAppAPINativeAOT.Services;

internal class Program
{
    private static void Main(string[] args)
    {
        var builder = WebApplication.CreateSlimBuilder(args);

        builder.Services.ConfigureHttpJsonOptions(options =>
        {
            options.SerializerOptions.TypeInfoResolverChain.Insert(0, AppJsonSerializerContext.Default);
        });

        // add test data
        SampleData.AddTestData();

        // Add services to the container.
        builder.Services.AddScoped<TodoService>();

        var app = builder.Build();

        // add endpoints
        app.MapTodoGroup();

        app.Run();
    }
}

[JsonSerializable(typeof(object))]
[JsonSerializable(typeof(Todo[]))]
[JsonSerializable(typeof(TodoDataModel[]))]
internal partial class AppJsonSerializerContext : JsonSerializerContext
{
}

public static class WebApplicationExtensions
{
    public static void MapTodoGroup(this WebApplication app)
    {
        var todosApi = app.MapGroup("/todos");

        // get all
        todosApi.MapGet("/", (TodoService todoService) => todoService.GetAll());

        // get all completed
        todosApi.MapGet("/complete", (TodoService todoService) => todoService.GetCompleted());

        // get by id
        todosApi.MapGet("/{id}", (int id, TodoService todoService) =>
            todoService.GetById(id) is { } todo
                ? Results.Ok(todo)
                : Results.NotFound());

        // add
        todosApi.MapPost("/", (Todo todo, TodoService todoService) =>
        {
            todoService.Add(todo);
            return Results.Created($"/{todo.Id}", todo);
        });

        // update
        todosApi.MapPut("/{id}", (int id, Todo inputTodo, TodoService todoService) =>
        {
            var todo = todoService.GetById(id);

            if (todo is null) return Results.NotFound();

            todoService.Update(id, inputTodo);

            return Results.NoContent();
        });

        // delete
        todosApi.MapDelete("/{id}", (int id, TodoService todoService) =>
        {
            if (todoService.GetById(id) is Todo todo)
            {
                todoService.DeleteById(id);
                return Results.NoContent();
            }

            return Results.NotFound();
        });
    }
}
```
The code is structured into two main sections: the ```Program``` and the ```WebApplicationExtensions```.

The ```Program.cs``` file contains the entry point of the application and is responsible for configuring the Blazor app and defining the main logic.

- The ```Main``` method is the entry point of the application. It creates a ```WebApplication builder``` using the ```WebApplication.CreateSlimBuilder``` method.
- The ```ConfigureHttpJsonOptions``` method is used to configure the ```JSON``` serialization options for the ```HTTP requests and responses```. In this code, it inserts a custom ```TypeInfoResolverChain``` at the beginning of the ```default``` options.
- The ```SampleData.AddTestData()``` method is called to add some ```test data``` to the ```Todo``` list.
- The ```TodoService``` is registered as a scoped service in the dependency injection container.
- The ```app.MapTodoGroup()``` method is called to define the ```endpoints``` for the Todo API.
- Finally, the ```app.Run()``` method is called to start the application.

The ```WebApplicationExtensions``` contains extension methods for the ```WebApplication class```, which are used to define the ```endpoints``` for the ```Todo API```.

- The ```todosApi``` variable is created by calling the ```app.MapGroup``` method with the ```"/todos"``` route.
- The ```todosApi.MapGet``` method is used to define a ```GET``` endpoint for retrieving all the ```Todo``` items.
- The ```todosApi.MapGet``` method is used to define a ```GET``` endpoint for retrieving all the completed ```Todo``` items.
- The ```todosApi.MapGet``` method is used to define a ```GET``` endpoint for retrieving a ```Todo``` item by its ```ID```.
- The ```todosApi.MapPost``` method is used to define a ```POST``` endpoint for adding a new ```Todo``` item.
- The ```todosApi.MapPut``` method is used to define a ```PUT``` endpoint for updating a ```Todo``` item.
- The ```todosApi.MapDelete``` method is used to define a ```DELETE``` endpoint for deleting a ```Todo``` item.



##### **BlazorApp Presentation Project**

<kbd>TodoClientService.cs</kbd>
```
using BlazorAppandMinimalAPIsNativeAOTCRUD.Core.DataModels;
using System.Text.Json;

namespace BlazorApp.Services;

public class TodoClientService(IHttpClientFactory httpClientFactory)
{
    public async Task<TodoDataModelRecord?> GetById(int id)
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.GetAsync($"todos/{id}");

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            using var contentStream = await httpResponseMessage.Content.ReadAsStreamAsync();

            return await JsonSerializer.DeserializeAsync<TodoDataModelRecord?>(contentStream, Program.TodoApiJsonOptions);
        }

        return null;
    }

    public async Task<TodoDataModelRecord[]?> GetCompletedAsync()
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.GetAsync("todos/complete");

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            using var contentStream = await httpResponseMessage.Content.ReadAsStreamAsync();

            return await JsonSerializer.DeserializeAsync<TodoDataModelRecord[]>(contentStream, Program.TodoApiJsonOptions);
        }

        return [];
    }

    public async Task<TodoDataModelRecord[]?> GetAllAsync()
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.GetAsync("todos");

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            using var contentStream = await httpResponseMessage.Content.ReadAsStreamAsync();

            return await JsonSerializer.DeserializeAsync<TodoDataModelRecord[]>(contentStream, Program.TodoApiJsonOptions);
        }

        return [];
    }

    public async Task<bool> AddAsync(TodoDataModel todo)
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.PostAsJsonAsync("todos", todo, Program.TodoApiJsonOptions, default);

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            return true;
        }

        return false;
    }

    public async Task<bool> UpdateAsync(int id, TodoDataModel todo)
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.PutAsJsonAsync($"todos/{id}", todo, Program.TodoApiJsonOptions, default);

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            return true;
        }

        return false;
    }

    public async Task<bool> DeleteByIdAsync(int id)
    {
        using var httpClient = httpClientFactory.CreateClient("TodoApi");

        var httpResponseMessage = await httpClient.DeleteAsync($"todos/{id}");

        if (httpResponseMessage.IsSuccessStatusCode)
        {
            return true;
        }

        return false;
    }
}
```
The ```TodoClientService``` class is a service in a ```Blazor app``` that interacts with a ```Todo API``` to perform ```CRUD``` (Create, Read, Update, Delete) operations on ```TodoDataModel``` records. It uses the ```HttpClientFactory``` to create an instance of ```HttpClient``` and makes ```HTTP requests``` to the ```Todo API endpoints```.


<kbd>Program.cs</kbd>
```
using BlazorApp.Components;
using BlazorApp.Services;
using System.Text.Json;

internal class Program
{
    public const string API_URL = "http://localhost:5000";

    public static readonly JsonSerializerOptions TodoApiJsonOptions = new() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    private static void Main(string[] args)
    {
...
        builder.Services.AddHttpClient("TodoApi", httpClient =>
        {
            httpClient.BaseAddress = new Uri(API_URL);
        });

        // Add services to the container.
        builder.Services.AddScoped<TodoClientService>();
        builder.Services.AddRazorComponents()
                        .AddInteractiveServerComponents();
...
    }
}
```
The ```Program.cs``` file, which is responsible for configuring and running the ```Blazor app```. Let's break down the code structure and understand its components:

HttpClient Configuration: This code configures an ```HttpClient``` named ```"TodoApi"``` and sets its ```base address``` to the ```API_URL``` constant.

Service Registration: This code registers the ```TodoClientService``` as a ```scoped``` service. The ```TodoClientService``` is responsible for interacting with the ```Todo API```.



#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppandMinimalAPIsNativeAOTCRUD
