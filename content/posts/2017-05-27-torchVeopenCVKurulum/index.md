---
title: "Torch, OpenCV Installation and Test"
date: 2017-05-27T00:00:00+00:00
hero: blog14_torchVeopenCVKurulum.png
description: Torch, OpenCV Installation and Test
menu:
  sidebar:
    name: Torch, OpenCV Installation and Testing
    identifier: torch-opencv-setup
    weight: -20170527
tags: [Torch, OpenCV Installation and Testing]
categories: [Torch, OpenCV Installation and Testing]

author:
  name: Akif T.
---


![torch](blog14_torchVeopenCVKurulum.png "torch")<br>

OpenCV and Torch installation on Ubuntu 16.04;

```
# Updates:
sudo apt-get update
sudo apt-get upgrade
shutdown -r 0

sudo apt-get install git 
#Torch Installation
#Run in terminal in order:
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh

source ~/.bashrc
source ~/.profile

#OpenCV Installation:

sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

2015-12-21 VERSION 3.1 OpenCV for Linux/Mac
cd ~
wget https://github.com/Itseez/opencv/archive/3.1.0.zip --no-check-certificate
unzip 3.1.0.zip
cd opencv-3.1.0
mkdir build
cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_GTK=ON -D WITH_OPENGL=ON ..

make
sudo make install

While in the openCV build folder;
source ~/.profile

luarocks install cv
luarocks install camera
luarocks install ffmpeg

/// If a luarocks package installation error occurs, run the following line to fix it and install the packages again:
/// sudo rm -rf ~/.cache/luarocks
```

IDE: 
[Download link](https://eclipse.org/ldt/ "Link")

Kurulumu Test etmek için:
[Download link](/files/TEST_LUA_Torch_OpenCV.zip "Link") , 
[Github link](https://github.com/akifmt/LuaOpenCVTorch "Link")

