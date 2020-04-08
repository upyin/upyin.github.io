---
layout: post
title: HTTP缓存机制详解
description: 详解HTTP缓存机制，包括强制缓存和协商缓存
tag: http缓存、强制缓存、协商缓存
category: 技术、总结
---
## 缓存概述

缓存是web应用中很常见且很重要的一个概念。当我们打开一个网页的时候，浏览器会发起各种请求以加载页面需要使用的资源文件，如果没有缓存，则每次的访问都需要向服务器请求对应的数据资源，这将造成很大的带宽浪费，而且降低用户的使用体验。缓存的出现，可以减少带宽的使用和对服务器的访问压力，同时可以快速的从本地缓存中获取数据提高用户的访问速度和体验。

根据客户端是否需要向服务器发送请求判断文件是否更新、是否需要取本地缓存文件，分成了两类缓存机制。一种是强制缓存，另一种是协商缓存。

## 强制缓存

强制缓存是直接从本地缓存数据库中直接取出资源，无需向服务器发送请求来确定资源文件是否更新。

![](/images/20150513httpcache/http-cache-force.png)

HTTP中用来判断是否命中强缓存的字段为Expires（HTTP1.0）和Cache-Control（HTTP1.1）。其中Cache-Control的优先级高于Expires。

### Expires

Expires是HTTP1.0协议中定义的。它的值是一个绝对时间（Wed, 13 May 2015 19:09:15 GMT）,代表这个资源的过期时间是2015年5月13日 19点09分15秒，在这个时间点之前都将从本地缓存数据库中获取资源。

### Cache-Control

Cache-Control是HTTP1.1协议中定义的。Cache-Control常见的字段含义：

| 字段     | 含义                                |
| -------- | ----------------------------------- |
| private  | 客户端可以缓存                      |
| public   | 客户端和代理服务器均可以缓存        |
| max-age  | 单位秒。表示缓存的资源将在X秒后过期 |
| no-cache | 需要使用协商缓存来校验是否过期      |
| no-store | 不可缓存                            |

### 强制缓存状态码

强制缓存取本地缓存资源时返回的状态码为200，资源的的Size分为两种：from memory cache 或者 from disk cache。

from memory cache表示缓存的资源存在内存中，当浏览器或标签页关闭后内存中的缓存就会被释放，重新打开页面将不再能取到该缓存资源。

from disk cache表示缓存的资源存在硬盘中，即使浏览器或标签页关闭，下次打开浏览器依旧能获取到缓存资源。

总所周知，因为内存的访问永远是最快的，所以当第一次打开页面取缓存资源时是from disk cache的，但再刷新页面时，取缓存资源将变为from memory cache。

## 协商缓存

协商缓存与强制缓存所不同的是，它需要向服务器发起请求来确认是否从缓存数据库取缓存资源。

![](/images/20150513httpcache/http-cache-diff.png)

协商缓存会向服务器发起一个http请求，然后根据服务器返回的标识来判断是否从本地缓存取资源数据，如果是，则直接取本地缓存数据不需要再从服务器下载资源文件，可以大大的减少带宽的使用。

用来标志是否取缓存的字段根据HTTP的协议版本，分成了Last-Modified、If-Modified-Since（HTTP1.0）和Etag、If-None-Match（HTTP1.1）。其中Etag的优先级高于Last-Modified。

### Last-Modified / If-Modified-Since

当浏览器第一次访问一个资源的时候，服务器会在response header中返回一个Last-Modifiied，代表这个资源最后的修改时间；当浏览器再次访问该资源时，会在request header中带上If-Modified-Since，其值未上次请求时response header中返回的Last-Modified值；然后服务器根据这个值来判断这段时间内资源是否有修改过，如果没有修改过，则返回状态码304，如果有修改过，则返回状态码200并返回最新的资源数据。

### Etag / If-None-Match

Etag/If-None-Match与上面不同的是，它是通过对比一个校验码来判断资源是否更改过，而不是通过修改时间来判断是否修改过。其对于 Last-Modified的优点在于，当文件进行修改后再次更改回来（例如文件中增加了一个字符，然后又把这个字符删除了）时，Last-Modified会认为这个资源已经被更新了，不能再取缓存，而Etag则会认为该资源没有进行更新，可以取缓存数据。这样也可以减少一些带宽的使用。

