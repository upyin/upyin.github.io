---
layout: post
title: Date对象的使用及倒计时效果实现
description: JS中Date对象的使用及倒计时效果的实现
tag: Date，setInterval
category: 技术、总结
---
工作中经常会涉及到需要实现倒计时的效果，功能的实现自然很简单，使用setInterval和Date对象就能实现。但在移动端，可能在实现的时候还需要注意一些坑。

## Date对象

Date对象是JavaScript中提供的处理日期和时间的对象。

### Date对象的语法

1.直接获取当前时间为标准时间

```javascript
var date = new Date();
console.log(date); // Sat Sep 16 2017 21:03:30 GMT+0800 (中国标准时间)
```

Date对象会自动把当前日期和时间保存为其初始值。

2.指定时间转变成标准时间

```javascript
var date = new Date("Sep 16,2017,21:03:30");
console.log(date); // Sat Sep 16 2017 21:03:30 GMT+0800 (中国标准时间)

// 支持的传参格式
// new Date("month dd,yyyy hh:mm:ss");
// new Date("month dd,yyyy");
// new Date(yyyy,month,dd,hh,mm,ss);
// new Date(yyyy,month,dd);
// new Date(ms);
```

### Date对象方法

Date对象常用的方法

| 方法          | 说明                                    |
| ------------- | --------------------------------------- |
| getFullYear() | 从Date对象                              |
| getMonth()    | 返回月份 0~11                           |
| getDay()      | 返回一周中的某一天0~6，可以用来表示星期 |
| getDate()     | 返回一个月中的某一天 1~31               |
| getHours()    | 返回小时 0~23                           |
| getMinutes()  | 返回分钟 0~59                           |
| getSeconds()  | 返回秒数 0~59                           |
| getTime()     | 返回从1970年1月1日至今的毫秒数          |

## 使用

### 获取当前时间

返回当前时间“2017年09月16日 21:15:20”

```javascript
var date = new Date();
var dateObj = {
  year: date.getFullYear(),
  month: date.getMonth()+1,
  date: date.getDate(),
  hours: date.getHours(),
  minutes: date.getMinutes(),
  seconds: date.getSeconds()
};
var dateStr = dateObj.year+"年"+dateObj.month+"月"+dateObj.date+"日 "+dateObj.hours+":"+dateObj.minutes+":"+dateObj.seconds;
```

### 倒计时

```javascript
var time = 20000; // 倒计时20s
var endTime = (+new Date()) + time; // 倒计时结束时间，当前时间+20s
var interval = setInterval(function(){
  var nowTime = +new Date();
  var leaveTime = parseInt((endTime-nowTime)/1000); // 单位s
  if (leaveTime < 0){
    clearInterval(interval);
  }
  console.log(leaveTime);
}, 1000);
```

上面的倒计时效果并没有直接使用time--来实现，而是先计算了最终的结束时间，然后用结束时间-开始时间，用差值来显示。因为在移动端，当用户手动滑动页面时，可能会阻止页面上的js继续执行，会造成倒计时的不准确。而使用差值来进行显示，即使js被阻塞，但当用户松开页面时，js能够正确的获取当前时间，且结束时间是不便的，那么差值将是准确的。