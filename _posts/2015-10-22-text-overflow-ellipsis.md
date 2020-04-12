---
layout: post
title: 多行截取省略号的实现
description: css中实现单行截取省略号，多行截取省略号
tag: white-space、ellipsis、line-clamp
category: 技巧、问题
---
经常会遇到文章开头需要显示3行摘要的需求，但是摘要的长度又是不可控的，那我们就需要对文本进行截取，超过3行显示省略号。下面就讲讲如何实现这样的效果吧。

从单行截取省略号讲起……

```css
{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

单行的截取省略号是我们最常见和最常使用的。实现的思路：使文本不换行，容器内的内容超出部分隐藏，文本超出时显示成省略号。

多行的截取省略号就要略微复杂一些了，需要使用到box来实现。

```css
{
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3; /* 显示3行 */
  overflow: hidden;
  text-overflow: ellipsis;
}
```

上面的css中使用到了line-clamp，但是这个属性是不规范的（为收录到css规范中），所以需要添加-webkit-前缀来使用，且只能使用在webkit内核的浏览器中。