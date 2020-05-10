---
layout: post
title: Vue组件间通信方式
description: 介绍Vue组件间的通信方式
tag: Vue、props、$emit、Bus、Vuex
category: 技术、总结
---
## 前言

组件是vue.js中最强大的功能之一，而组件实例的作用域是相互独立的，它们之间的数据无法相互使用。一般组件的关系可以分为3种：父子关系、兄弟关系、隔代关系，其中隔代关系又可以被视作是多次的父子关系。

![](/images/20190510communication/components.png)

针对组件间不同的关系，vue提供了很多种可以支持组件间相互数据传递的方式：props、$parent/$children、$attrs/$listeners、provide/inject、Event Bus、Vuex等。组件间数据通信的方式非常多，这里将只挑选最常用的方式进行介绍。

## 父子组件间数据通信

### 父组件向子组件传数据——props

父组件向子组件传递数据，最常用的就是props了。我们来看个示例，了解一下props的使用方式。

父组件：

```markup
<parent>
	<child v-bind="child-msg"></child>
</parent>

data(){
	return {
		childMsg:[1,2,3]
	}
}
```

1、通过在子组件上使用v-bind绑定数据，向子组件传数据

2、v-bind上绑定的数据的变量名必须是小写，或者使用 - 来代替驼峰

子组件：

```javascript
// 方式一
props: ['childMsg']

// 方式二
props: {
  childMsg: Array // 可以指定传入的类型，如果类型不合法则会警告
}

// 方式三
props: {
  childMsg: {
    type: Array,
      default: [0,0,0] // 可以设置默认值
  }
}
```

注意：官方定义的props是单向绑定，即父组件数据单向传递给子组件，所以请不要对子组件中接收到的数据直接进行修改。如果需要对子组件接收到的数据进行处理，请将数据赋值到一个新的data变量中，再对data变量进行修改。

### 子组件向父组件传递数据——$emit

子组件向父组件传递数据可以通过$emit，然后父组件中通过on来监听。通过一个示例，来了解子组件向父组件传数据。

子组件：

```markup
<template>
	<div @click="toFather"></div>
</template>

methods: {
	toFather() {
		this.$emit('tofa', 'dataMsg'); // 通过点击触发$emit向父组件传数据
	}
}
```

'cToF'：事件名，父组件中将根据这个名称来识别数据

'dataMsg'：此处为一个字符串，可以是其他类型

父组件：

```markup
<div>
	<child @tofa="change" :msg="msg"></child>
</div>

methods: {
	change(msg) {
		this.msg = msg;
	}
}
```

父组件中监听tofa事件，然后触发change方法，并将子组件中传递的数据赋值到msg上。

## 兄弟组件间数据通信

### Event Bus——$emit/$on

兄弟组件之间的数据通信，可以使用event bus来实现。这是一个轻量级的实现方案。

创建bus.js

```javascript
import Vue from 'vue';
export default new Vue;
```

组件A传递数据($emit)

```javascript
import bus from "@/bus"

<div @click="eve"></div>

methods: {
  eve() {
    bus.$emit('change', 'dataMsg');
  }
}
```

组件B接收数据($on)

```javascript
import bus from "@/bus"

<div v-bind="msg"></div>

mounted() {
  bus.$on('change', (data) => {
    this.msg = data;
  })
}
```

### Vuex

Vuex实现了一个单向数据流，在全局拥有一个State存放数据，当组件要更改State中的数据时，必须通过Mutation进行，Mutation同时提供了订阅者模式供外部插件调用获取State数据的更新。而当所有异步操作(常见于调用后端接口异步获取更新数据)或批量的同步操作需要走Action，但Action也是无法直接修改State的，还是需要通过Mutation来修改State的数据。最后，根据State的变化，渲染到视图上。

![](/images/20190510communication/vuex.png)

Vuex的详细使用可以移步 [官网](https://vuex.vuejs.org/zh/)

