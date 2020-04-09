---
layout: post
title: JS数组操作方法解析
description: JS中数组的操作方法解析
tag: 数组、shift、push、slice、splice、sort、map
category: 技术、总结
---
数组是开发中很常使用的，且有很多的api，这里对数组的api进行一个总结归纳，方便查看和使用。

### join()

功能：将数组中的所有元素都转为字符串并拼接在一起。join可以传一个参数（字符串）用来定义拼接时的分隔符。

```javascript
var arr = [1,2,3];
var arrnew = arr.join(","); // 1,2,3
console.log(arr); // [1,2,3]
```

### toString()和toLocaleString()

功能：toString()方法和join()方法一样，用来拼接数组，但toString()方法只能用逗号来当做分隔符，不能自定义分隔符。toLocaleString()方法是将数组中的每个元素调用自身的toLocaleString()方法后得到的值，再按照逗号作为分隔符，进行拼接。

### concat()

功能：多个数组之间的拼接，返回新的数组，对原数组没有影响。

```javascript
var a1 = [1,2,3];
var a2 = [4,5,6];
var a3 = [7,8,9];
var anew = a1.concat(a2,a3); // [1,2,3,4,5,6,7,8,9]
```

### reverse()

功能：将数组中的元素进行倒序排序。会直接影响原数组。

```javascript
var a = [1,2,3];
a.reverse();
console.log(a); // [3,2,1]
```

### sort()

功能：对数组进行排序，并直接修改原数组。如果没有传入比较函数，默认会根据字母进行升序排序。如果数组元素不是字符串，则会调用toString()方法将元素转为字符串的unicode位点，然后根据字母进行排序。如果有传入比较函数，则会根据比较函数进行排序。

```javascript
var arr = ['A','C','B'];
arr.sort(); // ['A','B','C']
```

比较函数的两个参数：

sort的比较函数有两个默认参数，要在函数中接收这两个参数，这两个参数是数组中两个要比较的元素，通常我们用 a 和 b 接收两个将要比较的元素：

若比较函数返回值<0，那么a将排到b的前面;

若比较函数返回值=0，那么a 和 b 相对位置不变；

若比较函数返回值>0，那么b 排在a 将的前面；

```javascript
var arr = [10,1,5,3,6];
arr.sort(function(a,b){
  return a-b; // 升序
}); // [1,3,5,6,10]
arr.sort(function(a,b){
  return b-a; // 降序
}); // [10,6,5,3,1]
```

### slice()

功能：截取数组的长度，生成新的数组，对原数组不影响。slice()方法可以传0-2个参数，当不传参数的时候则直接返回数组本身；当传一个参数的时候，返回数组从该脚标开始到数组最后的一段数组；当传两个参数的时候，则返回数组在这两个脚标之间的一段数组。

```javascript
var arr = [1,2,3,4,5,6];
arr.slice(); // [1,2,3,4,5,6]
arr.slice(2); // [3,4,5,6]
arr.slice(1,4); // [2,3,4,5.6]
arr.slice(1,-2); // [2,3]
```

### splice()

功能：从数组中删除元素、插入元素或者替换元素，并返回被删除的元素数组。会直接影响原数组。splice()方法可以传0-多个参数，当不传参数的时候则直接返回该数组；当传一个参数的时候，则直接删除从该脚标开始的到最后的所有元素；当传两个参数的时候，则返回删除两个脚标之间的元素后的数组；当传多个元素的时候，则表示删除两个脚标之间的元素并在对应的位置重新插入后面的参数。

```javascript
var arr = [1,2,3,4,5,6];
// 下面的操作均当做arr=[1,2,3,4,5,6]重新赋值过
arr.splice(); // []   arr: [1,2,3,4,5,6]
arr.splice(1); // [2,3,4,5,6]   arr: [1]
arr.splice(1,3); // [2,3,4]   arr:[1,5,6]
arr.splice(1,3,8,9); // [2,3,4]   arr:[1,8,9,5,6]
```

### push()

功能：在数组的末尾添加一个或多个元素，并返回新数组长度。会直接修改原数组。

```javascript
var arr = [];
arr.push(1,2,3); // 3
console.log(arr); // [1,2,3]
```

### pop()

功能：从数组的末尾删除1个元素，并返回被删除的元素。会直接修改原数组。

```javascript
var arr = [1,2,3,4];
arr.pop(); // 4
console.log(arr); // [1,2,3]
```

### shift()

功能：删除数组的第一个元素，并返回被删除的元素。会直接修改原数组。

```javascript
var arr = [1,2,3,4];
arr.shift(); // 1
console.log(arr); // [2,3,4]
```

### unshift()

功能：向数组的开头插入一个或多个元素，并返回新的长度。会直接修改原数组。

```javascript
var arr = [3,4,5];
arr.unshift(1,2); // 5
console.log(arr); // [1,2,3,4,5]
```

### indexOf()和lastIndexOf()

功能：indexOf()方法用来查找数组中可以找到的第一个元素的索引，如果不存在则返回-1。lastIndexOf()方法用来查找数组中可以找到的最后一个元素的索引，如果不存在则返回-1。

```javascript
var arr = ['a','b','c','d'];
arr.indexOf('a'); // 0
arr.indexOf('a', 1); // -1
arr.lastIndexOf('d'); // 3
arr.lastIndexOf('d', 1); // -1
```

indexOf()的第二个参数，表示正序从该脚标开始向后查找，默认值-1。lastIndexOf()的第二个参数，表示逆序从第n个位置开始向前查找，默认值-1。

### forEach()

功能：遍历整个数组，为每个元素调用指定的函数。

```javascript
var arr = [1,2,3];
arr.forEach(function(key, value){
  console.log(key); // 数组脚标
  console.log(value); // 数组元素
});
```

### map()

功能：遍历数组的每一个元素并传递给指定的函数，返回一个新数组，对原数组没有影响。

```javascript
var arr = [1,2,3,4];
var arrnew = arr.map(function(x){
  return x+1;
});
console.log(arrnew); // [2,3,4,5]
```

### filter()

功能：对数组中的每一个元素执行给定的函数进行判断，返回满足过滤条件的元素所组成的数组，不会影响原数组。

```javascript
var arr = [1,2,2,3,3,4,5];
var arrnew = arr.filter(function(value, index, self){
  return self.indexOf(value) == i;
});
console.log(arrnew); // [1,2,3,4,5]
```

上面这个例子是通过filter()方法，巧妙的进行数组去重。

### every()和some()

功能：every()方法是判断数组中每一个元素是否都满足条件，只有所有元素都满足条件，才会返回true。some()方法是判断数组中是否存在满足条件的元素，只要有一个元素满足条件，就会返回true。

```javascript
var arr = [1,2,3,4,5];
arr.every(function(value, index, self){
  return value>0;
}); // true
arr.every(function(value){
  return value>3;
}); // false
arr.some(function(value, index, self){
  return value>3;
}); // true
```

every()和some()的语法：

```bash
array.every(function(currentValue, index, arr), thisValue)
array.some(function(currentValue, index, arr), thisValue)

function(必须): 数组中每个元素需要调用的函数。

// 回调函数的参数
1. currentValue(必须),数组当前元素的值
2. index(可选), 当前元素的索引值
3. arr(可选),数组对象本身

thisValue(可选): 当执行回调函数时this绑定对象的值，默认值为undefined
```

### reduce()和reduceRight()

功能：reduce()方法对累加器和数组中的每个元素（从前向后）应用一个函数，最终合并为一个值。reduceRight()方法与reduce()方法一样，不同之处在于执行方向相反（从后向前）。

```javascript
var arr = [1,2,3,4,5];
arr.reduce(function(a,b){
  return a+b;
}); // 15
```

reduce()和reduceRight()的语法：

```bash
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
array.reduceRight(function(total, currentValue, currentIndex, arr), initialValue)

function(必须): 数组中每个元素需要调用的函数。

// 回调函数的参数
1. total(必须)，初始值, 或者上一次调用回调返回的值
2. currentValue(必须),数组当前元素的值
3. index(可选), 当前元素的索引值
4. arr(可选),数组对象本身

initialValue(可选): 指定第一次回调 的第一个参数。
回调第一次执行时:
1.如果 initialValue 在调用 reduce 时被提供，那么第一个 total 将等于 initialValue，此时 currentValue 等于数组中的第一个值；
2.如果 initialValue 未被提供，那么 total 等于数组中的第一个值，currentValue 等于数组中的第二个值。此时如果数组为空，那么将抛出 TypeError。
3.如果数组仅有一个元素，并且没有提供 initialValue，或提供了 initialValue 但数组为空，那么回调不会被执行，数组的唯一值将被返回。
```

### Array.of()

功能：of()方法是es6提供的数组方法，它返回由所有参数值组成的数组，如果没有参数，就返回一个空数组。

```javascript
var a = Array.of(1,2,3); // [1,2,3]
var b = Array.of(1); // [1]
```

### Array.from()

功能：from()方法是es6提供的数组方法，用于将两类对象转为真正的数组。不改变原对象，返回新的数组。

```javascript
var obj = {0:'a',1:'b',2:'c',length:3};
var arr = Array.from(obj); // ['a','b','c']
var arr2 = Array.from('hello'); // ['h','e','l','l','o']
var arr3 = Array.from(new Set(['a','b'])); // ['a','b']
```

from()方法的语法：

```bash
参数：
第一个参数(必需):要转化为真正数组的对象。
第二个参数(可选): 类似数组的map方法，对每个元素进行处理，将处理后的值放入返回的数组。
第三个参数(可选): 用来绑定this。
```

### copyWithin()

功能：copyWithin()方法是es6提供的数组方法。功能是在当前数组内部，将指定位置的成员复制到其他位置,并返回这个数组。调用该方法会直接修改原数组。

```javascript
var arr = ['OB1','Koro1','OB2','Koro2','OB3','Koro3','OB4','Koro4','OB5','Koro5'];
// 2位置开始被替换,3位置开始读取要替换的 5位置前面停止替换
arr.copyWithin(2,3,5); //["OB1","Koro1","Koro2","OB3","OB3","Koro3","OB4","Koro4","OB5","Koro5"]
```

copyWithin()方法的语法：

```bash
array.copyWithin(target, start = 0, end = this.length)

三个参数都是数值，如果不是，会自动转为数值.
target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
end（可选）：到该位置前停止读取数据，默认等于数组长度。使用负数可从数组结尾处规定位置。

浏览器兼容(MDN): chrome 45,Edge 12,Firefox32,Opera 32,Safari 9, IE 不支持
```

### fill()

功能：fill()方法是es6提供的数组方法。功能是使用给定值，填充一个数组。调用该方法会直接修改原数组。

```javascript
var arr = ['a','b','c'];
arr.fill(7); // [7, 7, 7]

var arr2 = ['a', 'b', 'c'];
arr2.fill(7, 1, 2); // ['a', 7, 'c']
```

fill()方法的语法：

```bash
参数:
第一个参数(必须): 要填充数组的值
第二个参数(可选): 填充的开始位置,默认值为0
第三个参数(可选)：填充的结束位置，默认是为this.length
```

### find()和findIndex()

功能：find()方法用于找出第一个符合条件的数组成员，并返回该成员，如果没有符合条件的成员，则返回undefined。findIndex()方法返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。这两个方法都可以识别NaN,弥补了indexOf的不足。

```javascript
var arr = [1,2,3,4,-1];
arr.find(function(x){
  return x < 0;
}); // -1

var arr = [1,2,3,4,-1];
arr.findIndex(function(x){
  return x < 0;
}); // 4
```

find()和findIndex()方法的语法：

```bash
arr.find(function(currentValue, index, arr), thisArg)
arr.findIndex(function(currentValue, index, arr), thisArg)

function(必须): 数组中每个元素需要调用的函数。

// 回调函数的参数
1. currentValue(必须),数组当前元素的值
2. index(可选), 当前元素的索引值
3. arr(可选),数组对象本身

thisValue(可选): 当执行回调函数时this绑定对象的值，默认值为undefined

浏览器兼容(MDN):Chrome 45,Firefox 25,Opera 32, Safari 8, Edge yes,
```

### keys()、values()和entries()

功能：这三个方法是es6提供的数组方法。三个方法都返回一个新的 Array Iterator 对象，对象根据方法不同包含不同的值。

```javascript
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}
// 0
// 1
 
for (let elem of ['a', 'b'].values()) {
    console.log(elem);
}
// 'a'
// 'b'
 
for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

注意事项：

在for..of中如果遍历中途要退出，可以使用break退出循环。

如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历:

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

兼容性：

entries()浏览器兼容性(MDN):Chrome 38, Firefox 28,Opera 25,Safari 7.1

keys()浏览器兼容性(MDN):Chrome 38, Firefox 28,Opera 25,Safari 8

### includes()

功能：includes()方法是es7提供的数组方法。功能是返回一个布尔值，表示某个数组是否包含给定的值。

```javascript
let arr = ['OB','Koro1',1,NaN];
// let b = arr.includes(NaN); // true 识别NaN
// let b = arr.includes('Koro1',100); // false 超过数组长度 不搜索
// let b = arr.includes('Koro1',-3); // true 从倒数第三个元素开始搜索 
// let b = arr.includes('Koro1',-100); // true 负值绝对值超过数组长度，搜索整个数组
```

includes()方法的语法：

```bash
array.includes(searchElement,fromIndex=0)

参数：
searchElement(必须):被查找的元素
fromIndex(可选):默认值为0，参数表示搜索的起始位置，接受负值。正值超过数组长度，数组不会被搜索，返回false。负值绝对值超过长数组度，重置从0开始搜索。

兼容性(MDN): chrome47, Firefox 43,Edge 14,Opera 34, Safari 9,IE 未实现
```

includes方法是为了弥补indexOf方法的缺陷而出现的：indexOf方法不能识别NaN；indexOf方法检查是否包含某个值不够语义化，需要判断是否不等于-1，表达不够直观。

