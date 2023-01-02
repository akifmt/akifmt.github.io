---
title: "Node.js Deployment and Publishing on IIS - iisnode"
date: 2019-08-08T00:00:00+00:00
hero: blog17_IISNodejs.png
description: Node.js Deployment and Publishing on IIS - iisnode
menu:
  sidebar:
    name: Node.js Deployment and Publishing on IIS - iisnode
    identifier: iis-nodejs-deployment-publish-iisnode
    weight: -20190808
tags: [IIS, Node.js, Deployment, Publish, iisnode]
categories: [IIS, Node.js, Deployment, Release, iisnode]

author:
  name: Akif T.
---

![iisnode](blog17_IISNodejs.png "iisnode")<br>

**Node.js Deployment and Publishing on IIS - iisnode**

Tested on Windows Server 2016 x64 and IIS 10.0. All the requirements for different versions are the same, only the appropriate version for iisnode should be installed.

**Requirements:**

- WebPlatformInstaller: [WebPlatformInstaller](https://www.microsoft.com/en-us/download/details.aspx?id=6164 "WebPlatformInstaller")
- IIS URL Rewrite extension: [IIS URL Rewrite extension](https://www.iis.net/downloads/microsoft/url-rewrite "IIS URL Rewrite extension")
- Node.js: [node.js](https://nodejs.org "node.js")
- iisnode v0.22.1 x64 : [iisnode v0.22.1 x64 ](https://github.com/tjanczuk/iisnode/releases/download/v0.2.21/iisnode-full-v0.2.21-x64.msi "iisnode v0.22.1 x64 ")

**After installation, the project should have "web.config".** <br />
The following config example can also be used with Express Framework. The app will be started with **server.js**. Outputs in the console can be checked from the folder specified with **logDirectory**. The **debug** sections in the "config" should be updated when going to the "production" phase.

```xml
<configuration>
  <system.webServer>
	<httpErrors existingResponse="PassThrough" />
    <handlers>
      <add name="iisnode" path="server.js" verb="*" modules="iisnode" />
    </handlers>

	<iisnode      
      node_env="%node_env%"
      nodeProcessCountPerApplication="1"
      maxConcurrentRequestsPerProcess="1024"
      maxNamedPipeConnectionRetry="100"
      namedPipeConnectionRetryDelay="250"      
      maxNamedPipeConnectionPoolSize="512"
      maxNamedPipePooledConnectionAge="30000"
      asyncCompletionThreadCount="0"
      initialRequestBufferSize="4096"
      maxRequestBufferSize="65536"
      watchedFiles="*.js;iisnode.yml"
      uncFileChangesPollingInterval="5000"      
      gracefulShutdownTimeout="60000"
      loggingEnabled="true"
      logDirectory="C:\Logs"
      debuggingEnabled="true"
      debugHeaderEnabled="false"
      debuggerPortRange="5058-6058"
      debuggerPathSegment="debug"
      maxLogFileSizeInKB="128"
      maxTotalLogFileSizeInKB="1024"
      maxLogFiles="20"
      devErrorsEnabled="true"
      flushResponse="false"      
      enableXFF="false"
      promoteServerVars=""
      configOverrides="iisnode.yml"
     />
	
    <rewrite>
      <rules>
        <rule name="rulename">
          <match url="/*" />
          <action type="Rewrite" url="server.js" />
        </rule>
      </rules>
    </rewrite>
    
  </system.webServer>
</configuration>
```