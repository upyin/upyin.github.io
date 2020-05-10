---
layout: post
title: 使用Vue.observable()进行状态管理
description: 介绍如何使用Vue.observable()进行状态管理
tag: Vue、observable、状态管理
category: 技术、总结
---
Vue.js在2.6.0版新增了一个全局的方法Vue.observable()，用来进行状态管理，我们可以用它来实现简单的组件间的数据通信。

## 介绍一下Vue.observable(object)

### 新增版本

Vue.js 2.6.0

### 参数

{object} object

### 用法

让一个对象可响应。Vue内部会用它来处理data函数返回的对象。

返回的对象可以直接用于渲染函数和计算属性内，并且会在发生变更时触发相应的更新。也可以作为最小化的跨组件状态存储器，用于简单的场景：

```javascript
const state = Vue.observable({count:0})

const Demo = {
  render(h) {
    return h('button', {
      on: { click: () => { state.count++ }}
    }, `count is: ${state.count}`)
  }
}
```

### 注意

在Vue 2.x中，被传入的对象会直接被Vue.observable变更，它和被返回的对象是同一个对象。在Vue 3.x中，则会返回一个可响应的代理，而对源对象直接进行变更仍然是不可响应的。因此，为了向前兼容，推荐始终操作使用Vue.observable返回的对象，而不是传入源对象。

## Vue.observable()使用示例

搭建个简单的脚手架，在项目src下创建一个store目录并创建一个store.js文件，在组件里使用提供的store和mutation方法，从而实现多个组件共享数据状态。

store.js文件包含一个store和一个mutations，分别用来指向数据和处理方法：

```javascript
import Vue from 'vue';

export let store = Vue.observable({
  isPlaying: false,
  audioId: ''
});

export let mutations = {
  setPlayStatus(status) {
    store.isPlaying = status;
  },
  setAudioId(id) {
    store.audioId = id;
  }
}
```

组件index.vue中引用，在组件里使用数据和方法：

```markup
<template>
	<div class="container">
		<div @click="setPlayStatus(true)">{{ isPlaying }}</div>
		<div @click="setAudioId("123")">{{ audioId }}</div>
	</div>
</template>
<script>
import {store, mutations} from "@/store/store"
export default {
	data () {
		return {}
	},
	computed:{
		isPlaying() {
			return store.isPlaying;
		},
		audioId() {
			return store.audioId;
		}
	},
	methods:{
		setPlayStatus: mutations.setPlayStatus,
		setAudioId: mutations.setAudioId
	}
}
</script>
```

