---
layout: post
title: Vue插件开发及使用
description: 介绍vue框架中插件的开发和使用
tag: Vue、插件、plugins
category: 技术、总结
---
## 什么是插件

如官网所述的，插件通常用来为Vue添加全局功能。插件的功能范围没有严格的限制，一般有这几种：

1、添加全局方法或者property。如：vue-custom-element

2、添加全局资源：指令/过滤器/过渡等。如：vue-touch

3、通过全局混入来添加一些组件选项。如：vue-router

4、添加Vue实例方法，通过把它们添加到Vue.prototype上实现。

5、一个库，提供自己的API，同时提供上面提到的一个或多个功能。如：vue-router

## 使用插件

通过全局方法Vue.use()使用插件。

```javascript
Vue.use(MyPlugin, {option: true})

new Vue({
  // ...
})
```

1、插件的使用要在Vue实例初始化之前。

2、Vue.use支持传入可选的选项对象来扩展。

3、Vue.use会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件。

## 开发插件

Vue.use使用插件时，会自动调用插件的install方法。因此Vue插件应该暴露一个install方法，这个方法的第一个参数是Vue构造器，第二个参数是可选的选项对象。

```javascript
MyPlugin.install = function (Vue, options) {
  // 1.添加全局方法或property
  Vue.myGlobalMethod = function () {
    // todo...
  }
  
  // 2.添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // todo...
    }
  })
  
  // 3.注入组件选项
  Vue.mixin({
    created: function () {
      // todo...
    }
  })
  
  // 4.添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // todo...
  }
}
```

**给插件添加自动调用Vue.use()**

Vue.js官方提供的一些插件（例如 vue-router）在检测到Vue是可访问的全局变量时会自动调用Vue.use()。这在cdn方式引入插件，可以显得很方便。

但是需要注意，在像CommonJS这样的模块环境中，应该始终显式地调用Vue.use()。

## 发布插件

开发完的插件如何让其他人能够使用到，我们可以通过以下的步骤来将插件发布到npm上。

1、需要npm账号，没有需要注册。

2、项目根目录下，执行npm login登录账号。

3、执行命令npm publish进行发布。

注：每次发布，需要修改package.json的版本号。