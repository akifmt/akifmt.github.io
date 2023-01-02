---
title: "Combining NodeJS Versions"
date: 2019-02-10T00:00:00+00:00
hero: blog16_NodeJSFarkliSurumleriBiraradaKullanma.png
description: Mixing Different Versions of NodeJS
menu:
   sidebar:
     name: Mixing Different Versions of NodeJS
     identifier: nodejs-different-versions
     weight: -20190210
tags: [NodeJS, Different, Versions, Together, Using]
categories: [NodeJS, Different, Versions, Together, Using]

author:
  name: Akif T.
---

![nodejs](blog16_NodeJSFarkliSurumleriBiraradaKullanma.png "vnc")<br>

Combining NodeJS Versions;

```
# Setup:
npm install -g nvmw # nvmw install
nvmw install v8.12.0 # installation of the versions to be used
nvmw use v8.12.0 # Use the specific version

# Use:
nvmw help # help
nvmw install [version] # Version install [version]
nvmw uninstall [version] # Uninstall version [version]
nvmw use [version] # Change version [version]
nvmw ls # List of installed versions
```

NodeJS Releases List: [Versions](https://nodejs.org/en/download/releases/ "Releases")

NPM: [NPM Link](https://www.npmjs.com/package/nvmw "NPM Link")

Github: [Github Link](https://github.com/hakobera/nvmw "Github Link")
