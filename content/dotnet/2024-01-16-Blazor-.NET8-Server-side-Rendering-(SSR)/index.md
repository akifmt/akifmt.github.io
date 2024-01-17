---
title: "Blazor .NET8 Server-side Rendering (SSR)"
date: 2024-01-16T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor .NET8 Server-side Rendering (SSR)
menu:
  dotnet:
    name: Blazor .NET8 Server-side Rendering (SSR)
    identifier: blazor-dotnet8-server-side-rendering-SSR
    weight: -20240116
tags: [dotnet8, .NET8, Blazor, Server-side Rendering, SSR]
categories: [dotnet8, .NET8, Blazor, Server-side Rendering, SSR]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet" style="border-radius: 20px;"><br>
<p>

#### **Blazor .NET8 Server-side Rendering (SSR)**
Blazor .NET 8 offers several rendering options, including Server-side Rendering (SSR). SSR allows the initial rendering of a Blazor application to be performed on the server before being sent to the client. This approach provides benefits such as improved performance, SEO-friendliness, and better accessibility.

In Blazor .NET 8, there are two main ways to implement SSR: Global SSR with Prerendering and Per Page/Component SSR with Prerendering.

##### **Enable Global Rendering**
Global SSR with Prerendering involves rendering the entire application on the server and sending the pre-rendered HTML to the client. This approach is suitable for applications where the content is mostly static and doesn't require frequent updates. It ensures that the initial page load is fast and provides a good user experience.

<kbd>App.razor</kbd>
```
...
<HeadOutlet @rendermode="@InteractiveServer" />
...
<Routes @rendermode="@InteractiveServer" />
...
```
In the code snippet above, we have two components: HeadOutlet and Routes. These components are responsible for rendering the head section and routing in the application, respectively. The ```@rendermode``` attribute is set to ```@InteractiveServer```, which enables SSR and prerendering.

##### **Enable Global Rendering without Prerendering**

<kbd>App.razor</kbd>
```
...
<HeadOutlet @rendermode="new InteractiveServerRenderMode(prerender: false)" />
...
<Routes @rendermode="new InteractiveServerRenderMode(prerender: false)" />
...	
```
To enable global rendering without prerendering, we set the rendermode parameter of both components to an instance of ```InteractiveServerRenderMode``` with the ```prerender``` parameter set to false. This instructs the components to skip the prerendering process and perform rendering on the server dynamically.

By disabling prerendering, the application will be fully rendered on the server and sent to the client as ```HTML```. The client-side ```JavaScript``` and ```WebAssembly``` code will still be loaded and executed, but the initial ```HTML``` content will be available immediately, providing a faster initial load time.

##### **Enable per Page/Component Rendering**
Per Page/Component SSR with Prerendering allows you to selectively choose which pages or components should be pre-rendered on the server. This approach is useful when you have dynamic content that needs to be updated frequently. By pre-rendering specific pages or components, you can strike a balance between performance and dynamic content updates.

<kbd>App.razor</kbd>
```
...
<HeadOutlet />
...
<Routes />
...
```
The ```<HeadOutlet />``` component is responsible for rendering the ```<head>``` section of the HTML document. It is required for client-side rendering.

<kbd>Counter.razor</kbd>
```
@page "/counter"
...
@rendermode InteractiveServer
...
```
	
##### **Enable per Page/Component Rendering without Prerendering**

<kbd>Counter.razor</kbd>
```
@page "/counter"
...
@rendermode @(new InteractiveServerRenderMode(prerender: false))
...
```
In the above code snippet, we have a Blazor page with the route ```/counter```. By setting ```@rendermode @(new InteractiveServerRenderMode(prerender: false))```, we disable prerendering for this specific page. This means that the page will be rendered on the server and sent to the client without any prerendering.

##### **Enable per Page/Component Rendering with StreamRendering**
StreamRendering attribute in Blazor, is available in Blazor with .NET 8 and allows us to optimize the rendering process by rendering components or pages individually instead of rendering the entire application.

<kbd>Weather.razor</kbd>
```
@page "/weather"
...
@attribute [StreamRendering]
...
```
Enabling ```per page/component rendering``` using the ```StreamRendering``` attribute in Blazor with .NET 8 can greatly improve the performance of your application. By rendering only the necessary components or pages when there is a state change, you can optimize the rendering process and provide a smoother user experience.

##### **Enable AUTO Rendering**
Blazor .NET 8 also provides an AUTO rendering option, which automatically determines the appropriate rendering mode based on the client's capabilities. This allows the application to adapt to different scenarios, providing the best possible experience for each user. Automatic (Auto) rendering determines how to render the component at runtime.

<kbd>Autorenderpage.razor</kbd>
```
@page "/autorenderpage"
...
@rendermode InteractiveAuto
...			
```
In the code snippet above, we define a Blazor page with the ```@page``` directive, specifying the URL route for the page. The ```@rendermode``` directive is used to set the render mode to ```InteractiveAuto```, enabling auto rendering for the page.

##### **Rendering by the Path**

<kbd>App.razor</kbd>
```
...
 <HeadOutlet @rendermode="@RenderModeForPage" />
...
<Routes @rendermode="@RenderModeForPage" />
...
@code {
	[CascadingParameter]
	private HttpContext HttpContext { get; set; } = default!;

	private static IComponentRenderMode InteractiveServerWithoutPrerendering = new InteractiveServerRenderMode(prerender: false);

	// change render mode according to Path
	private IComponentRenderMode? RenderModeForPage => HttpContext.Request.Path.StartsWithSegments("/Account") ? null : InteractiveServer;
}
```
the ```HeadOutlet``` component and the Routes component are rendered based on the ```RenderModeForPage``` property. The ```RenderModeForPage``` property is determined by the current path of the HTTP request. If the path starts with "/Account", the render mode is set to ```null```, which means the ```default``` render mode will be used.

{{< alert type="info" >}}
ℹ️ Information
- The default render mode is ```Static```.
- The default prerender mode is ```True```.
- The default StreamRendering attribute parameter is ```True```.
- Can not switch to a different interactive render mode in a child component.
{{< /alert >}}



#### **Source**
https://learn.microsoft.com/en-us/aspnet/core/blazor/components/render-modes?view=aspnetcore-8.0
