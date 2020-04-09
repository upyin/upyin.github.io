---
layout: post
title: jQuery获取input的checked属性的方法(attr、prop)对比
description: 使用jquery的attr、prop方法来获取input的checked属性问题
tag: attr、prop、checked
category: 技巧、问题
---
经常使用jQuery的attr方法获取checked属性值，获取的值的大小为未定义，此时可以用prop方法获取其真实值，下面介绍这两种方法的区别。

1.通过prop方法获取checked属性，获取的checked返回值未boolean，选中为true，未选中为false。

```javascript
$("#selectId").prop("checked");
$("#selectId").prop("checked", true);
```

2.通过attr方法获取checked属性，如果input中的checked属性未定义，则不管当前是否选中，都会返回undefined；如果当前input中初始化已定义checked属性，则不管是否选中，都会返回checked。

```javascript
$("#selectId").attr("checked"); // 未定义时返回undefined，已定义时返回checked
$("#selectId").attr("checked","checked");
```

总结，因此建议使用prop方法来获取和设置input的checked属性。