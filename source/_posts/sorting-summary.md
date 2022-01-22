---
title: 排序算法总结
date: 2022-01-08 23:06:11
tags: [算法]
copyright: true
---
复习手写排序

# 冒泡排序

冒泡排序的基本思想是，**对相邻的元素进行两两比较**，顺序相反则进行交换，这样，每一趟会将最小或最大的元素“浮”到顶端，最终达到完全有序。

```js
function bubbleSort(arr) {
    const len = arr.length;
    if (!Array.isArray(arr) || len <= 1) {
        return arr;
    }

    for (let i = 0; i < len; i++) {
        let flag = false; // 标记是否发生了交换
        for (let j = 0; j < len - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                flag = true;
            }
        }
        if (!flag) {
            return arr;
        }
    }
    return arr;
}
```
**为什么第二层循环的结束下标是 len - 1 - i**？
随着外层循环的进行，数组尾部的元素会渐渐变得有序当我们走完第 1 轮循环的时候，最大的元素被排到了数组末尾；走完第 2 轮循环的时候，第 2 大的元素被排到了数组倒数第 2 位；走完第3轮循环的时候，第 3 大的元素被排到了数组倒数第 3 位......以此类推，走完第 n 轮循环的时候，数组的后 n 个元素就已经是有序的，所以后面的部分就不需要再排序了。

# 选择排序

选择排序的基本思想为每一趟从待排序的数据元素中**选择最小**（或最大）的一个元素作为首元素，直到所有元素排完为止。在算法实现时，每一趟确定最小元素的时候会通过不断地比较交换来使得首位置为当前最小，交换是个比较耗时的操作。其实我们很容易发现，在还未完全确定当前最小元素之前，这些交换都是无意义的。我们可以通过设置一个变量 min，每一次比较仅存储较小元素的数组下标，当轮循环结束之后，那这个变量存储的就是当前最小元素的下标，此时再执行交换操作即可。
```js
function selectSort(arr) {
    const len = arr.length;
    if (!Array.isArray(arr) || len <= 1) {
        return arr;
    }

    let minIndex;
    for (let i = 0; i < len - 1; i++) {
        minIndex = i;
        for (let j = i; j < len; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        if (minIndex !== i) {
            [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
        }
    }
    return arr;
}
```

# 插入排序

直接插入排序基本思想是每一步将一个待排序的记录，**插入到前面已经排好序的有序序列**中去，直到插完所有元素为止。
```js
function insertSort(arr) {
    const len = arr.length;
    if (!Array.isArray(arr) || len <= 1) {
        return arr;
    }

    for (let i = 1; i < len; i++) {
        let j = i;
        let temp = arr[i];

        while (j > 0 && arr[j - 1] > temp) {
            arr[j] = arr[j - 1];
            j--;
        }

        arr[j] = temp;
    }

    return arr;
}
```

# 归并排序

归并排序是利用归并的思想实现的排序方法，该算法采用经典的分治策略。**递归**的将数组两两分开直到只包含一个元素，然后将数组排序**合并**，最终合并为排序好的数组。
**模拟过程**
- 递归分割
- [8, 7, 6, 5, 4, 3, 2, 1]
- [8, 7, 6, 5,| 4, 3, 2, 1]
- [8, 7,| 6, 5,| 4, 3,| 2, 1]
- [8,| 7,| 6,| 5,| 4,| 3,| 2,| 1]
- 合并
- [7, 8,| 5, 6,| 3, 4,| 1, 2]
- [5, 6, 7, 8,| 1, 2, 3, 4]  
- [1, 2, 3, 4, 5, 6, 7, 8]  

```js
function mergeSort(arr) {
    const len = arr.length;
    if (len <= 1) {
        if (!Array.isArray(arr) || len <= 1) return arr;
    }
    // 分割点
    const mid = Math.floor(len / 2);
    // 递归分割左子数组
    const left = mergeSort(arr.slice(0, mid));
    // 递归分割右子数组
    const right = mergeSort(arr.slice(mid, len));
    // 合并左右两个有序数组
    arr = merge(left, right);

    return arr;
}

function merge(arr1, arr2) {
    let i = 0, j = 0;
    const res = [];
    const len1 = arr1.length, len2 = arr2.length;
    while (i < len1 && j < len2) {
        if (arr1[i] < arr2[j]) {
            res.push(arr1[i]);
            i++;
        } else {
            res.push(arr2[j]);
            j++;
        }
    }

    // 若其中一个子数组首先被合并完全，则直接拼接另一个子数组的剩余部分
    if (i < len1) {
        return res.concat(arr1.slice(i));
    } else {
        return res.concat(arr2.slice(j));
    }
}
```

# 快速排序

快速排序的基本思想是通过一趟排序将要排序的数据**分割成独立的两部分**，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

```js
function quickSort(arr, left = 0, right = arr.length - 1) {
  const len = arr.length;
  if (!Array.isArray(arr) || len <= 1) {
    return arr;
  }
  if (left < right) {
    const index = partion(arr, left, right);
    quickSort(arr, left, index - 1);
    quickSort(arr, index + 1, right);
  }

  return arr;
}

// 以基准值为轴心，划分左右子数组的过程
function partion(arr, left, right) {
  if (right > left) {
    // 随机快排，防止遇到有序数组导致复杂度降到 n 方
    let randomIndex = Math.floor(Math.random() * (right - left)) + left + 1;
    [arr[left], arr[randomIndex]] = [arr[randomIndex], arr[left]];
  }

  const pivot = arr[left];
  let i = left, j = right;

  while (i < j) {
    // 此处必须先移动 j 后移动 i
    while (i < j && arr[j] >= pivot) j--;
    while (i < j && arr[i] <= pivot) i++;

    [arr[i], arr[j]] = [arr[j], arr[i]];
  }

  [arr[i], arr[left]] = [arr[left], arr[i]];
  return i;
}

const ans = quickSort([5, 1, 1, 2, 0, 0]);

console.log(ans);
```

# 堆排序

堆排序的基本思想是：将待排序序列构造成一个大顶堆，此时，**整个序列的最大值就是堆顶的根节点**。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余 n - 1 个元素重新构造成一个堆，这样会得到 n 个元素的次小值。如此反复执行，便能得到一个有序序列了。

```js
function heapSort(arr) {
    const len = arr.length;

    if (!Array.isArray(arr) || len <= 1) return arr;

    buildMaxHeap(arr); // 将传入的数组建立为大根堆

    // 每次循环，将首个元素（即最大元素）与末尾元素交换，然后剩下的元素重新构建为大根堆
    for (let i = len - 1; i >= 0; i--) {
        [arr[0], arr[i]] = [arr[i], arr[0]];
        adjustMaxHeap(arr, 0, i);
    }

    return arr;
}

function adjustMaxHeap(arr, root, heapSize) {
    let maxIndex, lchild, rchild;
    while (true) {
        maxIndex = root; // 根所在值即最大值
        lchild = root * 2 + 1; // 左孩子下标
        rchild = root * 2 + 2; // 右孩子下标

        // 如果左子元素存在，且左子元素大于最大值，则更新最大值索引
        if (lchild < heapSize && arr[maxIndex] < arr[lchild]) {
            maxIndex = lchild;
        }

        // 如果右子元素存在，且右子元素大于最大值，则更新最大值索引
        if (rchild < heapSize && arr[maxIndex] < arr[rchild]) {
            maxIndex = rchild;
        }

        // 如果最大元素被更新了，则交换位置，以保证根的值最大，同时还要更新根的值
        if (maxIndex !== root) {
            [arr[root], arr[maxIndex]] = [arr[maxIndex], arr[root]];
            root = maxIndex;
        } else {
            // 如果未被更新，说明该子树满足大根堆的要求，退出循环
            break;
        }
    }
}

function buildMaxHeap(arr) {
    const len = arr.length, lastElem = len >> 1 - 1; // 数组长度，最后一个非叶子元素

    for (let i = len; i >= 0; i--) {
        adjustMaxHeap(arr, i, len);
    }
}
```

参考资料：
https://juejin.cn/book/6844733800300150797/section/6844733800367439885
https://www.kancloud.cn/pillys/qianduan/2051370