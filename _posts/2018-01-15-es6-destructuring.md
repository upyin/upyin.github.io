---
layout: post
title: ES6系列教程之解构赋值
description: ES6系列教程之变量的解构赋值
tag: ES6、解构赋值
category: 技术、总结
---
本文是ES6系列教程的第一篇正文，将讲述ES6中的新特性——解构赋值。

## 数组的解构赋值

在以前，我们给变量赋值，只能通过单个变量的赋值来实现。

```javascript
let a = 1;
let b = 2;
let c = 3;
```

ES6允许我们按照一定的模式，从数组中提取值，按照对应位置，对变量赋值。

```javascript
let [a, b, c] = [1, 2, 3]; // a=1, b=2, c=3
let [x, [y], z] = [1, [2], 3]; // x=1, y=2, z=3
let [n, ...m] = [1,2,3]; // n=1, m=[2,3]
let [q, w, ...e] = [1]; // q=1, w=undefined, e=[]

// 可以指定默认值
let [s, o = true] = [1]; // s=1, o=true
let [i = 1] = [undefined]; // i=1
let [j = 1] = [null]; // j=null
// 说明：ES6中使用严格的相等运算符(===)，判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效
```

如果等号的右边不是数组（不是可遍历的结构），则将报错。

```javascript
// 报错
let [a] = 1;
let [a] = false;
let [a] = NaN;
let [a] = undefined;
let [a] = null;
let [a] = {};
```

## 对象的解构赋值

不仅数组可以进行解构，对象也可以进行解构赋值。

```javascript
let {a, b} = {a:1, b:2}; // a=1, b=2
```

对象的解构与数组有一个重要的不同。数组是按照顺序进行对应的赋值，而对象因为没有顺序，所以是根据变量名进行对应赋值。如果，没有对应的变量名将会是undefined。

```javascript
let {a, b} = {a:1}; // a=1, b=undefined

// 可以指定默认值
let {x=1, y} = {y:2}; // x=1, y=2
```

如果变量名与属性名不一致，则需要写成下面这样。

```javascript
let {first:f, last:l} = {first:'hellow', last:'world'};
// f='hello', l=world
```

### 使用技巧

通过对象的解构赋值，可以将已有的对象的方法赋值到某个变量。

```javascript
let {log, sin, cos} = Math;
let {log} = console;
```

## 解构赋值的用途

### 交换变量的值

```javascript
let x = 1;
let y = 1;

[x, y] = [y, x];
```

### 从函数返回多个值

```javascript
function example(){
  return [1, 2, 3];
}
let [a, b, c] = example();

function example2(){
  return {
    a:1,
    b:2
  };
}
let {a, b} = example2();
```

### 函数参数的定义

```javascript
// 有序
function f([x, y, z]){...}
f([1, 2, 3]);

// 无序
function f2({x, y, z}){...}
f2({z:3, y:2, x:1});
```

### JSON数据提取

```javascript
let jsonData = {
  id: 1,
  status: 'ok',
  data:[2,3,4,5]
};

let {id, status, data:number} = jsonData;
```

