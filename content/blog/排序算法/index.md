---

     title: 排序算法
     date: 2021-08-29T13:59:33.925Z
     description: 排序算法

---

- 让数据有序
- 原地排序: 在排序的过程中,不需要开辟额外的空间来完成排序

## 选择排序

- 可以进行原地排序
- 选择排序保证的是已经参与排序的元素就是最终结果中的排序结果。
- 选择排序的时间复杂度是稳定的 O(n^2)级别的
- 循环不变量

1. 数组的区间 [i,n) 是未排序的
2. 数组的区间 (0,i] 是已排序的(`当 i等于数据规模n的时候,循环就结束了`)

- 循环体

1. 将 arr[i,n)中的最小值放到 arr[i]的位置

- 实现

```js
const arr = [2, 4, 5, 3, 9, 7, 8, 6, 1];
function swap(arr, pos1, pos2) {
  [arr[pos1], arr[pos2]] = [arr[pos2], arr[pos1]];
}
function sort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minIdx = i;
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[minIdx] > arr[j]) {
        minIdx = j;
      }
    }
    swap(arr, i, minIdx);
  }
  return arr;
}
console.log(sort(arr));
```

- 时间复杂度

1. O(n^2): 等差数列求和

```js
1 + 2 + 3 + 4 + 5 ...n
```

2. 复杂度分析中高阶的指数更趋近于算法的时间复杂度

```js
1/2n^2  + 1/2n  = O(n^2)
```

- 数组是否有序

```js
function isSorted(arr) {
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] < arr[i - 1]) {
      return false;
    }
  }
  return true;
}
```

## 插入排序

- 可以进行原地排序
- 插入排序保证的是已经参与排序的元素是有序的。
- 循环不变量

1. 数组的区间 [i,n) 是未排序的
2. 数组的区间 (0,i] 是已排序的(`当 i等于数据规模n的时候,循环就结束了`)

- 循环体

1. 将 arr[i] 放到合适的位置

- 实现

```js
const { isSorted, getRandomArr, swap } = require("./helper");

const arr = getRandomArr(10000, 100);
console.log(arr);
function sort(arr) {
  for (let i = 0; i < arr.length; i++) {
    // for (let j = i; j - 1 >=  0; j--) {
    //     if (arr[j] < arr[j - 1]) {
    //         swap(arr,j, j - 1)
    //     }else{
    //         break;
    //     }
    // }
    /** 简化版 */
    for (let j = i; j - 1 >= 0 && arr[j] < arr[j - 1]; j--) {
      swap(arr, j, j - 1);
    }
  }
  return arr;
}
const startTime = Date.now();
sort(arr);
const endTime = Date.now();
console.log(`is sorted: ${isSorted(arr)}`);
console.log(`run time: ${(endTime - startTime) / 1000}s`);
```

- 优化

1. 每一次交换都会经过`3` 条指令,所以将交换优化为平移。

```js
function moveSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    const v = arr[i]; // 暂存 arr[i] 的值
    let j;
    console.log();
    for (j = i; j - 1 >= 0 && v < arr[j - 1]; j--) {
      arr[j] = arr[j - 1];
    }
    arr[j] = v;
  }
}
```

- 特性

1. 对于有序数组,插入排序的复杂度是 O(n)，内层循环的复杂度为 O(1)
2. 在对一个近乎有序的数据进行排序的时候,应该使用插入排序。
