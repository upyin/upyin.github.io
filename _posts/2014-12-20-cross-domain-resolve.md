---
layout: post
title: 跨域解决方案简述
description: 什么是跨域？如何解决跨域？
tag: jsonp、cors、nginx、postmessage
category: 总结、技术
---
## 什么是跨域

不同域名之间的数据互动即为跨域。例如我们常见的ajax请求、cookie、localStorage、sessionStorage等。提到跨域，自然要提到**同源策略**。

### 什么是同源策略

同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

同源策略会限制的行为：

```bash
1.H5本地存储的数据无法读取（cookie、localStorage、sessionStorage、indexDB）
2.DOM和JS对象无法获得
3.ajax请求不能发送
```

### 存在跨域的场景

```bash
1.不同域名
http://www.domain.com
http://www.domain2.com
2.不同子域名
http://a.domain.com
http://b.domain.com
3.域名和域名对应的ip
http://www.domain.com
http://192.168.2.3
4.不同端口
http://www.domain.com:8000
http://www.domain.com:7000
5.不同协议
http://www.domain.com
https://www.domain.com
```

## 跨域的解决方案

```bash
1.jsonp
2.document.domain+iframe
3.postMessage
4.CORS(跨域资源共享)
5.nginx代理
6.WebSocket协议
```

### jsonp解决方案

我们知道，页面中引用的css、js、img等静态资源是可以和页面域名所不相同的，而这是被浏览器所允许的。基于此原理，我们可以通过动态的创建script标签，并请求一个带参数的网址链接实现跨域通信。

前端请求：

```javascript
var script = document.createElement("script");
script.type = "text/javascript";

// 传参一个回调函数名给后端
script.src = "http://www.domain2.com:8000/data?op=getData&callback=jsonp1";
document.head.appendChild(script);

// 执行回调函数
function jsonp1(res) {
  console.log(res);
}
```

后端返回数据：

```javascript
jsonp1({errorcode:0, data:{}})
```

由于jsonp是使用创建script标签的方式实现的，所以只能支持get请求方式，无法使用post请求方式。

### document.domain+iframe解决方案

此方法仅限主域相同，子域不同的跨域场景。是通过设置两个页面的document.domain为基础主域，来解决跨域问题。

父窗口（http://www.domain.com/a.html）

```markup
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = "domain.com";
    var user = "admin";
</script>
```

子窗口（http://child.domain.com/b.html）

```javascript
document.domain = "domain.com";
// 获取父窗口中的变量
console.log(window.parent.user);
```

### postMessage解决方案

postMessage是HTML5 XMLHttpRequest Level 2 中的API，且是为数不多可以跨域操作的window属性之一，它可以解决以下方面的问题：

```bash
1.页面和其打开的新窗口的数据传递
2.多窗口之间消息传递
3.页面与嵌套的iframe消息传递
4.上面三个场景的跨域数据传递
```

postMessage的用法：

```bash
postMessage(data, origin)
data：html5规范支持任意基本类型或可复制的对象，但部分浏览器只支持字符串，所以最好使用JSON.stringify()序列化一下。
origin：协议+主机+端口号，也可以设置为“*”，表示可以传递给任意窗口，如果要指定和当前窗口同源的话设置为“/”。
```

代码示例：

页面A：http://www.domain1.com/a.html

```markup
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>       
    var iframe = document.getElementById('iframe');
    iframe.onload = function() {
        var data = {
            name: 'aym'
        };
        // 向domain2传送跨域数据
        iframe.contentWindow.postMessage(JSON.stringify(data), 'http://www.domain2.com');
    };

    // 接受domain2返回数据
    window.addEventListener('message', function(e) {
        console.log(e.data);
    }, false);
</script>
```

页面B：http://www.domain2.com/b.html

```javascript
// 接收domain1的数据
window.addEventListener('message', function(e) {
    console.log(e.data);
    var data = JSON.parse(e.data);
    if (data) {
        data.number = 16;
        // 处理后再发回domain1
        window.parent.postMessage(JSON.stringify(data), 'http://www.domain1.com');
    }
}, false);
```

### CORS（跨域资源共享）

cors包括两种方式，一种是普通的跨域请求，一种是需要带cookie的跨域请求。

普通的跨域请求：只需要服务器设置head头，添加Access-Control-Allow-Origin即可。

```bash
Access-Control-Allow-Origin: "*" // 允许所有域名跨域请求
Access-Control-Allow-Origin: "http://www.domain.com" // 只允许域名为www.domain.com的跨域请求
```

带cookie的跨域请求：不仅需要设置服务器返回的head头，前端也需要设置withCredentials。

需要注意的是，由于同源策略的限制，所读取的cookie为跨域请求接口所在域的cookie，而非当前页。

目前，所有浏览器均支持该功能（IE8+：IE8/9需要使用XDomainRequest对象来支持CORS），CORS也已经成为主流的跨域解决方案。

原生ajax设置：

```javascript
var xhr = new XMLHttpRequest(); // IE8/9需用window.XDomainRequest兼容

// 前端设置是否带cookie
xhr.withCredentials = true;

xhr.open('post', 'http://www.domain2.com:8080/login', true);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('user=admin');

xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
        alert(xhr.responseText);
    }
};
```

Jquery ajax：

```javascript
$.ajax({
    ...
   xhrFields: {
       withCredentials: true    // 前端设置是否带cookie
   },
   crossDomain: true,   // 会让请求头中包含跨域的额外信息，但不会含cookie
    ...
});
```

Vue框架axios设置：

```javascript
axios.defaults.withCredentials = true
```

当使用带cookie的跨域请求时，后端设置的Access-Control-Allow-Origin不能再为“*”了，否则会报错。

后端设置：

```bash
// 允许跨域访问的域名：若有端口需写全（协议+域名+端口），若没有端口末尾不用加'/'
setHeader("Access-Control-Allow-Origin", "http://www.domain1.com"); 

// 允许前端带认证cookie：启用此项后，上面的域名不能为'*'，必须指定具体的域名，否则浏览器会提示错误
setHeader("Access-Control-Allow-Credentials", "true"); 
```

### nginx反向代理方案

跨域原理： 同源策略是浏览器的安全策略，不是HTTP协议的一部分。服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就不存在跨越问题。

实现思路：通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。

### WebSocket解决方案

WebSocket协议是HTML5的一种新协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。

这里就不具体展开讲述WebSocket的使用api了。