---
title: "VMware and Hyper-V Combine"
date: 2017-02-22T00:00:00+00:00
hero: blog9_vmwarehyperv.png
description: VMware and Hyper-V Combine
menu:
  sidebar:
    name: VMware and Hyper-V Combine
    identifier: vmware-ve-hyperv
    weight: -20170222
tags: [vmware hyper v]
categories: [vmware hyper v]

author:
  name: Akif T.
---

![vmware](blog9_vmwarehyperv.png "vmware")<br>

**To turn off Hyper-V;** <br>
As administrator on cmd: <br>
```
bcdedit /set hypervisorlaunchtype off
```
*vmware available after reboot.*

**To turn Hyper-V back on** <br>
As administrator on cmd: <br>
```
bcdedit /set hypervisorlaunchtype auto
```
*Hyper-V is available upon reboot.*
