---
title: "Raspberry Pi Remote Desktop Turkish Keyboard Problem"
date: 2016-09-18T00:00:00+00:00
hero: blog4_rasPiRDC.png
description: Raspberry Pi Remote Desktop Turkish Keyboard Problem
menu:
   sidebar:
    name: Raspberry Pi Remote Desktop Turkish Keyboard Problem
    identifier: raspberry-pi-remote-desktop
    weight: -20160918
tags: [Raspberry Pi, Remote Desktop Connection]
categories: [Raspberry Pi, Remote Desktop Connection]

author:
  name: Akif T.
---

![RasPi](blog4_rasPiRDC.png "RasPi")<br>
<br>
**Problem:** Raspbery Pi Remote Connection TR Keyboard Not Recognizing <br>
Solution: missing km-041f.ini file<br>
<br>
- Copy the km-041f.ini file to the /etc/xrdp/ directory on the Raspberry Pi. When xrdp is restarted, TR characters are no longer a problem.<br>
The thing to note is that you must access the directory as root.<br>
The step I followed; <br>
1. sudo pcmanfm on RasPi <br>
2. Copy the km-041f.ini file to /etc/xrdp/.<br>
3. sudo service xrdp restart<br>
**Note:** For those who have problems finding the file on the net, I downloaded it from this repo: [Link](https://github.com/Sighillrob/ulteo4Kode4kids/tree/master/xrdp/instfiles "asd")
