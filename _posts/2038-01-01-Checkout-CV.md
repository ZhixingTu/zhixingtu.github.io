---
layout:     cvinpost
title:      "📄 Check Out My CV Here"
subtitle:   "📌 Sticky on Top"
date:       2038-01-01 12:00:00
author:     "Shicong Liu"
header-img: "img/home-dark.jpg"
catalog: true
mathjax: true
comment: true
tags:
    - CV
---

<div>
    {% capture CV_en %}{% include cv/inpost.html %}{% endcapture %}
    {{ CV_en | markdownify }}
</div>