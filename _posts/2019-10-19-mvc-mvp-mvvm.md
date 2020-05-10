---
layout: post
title: MVC、MVP和MVVM
description: 介绍MVC、MVP和MVVM的区别
tag: MVC、MVP、MVVM
category: 技术、总结
---
## MVC

![](/images/20191019mvvm/mvc.png)

视图（View）：用户界面

控制器（Controller）：业务逻辑

模型（Model）：数据保存

在MVC模式下，数据都是单向通信的：

1）View传送指令到Controller 

2） Controller完成业务逻辑后，要求Model改变状态

Model将新的数据发送到View，用户得到反馈

## MVP

![](/images/20191019mvvm/mvp.png)

MVP模式将Controller改名为Presenter，同时改变了通信方向。

1）View与Model不再发生联系，都通过Presenter传递

2）View和Presenter、Model和Presenter之间的通信，都是双向的

3）View中不部署任何业务逻辑，称为“被动视图”（Passive View），没有任何主动性；Presenter中部署了所有的逻辑。

## MVVM

![](/images/20191019mvvm/mvvm.png)

MVVM模式将Presenter改名为ViewModel，基本上与MVP模式完全一致。唯一的区别是，它采用双向绑定（data-binding）：View的变动，自动反映到ViewModel，反之亦然。

