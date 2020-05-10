---
layout: post
title: Vue过滤器
description: 介绍vue框架中的过滤器filter
tag: Vue、filter、过滤器
category: 技术、总结
---
Vue中的过滤器真是一大利器，接下来我们就讲讲它的厉害之处。以及怎么去使用，来提高我们的开发效率。

## 什么是过滤器

过滤器顾名思义，它是一个工具，用来对传入的数据进行过滤和修饰。例如，传入一串字符串“hello”，然后我们要通过过滤器判断当前字符串是否包含“e”，包含则返回该字符串，否则返回空字符串。又或者，传入一个字符串“hello”，然后我们给它进行一点修饰，加上一个字符串“world”，然后返回一个新的字符串“hello world”。

## 如何使用过滤器

### 使用

过滤器的使用可以分为两种：双花括号插值和v-bind表达式。过滤器应该被添加在JavaScript表达式的尾部，由“管道”符号指示：

**双花括号插值**

```markup
{{ message | filter }}
```

**v-bind表达式**

```markup
<div v-bind:id="rawId | filter"></div>
```

### 注册

过滤器的注册可以分为全局注册和局部注册。全局注册过滤器可以使用在任何地方，局部注册的过滤器则使用在组件内。

**全局定义过滤器**

需要在创建Vue实例之前全局定义过滤器

```javascript
Vue.filter('globalFilter', function(vale){
	return value + "！";
});

new Vue({
	// ……
})
```

**局部定义过滤器**

在一个组件的选项中定义本地的过滤器

```javascript
filters: {
  componentsFilter:function(value){
    return value + "！";
  }
}
```

注意：当全局过滤器和局部过滤器重名时，会采用局部过滤器。

### 过滤器的参数写法

**方式一：过滤器可以串联**

```markup
{{ message | filterA | filterB }}
```

这个例子，message是作为参数传给filterA函数，而filterA函数的返回值作为参数传给filterB函数，最终结果显示是由filterB返回的。

举个例子：

```javascript
<div>{{ '2019' | filterA | filterB }}</div>

filters:{
  filterA:function(value){
    return value + '年';
  },
  filterB:function(value){
    return value + " 你好！";
  }
}

// 2019年 你好！
```

**方式二：过滤器可以带参数**

```markup
{{ message | filterA('arg1',arg2) }}
```

这里，filterA被定义为接收三个参数的过滤器函数。其中message的值作为第一个参数，普通字符串'arg1'作为第二个参数，表达式arg2的值作为第三个参数。

举个例子：

```javascript
<div>{{ '2019' | filterA('03', '09') }}</div>

filters:{
  filterA:function(value, arg1, arg2){
    return `${value}-${arg1}-${arg2}`;
  }
}

// 2019-03-09
```

**方式三：传入过滤器的值可以是多个**

```markup
{{ '2019年','03月' | filterB }}
```

上述代码表示"2019年"和'03月'分别作为参数传给filterB。

举个例子：

```javascript
<div>{{ '2019年','03月' | filterB }}</div>

filters:{
  filterB:function(arg1, arg2){
    return arg1 + arg2;
  }
}

// 2019年03月
```

