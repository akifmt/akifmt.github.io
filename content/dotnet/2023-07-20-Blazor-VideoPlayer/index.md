---
title: "Video Player in Blazor"
date: 2023-07-20T00:00:00+00:00
hero: video_player_in_blazor.png
description: Video Player in Blazor
menu:
  dotnet:
    name: Video Player in Blazor
    identifier: video-player-in-blazor
    weight: -20230720
tags: [ Dotnet, Video Player in Blazor]
categories: [ Dotnet, Video Player in Blazor]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="video_player_in_blazor.png" alt="video_player_in_blazor" title="video_player_in_blazor"><br>
<p>

##### **Video Player in Blazor**

<p style="text-align: center;">
<img src="videoplayer.PNG" alt="videoplayer" title="videoplayer"><br>
<p>

This code provides a video player component in a Blazor application. It allows users to watch videos with controls and captions. The video player is customizable using the Plyr.io library.

Blazor: Blazor is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables full-stack development with .NET.
Plyr.io: Plyr.io is a JavaScript library that provides a customizable video player with a modern UI. It supports various features like controls, captions, and responsive design.

The code is structured as a Blazor component with a Razor markup file (.razor) and a code-behind file (.razor.cs). The Razor markup file defines the UI elements and the code-behind file contains the logic for the component.

The code uses the `@page` directive to define the URL routes for the component. It injects the `IJSRuntime` service to interact with JavaScript code.

The `video` variable is used to store the selected video. If the `video` is null, a loading message is displayed. Otherwise, the video player is rendered with the Plyr.io library.

The code dynamically generates `<source>` elements for the video files and `<track>` elements for the captions. It also provides a fallback download link for browsers that don't support the `<video>` element.

The `OnInitializedAsync` method is called when the component is initialized. It retrieves the video based on the `Id` parameter from the `Data.VideosData.Videos` collection.

The `OnAfterRenderAsync` method is called after the component has been rendered. It loads a custom video player using JavaScript interop. The `LoadCustomPlayer` function is invoked from a JavaScript file located in the `./js/components/video.js` path.

The provided code demonstrates how to create a video player component in a Blazor application using the Plyr.io library. It allows users to watch videos with controls and captions. The code can be customized further to meet specific requirements and integrate additional features.

###### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppVideoPlayer