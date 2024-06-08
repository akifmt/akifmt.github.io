---
title: "Blazor Serilog Logging to Console, File and Database"
date: 2023-08-15T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Serilog Logging to Console, File and Database
menu:
  dotnet:
    name: Blazor Serilog Logging to Console, File and Database
    identifier: blazor-serilog-logging-to-console-file-and-database
    weight: -20230815
tags: [ Dotnet, Blazor Serilog Logging to Console File and Database]
categories: [ Dotnet, Blazor Serilog Logging to Console File and Database]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

#### **Blazor Serilog Logging to Console, File and Database**


{{< alert type="warning" >}}
Warning

- This post is about ```.NET 6```.
- ```.NET 8``` version is in the link below:  
Link: [Blazor Radzen .NET 8 Serilog Logging to Console, File and Database](/dotnet/2024-05-29-blazor-radzen-.net8-serilog-logging/)

{{< /alert >}}


Logging: Logging is the process of recording events, messages, or exceptions that occur during the execution of an application. It helps developers understand the behavior of the application, diagnose issues, and track its performance. Logging is an essential aspect of software development and plays a crucial role in maintaining and troubleshooting applications.


<img src="serilog_logo.png" alt="serilog_logo" title="serilog_logo" style="height:100px">

Serilog: Serilog is a popular logging library for .NET applications. It provides a flexible and extensible logging framework that allows developers to capture and store log events for debugging, monitoring, and analysis purposes. Serilog supports various logging sinks, including console logging, file logging, and database logging.

Console Logging: Console logging is a type of logging where log messages are displayed in the console window. It is useful during development and debugging.

File Logging: File logging is a type of logging where log messages are written to a file. It helps in storing log data for future analysis and troubleshooting.

SQLite Database Logging: SQLite database logging is a type of logging where log messages are stored in an SQLite database. It provides a structured way to store and query log data.

##### **Log.cs**
The provided code defines a Log model class within the BlazorAppSerilogLogging.Models namespace. The Log class has the following properties:

```
namespace BlazorAppSerilogLogging.Models;

public class Log
{
    public int id { get; set; }
    public DateTime Timestamp { get; set; }
    public string Level { get; set; } = string.Empty;
    public string Exception { get; set; } = string.Empty;
    public string RenderedMessage { get; set; } = string.Empty;
    public string Properties { get; set; } = string.Empty;
}
```
id: An integer property that represents the unique identifier of the log entry.

Timestamp: A DateTime property that stores the timestamp when the log entry was created.

Level: A string property that indicates the log level of the entry (e.g., Information, Warning, Error).

Exception: A string property that holds the exception details, if any, associated with the log entry.

RenderedMessage: A string property that contains the formatted log message.

Properties: A string property that stores additional properties or metadata related to the log entry.


##### **ApplicationLoggerDbContext.cs**
The ```ApplicationLoggerDbContext``` class is a C# code that represents the database context for logging in a Blazor application using Serilog. It is responsible for managing the connection to the database and providing access to the ```Logs``` table.
```
using BlazorAppSerilogLogging.Models;
using Microsoft.EntityFrameworkCore;

namespace BlazorAppSerilogLogging.Data;

public class ApplicationLoggerDbContext : DbContext
{
    public ApplicationLoggerDbContext(DbContextOptions<ApplicationLoggerDbContext> options)
    : base(options)
    {
    }

    public DbSet<Log> Logs => Set<Log>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
    }
}
```
The ```ApplicationLoggerDbContext``` class is defined within the ```BlazorAppSerilogLogging.Data``` namespace. It extends the ```DbContext``` class provided by Entity Framework Core.

The class has a constructor that takes an instance of ```DbContextOptions<ApplicationLoggerDbContext>``` as a parameter. This allows the class to configure the database connection options.

The class also defines a property called ```Logs``` of type ```DbSet<Log>```. This property represents the ```Logs``` table in the database and allows you to query and manipulate log data.

The ```OnModelCreating``` method is overridden but left empty in this code example. This method is used to configure the database model and define relationships between entities.


##### **LoggerService.cs**
The ```LoggerService``` class is defined within the ```BlazorAppSerilogLogging.Data``` namespace. It implements several methods for logging operations and interacts with the ```ApplicationLoggerDbContext``` class.

```
using BlazorAppSerilogLogging.Models;
using Microsoft.EntityFrameworkCore;

namespace BlazorAppSerilogLogging.Data;

public class LoggerService
{
    private readonly ILogger<LoggerService> _logger;
    private readonly ApplicationLoggerDbContext _loggerDbContext;

    public LoggerService(ILogger<LoggerService> logger, ApplicationLoggerDbContext loggerDbContext)
    {
        _logger = logger;
        _loggerDbContext = loggerDbContext;
    }

    public async Task<IEnumerable<Log>> GetLogsAsync()
    {
        _logger.LogInformation($"Called GetLogsAsync");
        return await _loggerDbContext.Logs.OrderByDescending(x => x.Timestamp).ToListAsync();
    }

    public async Task<Log?> GetLogAsync(int id)
    {
        _logger.LogInformation($"Called GetLogAsync", id);
        return await _loggerDbContext.Logs.FirstOrDefaultAsync(x => x.id == id);
    }

    public async Task<bool?> DeleteLogsAsync()
    {
        _logger.LogInformation($"Called DeleteLogsAsync");
        var all = await _loggerDbContext.Logs.ToListAsync();
        _loggerDbContext.Logs.RemoveRange(all); ;
        await _loggerDbContext.SaveChangesAsync();
        _logger.LogInformation($"Deleted All Logs.");
        return true;
    }
}
```
The class has the following members:

_logger: An instance of the ```ILogger<LoggerService>``` interface, which is used for logging messages.

_loggerDbContext: An instance of the ```ApplicationLoggerDbContext``` class, which represents the database context for logging.

The constructor of the ```LoggerService``` class takes in an ```ILogger<LoggerService>``` instance and an ```ApplicationLoggerDbContext``` instance as parameters. These dependencies are injected into the class using dependency injection.


##### **Index.razor**
Index.razor is a Blazor component that displays logs retrieved from a logging service. It allows users to view and delete logs. The logs are fetched asynchronously and displayed in a table format.

```
@page "/Logs"

@using AutoMapper
@using BlazorAppSerilogLogging.Data;
@using BlazorAppSerilogLogging.Models;
@using BlazorAppSerilogLogging.Services;
@using BlazorAppSerilogLogging.ViewModels;

@inject IMapper Mapper
@inject NavigationManager NavigationManager
@inject LoggerService LoggerService

<PageTitle>Index</PageTitle>

<h1>Index</h1>
<p>
    <button class="btn btn-danger" @onclick="() => DeleteAllLogs()">Delete All Logs</button>
</p>
@if (logs == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>@nameof(LogViewModel.id)</th>
                <th>@nameof(LogViewModel.Timestamp)</th>
                <th>@nameof(LogViewModel.Level)</th>
                <th>@nameof(LogViewModel.Exception)</th>
                <th>@nameof(LogViewModel.RenderedMessage)</th>
                <th>@nameof(LogViewModel.Properties)</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            @foreach (var log in logs)
            {
                <tr>
                    <td>@log.id</td>
                    <td>@log.Timestamp</td>
                    <td>
                        <span class="text-@Helpers.LogEventLevelHelper.GetBootstrapUIClass(log.Level)">
                            @log.Level
                        </span>
                    </td>
                    <td>@log.Exception</td>
                    <td>@log.RenderedMessage</td>
                    <td>@log.Properties</td>
                    <td>
                    </td>
                </tr>
            }
        </tbody>
    </table>
}


@code {
    private IEnumerable<LogViewModel>? logs;

    protected override async Task OnInitializedAsync()
    {
        if (logs == null)
        {
            var result = await LoggerService.GetLogsAsync();
            logs = Mapper.Map<IEnumerable<Log>, IEnumerable<LogViewModel>>(result);
        }
    }

    private async void DeleteAllLogs()
    {
        await LoggerService.DeleteLogsAsync();
    }

}
```
This code block is executed when the component is initialized. It calls the ```GetLogsAsync``` method of the ```LoggerService``` to fetch the logs asynchronously. The retrieved logs are then mapped to ```LogViewModel``` objects using ```AutoMapper```. The ```logs``` variable is assigned the mapped logs.

##### **Program.cs**

```
builder.Host.UseSerilog((ctx, lc) => lc
	.MinimumLevel.Information()
	//.WriteTo.Console(new JsonFormatter(), restrictedToMinimumLevel: Serilog.Events.LogEventLevel.Information)
	.WriteTo.Console(restrictedToMinimumLevel: Serilog.Events.LogEventLevel.Information)
	.WriteTo.Seq("http://localhost:5001", restrictedToMinimumLevel: Serilog.Events.LogEventLevel.Information)
	.WriteTo.File(serilogFileLoggerFilePath,
					restrictedToMinimumLevel: Serilog.Events.LogEventLevel.Verbose,
					rollingInterval: RollingInterval.Hour,
					encoding: System.Text.Encoding.UTF8)
	.WriteTo.SQLite(sqliteDbFilePath,
					tableName: "Logs",
					restrictedToMinimumLevel:
						builder.Environment.IsDevelopment() ? Serilog.Events.LogEventLevel.Information : Serilog.Events.LogEventLevel.Warning,
					storeTimestampInUtc: false,
					batchSize:
						builder.Environment.IsDevelopment() ? (uint)1 : (uint)100,
					retentionPeriod: new TimeSpan(0, 1, 0, 0, 0),
					maxDatabaseSize: 10)
);
```

Retrieves the connection string for the ```SQLite logger``` from the configuration file and modifies it to include the current directory path.

Configures Serilog with various log sinks, including ```console logging, Seq logging, file logging, and SQLite database logging```. It sets the minimum log level based on the application environment.

Registers the ```ApplicationLoggerDbContext``` and ```ApplicationDbContext``` services in the dependency injection container. It configures the ```ApplicationLoggerDbContext``` to use the ```SQLite logger connection``` string and the ```ApplicationDbContext``` to use an in-memory database.


We discussed the key concepts of ```logging, console logging, file logging, and SQLite database logging```. We also examined the code structure and provided code examples to illustrate the configuration process. By understanding this code, developers can effectively set up logging in their Blazor applications using Serilog.

#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppSerilogLogging