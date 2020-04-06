---
layout: post
title: 时间格式化小组件
description: 使用js封装一个自定义时间格式化的小组件
tag: Date、format
category: 技巧、问题
---
在工作学习中，经常会遇到这样的需求，页面上需要显示时间，且时间的格式展示成“2015-03-07 15:30:15”这个样子。

我们的第一个想法肯定是使用Date方法，然后获取年、月、日、时、分、秒，再根据上面的格式进行字符串拼接。

那如果页面中有其他的地方也需要展示时间，但展示形式不再是上面那个样子呢？要展示成“2015/03/07 15:30:15”怎么办？

那我们就封装一个方法吧，然后将方法挂载到Date的原型上，方便使用。

```javascript
Date.prototype.format = function(format) {
	var o = {
		"M+" : this.getMonth()+1, //month
		"d+" : this.getDate(), //day
		"h+" : this.getHours(), //hour
		"m+" : this.getMinutes(), //minute
		"s+" : this.getSeconds(), //second
		"q+" : Math.floor((this.getMonth()+3)/3), //quarter
		"S" : this.getMilliseconds() //millisecond
	}
	
  if(/(y+)/.test(format)) 
    format = format.replace(RegExp.$1, (this.getFullYear()+"").substr(4- RegExp.$1.length));
	
  for(var k in o)
    if(new RegExp("("+ k +")").test(format))
			format = format.replace(RegExp.$1, RegExp.$1.length==1? o[k] : ("00"+ o[k]).substr((""+ o[k]).length));
	
  return format;
}

var d = new Date().format('yyyy-MM-dd');
var d = new Date().format('hh:mm:ss');
```

