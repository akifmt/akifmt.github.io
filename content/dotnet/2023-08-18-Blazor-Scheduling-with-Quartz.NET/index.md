---
title: "Blazor Scheduling with Quartz.NET"
date: 2023-08-18T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Scheduling with Quartz.NET
menu:
  dotnet:
    name: Blazor Scheduling with Quartz.NET
    identifier: blazor-scheduling-with-quartz-NET
    weight: -20230818
tags: [ Dotnet, Blazor Scheduling with Quartz.NET]
categories: [ Dotnet, Blazor Scheduling with Quartz.NET]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

#### **Blazor Scheduling with Quartz.NET**

Blazor: Blazor is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables full-stack development with .NET.

<img src="quartz_logo.png" alt="quartz_logo" title="quartz_logo" style="height:100px">

Quartz.NET: Quartz.NET is a popular open-source job scheduling library for .NET applications. It provides a flexible and powerful way to schedule and execute jobs at specified intervals.

Scheduling: Scheduling refers to the process of defining when and how often a job or task should be executed. In this code, we use Quartz.NET to schedule jobs to be executed at specific times or intervals.

Jobs: In the context of this application, a job refers to a specific task or action that needs to be executed at a scheduled time or interval. Jobs can be created, paused, resumed, and deleted.

Triggers: Triggers are used to define when and how often a job should be executed. They specify the schedule, start time, end time, and other parameters for a job.

Job Class: A job class in Quartz.NET represents a unit of work that needs to be executed by the scheduler. It implements the ```IJob``` interface and defines the logic to be executed when the job is triggered.

##### **AddRandomBlogPostJob.cs**
The ```AddRandomBlogPostJob``` class is defined in the ```BlazorAppQuartzNETScheduler.Jobs``` namespace. It implements the ```IJob``` interface provided by Quartz.NET. The class is marked with the ```[DisallowConcurrentExecution]``` attribute, which ensures that the job will not be executed concurrently.

```
using BlazorAppQuartzNETScheduler.Data;
using BlazorAppQuartzNETScheduler.Models;
using BlazorAppQuartzNETScheduler.Services;
using Quartz;

namespace BlazorAppQuartzNETScheduler.Jobs;

[DisallowConcurrentExecution]
public class AddRandomBlogPostJob : IJob
{
    private readonly ILogger<AddRandomBlogPostJob> _logger;
    private readonly BlogPostService _blogPostService;

    public AddRandomBlogPostJob(ILogger<AddRandomBlogPostJob> logger, BlogPostService blogPostService)
    {
        _logger = logger;
        _blogPostService = blogPostService;
    }

    public async Task Execute(IJobExecutionContext context)
    {
        BlogPost model = SeedData.GetRandomPost();
        await _blogPostService.AddBlogPostAsync(model);
    }
}
```

The class has two dependencies injected through its constructor: ```ILogger<AddRandomBlogPostJob>``` and ```BlogPostService```. The ```ILogger``` is used for logging purposes, while the ```BlogPostService``` is a service responsible for adding blog posts to the application.

The ```Execute``` method is the main entry point for the job execution. It is called by the Quartz.NET scheduler when the job is triggered. Inside the ```Execute``` method, a random blog post model is obtained using the ```SeedData.GetRandomPost()``` method. Then, the ```AddBlogPostAsync``` method of the ```BlogPostService``` is called to add the blog post to the application.

##### **Program.cs**
Setting up the Quartz database:
```
string quartzConnectionString = builder.Configuration.GetConnectionString("QuartzConnectionString");
SqliteConnectionStringBuilder quartzSqliteConnectionStringBuilder = new SqliteConnectionStringBuilder(
    quartzConnectionString);
CheckandCreateQuartzDatabase(quartzSqliteConnectionStringBuilder).GetAwaiter().GetResult();
```
We retrieve the connection string for the Quartz database from the application's configuration. We then create a ```SqliteConnectionStringBuilder``` object using the connection string and pass it to the ```CheckandCreateQuartzDatabase``` method to check and create the Quartz database if necessary.

Adding Quartz services:
```
builder.Services.AddDbContext<QuartzDbContext>(options =>
{
    options.UseSqlite(quartzConnectionString);
});
builder.Services.AddQuartz(q =>
{
    q.UsePersistentStore(per =>
    {
        per.UseSQLite(quartzSqliteConnectionStringBuilder.ConnectionString);
        per.UseJsonSerializer();
    });
    q.UseMicrosoftDependencyInjectionJobFactory();
});
builder.Services.AddQuartzHostedService(opt =>
{
    opt.WaitForJobsToComplete = true;
});
```
We add the necessary Quartz services to the ```application's service``` collection. We configure the Quartz database context using the ```SQLite connection string```, enable persistent storage for ```Quartz jobs``` using SQLite, and specify the job factory to use ```Microsoft Dependency Injection```. We also configure the Quartz hosted service to wait for jobs to complete before shutting down.

Checking and creating jobs:
```
public static async Task CheckandCreateJobs(this IServiceCollection services)
{
    using (IServiceScope tmp = services.BuildServiceProvider().CreateScope())
    {
        await using var _context = tmp.ServiceProvider.GetRequiredService<ApplicationDbContext>();
        var seedData = tmp.ServiceProvider.GetRequiredService<SeedData>();
        await seedData.CheckandCreateJobsData();
    }
}
```
This code defines an extension method ```CheckandCreateJobs``` for the ```IServiceCollection``` interface. It creates a new scope using the application's service provider and retrieves the necessary services (```ApplicationDbContext``` and ```SeedData```). It then calls the ```CheckandCreateJobsData``` method to check and create the necessary jobs.


##### **Quartz.NET Models**
Quartz.NET provides various models and classes to represent different entities involved in scheduling, such as triggers, jobs, and schedulers. These models help in configuring and managing the scheduling process.
```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzBlobTrigger
{
    public string SchedName { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public byte[]? BlobData { get; set; }

    public virtual QrtzTrigger QrtzTrigger { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzCalendar
{
    public string SchedName { get; set; } = null!;
    public string CalendarName { get; set; } = null!;
    public byte[] Calendar { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzCronTrigger
{
    public string SchedName { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public string CronExpression { get; set; } = null!;
    public string? TimeZoneId { get; set; }

    public virtual QrtzTrigger QrtzTrigger { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzFiredTrigger
{
    public string SchedName { get; set; } = null!;
    public string EntryId { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public string InstanceName { get; set; } = null!;
    public long FiredTime { get; set; }
    public long SchedTime { get; set; }
    public long Priority { get; set; }
    public string State { get; set; } = null!;
    public string? JobName { get; set; }
    public string? JobGroup { get; set; }
    public byte[]? IsNonconcurrent { get; set; }
    public byte[]? RequestsRecovery { get; set; }
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzJobDetail
{
    public QrtzJobDetail()
    {
        QrtzTriggers = new HashSet<QrtzTrigger>();
    }

    public string SchedName { get; set; } = null!;
    public string JobName { get; set; } = null!;
    public string JobGroup { get; set; } = null!;
    public string? Description { get; set; }
    public string JobClassName { get; set; } = null!;
    public byte[] IsDurable { get; set; } = null!;
    public byte[] IsNonconcurrent { get; set; } = null!;
    public byte[] IsUpdateData { get; set; } = null!;
    public byte[] RequestsRecovery { get; set; } = null!;
    public byte[]? JobData { get; set; }

    public virtual ICollection<QrtzTrigger> QrtzTriggers { get; set; }
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzLock
{
    public string SchedName { get; set; } = null!;
    public string LockName { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzPausedTriggerGrp
{
    public string SchedName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzSchedulerState
{
    public string SchedName { get; set; } = null!;
    public string InstanceName { get; set; } = null!;
    public long LastCheckinTime { get; set; }
    public long CheckinInterval { get; set; }
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzSimpleTrigger
{
    public string SchedName { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public long RepeatCount { get; set; }
    public long RepeatInterval { get; set; }
    public long TimesTriggered { get; set; }

    public virtual QrtzTrigger QrtzTrigger { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzSimpropTrigger
{
    public string SchedName { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public string? StrProp1 { get; set; }
    public string? StrProp2 { get; set; }
    public string? StrProp3 { get; set; }
    public long? IntProp1 { get; set; }
    public long? IntProp2 { get; set; }
    public long? LongProp1 { get; set; }
    public long? LongProp2 { get; set; }
    public byte[]? DecProp1 { get; set; }
    public byte[]? DecProp2 { get; set; }
    public byte[]? BoolProp1 { get; set; }
    public byte[]? BoolProp2 { get; set; }
    public string? TimeZoneId { get; set; }

    public virtual QrtzTrigger QrtzTrigger { get; set; } = null!;
}
```

```
namespace BlazorAppQuartzNETScheduler.Models.Quartz;

public partial class QrtzTrigger
{
    public string SchedName { get; set; } = null!;
    public string TriggerName { get; set; } = null!;
    public string TriggerGroup { get; set; } = null!;
    public string JobName { get; set; } = null!;
    public string JobGroup { get; set; } = null!;
    public string? Description { get; set; }
    public long? NextFireTime { get; set; }
    public long? PrevFireTime { get; set; }
    public long? Priority { get; set; }
    public string TriggerState { get; set; } = null!;
    public string TriggerType { get; set; } = null!;
    public long StartTime { get; set; }
    public long? EndTime { get; set; }
    public string? CalendarName { get; set; }
    public long? MisfireInstr { get; set; }
    public byte[]? JobData { get; set; }

    public virtual QrtzJobDetail QrtzJobDetail { get; set; } = null!;
    public virtual QrtzBlobTrigger? QrtzBlobTrigger { get; set; }
    public virtual QrtzCronTrigger? QrtzCronTrigger { get; set; }
    public virtual QrtzSimpleTrigger? QrtzSimpleTrigger { get; set; }
    public virtual QrtzSimpropTrigger? QrtzSimpropTrigger { get; set; }
}
```


##### **QuartzDbContext.cs**
The ```QuartzDbContext``` class is defined in the ```BlazorAppQuartzNETScheduler.Data``` namespace. It extends the ```DbContext``` class and overrides the ```OnModelCreating``` method to configure the database schema.

The class includes properties for each of the ```Quartz.NET``` entities, such as ```QrtzBlobTrigger, QrtzCalendar, QrtzCronTrigger,``` etc. These properties represent database tables and are used to query and manipulate data related to ```Quartz.NET``` scheduling.

```
using BlazorAppQuartzNETScheduler.Models.Quartz;
using Microsoft.EntityFrameworkCore;

namespace BlazorAppQuartzNETScheduler.Data;

public partial class QuartzDbContext : DbContext
{
    public QuartzDbContext(DbContextOptions<QuartzDbContext> options)
        : base(options)
    {
    }

    public virtual DbSet<QrtzBlobTrigger> QrtzBlobTriggers { get; set; } = null!;
    public virtual DbSet<QrtzCalendar> QrtzCalendars { get; set; } = null!;
    public virtual DbSet<QrtzCronTrigger> QrtzCronTriggers { get; set; } = null!;
    public virtual DbSet<QrtzFiredTrigger> QrtzFiredTriggers { get; set; } = null!;
    public virtual DbSet<QrtzJobDetail> QrtzJobDetails { get; set; } = null!;
    public virtual DbSet<QrtzLock> QrtzLocks { get; set; } = null!;
    public virtual DbSet<QrtzPausedTriggerGrp> QrtzPausedTriggerGrps { get; set; } = null!;
    public virtual DbSet<QrtzSchedulerState> QrtzSchedulerStates { get; set; } = null!;
    public virtual DbSet<QrtzSimpleTrigger> QrtzSimpleTriggers { get; set; } = null!;
    public virtual DbSet<QrtzSimpropTrigger> QrtzSimpropTriggers { get; set; } = null!;
    public virtual DbSet<QrtzTrigger> QrtzTriggers { get; set; } = null!;

}
```

the ```QuartzDbContext``` class is defined with a constructor that accepts ```DbContextOptions<QuartzDbContext>``` as a parameter. This allows the class to be configured with the necessary options for connecting to the database.

The class includes properties for each of the Quartz.NET entities, such as ```QrtzBlobTriggers, QrtzCalendars,``` etc. These properties are used to query and manipulate data related to Quartz.NET scheduling.

The ```OnModelCreating``` method is overridden to configure the database schema using the ```ModelBuilder``` object. In this example, the configuration for the ```QrtzBlobTrigger``` entity is provided, including the table name, column mappings, and foreign key relationship.


##### **Jobs.razor**
the Jobs page component demonstrates how to display a list of jobs and their ```trigger``` states, as well as how to ```pause or resume all jobs```. With this application, users can easily schedule and manage their jobs with ease and efficiency.
```
@page "/Jobs"

@using BlazorAppQuartzNETScheduler.Models.Quartz
@using BlazorAppQuartzNETScheduler.Data
@using Microsoft.EntityFrameworkCore;
@using Quartz;

@inject QuartzDbContext QuartzDbContext
@inject ISchedulerFactory SchedulerFactory
@inject NavigationManager NavigationManager

<PageTitle>Index</PageTitle>

<h1>Index</h1>
<p>
    <button class="btn btn-primary" @onclick="() => ResumeAllJobs()">Resume All Jobs</button>
    <button class="btn btn-danger" @onclick="() => PauseAllJobs()">Pause All Jobs</button>
</p>
@if (jobs == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>@nameof(QrtzJobDetail.JobName)</th>
                <th>@nameof(QrtzJobDetail.JobGroup)</th>
                <th>TriggerState</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var job in jobs)
            {
                <tr>
                    <td>@job.JobName</td>
                    <td>@job.JobGroup</td>
                    <td>
                        @foreach (var trigger in job.QrtzTriggers)
                        {
                            <p>
                                TriggerState: @($"{trigger.TriggerState}")
                            </p>
                        }
                    </td>
                </tr>
            }
        </tbody>
    </table>
}

@code {
    private IEnumerable<QrtzJobDetail>? jobs;
    private IScheduler? scheduler;

    protected override async Task OnInitializedAsync()
    {
        if (jobs is null)
            await LoadData();

        scheduler = await SchedulerFactory.GetScheduler();
    }

    private async Task LoadData()
    {
        jobs = await QuartzDbContext.QrtzJobDetails.Include(x => x.QrtzTriggers).ToListAsync();
    }

    async void ResumeAllJobs()
    {
        await scheduler.ResumeAll();
        NavigationManager.NavigateTo("/Jobs", true);
    }

    async void PauseAllJobs()
    {
        await scheduler.PauseAll();
        NavigationManager.NavigateTo("/Jobs", true);
    }
}
```

#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppQuartzNETScheduler