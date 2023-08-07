---
title: "Blazor Using NPM Packages"
date: 2023-08-07T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Using NPM Packages
menu:
  dotnet:
    name: Blazor Using NPM Packages
    identifier: blazor-using-npm-packages
    weight: -20230807
tags: [ Dotnet, Blazor Using NPM Packages]
categories: [ Dotnet, Blazor Using NPM Packages]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

#### **Blazor Using NPM Packages**

- Create a new folder named ```npm_packages``` in your Blazor project.

- Open the npm_packages folder directory via command prompt and run the following command to initialize NPM in the application:
```
npm init -y
```
This will create a new ```package.json``` file in the ```npm_packages``` directory.

- Install the ```webpack and webpack-cli``` packages as development dependencies by running the following command:
```
npm install webpack webpack-cli --save-dev
```

- Modify the scripts section of the ```package.json``` file to add the following build script:
```
"scripts": {
    "build": "webpack ./src/index.js --output-path ../wwwroot/js --output-filename index.bundle.js"
  },
```
This build script will use webpack to bundle the JavaScript files in the src folder into a single file called ```index.bundle.js``` in the ```wwwroot/js``` folder.

- Install the NPM package that you want to use in your Blazor application. For example, to install the ```chart.js``` package, run the following command:
```
npm i chart.js
```

- Build your Blazor application by running the following command:
```
npm run build
```
This will run the webpack build script that you defined in the ```package.json``` file.

- Modify the ```csproj``` file to add a pre-build step that will run the npm build script. This will ensure that the NPM packages are installed and bundled before your Blazor application is built. To do this, add the following code to the PreBuild target in the csproj file:
```
<Target Name="PreBuild" BeforeTargets="PreBuildEvent">
    <Exec Command="npm install" WorkingDirectory="npm_packages" />
    <Exec Command="npm run build" WorkingDirectory="npm_packages" />
  </Target>
```

- Edit the ```_Layout.cshtml``` file and add the following code to the head section:
```
<script src="~/js/index.bundle.js"></script>
```
This will load the ```index.bundle.js``` file that was created by the webpack build script.

- Run your Blazor application and the NPM package that you installed should be available to use in your Blazor code.


##### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppNPMPackages