---
layout: post
title: CSS页面渲染优化之will-change
description: css3的will-change的介绍及使用
tag: will-change
category: 技术、总结
---
当我们通过某些行为(点击、移动或滚动)触发页面进行大面积绘制的时候，浏览器往往是没有准备的，只能被动使用CPU去计算与重绘，由于没有事先准备，应付渲染够呛，于是掉帧卡顿。而CSS属性will-change为web开发者提供了一种告知浏览器该元素会有哪些变化的方法，这样浏览器可以在元素属性真正发生变化之前提前做好对应的优化准备工作。 这种优化可以将一部分复杂的计算工作提前准备好，使页面的反应更为快速灵敏。

## 语法

```css
will-change: auto;
will-change: scroll-position;
will-change: contents;
will-change: transform;        // Example of <custom-ident> 
will-change: opacity;          // Example of <custom-ident>
will-change: left, top;        // Example of two <animateable-feature>
will-change: unset;
will-change: initial;
will-change: inherit;
```

**auto**：表示没有特别指定哪些属性会变化，浏览器需要自己去猜，然后使用浏览器经常使用的一些常规方法优化。

**scroll-position**：表示开发者希望在不久后改变滚动条的位置或者使之产生动画。

**contents**：表示开发者希望在不久后改变元素内容中的某些东西，或者使它们产生动画。

**<custom-ident>**：表示开发者希望在不久后改变指定的属性名或者使之产生动画。如果属性名是简写，则代表所有与之对应的简写或者全写的属性。

## 注意

1、**不要将 will-change 应用到太多元素上**：浏览器已经尽力尝试去优化一切可以优化的东西了。有一些更强力的优化，如果与 will-change 结合在一起的话，有可能会消耗很多机器资源，如果过度使用的话，可能导致页面响应缓慢或者消耗非常多的资源。

2、**有节制地使用：**通常，当元素恢复到初始状态时，浏览器会丢弃掉之前做的优化工作。但是如果直接在样式表中显式声明了 will-change 属性，则表示目标元素可能会经常变化，浏览器会将优化工作保存得比之前更久。所以最佳实践是当元素变化之前和之后通过脚本来切换 will-change 的值。

3、**不要过早应用 will-change 优化：**如果你的页面在性能方面没什么问题，则不要添加 will-change 属性来榨取一丁点的速度。 will-change 的设计初衷是作为最后的优化手段，用来尝试解决现有的性能问题。它不应该被用来预防性能问题。过度使用 will-change 会导致大量的内存占用，并会导致更复杂的渲染过程，因为浏览器会试图准备可能存在的变化过程。这会导致更严重的性能问题。

4、**给它足够的工作时间：**这个属性是用来让页面开发者告知浏览器哪些属性可能会变化的。然后浏览器可以选择在变化发生前提前去做一些优化工作。所以给浏览器一点时间去真正做这些优化工作是非常重要的。使用时需要尝试去找到一些方法提前一定时间获知元素可能发生的变化，然后为它加上 will-change 属性。

