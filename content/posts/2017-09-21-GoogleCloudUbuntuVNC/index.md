---
title: "VNC Server on Google Cloud Ubuntu"
date: 2017-09-21T00:00:00+00:00
hero: blog15_GoogleCloudUbuntuVNC.jpg
description: Google Cloud Ubuntu VNC Server
menu:
   sidebar:
     name: VNC Server on Google Cloud Ubuntu
     identifier: google-cloud-ubuntu-vnc-server
     weight: -20170921
tags: [Google, Cloud, Ubuntu, 16.04, VNC, Server]
categories: [Google, Cloud, Ubuntu, 16.04, VNC, Server]

author:
  name: Akif T.
---

![vnc](blog15_GoogleCloudUbuntuVNC.jpg "vnc")<br>

VNC Server on Google Cloud Ubuntu 16.04;

```
# Updates:
sudo apt-get update
sudo apt-get upgrade

# Setup:
sudo apt install xfce4 xfce4-goodies tightvncserver

vncserver

vncserver -kill :1

mv ~/.vnc/xstartup ~/.vnc/xstartup.bak

nano ~/.vnc/xstartup
xstartup content:
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &

sudo chmod +x ~/.vnc/xstartup

vncserver

```

To connect via Windows:
[Download link](https://www.realvnc.com/en/connect/download/viewer/ "Link")


