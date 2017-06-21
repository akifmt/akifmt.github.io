---
layout: single-tr
title:  "Torch, OpenCV Kurulum ve Test"
excerpt: "Torch, OpenCV Kurulum ve Test"
header:
  teaser: "blogimages/blog14_torchVeopenCVKurulum.png"
categories:
  - blog-tr
tags:
  - Torch, OpenCV Kurulum ve Test
---

![epub](/images/blogimages/blog14_torchVeopenCVKurulum.png "torch")<br>

Ubuntu 16.04 üzerinde OpenCV ve Torch kurulumu;

```
# Güncellemeler:
sudo apt-get update
sudo apt-get upgrade
shutdown -r 0

sudo apt-get install git 
#Torch Kurulumu
#Terminal de sırayla calistir:
git clone https://github.com/torch/distro.git ~/torch --recursive
cd ~/torch; bash install-deps;
./install.sh

source ~/.bashrc
source ~/.profile

#OpenCV Kurulumu:

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

openCV build klasoru içinde iken;
source ~/.profile

luarocks install cv
luarocks install camera
luarocks install ffmpeg

/// Eger luarocks paket kurulum hatası oluşursa düzeltmek için aşağıdaki satırı çalıştırıp tekrar paketleri kurun:
/// sudo rm -rf ~/.cache/luarocks
```

IDE: 
[İndirme Linki](https://eclipse.org/ldt/ "Link")

Kurulumu Test etmek için:
[İndirme Linki](/files/TEST_LUA_Torch_OpenCV.zip "Link") , 
[Github Linki](https://github.com/akifmt/LuaOpenCVTorch "Link")




