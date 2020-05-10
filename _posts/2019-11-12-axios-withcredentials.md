---
layout: post
title: axios跨域及携带cookie设置
description: axios跨域及携带cookie设置
tag: axios、proxyTable、withCredentials
category: 问题、技巧
---
## 跨域设置

在config目录下的index.js中的dev下进行如下配置

```json
proxyTable: { // 设置地址代理，跨域请求外部链接
  '/api': {
    target: 'http://a.b.com', // 设置你调用的接口域名和端口号
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
}
```

发送请求时加 headers

```javascript
this.axios({
  method: 'get',
  url: '',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  data:{}
}).then(function(res){
  // ...
}).catch(function(error){
  // console.log(error.response)
})
```

## 携带cookie

axios对象上增加配置

```javascript
axios.defaults.withCredentials = true;
```

