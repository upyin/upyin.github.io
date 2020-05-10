---
layout: post
title: Vue混入(mixins)
description: 介绍vue框架中的混入
tag: Vue、mixins、混入
category: 技术、总结
---
## 什么是混入(mixins)

混入(mixins)提供了一种非常灵活的方式，来分发Vue组件中的可服用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

## 混入(mixins)的使用

首先看一个示例，来初步了解一下混入的使用方式。

```javascript
// 定义一个混入对象
const myMixin = {
  created() {
    this.hello();
  },
  methods: {
    hello() {
      console.log('hello from myMixin');
    }
  }
}

// 定义一个使用混入对象的组件
const Component = Vue.extend({
  mixins: [myMixin]
})

let component = new Component(); // "hello from myMixin"
```

### 选项合并

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

1、数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

2、同名钩子函数（如 created）将合并为一个数组，因此都将被调用。混入对象的钩子将在组件自身钩子之前调用。

3、值为对象的选项（如 methods、components和directives），将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

### 全局混入

混入也可以进行全局注册。一旦使用了全局混入，它将影响每一个之后创建的Vue实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。

```javascript
Vue.mixin({
  created: function() {
    var myOption = this.$options.myOption;
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello'
})
// 'hello'
```

