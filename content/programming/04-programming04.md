---
title: "Association, Aggregation, Composition"
date: 2016-01-01T00:00:00+00:00
hero: /images/posts/default-hero.jpg
description: "Association, Aggregation, Composition"
tags: [encapsulation]
menu:
  sidebar:
    name: "Association, Aggregation, Composition"
    identifier: programming04
    parent: programming00
    weight: 504
author:
  name: Akif T.
  image: /images/authors/akif.png
---

**Association:**
A relationship where all objects have their own lifecycle and there is no owner.
![pr41](/images/programmingimages/programming04_1_association.jpg "pr41")<br>

**Aggregation:**
A specialised form of Association where all objects have their own lifecycle, but there is ownership and child objects can not belong to another parent object.
![pr42](/images/programmingimages/programming04_2_aggregation.jpg "pr42")<br>

**Composition:**
A specialised form of Aggregation and we can call this as a “death” relationship. It is a strong type of Aggregation. Child object does not have its lifecycle and if parent object is deleted, all child objects will also be deleted.
![pr43](/images/programmingimages/programming04_3_composition.jpg "pr43")<br>