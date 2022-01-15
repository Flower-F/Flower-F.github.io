---
title: 动态规划常见题目
date: 2022-01-14 12:03:51
tags: [leetcode, 算法]
copyright: true
---
dp 问题的思考过程主要包括 5 个方面：
- dp 数组的含义
- base case，即边界
- 状态，即会变化的变量
- 选择：导致状态变化的行为
- 状态转移方程

本文总结常见的动态规划题目：
- [下降路径最小和](https://leetcode-cn.com/problems/minimum-falling-path-sum/)
- [零钱兑换](https://leetcode-cn.com/problems/coin-change/)
- [最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

# 下降路径最小和
这题属于是直接给出状态转移方程的 dp，所以不需要进行多余分析，直接确定边界就行了。
```js
/**
 * @param {number[][]} matrix
 * @return {number}
 */
var minFallingPathSum = function(matrix) {
    const m = matrix.length, n = matrix[0].length;
    const dp = new Array(m).fill(0).map(() => new Array(n).fill(0));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (i === 0) {
                // 边界：第一行，最小路径和等于自己本身
                dp[i][j] = matrix[i][j];
            } else if (j === 0) {
                // 每行的最左边
                dp[i][j] = Math.min(dp[i - 1][j], dp[i - 1][j + 1]) + matrix[i][j];
            } else if (j === n - 1) {
                // 每行的最右边
                dp[i][j] = Math.min(dp[i - 1][j], dp[i - 1][j - 1]) + matrix[i][j];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i - 1][j - 1], dp[i - 1][j + 1]) + matrix[i][j];
            }
        }
    }

    return Math.min(...dp[m - 1]);
};
```

# 零钱兑换
- dp 含义：状态只有一个，所以是一维数组。定义为输入目标金额 i，凑出该目标金额 i 的最少硬币数量为 dp[i]
- 边界：目标金额为 0，显然此时返回 0
- 状态：硬币数量无限，硬币的面额题目固定了，所以变量只有目标金额
- 选择：目标金额为什么会变化，是因为你选择了硬币
- 状态转移方程：dp[i] = min { dp[i - coins[i]] + 1, dp[i] }

```js
/**
 * @param {number[]} coins
 * @param {number} amount
 * @return {number}
 */
var coinChange = function(coins, amount) {
    const dp = new Array(amount + 1).fill(amount + 1);

    // base case
    dp[0] = 0;

    for (let i = 0; i <= amount; i++) {
        for (const coin of coins) {
            if (i - coin < 0) {
                continue;
            }
            // 状态转移
            dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        }
    }

    return dp[amount] === amount + 1 ? -1 : dp[amount];
};
```

# 最长递增子序列
- dp 含义：dp[i] 表示以 nums[i] 结尾的最长递增子序列的长度
以 1 4 3 4 2 为例：
    - dp[0] 指 1 为结尾的最长递增子序列 1 的长度，即 1
    - dp[1] 指 4 为结尾的最长递增子序列 1 4 的长度，即 2
    - 同理，dp[2] 为 1 3 的长度 2，dp[3] 为 1 3 4 的长度 3，dp[4] 为 1 2 的长度 2
- 边界：如上例所示，显然 dp[i] 初始值为 1，因为以 nums[i] 结尾的最长递增子序列至少也要包含它自己
- 状态：最长递增子序列的长度
- 选择：更换了一个新的子序列结尾，就会导致最长递增子序列的长度变化
- 状态转移方程：举个例子，对于 1 4 3 4 2 3 来说，dp[0] = 1，dp[1] = 2，dp[2] = 2，dp[3] = 3，dp[4] = 2。现在要求的是 dp[5]，因为序列必须递增，所以我们从前面的子序列选取时，要满足的第一个条件是：**结尾的字符比 nums[5] 要小**；在此基础上，我们需要选取前面最长的一个子序列

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    const len = nums.length;
    // 边界，要全部初始化为 1
    const dp = new Array(len).fill(1);

    for (let i = 0; i < len; i++) {
        for (let j = 0; j < i; j++) {
            // 判断是否满足递增要求
            if (nums[j] < nums[i]) {
                // 状态转移
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }

    // 最终结果是 dp 中的最大值
    return Math.max(...dp);
};
```

## 变式一
如何输出这个子序列（若有多个满足条件，输出其中一个即可）？
使用一个 sequence 数组记录一下所有的子序列即可。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function (nums) {
    const len = nums.length;
    // 记录所有的子序列
    const sequence = [];
    // 边界，要全部初始化为 1
    const dp = new Array(len).fill(1);
    // 边界，所有的 sequence[i] 设置为对应的 nums[i]
    for (let i = 0; i < len; i++) {
        sequence[i] = nums[i];
    }

    for (let i = 0; i < len; i++) {
        for (let j = 0; j < i; j++) {
            // 判断是否满足递增要求
            if (nums[j] < nums[i]) {
                if (dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    sequence[i] = sequence[j] + ' ' + nums[i];
                }
            }
        }
    }

    // 找到序列对应的下标
    let maxLen = 0, maxIndex = 0;
    for (let i = 0; i < len; i++) {
        if (maxLen < dp[i]) {
            maxIndex = i;
        }
    }

    return sequence[maxIndex];
};
```

## 变式二
如何把时间复杂度优化到 O(NlogN)？
使用贪心算法。
- 新建数组 ans，用于保存最长上升子序列（其实是假的）。
- 对原序列进行遍历，将每位元素二分插入 ans 中。
    - 如果 ans 中元素都比它小，将它插到最后
    - 否则，用它覆盖掉比它大的元素中最小的那个
总之，思想就是让 ans 中存储比较小的元素。这样 ans 未必是真实的最长上升子序列，但长度是对的。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var lengthOfLIS = function(nums) {
    const len = nums.length;
    
    const ans = [];
    ans.push(nums[0]);

    for (let i = 1; i < len; i++) {
        // ans 中元素都比它小，将它插到最后
        if (nums[i] > ans[ans.length - 1]) {
            ans.push(nums[i]);
        } else {
            // 二分插入
            let left = 0, right = ans.length - 1;
            while (left < right) {
                const mid = (left + right) >> 1;
                if (ans[mid] < nums[i]) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            // 用它覆盖掉比它大的元素中最小的那个
            ans[left] = nums[i];
        }
    }

    return ans.length;
};
```



参考资料：
1. https://labuladong.gitee.io/algo/3/23/70/
2. https://leetcode-cn.com/problems/minimum-falling-path-sum/solution/nickxue-xi-ji-hua-xi-lie-dong-tai-gui-hu-fbsb/
3. https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-e/
4. https://blog.csdn.net/weixin_30360497/article/details/94884825