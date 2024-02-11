---
title: "Self-hosted CI/CD Aracı Teamcity Kurulumu"
date: 2024-02-11T00:00:00+00:00
hero: teamcity.jpg
description: Self-hosted CI/CD Aracı Teamcity Kurulumu
menu:
  sidebar:
    name: Self-hosted CI/CD Aracı Teamcity Kurulumu
    identifier: setting-up-teamcity-hassle-free-ci/cd-tool-on-self-hosted-server
    weight: -20240211
tags: [CI/CD, Teamcity, Self-hosted]
categories: [CI/CD, Teamcity, Self-hosted]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="teamcity.jpg" alt="teamcity" title="teamcity" style="border-radius: 20px;"><br>
<p>

#### **Self-hosted CI/CD Aracı Teamcity Kurulumu**

JetBrains tarafından geliştirilen ```TeamCity```, yazılım geliştirme sürecini otomatikleştirmek ve kolaylaştırmak için tasarlanmış bir sürekli entegrasyon ve sürekli dağıtım ```(CI/CD)``` platformudur. Gelişim ekiplerinin yazılım uygulamalarını verimli bir şekilde ```derlemelerine```, ```test``` etmelerine ve ```dağıtmalarına``` yardımcı olur, ```geliştirme döngülerini``` hızlandırır ve ```yüksek kaliteli kodu``` daha hızlı teslim eder.

<kbd>Adım 1</kbd> <b>TeamCity'yi İndirin</b>
- JetBrains web sitesine gidin: https://www.jetbrains.com/teamcity/download/
- ```"Tomcat ve Java ile Paketlenmiş Windows Yükleyici"``` seçeneğini seçin.
- İstediğiniz ```sürümü``` seçin ve ```"İndir"``` düğmesine tıklayın.

{{< alert type="info" >}}
ℹ️ Bilgi
- TeamCity Professional (Sonsuza kadar ücretsiz, ticari kullanım dahil)
{{< /alert >}}


<kbd>Adım 2</kbd> <b>Kurulum</b>
- İndirilen ```.exe``` dosyasını çalıştırın.
- ```TeamCity``` Kurulum Yardımcısı talimatlarını izleyin:
- Kurulum için ```TeamCity Ana Dizinini``` seçin.
- ```İsteğe bağlı olarak``` varsayılan ```Sunucu Portunu (8111)``` ve ```Ajan Portunu (9090)``` değiştirin.
- ```"Install TeamCity server and one build agent"``` seçeneğini seçin.
- Hizmet için bir ```kullanıcı hesabı``` seçin (ilgili izinlerle).
- Ayarları inceleyin ve ```doğrulayın```.


<kbd>Adım 3</kbd> <b>TeamCity'yi Başlatın</b>
- Kurulumdan sonra sihirbaz otomatik olarak ```TeamCity```'yi başlatacaktır.
- Başlamazsa, bir komut istemi açın ve ```TeamCity_Home\bin``` dizinine gidin.
- ```runAll.bat start``` komutunu çalıştırın.


{{< alert type="info" >}}
ℹ️ Bilgi

TeamCity Sunucu Başlangıç Özelliklerini Yapılandırma;
- ```TeamCity_Home\conf``` dizinine gidin
- ```teamcity-startup.properties``` dosyasını açın

```
teamcity.data.path=TEAMCITY_DATA_PATH
teamcity.server.git.executable.path=CUSTOM_GIT_PATH\\git.exe
```

Daha fazla bilgi: https://www.jetbrains.com/help/teamcity/teamcity-documentation.html

{{< /alert >}}
