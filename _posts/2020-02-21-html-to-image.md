---
layout: post
title: HTML页面生成图片
description: 使用html2canvas将HTML页面生成图片
tag: html2canvas、canvas
category: 技术、总结
---
 信息化时代，信息的爆炸和各厂之间的拉人竞争极其激烈。如何能获取到更多的用户，达到更高的曝光率，图片的分享便是一种很有效的方式。相对于普通的网页链接分享，图片的分享能够非常直接的展示分享内容，而网页链接则只有短短的一个标题和描述只有在点击进入的时候才能看到内容。

图片的分享有两种，一种是纯静态图片（内容固定不变），一种是动态图片（内容是个性化的）。对于第一种图片就没什么可说的了，主要来讲讲第二种动态内容的图片如何生成。

![](/images/20200221htmltoimage/toimage.jpeg)

上面就是一个动态内容的图片，每个用户的内容显示的是不一样的。网页上如何实现生成这样的一张图片呢？html2canvas闪亮登场~

### html2canvas

部分：

```markup
<div id="container">
	<div>展示动态数据的内容</div>
</div>
```

JavaScript部分：

```javascript
import html2canvas from 'html2canvas'

let container = docuemnt.getElementById("container");
html2canvas(container).then(canvas => {
  let imgData = canvas.toDataURL(); // 得到base64的图片
});
```

生成的图片重新插入到页面上，并引导用户保存图片到本地，就大功告成了。

### 一些坑

**截图模糊**

```javascript
let container = document.getElementById("container"),
    width = container.clientWidth, // DOM的宽度
    height = container.clientHeight, // DOM的高度
    canvas = document.createElement("canvas"), // 创建Canvas
    scale = 2; // 定义缩放2倍

// 设置canvas的画布宽高和样式
canvas.width = width * scale;
canvas.height = height * scale;
canvas.style.width = width * scale + 'px';
canvas.style.height = height * scale + 'px';
canvas.getContext('2d').scale(scale, scale);

// 设置html2canvas的参数
let opts = {
  scale: scale,
  canvas: canvas,
  width: width,
  height: height
};
html2canvas(container, opts).then();
```

**提示跨域**

1、设置参数useCORS: true

```javascript
let opts = {
  useCORS: true
};
html2canvas(container, opts).then();
```

2、确定图片请求是否支持跨域 Access-Control-Allow-Origin:*

