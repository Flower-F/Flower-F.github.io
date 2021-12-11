---
title: js 数据类型
date: 2021-12-11 20:37:50
tags: [javascript]
copyright: true
---
# 8 种数据类型
最新的 ECMAScript 标准定义了 8 种数据类型：
## 7 种基本类型
- Number
- String
- Boolean
- undefined
- null
- Symbol
- BigInt

## 1 种引用类型
- Object

# Boolean
布尔类型可以有两个值：true 和 false。

# Null
Null 类型只有一个值： null。

# Undefined
通过 var 声明的变量，在没有被赋值之前，变量会有个默认值 undefined。

# Number
Number 的范围：[-2^53, 2^53]。
特殊的 Number：Infinity, -Infinity, NaN。
数字类型中只有 0 一个整数有 2 种表示方法： 0 可表示为 -0 和 +0（0 是 +0 的简写）。 在实践中，这也几乎没有影响。 例如 +0 === -0 为真。 但是，在除以0的时候要注意：
```js
1 / +0; // Infinity
1 / -0; // -Infinity
-1 / -0; // Infinity
```

# BigInt
BigInt 表示任意精度的整数。使用 BigInt，可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。BigInt 是通过在整数末尾附加 n 或调用构造函数来创建的。
```js
const x = 2n ** 53n; // 9007199254740992n
const y = x + 1n; // 9007199254740993n
```
BigInt 不能与数字互换操作。否则，将抛出 TypeError。

# String
不同于类 C 语言，JavaScript 字符串是不可更改的。这意味着字符串一旦被创建，就不能被修改。
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/205644.jpg)
但是，可以基于对原始字符串的操作来创建新的字符串。例如：
- 获取一个字符串的子串可通过选择个别字母或者使用 String.substr()
- 两个字符串的连接使用连接操作符 +

# Object
在计算机科学中，对象是指内存中的可以被标识符引用的一块区域。在 JavaScript 里，对象可以被看作是一组属性的集合。一个 JavaScript 对象就是键和值之间的映射。**键是一个字符串（或者 Symbol）**，值可以是任意类型的值。
ECMAScript 定义的对象中有两种属性：数据属性和访问器属性。
## 数据类型
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/205941.jpg)
## 访问器属性
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/210049.jpg)

# null 和 undefined 的区别
1995 年 JavaScript 诞生时，最初像 Java 一样，只设置了 null 作为表示 “无” 的值。
根据 C 语言的传统，null 被设计成可以自动转为 0。
但是，JavaScript 的设计者 Brendan Eich，觉得这样做还不够，有两个原因：
- 首先，null 像在 Java 里一样，被当成一个对象。但是，JavaScript 的数据类型分成原始类型（primitive）和合成类型（complex）两大类，Brendan Eich 觉得表示 “无” 的值最好不是对象。
- 其次，JavaScript 的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich 觉得，如果 null 自动转为 0，很不容易发现错误。

因此，Brendan Eich 又设计了一个 undefined。

1、undefined 不是关键字，而 null 是关键字。这意味着 undefined 可以被重写，使用起来可能会不安全。
2、undefined 和 null 被转换为布尔值的时候，两者都为 false。
3、undefined 和 null 进行 == 比较时两者相等，进行 === 比较时两者不等。
4、使用 Number() 对 undefined 和 null 进行类型转换，undefined 是 NaN，null 是 0。