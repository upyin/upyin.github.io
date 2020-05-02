---
layout: post
title: navigator对象简析
description: 简述javascript中，window.navigator对象的使用方法
tag: navigator、window.navigator
category: 总结、技术
---
## 什么是navigator

navigator对象是window上的对象，所以也可以通过window.navigator方式来使用。navigator对象包含了有关浏览器的基本信息。

## navigator对象属性

navigator对象包含以下常见的属性：

| 属性            | 描述                                         |
| --------------- | -------------------------------------------- |
| appCodeName     | 返回浏览器的代码名                           |
| appMinorVersion | 返回浏览器的次级版本                         |
| appName         | 返回浏览器的名称                             |
| appVersion      | 返回浏览器的平台和版本信息                   |
| browserLanguage | 返回当前浏览器的语言                         |
| cookieEnable    | 返回浏览器是否启用了cookie的布尔值           |
| cpuClass        | 返回浏览器系统的CPU等级                      |
| onLine          | 返回系统当前是否处于脱机模式的布尔值         |
| platform        | 返回运行浏览器的操作系统平台                 |
| systemLanguage  | 返回系统使用的默认语言                       |
| userAgent       | 返回由客户端发送给服务器的user-agent头部的值 |
| userLanguage    | 返回用户设置的系统的语言设置                 |

## 应用场景

移动端还是PC端

```javascript
!!navigator.userAgent.match(/AppleWebKit.*Mobile*/)
```

当前操作系统是android还是ios

```javascript
// android
navigator.userAgent.indexOf('Android') > -1 || navigator.userAgent.indexOf('Linux') > -1

// ios
navigator.userAgent.indexOf('iPhone') > -1 || navigator.userAgent.indexOf('iPad') > -1
```

判断当前所处浏览器类型

```javascript
// IE
navigator.userAgent.indexOf('Trident') > -1

// opera
navigator.userAgent.indexOf('Presto') > -1

// safari && chrome
navigator.userAgent.indexOf('AppleWebKit') > -1

// fireFox
navigator.userAgent.indexOf('Gecko') > -1 && navigator.userAgent.indexOf('KHTML') == -1

```

