---
title: "Vmware ve Hyper-V Birlikte Kullan(mak)"
date: 2017-02-22T00:00:00+00:00
hero: /images/posts/blog9_vmwarehyperv.png
description: "Vmware ve Hyper-V Birlikte Kullan(mak)"
tags: [vmware hyper v]
menu:
  sidebar:
    name: "Vmware ve Hyper-V Birlikte Kullan(mak)"
    identifier: vmware-ve-hyperv
    weight: -20170222
author:
  name: Akif T.
  image: /images/authors/akif.png
---

![vmware](/images/posts/blog9_vmwarehyperv.png "vmware")<br>

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
