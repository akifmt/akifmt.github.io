---
title: "Google Cloud Ubuntu Üzerinde VNC Server"
date: 2017-09-21T00:00:00+00:00
hero: blog15_GoogleCloudUbuntuVNC.jpg
description: Google Cloud Ubuntu VNC Server
menu:
  sidebar:
    name: Google Cloud Ubuntu Üzerinde VNC Server
    identifier: google-cloud-ubuntu-vnc-server
    weight: -20170921
tags: [Google, Cloud, Ubuntu, 16.04, VNC, Server]
categories: [Google, Cloud, Ubuntu, 16.04, VNC, Server]

author:
  name: Akif T.
---

![vnc](blog15_GoogleCloudUbuntuVNC.jpg "vnc")<br>

Google Cloud Ubuntu 16.04 üzerinde VNC Server;

```
# Güncellemeler:
sudo apt-get update
sudo apt-get upgrade

# Kurulum:
sudo apt install xfce4 xfce4-goodies tightvncserver

vncserver

vncserver -kill :1

mv ~/.vnc/xstartup ~/.vnc/xstartup.bak

nano ~/.vnc/xstartup
xstartup içeriği:
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &

sudo chmod +x ~/.vnc/xstartup

vncserver

```

Windows üzerinden bağlanmak için: 
[İndirme Linki](https://www.realvnc.com/en/connect/download/viewer/ "Link")


