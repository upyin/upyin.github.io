---
layout: post
title: HTML中的data属性的使用技巧
description: HTML中data属性的使用技巧
tag: data属性
category: 技巧、问题
---
我们经常会在html上绑定一些数据，用于后续的处理操作。这次在工作中使用data属性的时候，遇到了一些奇特的现象，所以做个简单的总结。

### data-n

data属性后面的单词必须为小写，如果是大写，则也将转换为小写。

```markup
<div class="example" data-example="test" data-index="0"></div>
<div class="example2" data-index="2"></div>
```

data属性的值为字符串类型的。但是在使用jquery的data()方法获取值的时候，如果值为0则返回字符串类型，如果为其他整数则返回数值类型。

```javascript
document.getElementsByClassName("example")[0].getAttribute("data-index"); // "0"
$(".example").data("index"); // "0"

document.getElementsByClassName("example2")[0].getAttribute("data-index"); // "2"
$(".example2").data("index"); // 2
```

