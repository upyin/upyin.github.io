---
layout: post
title: web端实现复制内容到剪切板
description: web端使用javascript实现复制内容到剪切板
tag: Selection API、execCommand API
category: 技巧、问题
---
在逛各种技术网站的时候，经常会看到代码区域右上角有个“复制”的提示，点击复制就可以将内容复制到剪切板中。

![](/images/20160717selection/copy.png)

如上效果的实现。之前该效果都是通过js+flash结合的方式，但随着HTML5技术的发展，Flash已经在很多场合不适用了，甚至被屏蔽。那如何使用纯js来实现呢？这里就讲述下使用HTML5推出的Selection API和execCommand API来实现复制的功能。

话不多说，直接上代码！

```javascript
function copy(copyTxt){
    // 创建dom节点
    var aux = document.createElement("div");
    aux.setAttribute("class", "sharecopy");
    aux.innerHTML = copyTxt;
    aux.style.display = "none";
    document.body.appendChild(aux);
    // 复制到剪切板
    copyDOM = document.querySelector(".sharecopy");
    var range = document.createRange();
    range.selectNode(copyDOM);
    // range.selectNodeContents(copyDOM);
    window.getSelection().addRange(range);
    try{
        var successful = document.execCommand("copy");
    } catch(e){}
    // 清除节点
    window.getSelection().removeAllRanges();
    document.querySelector(".sharecopy").remove();
}

copy("复制测试<br>复制换行");
```

### 注意！注意！

创建的元素不能display:none;

2、创建的元素不能visibility:hidden;

页面样式中不能设置：-webkit-user-select:none;

