---
layout: post
title: 前端性能监控之Performance介绍
description: 前端性能监控之Performance
tag: Performance
category: 技术、总结
---
根据MDN介绍，Web Performance API允许网页访问某些函数来测量网页和Web应用程序的性能，包括Navigation Timing API和高分辨率时间数据。因此我们可以通过使用Performance API来监测网页的加载性能，以便分析之用。

因为，很多数据必须在页面完全加载之后才能得到，所以最好是在页面加载完成（onload）之后再调用该API方法。

## 属性

### timing（PerformanceTiming）

从输入url到页面完全加载的全过程的时间统计，会返回一个PerformanceTiming对象，单位为毫秒。

![](/images/20161016performance/timing.jpg)

按触发顺序排列所有属性：

**navigationStart**：在同一个浏览器上下文中，前一个网页（与当前页面不一定同域）unload 的时间戳，如果无前一个网页 unload ，则与 fetchStart 值相等。

**unloadEventStart**：前一个网页（与当前页面同域）unload 的时间戳，如果无前一个网页 unload 或者前一个网页与当前页面不同域，则值为 0。

**unloadEventEnd**：和 unloadEventStart 相对应，返回前一个网页 unload 事件绑定的回调函数执行完毕的时间戳。

**redirectStart**：第一个 HTTP 重定向发生时的时间。有跳转且是同域名内的重定向才算，否则值为 0。

**redirectEnd**：最后一个 HTTP 重定向完成时的时间。有跳转且是同域名内的重定向才算，否则值为 0。

**fetchStart**：浏览器准备好使用 HTTP 请求抓取文档的时间，这发生在检查本地缓存之前。

**domainLookupStart**：DNS 域名查询开始的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等。

**domainLookupEnd**：DNS 域名查询完成的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等。

**connectStart**：HTTP（TCP） 开始建立连接的时间，如果是持久连接，则与 fetchStart 值相等,如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接开始的时间。

**connectEnd**：HTTP（TCP） 完成建立连接的时间（完成握手），如果是持久连接，则与 fetchStart 值相等,如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接完成的时间。

**注意！这里握手结束，包括安全连接建立完成、socks授权通过！**

**secureConnectionStart**：HTTPS 连接开始的时间，如果不是安全连接，则值为 0。

**requestStart**：HTTP 请求读取真实文档开始的时间（完成建立连接），包括从本地读取缓存,连接错误重连时，这里显示的也是新建立连接的时间。

**responseStart**：HTTP 开始接收响应的时间（获取到第一个字节），包括从本地读取缓存。

**responseEnd**：HTTP 响应全部接收完成的时间（获取到最后一个字节），包括从本地读取缓存。

**domLoading**：开始解析渲染 DOM 树的时间，此时 Document.readyState 变为 loading，并将抛出 readystatechange 相关事件。

**domInteractive**：完成解析 DOM 树的时间，Document.readyState 变为 interactive，并将抛出 readystatechange 相关事件。

**注意！只是DOM树解析完成，这时候并没有开始加载网页内的资源！**

**domContentLoadedEventStart**：DOM 解析完成后，网页内资源加载开始的时间,文档发生 DOMContentLoaded事件的时间。

**domContentLoadedEventEnd**：DOM 解析完成后，网页内资源加载完成的时间（如 JS 脚本加载执行完毕），文档的DOMContentLoaded 事件的结束时间。

**domComplete**：DOM 树解析完成，且资源也准备就绪的时间，Document.readyState 变为 complete，并将抛出 readystatechange 相关事件。

**loadEventStart**：load 事件发送给文档，也即 load 回调函数开始执行的时间,如果没有绑定 load 事件，值为 0。

**loadEventEnd**：load 事件的回调函数执行完毕的时间,如果没有绑定 load 事件，值为 0。

常用计算方法：

DNS查询耗时 ：domainLookupEnd - domainLookupStart

TCP链接耗时 ：connectEnd - connectStart

request请求耗时 ：responseEnd - responseStart

解析dom树耗时 ： domComplete - domInteractive

白屏时间 ：responseStart - navigationStart

domready时间(用户可操作时间节点) ：domContentLoadedEventEnd - navigationStart

onload时间(总下载时间) ：loadEventEnd - navigationStart

### navigation

navigation属性用来告诉开发者，当前页面是通过什么方式导航过来的，包含两个属性type和redirectCount。

type标志页面导航类型：

| type常数          | 枚举值 | 说明                                                         |
| ----------------- | ------ | ------------------------------------------------------------ |
| TYPE_NAVIGATE     | 0      | 普通进入，包括：点击链接、在地址栏中输入 URL、表单提交、或者通过除下表中 TYPE_RELOAD 和 TYPE_BACK_FORWARD 的方式初始化脚本。 |
| TYPE_RELOAD       | 1      | 通过刷新进入，包括：浏览器的刷新按钮、快捷键刷新、location.reload()等方法。 |
| TYPE_BACK_FORWARD | 2      | 通过操作历史记录进入，包括：浏览器的前进后退按钮、快捷键操作、history.forward()、history.back()、history.go(num)。 |
| TYPE_UNDEFINED    | 255    | 其他非以上类型的方式进入。                                   |

redirectCount表示到达最终页面前，重定向的次数，该属性有同源策略限制，只能检测同源的重定向。

### memory

描述内存的多少，是Chrome浏览器中添加的一个非标准属性。

属性值包括：

jsHeapSizeLimit：内存大小限制。

totalJSHeapSize：可使用的内存。

usedJSHeapSize：JS对象(包括V8引擎内部对象)占用的内存，不能大于totalJSHeapSize，如果大于，有可能出现了内存泄漏。

## 方法

### getEntries()

获取所有资源请求的时间数据,这个函数返回一个按startTime排序的对象数组，数组成员除了会自动根据所请求资源的变化而改变以外，还可以用mark(),measure()方法自定义添加，该对象的属性中除了包含资源加载时间还有以下五个属性。

**name**：资源名称，是资源的绝对路径或调用mark方法自定义的名称。

**startTime**：开始时间。

**duration**：加载时间

**entryType**：资源类型，entryType类型不同数组中的对象结构也不同。

| 值         | 该类型对象                  | 说明                                                         |
| ---------- | --------------------------- | ------------------------------------------------------------ |
| mark       | PerformanceMark             | 通过mark()方法添加到数组中的对象                             |
| measure    | PerformanceMeasure          | 通过measure()方法添加到数组中的对象                          |
| paint      | PerformancePaintTiming      | 值为first-paint'首次绘制、'first-contentful-paint'首次内容绘制 |
| resource   | PerformanceResourceTiming   | 所有资源加载时间，用处最多                                   |
| navigation | PerformanceNavigationTiming | 现除chrome和Opera外均不支持，导航相关信息                    |
| frame      | PerformanceFrameTiming      | 现浏览器均未支持                                             |

**initiatorType**：谁发起的请求。

| 发起对象                             | 值                              | 说明                                                   |
| ------------------------------------ | ------------------------------- | ------------------------------------------------------ |
| a Element                            | link / script / img / iframe 等 | 通过标签形式加载的资源，值是该节点名的小写形式         |
| a CSS resourc                        | css                             | 通过css样式加载的资源，比如background的url方式加载资源 |
| a XMLHttpRequest object              | xmlHttpRequest /   fetch        | 通过xhr加载的资源                                      |
| a PerformanceNavigationTiming object | navigator                       | 当对象是PerformanceNavigationTiming时返回              |

注意！

1、目前通过<audio>、<video>加载资源，initiatorType无法返回“audio”和“video”，chrome中只能返回空字符串，firfox中只能返回“other”。

2、如果一个图片在页面内使用img方式引入，又作为背景图片引入，那么initiatorType返回“img”。

3、performance.getEntries(params)这种形式仍出于草案阶段，目前仍有很多浏览器未支持。

4、使用该方法统计资源信息的时候首先可以合理利用clearResourceTimings清除已统计过的对象避免重复统计，其次要过滤掉因上报统计数据而产生的对象。

### getEntriesByName(name,type[optional])，getEntriesByType(type)

name：想要筛选出的资源名。

type：entryType的值中的一个。

返回值仍是一个数组，这个数组相当于getEntries()方法经过所填参数筛选后的一个子集。

### clearResourceTimings()

该方法无参数和返回值，可以清除目前所有entryType为“resource”的数据，用于写单页应用的统计脚本非常有用。

### mark(name)，measure(name, startMark, endMark)，clearMarks()，clearMeasures()

用于做标记和清除标记，供用户自定义统计一些数据，比如某函数运行耗时等。

name：自定义的名称，不要和getEntries()返回的数组中其他name重复。

startMark：作为开始时间的标记名称或PerformanceTiming的一个属性。

endMark：作为结束时间的标记名称或PerformanceTiming的一个属性。

创建标记：mark(name)

记录两个标记的时间间隔：measure(name, startMark, endMark)

清除指定标记：window.performance.clearMarks(name)

清除所有标记：window.performance.clearMarks()

清除指定记录间隔数据：window.performance.clearMeasures(name)

清除所有记录间隔数据：window.performance.clearMeasures()

### now()

performance.now()是当前时间与performance.timing.navigationStart的时间差，以微秒（百万分之一秒）为单位的时间，与 Date.now()-performance.timing.navigationStart的区别是不受系统程序执行阻塞的影响，因此更加精准。

