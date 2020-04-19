---
layout: post
title: CSS3动画效果
description: 使用CSS3动画属性实现动画效果
tag: keyframe、animation、transform
category: 技术、总结
---
随着互联网的普及流行，网页上的交互效果越加丰富多彩。之前的动画效果，大部分都是通过Flash动画和JavaScript实现的效果。但随着CSS3的推广和应用，使用CSS3中的动画属性来实现网页动画，大大增加了网页性能和展示效果。

## @keyframe

@keyframe规则是创建动画所需的，通过指定一个CSS样式和动画将逐步从目前的样式更改为新的样式。

示例：

```css
@keyframes myfirst
{
    from {background: red;}
    to {background: yellow;}
}
 
@-webkit-keyframes myfirst /* Safari 与 Chrome */
{
    from {background: red;}
    to {background: yellow;}
}
```

## animation

当在@keyframe创建动画时，需要把它绑定到一个选择器，否则动画不会有任何的效果。

animation方法是动画所需的，通过设置animation的值，来绑定动画效果。示例如下：

```css
div
{
    animation: myfirst 5s;
    -webkit-animation: myfirst 5s; /* Safari 与 Chrome */
}
```

## CSS3动画属性

下面的表格列出了@keyframe规则和所有动画属性

| 属性                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| @keyframe                 | 规定动画。                                                   |
| animation                 | 所有动画属性的简写属性，除了 animation-play-state 属性。     |
| animation-name            | 规定 @keyframes 动画的名称。                                 |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             |
| animation-timing-function | 规定动画的速度曲线。默认是 "ease"。                          |
| animation-fill-mode       | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
| animation-delay           | 规定动画何时开始。默认是 0。                                 |
| animation-iteration-count | 规定动画被播放的次数。默认是 1。                             |
| animation-direction       | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          |
| animation-play-state      | 规定动画是否正在运行或暂停。默认是 "running"。               |

