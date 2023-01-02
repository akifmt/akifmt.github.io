---
title: "EPub Cover Düzenle(mek)"
date: 2017-05-02T00:00:00+00:00
hero: blog13_epub.png
description: EPub cover ve içerik düzenle(mek)
menu:
  sidebar:
    name: EPub Cover Düzenle(mek)
    identifier: epub-cover
    weight: -20170502
tags: [epub calibre]
categories: [epub calibre]

author:
  name: Akif T.
---

![epub](blog13_epub.png "epub")<br>

Bazı e-kitapların cover sayfalarında uyumsuzluktan dolayı diğer cihazlarda görünmemektedir. Bunun için çözüm calibre. Sadece cover sayfası için değil içerikte düzenleme imkanı da sağlamaktadır. <br>

[İndirme Linki](http://calibre-ebook.com/download "Link")

Cover sayfası düzelemek için;
"Edit Metadata" seçeneği ile aşağıdaki satırı düzenlemek veya yoksa, eklemek yeterli olacaktır.

```
<item id="cover_image" href="cover.jpg" media-type="image/jpeg">
```
