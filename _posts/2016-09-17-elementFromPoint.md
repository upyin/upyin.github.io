---
layout: post
title: 使用elementFromPoint实现通讯录锚点功能
description: 使用elementFromPoint方法，实现通讯录的锚点功能
tag: elementFromPoint
category: 技术、总结
---
工作中经常会遇到需要实现地区列表选择、通讯录选择的需求，类似于下图。当在右侧的首字母条上下滑动时，能够快速的定位到具体的城市或人名。要实现该效果，我们可以使用js中的documet.elementFromPoint方法配合event.touches[0]方法实现。加下来我们重点讲述document.elementFromPoint方法的使用语法。

![](/images/20160917elementfrompoint/contact.png)

## elementFromPoint

### 概述

返回当前文档上处于指定坐标位置最顶层的元素，坐标是相对于包含该文档的浏览器窗口的左上角为原点来计算的，通常x和y坐标都应该为正数。

### 语法

```javascript
var element = document.elementFromPoint(x, y);
```

element是返回的DOM元素。

x和y是坐标数值，整数，不需要单位比如px。

### 注意

如果指定点处的元素属于另一个文档（例如，iframe的子文档），则返回调用该方法的文档的DOM中的元素（在iframe情况下，返回iframe本身）。如果给定点上的元素是匿名的或XBL生成的内容（如文本框的滚动条），则返回第一个非匿名祖先元素（如文本框）。

如果指定的点在文档的可见边界之外，或者坐标为负，则结果为空。

