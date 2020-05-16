---
layout: post
title: 使用filter:hue-rotate实现酷炫动效
description: 使用filter:hue-rotate实现酷炫动效
tag: hue-rotate
category: 技术、总结
---
![](/images/20200322huerotate/huerotate.git)

实现上面的酷炫动效，使用gif？那就out了。试试CSS中的过滤器filter:hue-rotate吧。

## 简介

hue-rotate() 通过旋转一个元素和其内容的hue值，来改变其样式。它是一个filter（过滤器）方法。

## 语法

hue-rotate滤镜支持传角度deg、圈数turn、弧度rad等。

```css
filter: hue-rotate(90deg);
filter: hue-rotate(0.5turn);
filter: hue-rotate(3.142rad);
```

## 滤镜与动效

要想实现文章一开始的动效，可以使用hue-rotate结合animation。

```css
.bird {
  animation: pulse 5s linear infinite;
}
@keyframes pulse {
  from { filter: hue-rotate(0); }
  to   { filter: hue-rotate(360deg); }
}
```

## 兼容性

![](/images/20200322huerotate/compatible.png)

现代浏览器基本都支持。



> [CSS filter:hue-rotate色调旋转滤镜实现按钮批量生产](https://www.zhangxinxu.com/wordpress/2018/11/css-filter-hue-rotate-button/)

> [hue-rotate() CSS层叠样式表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter-function/hue-rotate)

