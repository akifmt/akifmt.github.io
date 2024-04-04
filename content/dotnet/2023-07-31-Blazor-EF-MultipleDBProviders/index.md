---
title: "Blazor EF Migrations with Multiple Providers"
date: 2023-07-31T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor  EF Migrations with Multiple Providers
menu:
  dotnet:
    name: Blazor EF Migrations with Multiple Providers
    identifier: blazor-ef-migrations-with-multiple-providers
    weight: -20230731
tags: [ Dotnet, Blazor EF Migrations with Multiple Providers]
categories: [ Dotnet, EF Migrations with Multiple Providers]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

##### **Blazor Blazor Entity Framework Migrations with Multiple Providers**
This code demonstrates how to implement multiple database providers in a Blazor application using C#. It allows you to switch between different database providers, such as InMemory, SQLite, and SQL Server, based on the configuration settings.

Database Provider: Refers to the type of database being used, such as InMemory, SQLite, or SQL Server.

Connection String: A string that contains the necessary information to connect to a specific database.

Entity Framework: A popular object-relational mapping (ORM) framework that simplifies database access in .NET applications.

Migrations: A way to manage database schema changes over time, allowing you to update the database structure without losing data.

Dependency Injection: A design pattern that allows objects to be created and managed by a separate container, making it easier to manage dependencies between different components of an application.

###### **Program.cs**
The code consists of two main sections: the Main method and the ServiceCollectionExtensions class.

In the ```Main``` method:

- It creates a ```WebApplication``` builder.
- It retrieves the active database provider from the configuration settings.
- If a provider is specified, it configures the services and connection string for that provider.
- If the provider is InMemory, it checks and creates the database.
- It adds necessary services to the container.
- It builds the application and runs it.

In the ```ServiceCollectionExtensions``` class:
- It defines an extension method ```ConfigureServices``` for the ```IServiceCollection``` interface.
- It configures the ```ApplicationDbContext``` based on the specified provider and connection string.
- It adds services for ```SeedData``` and ```BlogPostService```.
- It configures AutoMapper for mapping between different models.
- It creates a mapper instance and adds it as a singleton service.

Here are some code examples to illustrate the usage of the code:

Configuring the services for SQLite provider:
```
services.ConfigureServices("Sqlite", "Data Source=mydatabase.db");
```
Checking and creating the database:
```
services.CheckAndCreateDatabase();
```
Using the SeedData service to create initial data:
```
var seedData = serviceProvider.GetRequiredService<SeedData>();
await seedData.CreateInitialData();
```

###### **appsettings.json**
The configuration settings for the Blazor application, including the database provider and connection strings.
```
{
  "DatabaseProvider": "InMemory",
  "ConnectionStrings": {
    "Sqlite": "",
    "SqlServer": "",
    "Localdb": ""
  },
```
DatabaseProvider: This property specifies the default database provider to be used by the Blazor application. In this example, the value is set to "InMemory", which means an in-memory database will be used. You can replace this value with the desired database provider, such as "Sqlite" or "SqlServer".

ConnectionStrings: This property contains a collection of connection strings for different database providers. In this example, three connection strings are provided: "Sqlite", "SqlServer", and "Localdb". Each connection string specifies the necessary details to connect to the respective database provider.

###### **buildschema.bat**

- Creating a migration:
```
dotnet ef migrations add Initial01 -c ApplicationDbContext --startup-project ./BlazorAppEFMultipleDBProviders --project ./AppMigrations/AppMigrations.Sqlite -- --DatabaseProvider Sqlite
```
This command adds a new migration named "Initial01" to the specified context (ApplicationDbContext). It generates the migration files in the ./Migrations/Initial01 directory. The --startup-project and --project flags specify the startup project and the project containing the migration files, respectively. The --DatabaseProvider Sqlite flag indicates that we are using the SQLite database provider.

- Generating a SQL script for a migration:
```
dotnet ef migrations script -c ApplicationDbContext -o ./AppMigrations/AppMigrations.Sqlite/Migrations/Initial01.sql --startup-project ./BlazorAppEFMultipleDBProviders --project ./AppMigrations/AppMigrations.Sqlite -- --DatabaseProvider Sqlite
```
This command generates a SQL script for the migration named "Initial01". The script is saved in the ./AppMigrations/AppMigrations.Sqlite/Migrations/Initial01.sql file. The flags have the same meaning as in the previous command.

- Running the application with seeding:
```
dotnet run --project ./BlazorAppEFMultipleDBProviders/BlazorAppEFMultipleDBProviders.csproj /seed
```
This command runs the application, specifically the BlazorAppEFMultipleDBProviders.csproj project, with the /seed argument. This argument triggers the seeding of initial data into the database.


###### **Conclusion**
Configuring multiple database providers in a Blazor application is essential when you need to work with different databases or switch between databases based on the environment. By using the provided JSON configuration, you can easily specify the desired database provider and connection strings for each provider. This flexibility allows your Blazor application to seamlessly interact with different databases, providing a robust and scalable solution.

This example demonstrates how to implement multiple database providers in a Blazor application using C#. By configuring the services and connection string based on the specified provider, you can easily switch between different databases. The code also shows how to use migrations and seed data to manage the database schema and initial data.

###### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppEFMultipleDBProviders
