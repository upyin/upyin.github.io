---
layout: post
title: JavaScript的this原理简析
description: 简述javascript中，this的指向问题，以及原理
tag: this
category: 总结、技术
---
## 抛砖引玉

示例：

```javascript
var obj = {
  a:1,
  fn:function(){
    console.log(this.a++);
  }
}

var fn = obj.fn;
var a = 2;

obj.fn(); // 2
fn(); // 3
```

通过上面的示例，可以看出来函数内使用了this，导致输出的结果截然不同。很多教科书式的回答是，this指的是函数运行时所在的环境。对于obj.fn()来说，函数fn()运行的环境是obj，所以this指向obj。对于fn()来说，函数fn()运行的环境是window全局，所以this指向了全局window。导致了两种不同的运行结果。

结果是知道了，但是为什么呢？函数的运行环境是怎么决定的？为什么obj.fn()就是在obj环境执行，而一旦var fn=obj.fn，fn()就变成全局环境执行了呢？

## 数据在内存中的存储

我们知道对象是指向存储在内存中的数据的地址。

```javascript
var obj = { a:1 };
```

在面向对象的语言中，我们知道，将一个对象赋值给一个变量的时候，这个变量实际上是指向对象所在内存中的地址。即obj实际上是指向存储{a:1}的这段内存的地址，也就是说，变量obj是一个地址引用。

```javascript
var b = obj.a;
```

上面的赋值操作，实际上的执行流程是：先从obj拿到内存地址，然后根据地址读取出对象{a:1}，最后返回对象的属性a对应的值。

内存中的原始对象是以字典结构保存，每一个属性名都对应一个属性描述对象。例如下面：

```json
{
  a: {
    [[value]]: 1
    [[writbale]]: true
    [[enumerable]]: true
    [[configurable]]: true
  }
}
```

## 函数

如果对象的属性是一个函数呢？

```javascript
var obj = {
  fn: function(){}
};
```

js引擎会将函数单独保存在内存中，然后将函数的地址赋值给fn属性的value属性。

内存中的数据字典如下：

```json
{
  fn: {
    [[value]]: 函数的地址
    ...
  }
}
```

由于函数式一个单独的值，所以它可以在不同的环境（上下文）执行。

```javascript
var fn = function(){};
var obj = {fn:fn};

fn(); // 单独执行
obj.fn(); // 在obj环境执行
```

## 环境变量

JavaScript允许在函数体内部，引用当前环境的其他变量。由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（上下文）。所以，this就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。

现在，再来看下文章最开始提到的问题，变一目了然。obj.fn()是通过obj所指向的内存地址，然后找到的fn，所以就是在obj的环境执行。一旦var fn=obj.fn，变量fn就直接指向了fn()函数本身在内存中的地址，而fn()函数本身在内存中是运行在全局环境下的，所以变量fn的执行环境就是全局环境了。