---
layout: post
title: JS中数组类型的判断
description: 使用Object.prototype.toString.call()、instanceof、Array.isArray()来区分数组
tag: Object.prototype.toString.call()、instanceof、Array.isArray()
category: 技术、总结
---
JavaScript中的数组类型判断是面试中最常问到的问题之一，因为数组是对象的子类，可以参考之前写的（[文章](http://www.yinups.com/blog/use-typeof-instanceof)）。既然数组是对象的子类，使用typeof无法区分数组和对象，那有哪些方法可以判断数组呢？

## 数组的判断方法——三兄弟

### Object.prototype.toString().call()

使用Object对象的toString属性将传入的参数的类型打印出来：

```javascript
Object.prototype.toString.call({});
// "[object Object]"

Object.prototype.toString.call([]);
// "[object Array]"

Object.prototype.toString.call(undefined);
// "[object Undefined]"

Object.prototype.toString.call(null);
// "[object Null]"

...
```

通过上面的代码示例，可以看出Object.prototype.toString.call()方法可以将判断任意类型。

### instanceof

instanceof运算符用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上。

```javascript
object instanceof constructor
```

参数：object—对象，constructor—某构造函数

```javascript
var arr = [1,2,3];
var obj = {a:1};

arr instanceof Array; // true
arr instanceof Object; // true

obj instanceof Array; // false
obj instanceof Object; // true
```

因为数组是对象类型的子类，所以当使用instanceof Object判断数组时，也会返回true。在区分变量是数组还是对象时，需要谨慎判断。

### Array.isArray()

Array.isArray()用于确定传递的值是否是一个数组（Array）。如果是数组，则返回true；否则返回false。

```javascript
Array.isArray([]); // true
Array.isArray([1]); // true
Array.isArray(new Array()); // true
Array.isArray(Array.prototype); // true

Array.isArray({}); // false
Array.isArray(null); // false
Array.isArray(); // false
```

### isArray优于instanceof

当检测Array实例时，Array.isArray()优于instanceof，因为Array.isArray能够检测iframes。

```javascript
var iframe = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[window.frames.length-1].Array;
var arr = new xArray(1,2,3); // [1,2,3]

Array.isArray(arr);  // true
arr instanceof Array; // false
```

## Array.prototype数组？对象？

```javascript
Array.prototype instanceof Array; // false
Array.prototype instanceof Object; // true

Array.isArray(Array.prototype); // true
```

上面的示例，让人顿时陷入了沉思……Array.prototype到底是数组？还是对象？

首先，我们先在浏览器中打印一下：

![](/images/20200111isarray/array.png)

Array.prototype打印出来是中括号包裹的数组，并且有length属性。但是，length的值又为0。

引用MDN上的一句话：

**鲜为人知的事实：Array.prototype本身也是一个Array。**

那为什么会出现上面instanceof Array为false的情况？而Array.isArray()方法得到的结果为true？

引用stackoverflow上的两篇文章：

[why-array-prototype-is-not-instanceof-array](https://stackoverflow.com/questions/28528505/why-array-prototype-is-not-instance-of-array)

[why-is-object-prototype-instanceof-object-false](https://stackoverflow.com/questions/26962278/why-is-object-prototype-instanceof-object-false)

因此结论：

```javascript
Array.prototype instanceof Array  // false

Array.prototype instanceof Array 等价于 Array.prototype.isPrototypeOf(Array.prototype)
```

```javascript
Array.isArray(Array.prototype) // true

Array.isArray(Array.prototype) 等价于 Object.prototype.toString.call(Array.prototype) === '[object Array]'
```

两者判断使用的对象是不一样的，一个使用的是Array 一个使用的是Object。因此使用Array.prototype instanceof Object的时候会返回true。

