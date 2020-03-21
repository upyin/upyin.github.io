---
layout: post
title: web存储方式的介绍
description: web存储方式有cookie，localStorage，sessionStorage，web database等
tag: cookie，localStorage，sessionStorage，web database
category: 总结、技术
---
web存储技术层出不穷，从最开始的cookie到现在的localStorage，web存储技术在向着更大存储容量，更安全的方向发展。接下来介绍当下使用最多的几种存储技术。包括它们的使用方法和优缺点。

## cookie

cookie是使用的最早而且最流行的存储技术。几乎每个网站都在使用cookie来存储用户的信息，和用户在网站中留下的足迹。当一个用户访问某个网站的时候，该网站便会发送一个或多个cookie，浏览器会将这些cookie记录在本地，并在下次发送http请求的时候将cookie一并发送给服务器。网站通过cookie来识别用户，以及将用户的用户名及密码存储在cookie中来方便用户自动登录，而不需要每次都输入用户名和密码。

#### cookie的不可跨域名性

cookie是不可跨域名来访问的，因此谷歌的cookie百度网站是访问不到的，同样的百度的cookie谷歌也访问不到的。这就保证了cookie的安全性，以防止恶意的网站获取其他网站的cookie用于非法的目的。

#### cookie的小容量性

cookie的大小基本在4k左右，因此不宜在cookie中存储太多的信息，cookie应该小而精，以减少每次http请求时的流量和耗时。

#### cookie的使用

cookie是以键／值对的方式进行存储的。一般在cookie中存储，cookie名，值，过期时间，作用域。

```javascript
document.cookie = "cookiename=value;cookievalue=value;expires=GTM_String;path=path";
```

cookie的名是用户自己定义的，但是cookie的值在复杂多变的情况下是不确定的，那要如何保证cookie的值中不出现一些特殊的符号(; , =)等，那就要使用escape()函数进行编码，它能将一些特殊的符号使用十六进制来表示，从而可以存储在cookie值中，而且可以避免中文乱码的出现。那取出值的时候呢，那就使用unescape()函数对cookie的值进行解码，然后再使用。

document.cookie看上去像一个属性，可以赋不同的值，但它和一般的属性不一样，改变它的值并不意味着丢失原来的值，而是在原来的值的后面接上新的值。

```javascript
document.cookie="name1=value1";
document.cookie="name2=value2";
//相当于
//document.cookie="name1=value1;name2=value2";
```

cookie的过期时间是可以设置的，但要注意的是，在设置时间的时候要使用toGTMString()方法进行转换。

```javascript
var date = new Date();
var expiresDays = 10;//十天
date.setTime(date.getTime() + expiresDays*24*60*60*1000);
document.cookie="expires="+date.toGTMString();
```

cookie的访问目录是可以设置的。需要使用path参数来设置。

```javascript
document.cookie="path=cookieDir";//cookieDir表示可访问cookie的目录。
document.cookie="path=/shop";//表示但前的cookie仅能在shop目录下使用
document.cookie="path=/";//设置目录为根目录，这样便可以在整个网站下使用cookie
```

cookie的访问域名也是可以设置的。需要使用domain参数来设置。

```javascript
document.cookie="domain=cookieDomain";//cookieDomain表示可以访问cookie的域名
document.cookie="domain=.google.com";//谷歌的整个域名(包括二级域名)都可以访问此cookie
```

因为cookie是以字符串的方式存储的，因此在取cookie的值时，便可以直接使用

```javascript
var str = document.cookie;
```

来获取cookie的值，然后再使用javascript中对字符串的处理方法进行处理。

删除cookie，要想删除cookie，只要给cookie设置一个过去的时间，浏览器便会删去cookie。

```javascript
var date = new Date();
date.setTime(date.getTime()-10000);//设置一个过去的时间
document.cookie="expires="+date.toGTMString();
```

## localStorage

localStorage作为当下最流行的web离线应用的存储方案，在于其大容量的存储特性，和永久不会过期的特性。localStorage是本地存储中使用最为流行的方式之一，现在的主流浏览器均已支持，包括手机浏览器也均已支持。下面说说它的特性以及使用方法。

#### 大容量性

localStorage的存储容量一般在5M左右。用来存储离线应用的数据已经绰绰有余了。

#### 永久不会过期

除非javascript中进行删除操作，否则localStorage将永久不会过期。

#### localStorage的使用

判断浏览器是否支持localStorage

```javascript
if(window.localStorage){
	//支持
}else{
	//不支持
}
```

localStorage是以键／值对的方式存储的

```javascript
//存储
window.localStorage.name = value;
window.localStorage['name'] = value;
window.localStorage.setItem('name', value);

//获取
var value = window.localStorage.name;
var value = window.localStorage['name'];
var value = window.localStorage.getItem('name');

//删除某个localStorage键值
window.localStorage.removeItem('name');
//删除所有键值
window.localStorage.clear();

//key()方法用于便利localStorage的键
var storage = window.localStorage;
for(var i=0; i < storage.length; i++){
		console.log(storage.key(i)+":"+storage.getItem(storage.key(i)));
}
```

## sessionStorage

sessionStorage同localStorage一样是HTML5中的新技术，但不同点在于，localStorage用于本地持久化的存储，而sessionStorage用于一个会话中的数据存储，当会话结束时，数据将被销毁。

sessionStorage的使用同localStorage一样，可以使用setItem(),getItem(),removeItem()和clear()方法进行存取和删除数据。

## Web SQL Database

Web SQL Database用于本地的数据库存储。不同于localSotrage的地方在于其能够像普通数据库一样使用sql语句进行查询和建立关系型数据库。但是目前浏览器对Web SQL Database的支持不是很好，尤其是手机上，基本都不支持Web SQL Database。下面简单的介绍下Web SQL Database的使用。

判断浏览器是否支持Web SQL Database。

```javascript
if(window.openDatabase){
	//支持
}else{
	//不支持
}
```

建立数据库连接

```javascript
db = openDatabase('dbName','version','description',size);
```

以上代码创建了一个数据库对象db,名称是dbName,版本号(1.0),描述信息和估计大小值。

执行查询操作

```javascript
db.transaction(
	function(transaction){
		transaction.executeSql(
			'select * from tableName where name=?',		//sql语句的字符串
			['abc'],									//插入到查询中问好所在处的字符串数据
			function(result){},							//成功时执行的函数
			function(tx,error){}						//失败时执行的函数
		);
	}
);
```

