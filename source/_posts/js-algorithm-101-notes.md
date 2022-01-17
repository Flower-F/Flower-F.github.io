---
title: js 版算法 101 笔记
date: 2022-01-17 10:59:29
tags: [javascript, 算法, 连载]
copyright: true
---
来源于[政采云前端团队](https://101.zoo.team/)

# 字符串
## [整数反转](https://leetcode-cn.com/problems/reverse-integer/)
- 方法一：使用 `reverse`
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    let num = parseInt(x.toString().split('').reverse().join(''));
    num = x < 0 ? -num : num;
    return num < -Math.pow(2, 31) || num > Math.pow(2, 31) - 1 ? 0 : num;
};
```

- 方法二：借鉴欧几里得算法，可以把空间复杂度降到 O(1)
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    let temp = Math.abs(x);

    let num = 0;
    while (temp !== 0) {
        num = num * 10 + (temp % 10);
        temp = Math.floor(temp / 10);
    }
    
    num = x < 0 ? -num : num;
    return num < -Math.pow(2, 31) || num > Math.pow(2, 31) - 1 ? 0 : num;
};
```

## [回文数](https://leetcode-cn.com/problems/palindrome-number/)
这题就是上面那题的变式
```js
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if (x < 0) {
        return false;
    }
    return x === reverse(x);

    // 反转数字
    function reverse(x) {
        let num = 0;
        while (x !== 0) {
            num = num * 10 + (x % 10);
            x = Math.floor(x / 10);
        }
        return num;
    };
};
```

## [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)
```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    if (s.length !== t.length) {
        return false;
    }
    
    const map = new Map();
    for (const ch of s) {
        map.set(ch, (map.get(ch) || 0) + 1);
    }

    for (const ch of t) {
        if (map.has(ch) && map.get(ch) > 0) {
            map.set(ch, map.get(ch) - 1);
        } else {
            return false;
        }
    }

    return true;
};
```

## [字符串转换整数](https://leetcode-cn.com/problems/string-to-integer-atoi/)
- 方法一：正则
```js
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function(s) {
    const maxVal = Math.pow(2, 31) - 1, minVal = -Math.pow(2, 31);

    // 书写正则
    // (-|\+)? 表示 - 或 + 或什么都没有
    // \d+ 为匹配数字
    const reg = /^(-|\+)?\d+/g;

    const groups = s.trim().match(reg);

    let res = 0;

    // 若匹配到了，取其第一项
    if (groups) {
        res = +groups[0];
    }

    if (res > maxVal) res = maxVal;
    if (res < minVal) res = minVal;

    return res;
};
```

- 方法二：使用 `parseInt`
```js
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function(s) {
    const maxVal = Math.pow(2, 31) - 1, minVal = -Math.pow(2, 31);

    let res = parseInt(s);
    if (isNaN(res)) {
        return 0;
    } 

    if (res > maxVal) res = maxVal;
    if (res < minVal) res = minVal;

    return res;
};
```

## [报数](https://leetcode-cn.com/problems/count-and-say/)
```js
/**
 * @param {number} n
 * @return {string}
 */
var countAndSay = function(n) {
    let str = '1';
    
    for (let i = 2; i <= n; i++) {
        const sb = [];
        let start = 0, pos = 0;

        while (pos < str.length) {
            while (pos < str.length && str[pos] === str[start]) {
                pos++;
            }
            sb.push('' + (pos - start) + str[start]);
            start = pos;
        }

        str = sb.join('');
    }

    return str;
};
```

## [反转字符串](https://leetcode-cn.com/problems/reverse-string/)

```js
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    for (let i = 0; i < s.length / 2; i++) {
        [s[i], s[s.length - 1 - i]] = [s[s.length - 1 - i], s[i]];
    }
};
```

## [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```js
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    const map = new Map();

    for (const ch of s) {
        map.set(ch, (map.get(ch) || 0) + 1);
    }

    for (let i = 0; i < s.length; i++) {
        if (map.get(s[i]) === 1) {
            return i;
        }
    }

    return -1;
};
```

## [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)
```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function (s) {
    let real = [];
    for (let i = 0; i < s.length; i++) {
        if (s[i] >= 'a' && s[i] <= 'z' || s[i] >= 'A' && s[i] <= 'Z' || s[i] >= '0' && s[i] <= '9')
            real.push(s[i].toLowerCase());
    }

    const realString = real.join('');
    for (let i = 0; i < realString.length / 2; i++) {
        if (realString[i] !== realString[realString.length - i - 1]) {
            return false;
        }
    }
    return true;
};
```

## [实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

- 暴力
```js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if (needle === '') {
        return 0;
    }
    for (let i = 0; i < haystack.length; i++) {
        let count = 0;
        for (let j = 0; j < needle.length && j + i < haystack.length; j++) {
            if (haystack[i + j] === needle[j]) {
                count++;
            }
            if (j === needle.length - 1 && count === needle.length) {
                return i;
            }
        }
    }

    return -1;
};
```

- 剪枝 + 遍历
```js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    // 剪枝
    if (needle === '') {
        return 0;
    }
    if (needle.length > haystack.length) {
        return -1;
    }
    if (needle.length === haystack.length) {
        return needle === haystack ? 0 : -1;
    }

    for (let i = 0; i <= haystack.length - needle.length; i++) {
        if (haystack[i] !== needle[0]) {
            continue;
        }
        if (haystack.substring(i, i + needle.length) === needle) {
            return i;
        }
    }

    return -1;
};
```

## [最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 让数组中的第一个元素成为前缀
    let prefix = strs[0];
    // 循环字符串数组，判断数组中的每一个元素是否与声明的前缀一样
    for(let i = 1; i < strs.length; i++) {
        while(!strs[i].startsWith(prefix)) {
            // 不一样就将声明的前缀长度减一
            prefix = prefix.substring(0, prefix.length - 1);
            // 判断是否存在公共前缀
            if(prefix === "") {
                return "";
            }
        }
    }
    return prefix;
};
```

## [最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
- dp
```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    // 长度为 1，一定回文
    // 长度为 2 或 3，判断首尾是否相同：s[i] === s[j]
    // 长度大于 3, 首尾字符相同，且去掉首尾之后的子串仍为回文：(s[i] === s[j]) && dp[i + 1][j - 1]
    const len = s.length;
    // dp[i][j] 表示 s[i..j] 是否是回文串
    const dp = new Array(len).fill(false).map(() => new Array(len).fill(false));

    // 边界，长度为 1 必定为回文
    for (let i = 0; i < len; i++) {
        dp[i][i] = true;
    }

    // 记录起点和长度，即可得到字符串
    let maxLen = 1, begin = 0;
    // 第一层枚举为子串长度
    for (let l = 2; l <= len; l++) {
        // 第二层枚举为左边界
        for (let i = 0; i < len; i++) {
            // j - i + 1 = l
            const j = l + i - 1;
            if (j >= len) {
                // 如果右边界越界，就可以退出当前循环
                break;
            }
            if (s[i] !== s[j]) {
                dp[i][j] = false;
            } else {
                if (l <= 3) {
                    dp[i][j] = true;
                } else {
                    dp[i][j] = dp[i + 1][j - 1];
                }
            }

            // 只要 dp[i][j] 为 true，就表示子串 s[i..j] 是回文
            if (dp[i][j] && l > maxLen) {
                maxLen = l;
                begin = i;
            }
        }
    }

    return s.substr(begin, maxLen);
};
```

- 暴力，需要注意的是这题暴力的解比 dp 更优
```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let res = '';
    for (let i = 0; i < s.length; i++) {
        // 返回以 i 为中心的回文串
        const a = palindrome(i, i);
        // 返回以 i 和 i + 1 为中心的回文串
        const b = palindrome(i, i + 1);
        if (a.length > res.length) {
            res =  a;
        }
        if (b.length > res.length) {
            res = b;
        }
    }

    return res;

    // 设置左右两个指针，是为了可以同时处理回文串长度为奇数和偶数的情况
    function palindrome(left, right) {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            left--;
            right++;
        }

        // 返回以 s[left] 和 s[right] 为中心的最长回文串
        // 此时 left 和 right 已经不符合要求了，所以返回的区间是 [left + 1, right - 1]
        return s.substring(left + 1, right);
    }
};
```

