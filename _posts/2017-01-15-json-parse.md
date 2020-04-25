---
layout: post
title: JSON对象解析之JSON.parse和JSON.stringify
description: JSON对象解析之JSON.parse和JSON.stringify，以及JSON.parse解析json字符串遇到换行符报错问题解决方案
tag: JSON.parse、JSON.stringify、\n、\n\r
category: 技术、总结
---
对JSON数据的操作处理，我们通常会使用到JSON.parse()和JSON.stringify()方法。

## JSON.parse()

JSON.parse()方法用于将一个JSON字符串转换为对象。

### 语法

```javascript
JSON.parse(text[, reviver]);
```

### 参数说明

text：必需，一个有效的JSON字符串。

reviver：可选，一个转换结果的函数，将为对象的每个成员调用该函数。

### 返回值

返回给定JSON字符串转换后的对象。

## JSON.stringify()

JSON.stringify()方法用于将JavaScrip值转换为JSON字符串。

### 语法

```javascript
JSON.stringify(value[, replacer[, space]])
```

### 参数说明

value：必需，要转换的JavaScript值（通常为对象或数组）

replacer：可选，用于转换结果的函数或数组。

如果replacer为函数，则JSON.stringify将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回undefined，则排除成员。根对象的键是一个空字符串：“”。

如果replacer是一个数组，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。

space：可选，文本添加缩进、空格和换行符，如果space是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果space大于10，则文本缩减10个空格。space也可以使用非数字，如：\t。

### 返回值

返回包含JSON文本的字符串。

## 使用场景

### 判断数组/对象是否相等

```javascript
let a = [1,2,3];
let b = [1,2,3];
JSON.stringify(a) === JSON.stringify(b); // true
```

### 将对象存储到localStorage/sessionStorage中

```javascript
let a = {a:1,b:2,c:3};
localStorage.setItem(JSON.stringify(a));
```

### 深度拷贝对象

```javascript
let a = {a:1,b:2,c:3};
let b = JSON.parse(JSON.stringify(a));
```

## 异常情况

使用JSON.parse解析json字符串时，如果字符串中包含换行符，则会报错。

pc浏览器去除\n的方法：

```javascript
let a = str.replace(/\n/g, '');
```

移动端浏览器去除\n方法：

```javascript
let a = str.replace(/\\n/g, '');
```

换行符还有可能是\r\n，\n\r等。