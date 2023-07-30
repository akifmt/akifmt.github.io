---
title: "Blazor Authentication and Authorization"
date: 2023-07-30T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Authentication and Authorization
menu:
  dotnet:
    name: Blazor Authentication and Authorization
    identifier: blazor-authentication-authorization
    weight: -20230730
tags: [ Dotnet, Blazor Authentication and Authorization]
categories: [ Dotnet, Blazor Authentication and Authorization]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

##### **Blazor Authentication and Authorization**
This example represents a Blazor application that allows users to view a list of blog posts. The application uses ASP.NET Core and Blazor authentication and authorization to control access to the blog posts.

Before diving into the code, let's understand some key concepts related to Blazor:

Blazor: Blazor is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables developers to write code that runs on the client-side in the browser using WebAssembly or on the server-side using SignalR.

ASP.NET Core: ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications. It provides a unified programming model for building web APIs, web UIs, and real-time applications.

Blazor Authentication and Authorization: Blazor provides built-in support for authentication and authorization. It allows developers to secure their applications by restricting access to certain resources based on user roles or policies.

Razor: Razor is a syntax for combining HTML markup with C# code in a single file. It is used in Blazor to create dynamic web UIs.

The code is structured as a Blazor component with a Razor markup file (.razor) and a code-behind file (.cs). The Razor markup file contains the HTML and Razor syntax for rendering the UI, while the code-behind file contains the C# code for handling events and data manipulation.

###### **List Posts (Index.razor)**
The code starts with the ```@page``` directive, which specifies the URL route for this component ("/BlogPost"). The ```@using``` directives import the necessary namespaces for using classes and services in the code.

The ```@inject``` directives are used to inject dependencies into the component. In this case, the ```IMapper, NavigationManager, and BlogPostService``` are injected. The ```IMapper``` is used for object mapping, the ```NavigationManager``` is used for navigation, and the ```BlogPostService``` is used for retrieving blog posts.

The ```<PageTitle>``` component sets the page title to "Index".

The ```<AuthorizeView>``` component is used to control access to the content based on the user's authorization. The ```Policy``` attribute specifies the policy required to access the content. Inside the ```<Authorized>``` block, the UI for displaying the list of blog posts is rendered.

The code checks if the ```blogPosts``` variable is null. If it is null, a loading message is displayed. Otherwise, a table is rendered with the blog post data. The ```@foreach``` loop iterates over each blog post in the ```blogPosts``` collection and displays the blog post's ID, title, and content. The ```AuthorizeView``` component is used to conditionally render the edit and delete links based on the user's authorization.

The ```<NotAuthorized>``` block is rendered when the user is not authorized to access the content.

The ```@code``` block contains the C# code for the component. The ```blogPosts``` variable is declared as an ```IEnumerable<BlogPostViewModel>``` and is initially set to null. The ```OnInitializedAsync``` method is overridden to retrieve the blog posts from the ```BlogPostService``` and map them to the ```BlogPostViewModel``` using the IMapper.

###### **Create Post (Create.razor)**
Let's take a look at some code examples to better understand how the blog post creation works:

Creating the BlogPostViewModel:
```
private BlogPostViewModel? blogPost;

protected override void OnInitialized()
{
    blogPost = new();
}
```
In the ```OnInitialized``` method, we initialize the ```blogPost``` variable as a new instance of the ```BlogPostViewModel``` class. This class represents the data structure for the blog post.

Handling the Valid Submit:
```
private async void HandleValidSubmit()
{
    var model = Mapper.Map<BlogPostViewModel, BlogPost>(blogPost);
    bool result = await BlogPostService.AddBlogPostAsync(model);
    if (result)
        NavigationManager.NavigateTo("/BlogPost");
}
```
The ```HandleValidSubmit``` method is called when the user clicks the submit button. It maps the ```blogPost``` object to the ```BlogPost``` model using AutoMapper. Then, it calls the ```AddBlogPostAsync``` method of the ```BlogPostService``` to add the blog post to the database. If the operation is successful, the user is redirected to the "/BlogPost" page.

###### **Post Detail (Details.razor)**
Here are some key code examples from the Blazor BlogPost Details page:

Retrieving the blog post data:
```
protected override async Task OnInitializedAsync()
{
    if (blogPost == null)
    {
        var result = await BlogPostService.GetbyId(id);
        if (result != null)
            blogPost = Mapper.Map<BlogPost, BlogPostViewModel>(result);
    }
}
```
In the ```OnInitializedAsync``` method, the blog post data is retrieved from the server using the ```BlogPostService.GetbyId``` method. If the result is not null, the data is mapped to a ```BlogPostViewModel``` object using AutoMapper and assigned to the ```blogPost``` variable.

Displaying the blog post details:
```
<dl class="row">
    <dt class="col-sm-2">
        @nameof(BlogPost.Id)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Id
    </dd>
    <dt class="col-sm-2">
        @nameof(BlogPost.Title)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Title
    </dd>
    <dt class="col-sm-2">
        @nameof(BlogPost.Content)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Content
    </dd>
</dl>
```
The blog post details are displayed using an HTML definition list ```(<dl>)``` with rows and columns. The ```@nameof``` directive is used to get the name of the properties of the ```BlogPost``` class, and the ```@blogPost``` variable is used to display the corresponding values.

###### **Edit Post (Edit.razor)**
Here are some code examples from the provided code:

Binding the blogPost.Title property to an input field:
```
<InputText @bind-Value="blogPost.Title" class="form-control" />
```
Handling the form submission and updating the blog post:
```
private async void HandleValidSubmit()
{
    var model = Mapper.Map<BlogPostViewModel, BlogPost>(blogPost);
    bool result = await BlogPostService.UpdateBlogPostAsync(id, model);
    if (result)
        NavigationManager.NavigateTo("/BlogPost");
}
```

###### **Delete Post (Delete.razor)**
Let's take a closer look at some code examples to understand the implementation:

Delete Confirmation:
```
<h3>Are you sure you want to delete this?</h3>
```
Blog Post Details:
```
<dl class="row">
    <dt class="col-sm-2">
        @nameof(BlogPost.Id)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Id
    </dd>
    <dt class="col-sm-2">
        @nameof(BlogPost.Title)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Title
    </dd>
    <dt class="col-sm-2">
        @nameof(BlogPost.Content)
    </dt>
    <dd class="col-sm-10">
        @blogPost.Content
    </dd>
</dl>
```
Delete Button:
```
<button class="btn btn-danger" @onclick="DeleteButtonClick">Delete</button> |
<a href="/BlogPost">Back to List</a>
```

###### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppAuth
