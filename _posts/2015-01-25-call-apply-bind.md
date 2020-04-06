---
layout: post
title: call、apply和bind方法的区别及使用
description: 简述js中call、apply、bind的区别，使用方法
tag: call、apply、bind
category: 总结、技术
---
## 什么是this

call、apply、bind的作用是改变函数运行时this的指向，所以在文章开始时，有必要说清楚this是什么。

this指的是函数运行时所在的环境。举个例子，来更深入的了解一下。

```javascript
var obj = {
  a:1,
  fn:function(){
    console.log(this.a)
  }
};

var fn = obj.fn;
var a = 2;

obj.fn(); // 1
fn(); // 2
```

通过上面的示例，我们可以较清晰的看出，this所指的环境。当调用obj.fn()函数时，this指向的是obj对象中的函数fn所运行的环境，即环境为obj，所以得到的结果是1；而直接执行函数fn，则this指向的是全局的环境，即环境为window，所以得到的结果为2。

我们已经初步了解了this，那么我们接下来说说我们的主题call、apply、bind的区别和作用吧。

## call、apply、bind方法的来源

call、apply、bind三个方法都是继承自Function.prototype中的，属于实例方法。我们可以使用hasOwnProperty来进行判断：

```javascript
console.log(Function.prototype.hasOwnProperty('call')); // true
console.log(Function.prototype.hasOwnProperty('apply')); // true
console.log(Function.prototype.hasOwnProperty('bind')); // true
```

对象、数组、函数都继承了Function.prototype对象的call、apply、bind方法，所以这三个方法在对象、数组、函数中也可使用。

## call

函数实例的call方法，可以指定该函数内部this的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数，并立即执行该函数。

call方法第一个参数是指定函数内部中this的指向（即函数运行时所在的作用域），后面的参数是函数调用时需要传递的参数。

第一个参数是必须的，可以是null、undefined、this，但是不能为空。设置为null、undefined、this表明函数此时处于全局作用域。

举个例子：

```javascript
var obj = {
  a:'position'
};
function fn(country, province, city){
  console.log(this.a+':'+country+' '+province+' '+city)
}

fn('china','zhejiang','hangzhou'); // undefined:china zhejiang hangzhou
fn.call(obj, 'china','zhejiang','hangzhou'); // position:china zhejiang hangzhou
```

可以看出，当直接执行fn函数时，this的指向是全局window，而全局上并没有设置a，所以输出结果是undefined；当使用fn.call方法时，将obj传进来，指定函数执行时的环境为obj（即this的作用域为obj），所以输出的结果为position。同时可以看出，后面传入的参数为fn函数的参数。

## apply

apply方法和call方法类似，第一个参数也是指定该函数内部this的指向（即函数执行时所在的作用域）；第二个参数和call不一样了，call方法是需要单独传参的，而apply方法则是通过数组的方式进行传参。

举个例子，和上面进行对比，能够更清晰的看一二来。

```javascript
var obj = {
  a:'position'
};
function fn(country, province, city){
  console.log(this.a+':'+country+' '+province+' '+city)
}

fn.apply(obj, ['china','zhejiang','hangzhou']); // position:china zhejiang hangzhou
```

通过上面的例子，可以较清晰的和call方法进行对比，即第二个参数的不同。call方法是单独一个个的传参进行设置函数的参数，而apply方法是直接通过数组的方式传参来设置函数的参数。

## bind

bind方法和call方法也是类似的，第一个参数也是指定该函数内部this的指向（即函数执行时所在的作用域）；第二个开始的参数，则为函数需要接收的参数列表。bind区别于call方法的地方，在于bind会返回一个新函数，而并未立即执行这个函数。

举个例子：

```javascript
var obj = {
  a:'position'
};
function fn(country, province, city){
  console.log(this.a+':'+country+' '+province+' '+city)
}

var fn2 = fn.bind(obj, 'china', 'zhejiang', 'hangzhou');

console.log(fn2); // function (...){...}
console.log(fn2()); // position:china zhejiang hangzhou
```

可以看出，经过bind后得到的是一个新的函数，而且并没有立即执行。

拓展，低版本不支持bind方法，我们通过call和apply实现一个。

```javascript
if (!Function.prototype.bind) {
    Function.prototype.bind = function () {
        var self = this,                        // 保存原函数
            context = [].shift.call(arguments), // 保存需要绑定的this上下文
            args = [].slice.call(arguments);    // 剩余的参数转为数组
        return function () {                    // 返回一个新函数
            self.apply(context, [].concat.call(args, [].slice.call(arguments)));
        }
    }
}
```

## 应用场景

求数组中最大和最小值

```javascript
var arr = [1,2,9,10];
var max = Math.max.apply(null, arr); // 10
var min = Math.min.apply(null, arr); // 1
```

将类数组转换为对象

```javascript
var arr = Array.prototype.slice.call({0:1, 2:2, length:2}); // [1,2]
```

数组追加

```javascript
var arr1 = [1,2,3];
var arr2 = [4,5,6];
var total = [].push.apply(arr1, arr2); // 6
console.log(arr1); // [1,2,3,4,5,6]
console.log(arr2); // [4,5,6]
```

判断变量类型

```javascript
function isArray(obj){
  return Object.prototype.toString.call(obj) == '[object Array]'
}
isArray([]); // true
isArray('test'); // false
```

利用call和apply做继承

```javascript
function Person(name,age){
    this.name = name
    this.age = age
    this.sayAge = function(){
        console.log(this.age)
    }
}
function Female(){
    Person.apply(this,arguments)//将父元素所有方法在这里执行一遍就继承了
}
var female = new Female('Lissa',18);
console.log(female.sayAge()); // 18
```

## 总结

call、apply、bind方法的区别：

1.第一个参数都是指定函数内部this的指向（即函数执行时所在的作用域）。

2.call、bind方法第二个及后面的参数，需要单独一个个填写，传递给函数；而apply则直接以数组的方式传递。

3.call、apply方法在调用时，便立即执行函数；而bind方法，则是返回一个新函数，并不会立即执行。