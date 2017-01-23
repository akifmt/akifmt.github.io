---
layout: single-tr
title:  "Remote Desktop ile Amazon AWS ve Azure üzerinde Uygulama Sesleri"
excerpt: "RDC Bağlantıda AWS ve Azure VPS üzerinde VAC ile Ses"
header:
  teaser: "blogimages/blog3_vac_teaser.png"
categories: 
  - blog-tr
tags:
  - amazon aws
  - microsoft azure
---

<img src="/images/blogimages/blog3_vac.png" style="width: 200px;"/><br>
AWS veya Azure üzerindeki VPS cihazlarda gerçek bir ses kartı bulunmadığı için bu cihazlar üzerinde ses almak mümküm olmamakta. Bunun için çözüm ise VA, yani virtual audio. Uygun uygulama çözümümüz VAC kurulumu ile çözülmekte. Uygulama sanal bir ses sürücüsü oluşturarak sesi cihazımıza yönlendirme sağlamaktadır.<br>
**Problem:** VPS üzerinden Ses <br>
**Çözüm:** Virtual Audio Cable 4.15 kurulumu ve Ayarlama<br>
- VPS üzerinde test ettiğim "Virtual Audio Cable 4.15" kurulumunu yapınız.<br>
- VPS üzerinde "tsconfig.msc" içerisinde RDP-Tcp -> client services -> audio <br>
seçeneğini kaldırın. VPS resetleyin.<br>
Bu aşamadan sonra ses hazır.<br>
![vac](/images/blogimages/blog3_vac.png){:width='200'}<br>
**İndirme linki:**<br>
Google -> Virtual Audio Cable 4.15




