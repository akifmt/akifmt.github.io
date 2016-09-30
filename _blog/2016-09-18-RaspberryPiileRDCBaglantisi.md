---
title:  "Raspberry Pi Uzak Masaüstü Türkçe Klavye Problemi"
excerpt: "Raspberry Pi Uzak Masaüstü Türkçe Klavye Problemi"
header:
  teaser: "blogimages/blog4_rasPiRDC.png"
categories: 
  - blog
tags:
  - Raspberry Pi
  - Remote Desktop Connection
---

![vac](/images/blogimages/blog4_rasPiRDC.png "RasPi")<br>
<br>
**Problem:** Raspbery Pi Uzak Bağlantı TR Klavye Tanımama <br>
Çözüm: km-041f.ini dosya eksikliği<br>
<br>
- km-041f.ini dosyayı Raspberry Pi üzerinde /etc/xrdp/ dizinine kopyalayın. xrdp yeniden başlatıldığında TR karakterler artık problem olmaz.<br>
Unutulmaması gereken kısım dizine root olarak erişmelisiniz.<br>
Benim izlediğim adım; <br>
1. RasPi üzerinde sudo pcmanfm <br>
2. km-041f.ini dosya /etc/xrdp/ buraya kopyalanır.<br>
3. sudo service xrdp restart<br>
**Not:** Dosyayı nette bulmakta problem yaşayanlar için, ben bu repodan indirdim: [Link](https://github.com/Sighillrob/ulteo4Kode4kids/tree/master/xrdp/instfiles "asd")









