---
title: "Raspberry Pi Uzak Masaüstü Türkçe Klavye Problemi"
date: 2016-09-18T00:00:00+00:00
hero: /images/posts/blog4_rasPiRDC.png
description: "Raspberry Pi Uzak Masaüstü Türkçe Klavye Problemi"
tags: [Raspberry Pi, Remote Desktop Connection]
menu:
  sidebar:
    name: "Raspberry Pi Uzak Masaüstü Türkçe Klavye Problemi"
    identifier: raspberry-pi-uzak-masaustu
    weight: -20160918
author:
  name: Akif T.
  image: /images/authors/akif.png
---

<img src="/images/posts/blog3_vac.png" style="width: 200px;"/><br>
AWS veya Azure üzerindeki VPS cihazlarda gerçek bir ses kartı bulunmadığı için bu cihazlar üzerinde ses almak mümküm olmamakta. Bunun için çözüm ise VA, yani virtual audio. Uygun uygulama çözümümüz VAC kurulumu ile çözülmekte. Uygulama sanal bir ses sürücüsü oluşturarak sesi cihazımıza yönlendirme sağlamaktadır.<br>
**Problem:** VPS üzerinden Ses <br>
**Çözüm:** Virtual Audio Cable 4.15 kurulumu ve Ayarlama<br>
- VPS üzerinde test ettiğim "Virtual Audio Cable 4.15" kurulumunu yapınız.<br>
- VPS üzerinde "tsconfig.msc" içerisinde RDP-Tcp -> client services -> audio <br>
seçeneğini kaldırın. VPS resetleyin.<br>
Bu aşamadan sonra ses hazır.<br>
![vac](/images/posts/blog3_vac.png){:width='200'}<br>
**İndirme linki:**<br>
Google -> Virtual Audio Cable 4.15




