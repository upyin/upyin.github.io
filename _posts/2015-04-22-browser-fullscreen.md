---
layout: post
title: H5实现浏览器全屏效果
description: 使用html5 js实现浏览器全屏效果
tag: requestFullscreen、fullscreenchange
category: 技巧、问题
---
项目中可能会有需求将浏览器设置为全屏展示，类似于F11的全屏效果。在HTML5中，W3C制定了关于全屏的API，就可以实现全屏幕的效果。

目前的支持情况

| 浏览器 | chrome | safari | firfox | IE   |
| ------ | ------ | ------ | ------ | ---- |
| 版本   | 15+    | 5.1+   | 10+    | 11+  |

全屏实现代码

```javascript
var docElm = document.documentElement;
//W3C  
if (docElm.requestFullscreen) {  
    docElm.requestFullscreen();  
}
//FireFox  
else if (docElm.mozRequestFullScreen) {  
    docElm.mozRequestFullScreen();  
}
//Chrome等  
else if (docElm.webkitRequestFullScreen) {  
    docElm.webkitRequestFullScreen();  
}
//IE11
else if (elem.msRequestFullscreen) {
    elem.msRequestFullscreen();
}
```

退出全屏实现代码

```javascript
if (document.exitFullscreen) {  
    document.exitFullscreen();  
}  
else if (document.mozCancelFullScreen) {  
    document.mozCancelFullScreen();  
}  
else if (document.webkitCancelFullScreen) {  
    document.webkitCancelFullScreen();  
}
else if (document.msExitFullscreen) {
    document.msExitFullscreen();
}
```

全屏事件监听

```javascript
document.addEventListener("fullscreenchange", function () {  
    fullscreenState.innerHTML = (document.fullscreen)? "" : "not ";
}, false);  
   
document.addEventListener("mozfullscreenchange", function () {  
    fullscreenState.innerHTML = (document.mozFullScreen)? "" : "not ";
}, false);  
   
document.addEventListener("webkitfullscreenchange", function () {  
    fullscreenState.innerHTML = (document.webkitIsFullScreen)? "" : "not ";
}, false);

document.addEventListener("msfullscreenchange", function () {
    fullscreenState.innerHTML = (document.msFullscreenElement)? "" : "not ";
}, false);
```

