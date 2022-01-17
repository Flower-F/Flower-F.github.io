---
title: 字节前端青训营第 2 天
date: 2022-01-16 14:04:55
tags: [javascript]
copyright: true
---
经过了昨天还算是友好的前菜，今天突然猛地来了一顿硬菜，让人有点措手不及（或者应该说是消化不良？）。不过想想这不就是我们来青训营的目的吗？见识以前可能只是听说过甚至完全没有听说过的知识，学习更先进的编程思维。总之，今天完全被月影老师圈粉了，甚至萌生了去极客买买买的想法（x

开篇，谨以此图表达我对 JavaScript 的热爱
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220116140922.png)

# 如何写好 JavaScript

## 各司其职

举个例子，写一段 js，控制一个网页支持浅色和深色两种浏览模式，如何实现？
很容易想到的一种实现方法是，加一个切换按钮，然后给该按钮添加事件，如下所示：
```js
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', e => {
    const body = document.body;
    if (e.target.innerHTML === 'light') {
        body.style.backgroundColor = 'black';
        body.style.color = 'white';
        e.target.innerHTML = 'dark';
    } else {
        body.style.backgroundColor = 'white';
        body.style.color = 'black';
        e.target.innerHTML = 'light';
    }
})
```
这段代码的问题所在：直接使用 js 操作了样式，没有做到**各司其职**。

修改版本一：
```js
const btn = document.getElementById('modeBtn');
btn.addEventListener('click', e => {
    const body = document.body;
    if (body.className !== 'night') {
        body.className = 'night';
    } else {
        body.className = '';
    }
})
```
通过添加或删除类名，实现状态切换，做到**各司其职**，使得代码结构得到优化。

修改版本二：完全使用 CSS 实现
这个主要是利用 CSS 的 checkbox 的 `checked` 伪类实现的。

```css
#modeCheckBox:checked + .content {
  background-color: black;
  color: white;
}
```

推荐一个利用了这个原理实现明暗主题变换的 React 项目：[戳这](https://github.com/ShaifArfan/artistic)

## 组件封装

组件设计的原则：
- 封装性
- 正确性
- 拓展性
- 复用性

如何使用原生 JavaScript 实现一个电商网站的轮播图？

*组件设计比较复杂的点往往在于控制的地方和组件本身有一定的耦合。 ——月影*

比如说当前轮播图的第几个小圆点标红，就是显示第几张图片，它们之间的状态是耦合的。我们不希望它们之间关系绑的太死，所以我们一般会使用自定义事件。

详细的实现见[青训营社区](https://forum.juejin.cn/youthcamp/post/7052254063840198686)

**划重点**，原文中最后说到：
这个轮播组件实现了封装性和正确性，但是缺少了可扩展性。这个组件只能满足自身的使用，它的实现代码很难扩展到其他的组件，当有功能变化时，也需要修改其自身内部的代码。
比如产品经理因为某种原因，希望将图片下方的小圆点暂时去掉，只保留左右箭头。那么在这个版本中，就需要这么做：
- 注释掉 html 中 `.slider__control` 相关的代码
- 修改 Slider 组件，注释掉与小圆点控制相关的代码
- 又或者，将来需要为这个组件添加新的用户控制，都需要对这个组件进行再修改

这样的修改常常涉及了**核心代码**的更改。那么，如何可以避免这样的修改，让组件具备可扩展性和复用性呢？

*在 JavaScript 代码中，一个方法一般来说最多只能有 15 行代码，超过了就需要重构。 ——月影*

### 解耦
#### 如何解耦
- 将控制元素（如小圆点）抽取成插件
- 插件与组件之间通过**依赖注入**的方式建立联系

#### 插件化
然后月影老师给了一个无敌的使用依赖注入实现的轮播图。核心要点是通过一个 `registerPlugins` 方法来完成插件的注册，然后分别实现三个插件（Controller、Next、Previous）。通过这样的重构，如果哪天 PM 要我们把小圆点删除掉，我们直接把小圆点对应的代码删掉就行了，不需要修改组件的核心代码。

#### 依赖注入
为什么需要依赖注入？
**插件的初始化依赖于组件。我们不希望组件知道插件的存在，但是插件需要知道组件的存在。**

#### 模板化
现在插件独立了，如果要删除小圆点，我们现在可以直接把对应插件的 JavaScript 代码删除掉，但是问题是我们还需要删掉 html 的对应代码，这样也是需要修改多个地方。因为我们还需要把 html 模板化。方法就是在 js 代码中实现一个 `render` 方法，来进行模板化的渲染。渲染方法里面可以加入一些参数，比如说有多少张图片，然后组件需要根据传入的参数进行渲染。另外插件也需要实现自己的 `render` 方法，来实现插件的 html 模板化；还有一个 `action` 函数，作用是根据组件依赖注入的实例来初始化插件数据。

#### 抽象化
进一步修改实现的思想为：
实现一个 `Component` 类，里面有一个 `render` 方法。`render` 方法是一个抽象的方法，然后轮播图组件和各插件继承于它，再实现各自的 `render` 函数。也就是说，所有的元素都是组件（没有插件），小圆点也是组件、图片展示也是组件，最后整个的轮播图组件则是由多个小的组件组合而成。

## 过程抽象
![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220116201358.png)
什么是过程抽象呢？举个例子，比如你有间小房子，房子有门、有窗，这里门和窗也就是数据。那么你可以开门，也可以开窗，开门和开窗就属于过程。我们不仅仅可以抽象数据，还可以抽象这个过程。

举个例子：
```html
<ul>
    <li>任务一：学习 html</li>
    <li>任务二：学习 css</li>
    <li>任务三：学习 JavaScript</li>
</ul>

<script>
const ulElem = document.querySelector('ul');
const liElems = ulElem.querySelectorAll('li');

liElems.forEach(liElem => {
    liElem.addEventListener('click', e => {
        e.target.className = 'completed';
        setTimeout(() => {
            ulElem.removeChild(e.target);
        }, 2000);
    });
});
</script>
```

假设我们现在有三个任务需要完成，我们完成了其中一个所以要点击删除它。现在点击其中一个，他将会在 2s 之后完成删除，但是假如我们在这过程中再次点击这一项，就会出现报错：

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220116202742.png)

报错的原因也很明显，点击的时候这一项已经被删除了，当然也就没有了，也无法执行删除操作。

这时候我们可以封装一个高阶函数 `once`，这个函数的目标就是限制删除只能进行一次，确保操作的安全性。

```js
function once(fn) {
  return function (...args) {
        if (fn) {
            const result = fn.apply(this, args);
            fn = null;
            return result;
        }
    }
}
```

观察这个函数的特征，它就是一个**高阶函数**。

### 高阶函数
- 以函数作为参数
- 以函数作为返回值
- 经常用于作为函数装饰器

常见的高阶函数还有：
- 防抖和节流
- consumer
- iterative

#### 防抖与节流
- 防抖：在某个时间段内，函数只执行一次；但如果在该时间段内再次触发了这个事件，就会重新计算函数的执行时间。
- 节流：在某个时间段内每隔一定时间，函数就执行一次。节流会降低函数的执行频率。

```js
// 防抖
// 先开启一个定时任务执行，定时任务完成后则清空
// 当再调用时，如果定时任务仍存在则清空原来任务，创建新的定时任务
function debounce(fn, delay = 200) {
    let timer = null;
    return function () {
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            fn.apply(this, arguments);
        }, delay);
    }
}
```

```js
// 节流
// 先开启一个定时任务执行，定时任务完成后则清空
// 当再调用时，如果定时任务仍存在则不执行任何操作，如果不存在就开启一个新的定时器
function throttle(fn, delay = 200) {
    let timer = null;
    return function () {
        if (!timer) {
            timer = setTimeout(() => {
                fn.apply(this, arguments);
                timer = null;
            }, delay);
        }
    }
}
```

测试防抖的代码如下，节流同理。
```js
const debounceFn = debounce(function () {
    console.log('我 resize 了');
}, 1000);

window.addEventListener('resize', debounceFn);
```

#### iterative
```js
// 检验是否可迭代
const isIterable = obj => obj !== null
    && typeof obj[Symbol.iterator] === 'function';

function iterative(fn) {
    return function (subject, ...rest) {
        if (isIterable(subject)) {
            // 把所有的执行结果存储于 result 中并返回
            const result = [];
            for (const obj of subject) {
                result.push(fn.apply(this, [obj, ...rest]));
            }
            return result;
        }
        return fn.apply(this, [subject, ...rest]);
    }
}
```

### 纯函数 & 非纯函数
- 纯函数：如果输入值确定，那么输出值确定，函数没有状态。示例：
```js
function add(a, b) {
    return a + b;
}
```
- 非纯函数：依赖于外部的环境，比如网络请求。示例：
```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
</ul>

<script>
    function setColor(elems, color) {
        for (const elem of elems) {
            elem.style.color = color;
        }
    }

    const els1 = document.querySelectorAll('li:nth-child(2n+1)');
    const els2 = document.querySelectorAll('li:nth-child(3n+1)');
    setColor(els2, 'blue');
    setColor(els1, 'red');
</script>
```
上面的代码中，两个 `setColor` 的调用顺序如果更换，执行的结果会不一样，所以它的结果会受外部环境的影响，是非纯函数。

使用高阶函数，可以减少系统里面非纯函数的数量，从而使得系统的稳定性和可靠性加强。比如说，我们现在需要实现两个函数，一个是 `setColor(elem, color)`，一次改变一个元素的颜色，还有一个 `setColors(elems, color)`，一次改变多个元素的颜色。显然这两个函数都是非纯函数，但是我们可以选择直接定义：

```js
const setColors = iterative(setColor);
```

这样就减少了一个非纯函数，有利于我们进行单元测试。

### 命令式 & 声明式

命令式：强调过程是什么
```js
const list = [1, 2, 3, 4];
const arr = [];

for (let i = 0; i < list.length; i++) {
    // 过程是给每个元素 × 2
    arr.push(list[i] * 2);
}
```

声明式：强调操作是什么
```js
const list = [1, 2, 3, 4];

// 操作是每个元素 x 2
const double = x => x * 2;

const arr = list.map(double);
```

假设现在需要实现一个开关，打开的时候显示字体 on，背景为绿色；关闭的时候显示字体 off，背景为红色。我们可以分别采用命令式和声明式实现。

命令式：
```html
<button class="off">off</button>

<style>
    button {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        border: none;
        outline: none;
    }

    .on {
        background-color: green;
    }

    .off {
        background-color: red;
    }
</style>

<script>
    const button = document.querySelector('button');
    button.addEventListener('click', (e) => {
        if (e.target.className === 'on') {
            e.target.className = 'off';
            e.target.innerHTML = 'off';
        } else {
            e.target.className = 'on';
            e.target.innerHTML = 'on';
        }
    })
</script>
```

优点：实现简单，代码比较容易理解。

声明式：
定义一个高阶函数 `toggle`，来实现状态的切换。
```js
function toggle(...actions) {
    return function (...args) {
        // 先将第一个操作取出，然后再将其 push 到列表的末尾，因此可以实现循环
        const action = actions.shift();
        actions.push(action);
        return action.apply(this, args);
    }
}

button.addEventListener('click', toggle(
    e => {
        e.target.className = 'on';
        e.target.innerHTML = 'on';
    },
    e => {
        e.target.className = 'off';
        e.target.innerHTML = 'off';
    }
))
```

优点：很容易拓展状态，比如说我想加一个 `warn` 状态，非常简单，直接在 `toggle` 函数中加入新的一项即可。

*所有的抽象都是为了提升可拓展性。 ——月影*

弄了一晚上终于把上半节课的笔记做完了，呜呜太难了。