---
layout: single-tr
title:  "Vmware ve Hyper-V Birlikte Kullan(mak)"
excerpt: "Vmware ve Hyper-V Birlikte Kullan(mak)"
header:
  teaser: "blogimages/blog9_vmwarehyperv.png"
categories: 
  - blog-tr
tags:
  - vmware hyper v
---

![vmware](/images/blogimages/blog9_vmwarehyperv.png "vmware")<br>

**Hyper-V kapatmak için;** <br>
Cmd üzerinde yönetici olarak: <br>
```
bcdedit /set hypervisorlaunchtype off
```
*Yeniden başlatıldığında vmware kullanılabilir.*

**Hyper-V tekrar açmak için** <br>
Cmd üzerinde yönetici olarak: <br>
```
bcdedit /set hypervisorlaunchtype auto
```
*Yeniden başlatıldığında Hyper-V kullanılabilir.*
