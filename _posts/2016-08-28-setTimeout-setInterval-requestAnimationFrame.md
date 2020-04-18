---
layout: post
title: setTimeout、setInterval和requestAnimationFrame区别与使用
description: 使用js实现动画效果的3种方式
tag: setTimeout、setInterval、requestAnimationFrame
category: 技术、总结
---
我们在实现网页动画的时候，除去CSS3的实现方式之外，我们最常用的便是JS中的setTimeout、setInterval和requestAnimationFrame三个API了。这三个API均为window上绑定的方法。

## 用法与区别

### setTimeout

用法：setTimeout(function, millseconds)

作用：用于延时给定的毫秒数之后执行特定的方法，如果需要在执行之前取消则需要调用clearTimeout(timeoutId)来清楚任务。timeoutId=setTimeout(function, millseconds)。

### setInterval

用法：setInterval(function, millseconds)

作用：用于每隔一段给定的毫秒数，执行特定的方法。该方法将无休止的执行下去，除非使用clearInterval(intervalId)方法来清除它。当调用clearInterval之后，将不再重复执行了。intervalId=setInterval(function, millseconds)。

### requestAnimationFrame

用法：requestAnimationFrame(function)

作用：用于动画，与setTimeout方法类似，不同的是setTimeout是用户指定的，而requestAnimationFrame是浏览器刷新频率决定的，一般遵循W3C标准，它在浏览器每次刷新页面之前执行。

注意：若你想在浏览器下次重绘之前继续更新下一帧动画，那么回调函数自身必须再次调用window.requestAnimationFrame()。

## 总结

requestAnimationFrame相对于setTimeout、setInterval将更加的节省浏览器的性能，因为它是根据用户显示器的刷新频率来设置动画的。当requestAnimationFrame的兼容性不如setTimeout和setInterval，需要对低版本的浏览器做好降级处理。

