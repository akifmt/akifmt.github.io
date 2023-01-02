---
title: "Application Sounds on Amazon AWS and Azure with Remote Desktop"
date: 2016-08-22T00:00:00+00:00
hero: blog3_vac_teaser.png
description: Voice with VAC on AWS and Azure VPS on RDC Connect
menu:
   sidebar:
    name: Application Voices on Amazon AWS and Azure with Remote Desktop
    identifier: remote-desktop-amazon-aws-azure
    weight: -20160822
tags: [remote desktop, amazon aws, microsoft azure]
categories: [remote desktop, amazon aws, microsoft azure]

author:
  name: Akif T.
---

<img src="blog3_vac.png" style="width: 200px;"/><br>
Since VPS devices on AWS or Azure do not have a real sound card, it is not possible to receive sound on these devices. The solution for this is VA, that is, virtual audio. Our convenient application solution is solved with VAC installation. The application creates a virtual sound driver and directs the sound to our device.<br>
**Problem:** Voice over VPS <br>
**Solution:** Virtual Audio Cable 4.15 installation and Setup<br>
- Install "Virtual Audio Cable 4.15" that I tested on VPS.<br>
- RDP-Tcp -> client services -> audio <br> in "tsconfig.msc" on VPS
remove the option. Reset VPS.<br>
After this step, the sound is ready.<br>
![vac](blog3_vac.png)<br>
**Download link:**<br>
Google -> Virtual Audio Cable 4.15




