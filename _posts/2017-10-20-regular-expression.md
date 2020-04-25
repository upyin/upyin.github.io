---
layout: post
title: JavaScript正则表达式
description: JavaScript正则表达式介绍和使用方法
tag: replace、search、RegExp
category: 技术、总结
---
正则表达式是由一个字符序列形成的搜索模式。当你在文本中搜索数据时，你可以用搜索模式来描述你要查询的内容。正则表达式可以是一个简单的字符或一个复杂的模式，可以用于所有文本搜索和文本替换操作。

## 语法

正则表达式的语法：** /正则表达式主体/修饰符**。其中修饰符是可选的。

## JavaScript方法

在JavaScript中正则的使用有两种方式，一种是使用字符串的方法search()和replace()，一种是使用RegExp对象方法中的test()和exec()。

### String.search()

用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。

```javascript
var str = "regular expression";
str.search("gular"); // 2
str.search(/gular/i); // 2
```

### String.match()

可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。match() 方法将检索字符串 String Object，以找到一个或多个与 regexp 匹配的文本。这个方法的行为在很大程度上有赖于 regexp 是否具有标志 g。如果 regexp 没有标志 g，那么 match() 方法就只能在 stringObject 中执行一次匹配。如果没有找到任何匹配的文本， match() 将返回 null。否则，它将返回一个数组，其中存放了与它找到的匹配文本有关的信息。

```javascript
var str = "hello world he say";
str.match(/he/g); // he,he
```

### String.replace()

用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

```javascript
var str = "regular expression";
str.replace("regular", "Regular"); // Regular expression
str.replace(/regular/g, "Regular"); // Regular expression
```

### RegExp.test()

用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false。

```javascript
var patt = /regular/;
patt.test("regular expression"); // true
```

### RegExp.exec()

用于检索字符串中的正则表达式的匹配。该函数返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

```javascript
var patt = /regular/i;
patt.exec("regular expression"); // regular
```

## 正则表达式

| 修饰符 | 说明                                                     |
| ------ | -------------------------------------------------------- |
| i      | 执行对大小写不敏感的匹配。                               |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m      | 执行多行匹配。                                           |

| 表达式 | 说明                               |
| ------ | ---------------------------------- |
| [abc]  | 查找方括号之间的任何字符。         |
| [^abc] | 查找任何不在方括号之间的字符。     |
| [0-9]  | 查找任何从 0 至 9 的数字。         |
| [A-Z]  | 查找任何从大写 A 到大写 Z 的字符。 |
| (x\|y) | 查找任何以\|分隔得选项。           |

| 元字符 | 说明                                        |
| ------ | ------------------------------------------- |
| \d     | 查找数字。                                  |
| \D     | 查找非数字字符。                            |
| \b     | 匹配单词边界。                              |
| \s     | 查找空白字符。                              |
| \w     | 查找单词字符。                              |
| \n     | 查找换行符。                                |
| \r     | 查找回车符。                                |
| \t     | 查找制表符。                                |
| \uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符。 |

| 量词   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| n+     | 匹配任何包含至少一个n的字符串。                              |
| n*     | 匹配任何包含零个或多个n的字符串。                            |
| n?     | 匹配任何包含零个或一个n的字符串。                            |
| n{X}   | 匹配包含 X 个 n 的序列的字符串。                             |
| n{X,Y} | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配。 |
| n$     | 匹配任何结尾为 n 的字符串。                                  |
| ^n     | 匹配任何开头为 n 的字符串。                                  |
| ?=n    | 匹配任何其后紧接指定字符串 n 的字符串。                      |
| ?!n    | 匹配任何其后没有紧接指定字符串 n 的字符串。                  |

