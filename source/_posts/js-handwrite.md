---
title: js 常见知识复习
date: 2022-02-11 22:02:59
tags: [javascript, 手写]
copyright: true
---

题目列表来源于：
https://bigfrontend.dev/

# 参数定长柯里化

```js
sum(1)(2)(3)(4)	// 10
sum(1, 2)(3)(4)	// 10
sum(1)(2, 3)(4)	// 10
```

实现一个函数同时可以求解上述表达式。

先要实现原函数

```js
function sum(a, b, c, d) {
  return a + b + c + d;
}
```

将其进行柯里化，目标是将它的参数**展开**，所以柯里化的过程实际上可以理解为一个递归展开参数的过程。展开的便捷就是当前参数个数等于函数需要的参数，不过为了确保完整性把 === 改成了 >= 而已。

```js
function curry(fn, ...args) {
  // fn.length 表示的是 fn 需要的参数个数
  // 当前函数的参数个数大于或等于原函数时，直接执行
  console.log("args", args);
  if (args.length >= fn.length) {
    return fn(...args);
  }
  // args2 是即将出现的参数
  return function (...args2) {
    console.log("args2", args2);
    return curry(fn, ...args, ...args2);
  };
}

// (1)(2, 3)
// args []，刚开始参数个数为零
// args2 [ 1 ]，即将出现参数 1
// args [ 1 ]，拼接后获得参数 1
// args2 [ 2, 3 ]，即将出现参数 [2, 3]
// args [ 1, 2, 3 ]，拼接后获得参数 [1, 2, 3]，完成任务

// (1, 2)(3)
// args []
// args2 [ 1, 2 ]
// args [ 1, 2 ]
// args2 [ 3 ]
// args [ 1, 2, 3 ]

const join = (a, b, c) => {
  return `${a}_${b}_${c}`;
};

const curriedJoin = curry(join);

curriedJoin(1, 2, 3); // '1_2_3'
curriedJoin(1)(2, 3); // '1_2_3'
curriedJoin(1, 2)(3); // '1_2_3'
```

# 参数不定长柯里化

```js
function curry() {
  // 接收第一次参数
  const args = [...arguments];

  // 接收第二次参数
  const inner = function() {
    args.push(...arguments);
    // 递归获取剩下的所有参数
    return inner;
  }

  inner.toString = function() {
    return args.reduce((acc, cur) => acc + cur);
  }

  return inner;
}
```

# 实现 Array.prototype.flat()

```js
function flat(arr, depth = 1) {
  return depth > 0 ?
    arr.reduce((prev, cur) => {
      if (Array.isArray(cur)) {
        return [...prev, ...flat(cur, depth - 1)];
      }
      return [...prev, cur];
    }, []) : arr;
}
```

# 实现 throttle()

```js
function throttle(fn, delay = 200) {
  let timer = null;
  return function(...args) {
    if (!timer) {
      timer = setTimeout(() => {
        fn.apply(this, args);
        timer = null;
    }, delay);
  }
}
```

# 实现 debounce()

```js
function debounce(fn, delay = 200) {
  let timer = null;
  return function(...args) {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  }
}
```

# 实现 shuffle()

Fisher-Yates shuffle 算法

每次删除一个数字，并将删除的数字移至数组末尾，即将每个被删除数字与最后一个未删除的数字进行交换。

```js
function shuffle(arr) {
  let i = arr.length;

  for (let i = arr.length; i >= 0; i--) {
    const j = Math.floor(Math.random() * i);
    [arr[i], arr[j]] = [arr[j], arr[i]]
  }

  return arr;
}
```

# 解密消息

在一个字符串的二维数组中，有一个隐藏字符串。

```
I B C A L K A
D R F C A E A
G H O E L A D
```

可以按照如下步骤找出隐藏消息

1. 从左上开始，向右下前进
2. 无法前进的时候，向右上前进
3. 无法前进的时候，向右下前进
4. 重复 2 和 3
5. 无法前进的时候，经过的字符就就是隐藏信息

比如上面的二维数组的话，隐藏消息就是 IROCLED

```js
/**
 * @param {string[][]} message
 * @return {string}
 */
function decode(message) {
  if (message.length === 0 || message[0].length === 0) {
    return '';
  }
  let res = '';
  let i = 0, j = 0;
  let top = true; // 表示目前正在向 i 增大方向行进，对应图中的向下行进

  while (j < message[0].length) {
    if (top) {
      res += message[i++][j++];
    } else {
      res += message[i--][j++];
    }
    if (i === message.length - 1) {
      top = false;
    }
    if (i === 0) {
      top = true;
    }
  }

  return res;
}
```

# 第一个错误的版本

题目链接：
https://leetcode-cn.com/problems/first-bad-version/

二分基础题

```js
/**
 * Definition for isBadVersion()
 * 
 * @param {integer} version number
 * @return {boolean} whether the version is bad
 * isBadVersion = function(version) {
 *     ...
 * };
 */

/**
 * @param {function} isBadVersion()
 * @return {function}
 */
var solution = function(isBadVersion) {
    /**
     * @param {integer} n Total versions
     * @return {integer} The first bad version
     */
    return function(n) {
      let l = 1, r = n;
      let ans = 1;
      while (l <= r) {
        const mid = l + ((r - l) >> 1);
        if (isBadVersion(mid)) {
          // 满足条件，寻找最左满足，因此收缩右边界
          r = mid - 1;
          ans = mid;
        } else {
          l = mid + 1;
        }
      }    

      return ans;
    };
};
```

# 实现 pipe()

`pipe` 会传入一个数组，数组中每一项是一个函数。`pipe` 会依次执行里面的多个函数，最后返回结果。假设每个函数都有且仅有一个参数。如下所示：

```
pipe([
  times(2),
  times(3)
])  
// x * 2 * 3

pipe([
  times(2),
  plus(3),
  times(4)
]) 
// (x * 2 + 3) * 4

pipe([
  times(2),
  subtract(3),
  divide(4)
]) 
// (x * 2 - 3) / 4
```

```js
/**
 * @param {Array<(arg: any) => any>} funcs 
 * @return {(arg: any) => any}
 */
function pipe(funcs) {
	return function (arg) {
		return funcs.reduce((acc, func) => {
			return func.call(this, acc);
		}, arg);
	}
}
```

# 栈实现队列

若 stack2 为空，则直接输入栈顶元素；否则，先把 stack1 的所有元素倒进 stack2 中。

```js
/* you can use this Class which is bundled together with your code

class Stack {
  push(element) { // add element to stack }
  peek() { // get the top element }
  pop() { // remove the top element}
  size() { // count of element }
}
*/

class Queue {
  constructor() {
    this.stack1 = [];
    this.stack2 = [];
  }
  enqueue(element) {
    // add new element to the rare
    this.stack1.push(element);
  }
  peek() {
    // get the head element
    if (this.stack2.length) {
      return this.stack2[this.stack2.length - 1];
    }
    while (this.stack1.length) {
      this.stack2.push(this.stack1.pop());
    }
    return this.stack2[this.stack2.length - 1];
  }
  size() {
    // return count of element
    return this.stack1.length + this.stack2.length;
  }
  dequeue() {
    // remove the head element
    if (this.stack2.length) {
      return this.stack2.pop();
    }
    while (this.stack1.length) {
      this.stack2.push(this.stack1.pop());
    }
    return this.stack2.pop();
  }
}
```

# 实现 memo()

对同一个函数，当传入相同参数的时候，直接返回上一次的结果而不经过计算。要求允许传入第二个参数决定缓存 key 的生成方式。

```js
/**
 * @param {Function} func
 * @param {(args:[]) => string }  [resolver] - cache key generator
 */
function memo(func, resolver = (...args) => args.join('_')) {
  const cache = new Map();

  return function(...args) {
    const key = resolver(...args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const val = func.apply(this, args);
    cache.set(key, val);
    return val;
  }
}
```

# 实现类似 jQuery 的 DOM wrapper

实现自己的 `$()`，只需要支持 `css(propertyName: string, value: any)` 即可。如下面所示：

```js
$('#button')
  .css('color', '#fff')
  .css('backgroundColor', '#000')
  .css('fontWeight', 'bold')
```

```js
function $(elem) {
  return {
    css: function(property, value) {
      elem.style[property] = value;
      // 最后返回原对象，因为需要支持链式调用
      return this;
    }
  }
}
```

# 实现基本的 Event Emitter

要求满足的条件为：

构造函数

```js
const emitter = new Emitter()
```

支持事件订阅

```js
const sub1  = emitter.subscribe('event1', callback1)
const sub2 = emitter.subscribe('event2', callback2)

// 同一个callback可以重复订阅同一个事件
const sub3 = emitter.subscribe('event1', callback1)
```

`emit(eventName, ...args)` 可以用来触发 callback

```js
emitter.emit('event1', 1, 2);
// callback1 会被调用两次
```

`subscribe()` 返回一个含有 `release()` 的对象，可以用来取消订阅。

```js
sub1.release()
sub3.release()
// 现在即使'event1'被触发, 
// callback1 也不会被调用
```

```js
class EventEmitter {
  constructor() {
    this.subscriptions = new Map();
  }

  subscribe(eventName, callback) {
    if(!this.subscriptions.has(eventName)) {
      this.subscriptions.set(eventName, new Set());
    }
    // 获取具体的事件列表
    const subscriptions = this.subscriptions.get(eventName);
    // 这里之所以要把 callback 放进一个 obj 里面，是因为题目允许重复订阅同一个事件
    // 以 obj 作为键值，不会重复
    const callbackObj = {
      callback
    }
    subscriptions.add(callbackObj);

    return {
      // 取消订阅
      release: () => {
        subscriptions.delete(callbackObj);
        if (subscriptions.size === 0) {
          this.subscriptions.delete(eventName);
        }
      }
    }
  }
  
  emit(eventName, ...args) {
  	const subscriptions = this.subscriptions.get(eventName);
    if (subscriptions) {
      subscriptions.forEach(callbackObj => {
        callbackObj.callback.apply(this, args);
      })
    }
  }
}
```

# 模拟 Map

JavaScript中有 Map，我们可以用任何 data 做 key，包括 DOM 元素。如下所示：

```js
const map = new Map()
map.set(domNode, somedata)
```

如果运行的 JavaScript 不支持 Map，我们如何能让上述代码工作？

方法是使用对象来模拟一个 map。

```js
class NodeStore {
  constructor() {
    this.nodes = {};
  }
   /**
   * @param {Node} node
   * @param {any} value
   */
  set(node, value) {
   node.storeKey = Symbol();
   this.nodes[node.storeKey] = value;
  }
  /**
   * @param {Node} node
   * @return {any}
   */
  get(node) {
   return this.nodes[node.storeKey];
  }
  
  /**
   * @param {Node} node
   * @return {Boolean}
   */
  has(node) {
    return this.nodes.hasOwnProperty(node.storeKey);
  }
}
```

# 在相同结构的树上寻找对应的节点

给定两个**完全一样**的 DOM Tree A 和 B，以及 A 中的元素 a，请找到 B 中对应的元素 b。

```js

/**
 * @param {HTMLElement} rootA
 * @param {HTMLElement} rootB - rootA and rootB are clone of each other
 * @param {HTMLElement} nodeA
 */
const findCorrespondingNode = (rootA, rootB, target) => {
  if (rootA === target) {
    return rootB;
  }

  // children 是一个 DOM 的 api，表示所有的子节点
  for (let i = 0; i < rootA.children.length; i++) {
    const res = findCorrespondingNode(rootA.children[i], rootB.children[i], target);
    // 注意这里必须要判断 res 是否存在
    if (res) {
      return res;
    }
  }
}
```

# 检测 data type

```js
/**
 * @param {any} data
 * @return {string}
 */
function detectType(data) {
  const res = Object.prototype.toString.call(data).slice(8, -1);
  return res.toLowerCase();
}
```

# 实现 JSON.stringify()

```js

/**
 * @param {any} data
 * @return {string}
 */
function stringify(data) {
  const type = typeof data;

  const undefinedOptions = ['undefined', 'function', 'symbol'];
  const stringOptions = ['number', 'boolean'];

  // 处理 undefined
  if (undefinedOptions.includes(type)) {
    return undefined;
  }

  // 处理 null
  if (Number.isNaN(data) || data === Infinity || data === -Infinity || data === null) {
    return "null";
  }

  // 处理 number 和 boolean
  if (stringOptions.includes(type)) {
    return `${data}`;
  }

  // 处理字符串
  if (type === 'string') {
    return `"${data}"`
  }

  // 错误处理
  if (typeof data === 'bigint') {
    throw new Error('stringify 无法序列化 bigint 数据类型');
  }

  // 剩下的就是 object 类型

  // 若存在 toJSON 函数，如 Date()，直接调用
  if (data.toJSON && typeof data.toJSON === 'function') {
    return stringify(data.toJSON());
  }

  // 处理数组
  if (Array.isArray(data)) {
    const result = [];
    data.forEach((item, index) => {
      // undefined、function 以及 symbol 变为 null
      if (undefinedOptions.includes(typeof item)) {
        result[index] = 'null';
      } else {
        result[index] = stringify(item);
      }
    })
    return "[" + result + "]";
  }

  // 处理普通对象
  const result = [];
  Object.keys(data).forEach((key, index) => {
    // 键值不能为 symbol
    // 值忽略 undefined、function 以及 symbol
    if(typeof key !== 'symbol' && !undefinedOptions.includes(typeof data[key])) {
      result.push(`"${key}":${stringify(data[key])}`);
    }
  })
  return `{${result.join(',')}}`;
}
```

# 实现 JSON.parse()

```js
function parse(str) {
  if (str === '' || str[0] === "'") {
    throw new Error();
  }
  // 特殊情况
  if (str === 'null') return null;
  if (str === '{}') return {};
  if (str === '[]') return [];
  // 判断 boolean
  if (str === 'true') return true;
  if (str === 'false') return false;
  // 判断 number
  if(+str === +str) return Number(str);
  // 判断 string
  if(str[0] === '"') {
    return str.slice(1, -1);
  }
  // 判断对象
  if (str[0] === '{') {
    return str.slice(1, -1).split(',').reduce((acc, cur) => {
      const index = cur.indexOf(':');
      const key = cur.slice(0, index);
      const value = cur.slice(index + 1);
      acc[parse(key)] = parse(value);
      return acc;
    }, {})
  }
  // 判断数组
  if (str[0] === '[') {
    return str.slice(1, -1).split(',').map((value) => parse(value));
  }
}
```

