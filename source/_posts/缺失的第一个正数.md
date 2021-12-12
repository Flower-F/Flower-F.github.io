---
title: 缺失的第一个正数
date: 2021-12-12 17:02:18
tags: [算法, leetcode]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/first-missing-positive/
解法分析：困难题，很无助，看懂题解花了半天。题解见
https://leetcode-cn.com/problems/first-missing-positive/solution/yi-miao-jiu-neng-gao-dong-de-shi-pin-jie-et3v/

```ts
function firstMissingPositive(nums: number[]): number {
    const len = nums.length;

    for (let i = 0; i < len; i++) {
        // 需要考虑指针移动情况
        // 大于 0；小于 len + 1；// 值为正数，且最大为 len + 1
        // 不等于 i + 1；// 若为 i + 1，无需交换
        // 两个交换的数相等时，防止死循环
        while (nums[i] > 0 && nums[i] < len + 1 &&
            nums[i] !== i + 1 && nums[i] !== nums[nums[i] - 1]) {
            const temp = nums[i];
            [nums[i], nums[temp - 1]] = [nums[temp - 1], temp];
        }
    }

    for (let i = 0; i < len; i++) {
        if (nums[i] !== i + 1) {
            return i + 1;
        }
    }

    return len + 1;
};
```

作者：chefyuan
链接：
https://leetcode-cn.com/problems/first-missing-positive/solution/yi-miao-jiu-neng-gao-dong-de-shi-pin-jie-et3v/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。