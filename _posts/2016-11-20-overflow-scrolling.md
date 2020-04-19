---
layout: post
title: 移动端滚动回弹效果实现之-webkit-overflow-scrolling
description: 使用-webkit-overflow-scrolling特性实现移动端滚动回弹的效果
tag: -webkit-overflow-scrolling
category: 技巧、问题
---
ios网页自带上下滚动回弹的效果，给用户的体验相当不错。那如果想实现左右滚动也有这样的效果？安卓端也有回弹的效果要怎么实现呢？

CSS3中提供了一个实现的方法：-webkit-overflow-scrolling。

### 概述

-webkit-overflow-scrolling特性控制元素在移动设备上是否使用滚动回弹效果。

### 属性值

auto：使用普通滚动，当手指从触摸屏上移开，滚动会立即停止。

touch：使用具有回弹效果的滚动，当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

### 兼容性

该特性是非标准的。只有-webkit-内核的浏览器支持，所以必须要携带-webkit-前缀。

### 示例

```css
-webkit-overflow-scrolling: touch; /* 当手指从触摸屏上移开，会保持一段时间的滚动 */
-webkit-overflow-scrolling: auto; /* 当手指从触摸屏上移开，滚动会立即停止 */
```

