---
layout: post
title: 网页端图片上传预览的实现
description: 使用js封装一个图片上传并预览的小组件
tag: FileReader、input[type=file]
category: 技巧、问题
---
经常会在网页上看到上传图片的功能，那怎么做到上传并预览图片呢？下面是个小的Demo供大家参考，可以实现上传功能和预览的功能。

页面上创建img和input：

```markup
<img src="" alt="点击上传图片" id="imgPreview" onclick="uploadImg();">
<input type="file" id="picture" onchange="previewImage(this,'imgPreview');" style="display:none;"/>
```

js部分的实现：

```javascript
function uploadImg(){
    $("#picture").trigger('click');
}

function previewImage(file, prvid) {
    /* file：file控件
     * prvid: 图片预览容器
     */
    var tip = "Expect jpg or png!"; // 设定提示信息
    var filters = {
        "jpeg"  : "/9j/4",
        "png"   : "iVBORw"
    }
    var prvbox = document.getElementById(prvid);
    prvbox.src = "";
    if (window.FileReader) { // html5方案
        for (var i=0, f; f = file.files[i]; i++) {
            var fr = new FileReader();
            fr.onload = function(e) {
                var src = e.target.result;
                if (!validateImg(src)) {
                    alert(tip)
                } else {
                    showPrvImg(src);
                }
            }
            fr.readAsDataURL(f);
        }
    } else { // 降级处理
        if ( !/\.jpg$|\.png$/i.test(file.value) ) {
            alert(tip);
        } else {
            showPrvImg(file.value);
        }
    }

    function validateImg(data) {
        var pos = data.indexOf(",") + 1;
        for (var e in filters) {
            if (data.indexOf(filters[e]) === pos) {
                return e;
            }
        }
        return null;
    }

    function showPrvImg(src) {
        prvbox.src = src;
        if(src.length > 34383){
            prvbox.src = "";
            alert("图片尺寸请不要大于20KB");
            return false;
        }
        $("#pictureBase").val(src);
    }
}
```

