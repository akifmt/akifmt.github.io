---
layout: single-tr
title:  "Google Cloud Ubuntu Üzerinde VNC Server"
excerpt: "Google Cloud Ubuntu VNC Server"
header:
  teaser: "blogimages/blog15_GoogleCloudUbuntuVNC.jpg"
categories:
  - blog-tr
tags:
  - Google, Cloud, Ubuntu, 16.04, VNC, Server
---

![vnc](/images/blogimages/blog15_GoogleCloudUbuntuVNC.jpg "vnc")<br>

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

