---
layout: post
title: WebAssembly简介
description: WebAssembly简介
tag: WebAssembly
category: 技术、总结
---
## 什么是WebAssembly

WebAssembly（缩写Wasm）是基于堆栈的虚拟机的二进制指令格式。

它是JavaScript的一种补充。它提供了一个途径，让写C++、C或者是Rust以及其他静态类型语言的人，把他们写的编译成一个模块，而这个模块可以被JavaScript调用，与JavaScript协同工作。

WebAssembly作为JavaScript的补充，提供了和C++一样的强类型机制。有了这些强类型的机制，引擎就可以省去大量时间的类型处理，将JavaScript像C++一样进行静态编译，从而实现将JavaScript的速度提升到和原生代码一样的程度。

## 设计目标

WebAssembly的设计目标包含以下几方面：

-快速：使网页的运行速度能够接近原生程序

-安全：内存安全的沙盒执行环境，将强制执行浏览器的同源和权限安全策略

-开发：维护web的无版本，经过功能测试和向后兼容的性质，能够访问和JavaScript相同的Web API

-可调式：以文本格式展示，以便调试、测试、学习和编程

-可移植：用于编译高级语言（如C / C++ / Rust），从而可以在Web上为客户端和服务器应用程序进行部署

## 开发

WebAssembly的开发指南可以参见 [官网](https://webassembly.org/)

