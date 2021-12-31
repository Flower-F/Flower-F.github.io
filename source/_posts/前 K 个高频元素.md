---
title: 前 K 个高频元素
date: 2021-12-31 11:27:39
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/top-k-frequent-elements/
解法分析：哈希表 + 桶排序

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function (nums, k) {
    // 哈希
    const map = new Map();
    let maxSize = 0;
    nums.forEach(num => {
        if (!map.has(num)) {
            map.set(num, 1);
            maxSize = Math.max(1, maxSize);
        } else {
            const count = map.get(num) + 1;
            map.set(num, count);
            maxSize = Math.max(count, maxSize);
        }
    });

    // 桶排
    // 这个地方有一个细节，长度是 maxSize + 1，不是 maxSize
    // 还要此处最好将所有值设置为 null
    const bucket = new Array(maxSize + 1).fill(null);
    map.forEach((value, key) => {
        if (bucket[value] === null) {
            bucket[value] = [];
        }
        bucket[value].push(key);
    })

    // 答案
    const res = [];
    for (let i = maxSize; i >= 0 && res.length < k; i--) {
        if (bucket[i] === null) {
            continue;
        }
        res.push(...bucket[i]);
    }

    return res;
};
```

顺便复习一下 null 和 undefined 的区别：
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