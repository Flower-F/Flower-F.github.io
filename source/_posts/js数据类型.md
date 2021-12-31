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

# Symbol
## 出现的原因
ES5 的对象属性名都是字符串，这容易造成属性名的冲突。ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。Symbol 值通过Symbol() 函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。**凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。**

```js
let s = Symbol();
console.log(typeof s); // symbol
```

注意：**Symbol函数前不能使用new命令**，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。

## 描述
Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
```js
let s1 = Symbol('foo');
let s2 = Symbol('bar');

console.log(s1); // Symbol(foo)
console.log(s2); // Symbol(bar)

console.log(s1.toString()); // "Symbol(foo)"
console.log(s2.toString()); // "Symbol(bar)"
```

如果 Symbol 的参数是一个对象，就会调用该对象的 toString() 方法，将其转为字符串，然后才生成一个 Symbol 值。
```js
const obj = {
  name: 'Jack'
};
const s = Symbol(obj);
console.log(s); // Symbol([object Object])
```

```js
const obj = {
  name: 'Jack',
  toString() {
      return 'hello'
  }
};
const s = Symbol(obj);
console.log(s); // Symbol(hello)
```

注意，Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值也是不相等的。

```js
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

console.log(s1 === s2); // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

console.log(s1 === s2); // false
```

## 类型转换
Symbol 值不能与其他类型的值进行运算，否则会报错。

但是，Symbol 值可以显式转为字符串。
```js
let s = Symbol('My symbol');
console.log(String(s)); // 'Symbol(My symbol)'
console.log(s.toString()); // 'Symbol(My symbol)'
```

另外，Symbol 值也可以转为布尔值。
```js
let s = Symbol();
console.log(Boolean(s)); // true
console.log(!s);  // false
```

## 应用
Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于**一个对象由多个模块构成的情况**非常有用，能防止某一个键被不小心改写或覆盖。
```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};
```

注意，Symbol 值作为对象属性名时，不能用点运算符。
```js
const mySymbol = Symbol();
const a = {};

a.mySymbol = 'Hello!';
console.log(a[mySymbol]); // undefined
console.log(a['mySymbol']); // "Hello!"
```
上面代码中，因为点运算符后面总是字符串，所以不会读取mySymbol作为标识名所指代的那个值，导致a的属性名实际上是一个字符串，而不是一个 Symbol 值。

同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。

```js
let s = Symbol();
let obj = {
  [s]: function (arg) { ... }
};
obj[s](123);
```

## 遍历
Symbol 作为属性名，遍历对象的时候，该属性不会出现在 for...in、for...of 循环中，也不会被 Object.keys()、Object.getOwnPropertyNames()、JSON.stringify() 返回。
但是，它也不是私有属性，有一个 Object.getOwnPropertySymbols() 方法，可以获取指定对象的所有 Symbol 属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
```js
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

console.log(objectSymbols);
// [Symbol(a), Symbol(b)]
```

## Symbol.for()
有时，我们希望重新使用同一个 Symbol 值，Symbol.for()方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。比如，如果你调用 Symbol.for("cat") 30 次，每次都会返回同一个 Symbol 值，但是调用 Symbol("cat") 30 次，会返回 30 个不同的 Symbol 值。
```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

console.log(s1 === s2); // true
```

## Symbol.keyFor()
Symbol.keyFor()方法返回一个已登记的 Symbol 类型值的key。
```js
let s1 = Symbol.for("foo");
console.log(Symbol.keyFor(s1)); // "foo"

let s2 = Symbol("foo");
console.log(Symbol.keyFor(s2)); // undefined
```
上面代码中，变量s2属于未登记的 Symbol 值，所以返回 undefined。

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

# 为什么需要堆和栈两个存储空间
因为 JavaScript 引擎需要用栈来维护程序**执行上下文**的状态（调用栈），如果栈空间太大的话（即所有数据都存储在栈空间中），会影响上下文的切换效率，进而影响整个程序的执行效率，所以通常情况下栈空间不会设置太大，用于存储基本类型这样的小数据，而引用类型将存储到堆中，并且会分配一个内存地址。该内存地址会存储到栈空间。

# 包装对象
## 什么是包装对象
JS 的数值，布尔，字符串类型的变量，在一定条件下，也可以自动变成对象，这就是原始类型的包装对象。包装对象其实是一种特殊的引用类型，其与引用类型的主要区别在于**生命周期**：
1. 一般的引用类型在使用 new 创建其实例时，在执行流离开当前作用域之前一直都保存在内存中
2. 包装类型的对象只存在该行代码的执行瞬间，然后会**立即销毁**。（也意味着在运行时不能为基本类型添加属性和方法）

以字符串为例，来演示该流程：
```js
const str = 'abc';
const strNew = str.substring(0, 2);
```
在运行到 str.substring(0, 2) 的时候其实偷偷执行了以下三步：
```js
let strObj = new String(str);
const strNew = strObj.substring(0, 2);
strObj = null;
```

## 拓展
1. 包装对象和同样的原始类型的值相等吗？
不相等。因为包装对象是引用类型，原始类型是基本类型；包装对象的最大目的，首先是使得 JavaScript 的对象涵盖所有的值（万物皆对象的思想），其次使得原始类型的值可以方便地调用某些方法。
2. 如何给基本类型添加属性和方法？
在基本包装对象的原型上添加，每个对象都有原型。
3.同一个字符串调用两次相同的方法其包装对象相等吗？
不相等。调用结束后，包装对象实例会自动销毁。这意味着，下一次调用字符串的属性时，实际是调用一个新生成的对象，而不是上一次调用时生成的那个对象，这也说明了为什么不能直接给字符串、数字、布尔值添加属性和方法。

参考资料：
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures
https://es6.ruanyifeng.com/#docs/symbol
https://blog.csdn.net/weixin_42213025/article/details/105809018
https://www.cnblogs.com/green-jcx/p/9391720.html
公众号前端点线面