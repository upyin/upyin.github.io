---
layout: post
title: word-wrap和word-break的区别
description: css中的word-wrap和word-break的区别和使用
tag: word-wrap、word-break
category: 技术、总结
---
项目中可能经常会出现文本内容超出文本框的情况，我们需要使用css中的换行属性来使文本长度过长时自动换行。经过网上的查找，我们得到了两个设置换行的css属性——word-wrap和word-break。这两个有什么区别呢？

## word-wrap

来看看wrod-wrap的属性值有哪些：

```css
/* 关键字 */
word-wrap: normal;
word-wrap: break-word;

/* 全局值 */
word-wrap: inherit;
word-wrap: initial;
word-wrap: unset;
```

## word-break

来看看word-break的属性值有哪些：

```css
/* 关键字 */
word-break: normal;
word-break: break-all; /* 允许任意非(Chinese/Japanese/Korean)文本间的单词断行 */
word-break: keep-all; /* 不允许(Chinese/Japanese/Korean)文本中的单次换行，智能在半角空格或连字符处换行。非(Chinese/Japanese/Korean)文本的行为实际上和normal一致 */

/* 全局值 */
word-break: inherit;
word-break: initial;
word-break: unset;
```

## 区别

word-break:break-all会对一行中的任意字符进行强制断行。即使是英文单词，在一行的末尾也会被强制断行。

word-wrap:break-word会根据一行中是否有可以换行的点，如空格、Chinese/Japanese/Korean之类的，则在这些换行点进行换行，而不会对英文单词强制换行。

