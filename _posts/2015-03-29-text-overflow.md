---
layout: post
title: CSS实现多行省略号效果
description: 使用css实现一行省略号，多行省略号效果
tag: text-overflow、ellipsis、line-clamp
category: 技巧、问题
---
我们经常会在工作中遇到需要显示摘要的地方，可能是一行的摘要，也可能是多行摘要，然后超出的文字使用省略号来代替。

如下效果：

```bash
单行文本超出部分显示省略号……
```

```bash
多行文本超出显示省略号，
这里是第二行文本呀，哈
这里是第三行超出显示……
```

要想实现上面的效果，可以使用css的样式来进行设置。

单行省略号：

```css
overflow:hidden;
text-overflow:ellipsis;
white-space:nowrap;
```

多行省略号：

```css
display:-webkit-box;
-webkit-box-orient:vertical;
-webkit-line-clamp:3;
overflow:hidden;
```

