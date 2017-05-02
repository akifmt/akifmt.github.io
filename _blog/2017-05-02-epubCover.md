---
layout: single-tr
title:  "EPub Cover Düzenle(mek)"
excerpt: "EPub cover ve içerik düzenle(mek)"
header:
  teaser: "blogimages/blog13_epub.png"
categories:
  - blog-tr
tags:
  - epub calibre
---

![epub](/images/blogimages/blog13_epub.png "epub")<br>

Bazı e-kitapların cover sayfalarında uyumsuzluktan dolayı diğer cihazlarda görünmemektedir. Bunun için çözüm calibre. Sadece cover sayfası için değil içerikte düzenleme imkanı da sağlamaktadır. <br>

[İndirme Linki](http://calibre-ebook.com/download "Link")

Cover sayfası düzelemek için;
"Edit Metadata" seçeneği ile aşağıdaki satırı düzenlemek veya yoksa, eklemek yeterli olacaktır.

```
<item id="cover_image" href="cover.jpg" media-type="image/jpeg">
```




