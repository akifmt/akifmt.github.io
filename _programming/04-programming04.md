---
title: "Association, Aggregation, Composition"
permalink: /programming/programming04/
excerpt: "Association, Aggregation, Composition"
modified: 2017-02-20T01:01:01-01:02
---

{% include base_path %}

**Association:**
a relationship where all objects have their own lifecycle and there is no owner.
![pr41](/images/programmingimages/programming04_1_association.jpg "pr41")<br>

**Aggregation:**
a specialised form of Association where all objects have their own lifecycle, but there is ownership and child objects can not belong to another parent object.
![pr42](/images/programmingimages/programming04_2_aggregation.jpg "pr42")<br>

**Composition:**
specialised form of Aggregation and we can call this as a “death” relationship. It is a strong type of Aggregation. Child object does not have its lifecycle and if parent object is deleted, all child objects will also be deleted.
![pr43](/images/programmingimages/programming04_3_composition.jpg "pr43")<br>