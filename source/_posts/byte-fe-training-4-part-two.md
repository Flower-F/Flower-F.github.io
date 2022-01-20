---
title: 字节前端青训营第 4 天（下）
date: 2022-01-19 16:45:33
tags: [css, javascript]
copyright: true
---

# 动画基本原理

## 插值

无论动画多么简单，始终需要定义两个基本状态，即开始状态和结束状态。没有它们，我们将无法定义插值状态，从而填补两者之间的空白。
插值可能是颜色，也可能是位置等属性，可以理解为是对某个上下文中一个值或多个值的估计。当图形的变化可以由线性方程表示，就是线性插值。

## 帧

- 帧：连续变换的多张画面，其中的每一幅画面都是一帧。
- 帧率:用于度量一定时间段内的帧数，通常的测量单位是 FPS (frame per second)。
- 帧率与人眼：一般每秒 10-12 帧人会认为画面是连贯的，这个现象称为视觉暂留。对于一些电脑动画和游戏来说低于 30FPS 会感受到明显卡顿，目前主流的屏幕、显卡输出为 60FPS，效果会明显更流畅。

## 空白补全

空白的补全方式有以下两种

- 补间动画：传统动画，主画师绘制关键帧，交给清稿部门，清稿部门的补间动画师补充关键帧进行交付。（类比到这里， 补间动画师由浏览器来担任，如 keyframe，transition）
- 逐帧动画（Frame By Frame）：从词语来说意味着全片每一帧逐帧都是纯手绘。（如 css 的 steps 实现精灵动画）

# 前端动画分类

## CSS 动画

优点:
- 浏览器会对 CSS3 动画做一些优化，导致 CSS3 动画性能上稍有优势。（新建一个图层来跑动画）
- CSS3 动画的代码相对简单。

缺点:
- 动画控制上不够灵活。
- 兼容性不佳。
- 部分动画无法实现。（视差效果、滚动动画）

主要属性：
- [`transform`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)：可以用于图形的旋转、移动、缩放等
- [`transition`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)：处理开始状态到结束状态的过渡效果
- [`animation`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)：通过 keyframe 定义开始状态、结束状态以及多个中间态，相比于 `transition` 可以处理更复杂的动画

## SVG 动画

优点：通过矢量元素实现动画，不同的屏幕下均可获得较好的清晰度。可以用于实现一些特殊的效果，如：描字，形变，墨水扩散等。

缺点：使用方式较为复杂，过多使用可能会带来性能问题。

实现 SVG 动画通常有三种方式，SMIL、JS、CSS

### SMIL

兼容性不理想，因此不过多讨论

### JS 操作 SVG

- Snap.svg
- anime.js
- HTML 原生的 Web Animation

例一：[SVG 文字变形](https://codepen.io/jiangxiang/pen/MWmdjeY)

[CSS 属性 filter](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)：可以将模糊或颜色偏移等效果应用于元素，它的属性 url 可以传入一个 svg

```js
elts.text2.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
elts.text2.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;

fraction = 1 - fraction;
elts.text1.style.filter = `blur(${Math.min(8 / fraction - 8, 100)}px)`;
elts.text1.style.opacity = `${Math.pow(fraction, 0.4) * 100}%`;
```

通过上面的代码控制 `blur` 和 `opacity` 不断地变化。每次切换单词的时候，当它的模糊程度快没有，就直接通过透明度把它隐藏掉，造成一种文字溶解的错觉。

例二：[JS 画笔](https://codepen.io/jiangxiang/pen/eYWagxq)

[JS 画笔的原理](https://codepen.io/jiangxiang/pen/LYzvvxd)：
`stroke-dashoffset`、`stroke-dasharray` 配合使用实现笔画效果。

- 属性 `stroke-dasharray` 可控制用来描边的点划线的图案范式。它是一个数列，数与数之间用逗号或者空白隔开，指定短划线和缺口的长度。如果提供了奇数个值，则这个值的数列重复一次，从而变成偶数个值。因此，5,3,2 等同于 5,3,2,5,3,2。
- `stroke-dashoffset` 属性指定了 dash 模式到路径开始的距离。（当使用了 `stroke-dasharray`，就进入了 dash 模式）

```html
<line stroke-dasharray="5, 5" x1="10" y1="10" x2="190" y2="10" />
<line stroke-dasharray="5, 10" x1="10" y1="30" x2="190" y2="30" />
```

上面第一个表示先走 5 像素实线，再走 5 像素的空白；第二个表示先走 5 像素实线，再走 10 像素空白。

计算 path 的长度：`path.getTotalLength()`，然后将 `stroke-dashoffset` 的值设置为该的长度，就能实现类似画画的效果。

## JS 动画

JS 可以通过操作 SVG、CSS、Canvas 等实现动画。

优点：

- 使用灵活，同样在定义一个动画的 keyframe 序列时，可以根据不同的条件调节若干参数（JS 动画函数）改变动画方式。（CSS 会有非常多的代码冗余），对比于 CSS 的 keyframe 粒度更粗，CSS 本身的时间函数是有限的，这块 JS 可以弥补。
- CSS 很难做到两个以上的状态转化。（要么使用关键帧，要么需要多个动画延时触发，再想到要对动画循环播放或暂停倒序等，复杂度极高）

缺点:

- 使用到 JS 运行时，调优方面不如 CSS 简单，CSS 调优方式固定。
- 对于性能和兼容性较差的浏览器，CSS 可以做到优雅降级，而 JS 需要额外的代码兼容。

## 对比与结论

- 当 UI 元素采用较小的独立状态时，使用 CSS。
- 在需要对动画进行大量控制时，使用 JavaScript。
- 在特定的场景下可以使用 SVG，可以使用 CSS 或 JS 去操作 SVG 变化。

# 实现前端动画

## animate 函数的实现

**requestAnimationFrame VS setTimeout VS setInterval**

JavaScript 动画应该通过 `requestAnimationFrame` 实现。该内置方法允许设置回调函数以在**浏览器准备重绘时**运行，因此不容易丢帧，但是 `setTimeout` 和 `setInterval` 容易丢帧。通常这很快，确切的时间取决于浏览器。另外，当页面在后台时，根本没有重绘，所以回调不会运行，此时动画将被暂停并且不会消耗资源，这也比 `setTimeout` 和 `setInterval` 更优。

```js
function animate({ easing, draw, duration }) {
  // 为什么不使用 Date.new()？
  // 因为 performance.now() 会以恒定速度自增，精确到微秒级别，而 Date.now() 容易被篡改
  const start = performance.now();
  // 因为动画是连续的，执行完这个动画以后可能还有别的动画
  // 所以我们返回 Promise 以支持后续的顺序调用
  return new Promise((resolve) => {
    requestAnimationFrame(function animate(time) {
      // timeFraction 是当前已经执行的时间与动画要持续的总时间的比值
      // progress 是一个介于 0 到 1 的值，表示执行绘画的进度
      
      // 如果 timeFraction 大于或等于 1，说明时间已经超过 duration，执行完成
      // 直接把 progress 的最终值 1 传过去就行了
      // 否则的话，继续执行 requestAnimationFrame
      let timeFraction = (time - start) / duration;
      if (timeFraction > 1) {
        timeFraction = 1;
      }

      const progress = easing(timeFraction);
      draw(progress);

      if (timeFraction < 1) {
        requestAnimationFrame(animate);
      } else {
        resolve();
      }
    });
  });
}

// draw 绘制函数
// draw 是一支画笔，它会被反复调用
// 传入的参数是当前执行的进度，是一个介于 0 到 1 之间的值
const ball = document.querySelector(".ball");
const draw = (progress) => {
  ball.style.transfrom = `translate(${progress}px, 0)`;
};

// easing 缓动函数
// 修改动画执行的节奏
const easing = (timeFraction) => {
  return timeFraction ** 2;
};
```

## JS 动画的核心思想

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b3159700498f41418f87469f348b591e~tplv-k3u1fbpfcp-zoom-1.image)

这里 r 是距离，v 是速度，t 是时间。动画就是在开始状态和结束状态之间插值，这里的 r 可以简单理解为插值。

另外我们还需要通过比例尺实现物体的缩放，使得动画可以在显示屏中正常显示。

## 简单动画

### [动画演示](https://codepen.io/jiangxiang/pen/rNmgVKK)

- 匀速直线
- 重力
- 摩擦力
- 平抛
- 旋转 + 平抛

### 贝塞尔曲线

贝塞尔曲线生成网站：[cubic-bezier.com](https://cubic-bezier.com)
