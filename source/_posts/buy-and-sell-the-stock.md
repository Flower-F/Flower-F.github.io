---
title: 买卖股票的最佳时机
date: 2021-12-03 17:50:53
tags: [算法, codetop]
copyright: true
---
题目链接：
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/
解法分析：
贪心，
如果我们真的在买卖股票，我们肯定会想：如果我是在历史最低点买的股票就好了！在题目中，我们只要用一个变量记录一个历史最低价格 minprice，我们就可以假设自己的股票是在那天买的。那么我们在第 i 天卖出股票能得到的利润就是 prices[i] - minprice。

因此，我们只需要遍历价格数组一遍，记录历史最低点，然后在每一天考虑这么一个问题：如果我是在历史最低点买进的，那么我今天卖出能赚多少钱？当考虑完所有天数之时，我们就得到了最好的答案。

```js
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    if(prices.length < 1) {
        return 0;
    }
    let minPrice = prices[0], ans = 0;
    for(let i = 1; i < prices.length; i++) {
        if(prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            const temp = prices[i] - minPrice;
            if(temp > ans) {
                ans = temp;
            }
        }
    }
    return ans;
};
```


参考题解：
作者：LeetCode-Solution
链接：
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/121-mai-mai-gu-piao-de-zui-jia-shi-ji-by-leetcode-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。