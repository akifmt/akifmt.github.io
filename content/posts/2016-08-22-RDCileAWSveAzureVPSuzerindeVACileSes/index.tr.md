---
title: "Remote Desktop ile Amazon AWS ve Azure üzerinde Uygulama Sesleri"
date: 2016-08-22T00:00:00+00:00
hero: blog3_vac_teaser.png
description: RDC Bağlantıda AWS ve Azure VPS üzerinde VAC ile Ses
menu:
  sidebar:
    name: Remote Desktop ile Amazon AWS ve Azure üzerinde Uygulama Sesleri
    identifier: remote-desktop-amazon-aws-azure
    weight: -20160822
tags: [remote desktop, amazon aws, microsoft azure]
categories: [remote desktop, amazon aws, microsoft azure]

author:
  name: Akif T.
---

<img src="blog3_vac.png" style="width: 200px;"/><br>
AWS veya Azure üzerindeki VPS cihazlarda gerçek bir ses kartı bulunmadığı için bu cihazlar üzerinde ses almak mümküm olmamakta. Bunun için çözüm ise VA, yani virtual audio. Uygun uygulama çözümümüz VAC kurulumu ile çözülmekte. Uygulama sanal bir ses sürücüsü oluşturarak sesi cihazımıza yönlendirme sağlamaktadır.<br>
**Problem:** VPS üzerinden Ses <br>
**Çözüm:** Virtual Audio Cable 4.15 kurulumu ve Ayarlama<br>
- VPS üzerinde test ettiğim "Virtual Audio Cable 4.15" kurulumunu yapınız.<br>
- VPS üzerinde "tsconfig.msc" içerisinde RDP-Tcp -> client services -> audio <br>
seçeneğini kaldırın. VPS resetleyin.<br>
Bu aşamadan sonra ses hazır.<br>
![vac](blog3_vac.png)<br>
**İndirme linki:**<br>
Google -> Virtual Audio Cable 4.15




