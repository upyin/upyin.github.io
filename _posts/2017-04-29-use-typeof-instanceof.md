---
layout: post
title: typeof与instanceof区别与使用
description: typeof与instanceof的区别与使用
tag: typeof、instanceof、undefined、null、boolean、string、number、object
category: 总结、技术
---
在讲typeof和instanceof之前，还是要先讲下JavaScript中的几种数据类型。JavaScript有5种简单的数据类型（基本类型）：Undefined、Null、Boolean、Number和String，还有一种复杂的数据类型－Object。

## Undefined类型

Undefined类型的值为undefined。当使用var声明变量但为对其初始化时，这个变量的值即为undefined：

```javascript
var a;
console.log(a == undefined);//true
```

## Null类型

Null类型的值为null。从逻辑角度看，null值表示一个空对象指针，因此，使用typeof检测null时会返回“object”：

```javascript
var a = null;
console.log(typeof a);//"object"
```

实际上，undefined值是派生自null值的，因此ECMA-262规定对它们的相等性测试要返回true

```javascript
console.log(undefined == null);//true
```

尽管null和undefined有这样的关系，但它们的用途完全不同。无论在什么情况下都没必要把一个变量的值显示的设置为undefined，可是同样的规则对null并不适用。

## Boolean类型

Boolean类型的值有两个，true和false。这两个值与数字值不是一回事，因此true不一定等于1，而false不一定等于0。可以调用类型转换函数(Boolean())讲任意类型的值转换为对应的Boolean类型的值：

| 数据类型  | 转换为true的值 | 转换为false的值 |
| --------- | -------------- | --------------- |
| Boolean   | true           | false           |
| String    | 任何非空字符串 | 空字符串        |
| Number    | 任何非0数字值  | 0和NaN          |
| Object    | 任何对象       | null            |
| Undefined | n/a（不适用）  | undefined       |

## Number类型

Number类型用来表示整数和浮点数值，以及NaN。在javascript中任何值除以0都会返回NaN而不会发生异常。

javascript中有一个isNaN()函数，这个函数接受一个参数，该参数可以是任何类型，而函数会返回这个值是否不为数值。即任何不能转换为数值的参数输入，都会返回true。

```javascript
console.log(isNaN(NaN));    //true
console.log(isNaN(10));	    //false
console.log(isNaN("10"));   //false
console.log(isNaN("blue")); //true
console.log(isNaN(true));   //false
```

Number()、parseInt()、parseFloat()，这三个函数可以把非数值，但是能转换为数值的数据类型转换为数值。其中，Number()函数对任何类型都可以使用，而parseInt()和parseFloat()只能传入字符串类型的参数。

#### Number()函数的转换规则：

- 1.如果是Boolean值，true和false将分别被替换为1和0
- 2.如果是数字值，只是简单的传入和返回
- 3.如果是null值，返回0
- 4.如果是undefined，返回NaN
- 5.如果是字符串，遵循下列规则：
- ·5.1如果字符串中只包含数字，则将其转换为十进制数值，前导的0会被忽略
- ·5.2如果字符串中包含有效的浮点格式，则将其转换为对应的浮点数
- ·5.3如果字符串中包含有效的十六进制格式，如“0xf”，则将其转换为相同大小的十进制整数值
- ·5.4如果字符串是空的，则将其转换为0
- ·5.5如果字符串中包含除了上述格式之外的字符，则将其转换为NaN
- 6.如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果为NaN，则调用对象的toString()方法，然后依照前面的规则转换返回的字符串

由于Number()函数在转换字符串时比较复杂而且不够合理，因此在处理整数的时候更常使用的是parseInt()函数。parseInt()函数在转换字符串时，更多的是看其是否符合数值模式。它会忽略字符串前面的空格，直至找到第一个非空字符。如果第一个字符不是数字字符或负号，parseInt()会返回NaN；也就是说，用parseInt()转换空字符串会返回NaN。如果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或遇到了一个非数字字符。如，“1234blue”会被转换为1234，“22.5”会被转换为22。

parseInt()能够将正确的十六进制或八进制的数字字符串转换为十进制的数值。

parseInt()有第二个参数，告诉函数，第一个参数，以第二个参数的标准转换为十进制的数值。

```javascript
parseInt("10",2);       //2(按二进制解析)
parseInt("10",8);       //8(按八进制解析)
parseInt("AF",16);      //175(按十六进制解析)
```

parseFloat()函数，将十进制的浮点数的字符串转换为数值型的浮点数。parseFloat()函数会忽略前导的0。

## String类型

String类型用于表示由零或多个16位Unicode字符组成的字符序列，即字符串。字符串可以由单引号或双引号表示。要把一个值转换为一个字符串有两种方式。第一种是使用toString()方法。

数值、布尔值、对象和字符串都有toString()方法，但null和undefined没有这个方法。其中数值在使用toString()方法时可以传递一个参数：输出数值的基数。

```javascript
var num = 10;
console.log(num.toString());        //"10"
console.log(num.toString(2));       //"1010"
console.log(num.toString(8));       //"12"
console.log(num.toString(10));      //"10"
console.log(num.toString(16));      //"a"
```

第二种方式，使用String()函数转换为字符串。这个函数能够将任何类型的值转换为字符串，包括null和undefined。String()函数对其他类型的转换同上面的toString()方法，而对于null，会转换为“null”，对于undefined会转换为“undefined”。

## Object类型

对象是一组键／值对的集合。对象可以通过new操作符来创建。

```javascript
var obj = new Object();
```

Object的每个实例都具有下列属性和方法：
- constructor —— 保存着用于创建当前对象的函数
- hasOwnProperty(propertyName) —— 用于检查给定的属性在当前对象实例中是否存在。其中，参数必须以字符串的形式指定
- isPrototypeOf(object) —— 用于检查传入的对象是否是另一个对象的原型
- propertyIsEnumerable(propertyName) —— 用于检查给定的属性是否能够使用for-in语句来枚举
- toString() —— 返回对象的字符串表示
- valueOf() —— 返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同

## typeof与instanceof使用

typeof用于获取一个变量或表达式的类型，typeof一般只返回以下的结果：
- number
- boolean
- string
- function
- object(null,数组,对象)
- undefined

因为对null、数组和对象使用typeof时，返回的都是object，因而需要判断某个变量是对象还是数组时就需要使用instanceof了。

instanceof用于判断一个变量是否为某个对象的实例。

```javascript
var a = new Array();
console.log(a instanceof Array);     //true
console.log(a instanceof Object);    //true
```

因为Array是Object的子类。那么如何来判断某个变量是对象还是数组呢？

```javascript
console.log(a instanceof Array);     //返回true则变量是数组，返回false则判断下一条
console.log(a instanceof Object);    //返回true则是对象不是数组，返回false则既不是对象也不是数组
```

