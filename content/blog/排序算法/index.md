---

     title: 排序算法
     date: 2021-08-26T14:36:11.466Z
     description: 排序算法

---

- 让数据有序
- 原地排序: 在排序的过程中,不需要开辟额外的空间来完成排序

## 选择排序

- 可以进行原地排序
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
