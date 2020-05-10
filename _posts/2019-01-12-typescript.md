---
layout: post
title: 走进TypeScript
description: TypeScript介绍
tag: TypeScript、TS
category: 技术、总结
---
TypeScript是什么？就如官网所说的，它是JavaScript的超集，它可以编译成纯JavaScript。

虽然官网上阐述着TypeScript可以在任何浏览器、任何计算机和任何操作系统上运行，但目前最优的做法是先编译成JavaScript。因为目前浏览器并不直接兼容。

## TypeScript优点

### 始于JavaScript，归于JavaScript

TypeScript是JavaScript的超集，完全兼容JavaScript的语法。它是在JavaScript的现有语法基础上，增加了类型校验。并且可以通过node编译，将TypeScript编译成简洁的JavaScript代码，然后运行在任何浏览器、node环境中。

### 强大的工具构建 大型应用程序

TypeScript相较于JavaScript的优势所在，即是引入了类型的概念。众所周知，JavaScript是一门弱类型语言，一个用let声明的变量可以被赋值和修改为各种类型的值。JavaScript由于这样的特性，所以在浏览器运行到具体的语句块时，才能发现代码的正确与否。

而TypScript引入了类型，它允许开发者在开发应用时，就可以使用高效的开发工具和常用操作来进行静态检测，在运行代码之前便可以预先检查代码的正确性。

### 先进的JavaScript

TypeScript提供最新的和不断发展的JavaScript特性，包括那些来自2015年的ECMAScript和未来的提案中的特性，比如异步功能和Decorators，以帮助建立健壮的组件。

## TypeScrip和代码检查工具的区别

对于初学者，提到TypeScript便自然会想到代码检查工具（如JsHint、JsLint、EsLint）,它们其实是完全不同的两个东西。

代码检查工具是基于语法和规范的校验，例如使用了未定义的变量，括号未闭合，缩进的大小，未使用完全相等===，声明的变量未使用等。这些检测确实能够在运行前提前发现问题，但还是有所不足，比如一些参数类型、个数的错误检测不到。

```javascript
// 未使用ts的代码
let a = '123';
a.split(''); // ['1','2','3']
a=123;
a.split(''); // Uncaught TypeError: a.splict is not a function

// 使用ts的代码
let a: string = '123';
a=123; // 编译时便会报错，提示类型不匹配
```

通过上面的代码示例，我们可以发现，即使使用了EsLint这种代码检查工具也是无法预先在编译中发现问题的，只有当浏览器运行时才会报错，为时已晚。而使用TS，则能够在编译中提前发现问题，因为已经定义了变量a的类型为string字符串，却给它赋值了数值类型。

## 前景

React Native拥抱TS。详见 [《Using TypeScript with React Native》](https://reactnative.dev/blog/2018/05/07/using-typescript-with-react-native)

Vue3.0拥抱TS。[Vue对TypeScript的支持](https://cn.vuejs.org/v2/guide/typescript.html)

可见，TypeScript的还是有很大的前景的。

## 使用

具体的用法和语法可以看官方文档。 超链 [TypeScript中文网](https://www.tslang.cn/)

