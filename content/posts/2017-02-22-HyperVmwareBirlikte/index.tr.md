---
title: "Vmware ve Hyper-V Birlikte Kullan(mak)"
date: 2017-02-22T00:00:00+00:00
hero: blog9_vmwarehyperv.png
description: Vmware ve Hyper-V Birlikte Kullan(mak)
menu:
  sidebar:
    name: Vmware ve Hyper-V Birlikte Kullan(mak)
    identifier: vmware-ve-hyperv
    weight: -20170222
tags: [vmware hyper v]
categories: [vmware hyper v]

author:
  name: Akif T.
---

![vmware](blog9_vmwarehyperv.png "vmware")<br>

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
