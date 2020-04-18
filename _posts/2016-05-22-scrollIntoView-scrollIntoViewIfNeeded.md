---
layout: post
title: scrollIntoView与scrollIntoViewIfNeeded介绍
description: 解决ios端输入框被键盘挡住的问题
tag: scrollIntoView、scrollIntoViewIfNeeded
category: 技术、总结
---
根据MDN的描述，Element.scrollIntoVew()方法让当前元素滚动到浏览器窗口的可视区域内。而Element.scrollIntoViewIfNeeded()方法也是用来将不在浏览器窗口的可见区域内的元素滚动到浏览器窗口的可见区域。但如果该元素已经在浏览器窗口的可见区域内，则不会发生滚动。

因此，再有什么需要回到顶部、去到置顶位置和键盘弹出挡住输入框之类的需求，便可以简单的解决了。

## scrollIntoView

### 语法参数

```javascript
element.scrollIntoView(); // 等同于element.scrollIntoView(true) 
element.scrollIntoView(alignToTop); // Boolean型参数 
element.scrollIntoView(scrollIntoViewOptions); // Object型参数
```

alignToTop（可选）[Boolean]
如果为true，元素的顶端将和其所在的滚动区域的顶端对齐。相应的scrollIntoViewOptions: {block: "start", inline: "nearest"}。这是这个参数的默认值。如果为false，元素的底端将和其所在滚动区的可视区域的底端对齐。相应的scrollIntoVewOptions: {block: "end", inline: "nearest"}。

scrollIntoVewOptions（可选）[Object]

behavior—可选，定义动画过度效果，"auto"或"smooth"，默认"auto"。

block—可选，定义垂直方向的对齐，"start"，"center"，"end"或"nearest"，默认为"start"。

inline—可选，定义水平方向的对齐，"start"，"center"，"end"或"nearest"，默认为"nearest"。

### 兼容性

![](/images/20160522scrollintoview/scrollIntoView.png)

## scrollIntoViewIfNeeded

### 语法参数

```javascript
element.scrollIntoViewIfNeeded(); // 等同于element.scrollIntoViewIfNeeded(true) 
element.scrollIntoViewIfNeeded(true); 
element.scrollIntoViewIfNeeded(false); 
```

opt_center[Boolean]

如果为true，则元素将在其所在滚动区的可视区域中居中对齐。如果为false，则元素将与其所在滚动区的可视区域最近的边缘对齐；根据可见区域最靠近元素的哪个边缘，元素的顶部将与可见区域的顶部边缘对准，或者元素的底部边缘将与可见区域的底部边缘对准。

### 兼容性

![](/images/20160522scrollintoview/scrollIntoViewIfNeeded.png)

