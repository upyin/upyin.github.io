---
layout: post
title: JS获取可视区宽高
description: 获取可视区的宽高、网页的宽高、分辨率的宽高
tag: clientWidth、screen.width、availWidth
category: 技巧、问题
---
经常会遇到需要获取可视区的宽高、网页的宽高、分辨率的宽高，涉及到很多的方法，让人混淆。有必要做个简单的整理，来区分各个方法的作用。

```bash
网页可见区域宽： document.body.clientWidth
网页可见区域高： document.body.clientHeight
网页可见区域宽： document.body.offsetWidth (包括边线的宽)
网页可见区域高： document.body.offsetHeight (包括边线的高)
网页正文全文宽： document.body.scrollWidth
网页正文全文高： document.body.scrollHeight
网页被卷去的高： document.body.scrollTop
网页被卷去的左： document.body.scrollLeft
网页正文部分上： window.screenTop
网页正文部分左： window.screenLeft
屏幕分辨率的高： window.screen.height
屏幕分辨率的宽： window.screen.width
屏幕可用工作区高度： window.screen.availHeight
屏幕可用工作区宽度： window.screen.availWidth
```

