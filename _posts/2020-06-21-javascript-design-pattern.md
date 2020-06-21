---
layout: post
title: JavaScript常见设计模式
description: JavaScript常见的设计模式简析
tag: 单例模式、策略模式、代理模式、发布-订阅模式、适配器模式、迭代器模式、中介者模式
category: 技术、总结
---
在面向对象软件设计过程中针对特定问题的简洁而优雅的解决方案，即为设计模式。

举个例子，假设在某大型商场下有个空地拿来停车，如果空地上没有按一定的规则划分好车位，则来一辆车就会看哪里有空间直接停下来，久而久之可能里面的车就杂乱无章了。而按照一定的规则划分好车位，按车位进行停车，虽然增加了一定的成本，但是为后续的维护带来了极大的便利，这就是一种设计模式。

学习设计模式，有助于在软件开发中写出可复用和可维护性高的程序代码。

## 设计原则

在讲JavaScript中常见的设计模式之前，我们先来讲讲设计的原则是什么。

### 单一职责原则（SRP）

一个对象或方法只做一件事情。如果一个方法承担了过多的职责，那么在需求的变迁过程中，需要改写这个方法的可能性就越大。应该把对象或方法划分成较小的粒度，使其在后续的使用过程中拥有更高的可复用性。

### 最少知识原则（LKP）

一个软件实体应当尽可能少地与其他实体发生相互作用，应当尽量减少对象之间的交互。如果两个对象之间不必彼此直接通信，那么这两个对象就不要发生直接的联系，可以转交给第三方进行处理，降低模块之间的耦合度。

### 开放-封闭原则（OCP）

软件实体（类、模块、函数）等应该是可以扩展的，但不可修改的。当需要改变一个程序的功能或者给这个程序增加新功能的时候，可以使用增加代码的方式，而尽量避免改动程序的源代码，防止影响原系统的稳定。在模块的设计和封装中，需要充分考虑模块的API接口的设计是否合理和可扩展。

## 常见设计模式

### 单例模式

单例模式就是保证一个类仅有一个实例，并提供一个访问它的全局访问点。要想实现单例模式，即在实现方法中先判断实例是否存在，如果存在则直接返回，如果不存在就创建一个再返回。

举个例子，例如只能定义一个管理员，则不管调用几次都只能生成一个管理员：

```javascript
class Singleton {
  constructor(name) {
    this.name = name;
    this.getName();
  }
  getName() {
    return this.name;
  }
}

let getInstance = (function(){
        var instance = null;
        return function(name){
            if(!instance){
                instance = new Singleton(name);
            }
            return instance;
        }
    }
)()

const a = getInstance('Tom');
const b = getInstance('Jack');

console.log(a===b); //true
```

### 策略模式

策略模式就是定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。其核心是将算法的使用和算法的实现分离开来。

一个基于策略模式的程序至少由两部分组成：第一部分是一组策略类，策略类封装了具体的算法，并负责具体的计算过程；第二部分是环境类Context，Context接受客户的请求，随后把请求委托给某一个策略类，要做到这点，说明Context中要维持对某个策略对象的引用。

举个例子，例如要买什么样的车需要花多少的钱：

```javascript
const Car = {
  Benz() {
    return 100+'万元';
  },
  Volvo() {
    return 60+'万元';
  },
  QQ() {
    return 10+'万元';
  }
};

let needMoney = function(carType) {
  return `要想买${carType}汽车，需要花费${Car[carType]}。`;
}

console.log(needMoney('QQ'));
```

### 代理模式

代理模式就是为一个对象提供一个代用品或占位符，以便控制对它的访问。

举个例子，例如图片的懒加载：

```javascript
let imgFunc = (function() {
    let imgNode = document.createElement('img');
    document.body.appendChild(imgNode);
    return {
        setSrc(src) {
            imgNode.src = src;
        }
    }
})();

let proxyImage = (function() {
    let img = new Image();
    img.onload = function() {
        imgFunc.setSrc(this.src);
    }
    return {
        setSrc(src) {
            imgFunc.setSrc('loading,gif');
            img.src = src;
        }
    }
})();

proxyImage.setSrc('pic.png');
```

### 发布-订阅模式

发布-订阅模式也被称作观察者模式，定义了对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。

与传统的发布-订阅模式实现方式（将订阅者自身当成引用传入发布者）不同，在JavaScript中通常使用注册回调函数的形式来订阅。

举个例子，JavaScript中的事件就是经典的发布-订阅模式的实现：

```javascript
// 订阅
document.body.addEventListener('click', function(){
  console.log('trigger click1.');
}, false);

document.body.addEventListener('click', function(){
  console.log('trigger click2.');
}, false);

// 发布
document.body.click();// trigger click1. trigger click2.
```

### 适配器模式

适配器模式就是解决两个软件实体间的接口不兼容的问题、对不兼容的部分进行适配。

举个例子，第三方接口只接受数组作为参数，则需要对原始数据进行处理：

```javascript
// 第三方接口
function thirdInterface(arr) {
  arr.forEach(function(item){
    console.log(item);
  });
}

// 将对象转为数组
function objToArr(data) {
  let temp = [];
  for(let i in data) {
    temp.push(data[i]);
  }
  return temp;
}

const data = {
  0: 'A',
  1: 'B',
  2: 'C'
};
thirdInterface(objToArr(data));
```

### 迭代器模式

迭代器模式是指提供一种方法，顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。在使用迭代器模式之后，即使不关心对象的内部构造，也可以按顺序访问其中的每个元素。

举个例子，使用迭代器模式实现一个类似map的方法：

```javascript
function each(obj, callback) {
  let value;
  
  if (Array.isArray(obj)) {
    for (let i=0; i<obj.length; i++) {
      value = callback.call(obj[i], i, obj[i]);
      
      if (value === false) {
        break;
      }
    }
  } else {
    for (let i in obj) {
      value = callback.call(obj[i], i, obj[i]);
      
      if (value === false) {
        break;
      }
    }
  }
}

each([1,2,3], function(index, value) {
  console.log(index, value);
});

each({a:1, b:2, c:3}, function(index, value) {
  console.log(index, value);
});
```

### 中介者模式

中介者模式是指所有的相关对象都通过中介者对象来通信，而不是互相引用。所以当一个对象发生改变时，只需要通知中介者对象即可。

举个例子，通过中介者来计算排名：

```javascript
const A = {
  score: 10,
  changeTo(score) {
    this.score = score;
    // 自己获取排名
    this.getRank();
  },
  getRank() {
    let scores = [this.score, B.score, C.score].sort(function(a,b) {
      return a < b;
    })
    
    console.log(scores.indexOf(this.score) + 1);
  }
};

const B = {
  score: 20,
  changeTo(score) {
    this.score = score;
    // 通过中介者获取排名
    rankMediator(B);
  }
};

const C = {
  score: 30,
  changeTo(score) {
    this.score = score;
    // 通过中介者获取排名
    rankMediator(C);
  }
};

// 中介者，计算排名
function rankMediator(person) {
  let scores = [A.score, B.score, C.score].sort(function(a,b) {
    return a < b;
  });
  
  console.log(scores.indexOf(person.score) + 1);
}

// A通过自身处理来获取排名
A.changeTo(100);

// B和C则有中介者进行处理
B.changeTo(200);
C.changeTo(50);
```

## 总结

其实设计模式已经融入到我们开发的方方面面了，只不过我们没有注意到而已。本文讲述了一些常用的设计模式，还有很多的设计模式如命令模式、状态模式、修饰者模式等没有阐述，感兴趣的读者可以通过《JavaScript设计模式》自行学习。通过学习设计模式，我们可以在平时的开发中产出更简洁优雅的代码、更高可复用的代码和更高可维护性的代码。