---
layout: post
title: 开启CSS3硬件加速
description: 使用css3特性开启硬件加速
tag: css3、硬件加速
category: 技术、总结
---
## 什么是硬件加速

硬件加速即为浏览器的渲染过程使用GPU进行处理，而非使用自带的比较慢的渲染器。通过硬件加速可以使animation和transition动画更加流畅。

## 如何开启硬件加速

Chrome，FireFox，Safari，IE9+和Opera这些主流的浏览器都支持硬件加速，只要使用特定的CSS语句就可以开启硬件加速。

使用3D效果来开启硬件加速：

```css
.speed-up {
   -webkit-transform: translate3d(250px,250px,250px)
   rotate3d(250px,250px,250px,-120deg)
   scale3d(0.5, 0.5, 0.5);
}
```

但是我们通常页面上可能并没有用到transform属性，使用上面的3D效果就有些不合适了，我们可以通过下面的方式来开启硬件加速：

```css
.speed-up{
   -webkit-transform: translateZ(0);
   -moz-transform: translateZ(0);
   -ms-transform: translateZ(0);
   -o-transform: translateZ(0);
   transform: translateZ(0);
}
```

在safari和chrome浏览器上，可能在使用animation或者transition时会出现闪烁的问题，可以使用以下的解决方法：

```css
.speed-up {
  -webkit-backface-visibility: hidden;
   -moz-backface-visibility: hidden;
   -ms-backface-visibility: hidden;
   backface-visibility: hidden;

   -webkit-perspective: 1000;
   -moz-perspective: 1000;
   -ms-perspective: 1000;
   perspective: 1000;

/**webkit上也可以用以下语句  **/
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   -ms-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
}
```

## 注意

硬件加速最好只用在animation或者transform上。不要滥用硬件加速，否则会增加性能的消耗，导致动画变的卡顿，物极必反。

