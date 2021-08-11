[toc]

---

     title: 数据结构(静态的)
     date: 2021-08-11T13:16:24.615Z
     description: [toc]

# 数

---

> 可以容纳数据的结构被称为数据结构

## 线性数据结构(一纬数据结构)

> 一纬数据结构：强调的是存储与顺序

### 数组

- [x] 特性
- 数组定长：数组长度是**不可变**的。`js引擎会自动优`

> 什么是数组扩容: 向操作系统申请一段内存 --> 将原数组的每一项复制到新到内存 --> 新增新的项

- 存储在物理空间是连续的
- 数组的变量，指向了数组第一个元素的位置。**首地址**

  > a[1]:方括号操作表示存储地址的偏移,_且通过偏移查询数据性能最好_

- [x] 优点
- 查询性能好
- [x] 缺点
- 因为空间必须得是连续的，所以如果数组比较大，且系统的**空间碎片**较多的时候容易存储不小。
  > 空间碎片：非连续的内存空间，且空间碎片多就会导致系统整理碎片，会非常的消耗性能
- 因为数组的长度是固定的，所以数组的内容难以被添加和删除
  > 增删数组的代价都是昂贵的。

### 链表

> 带有封装性质的数据结构
> 。一个节点:数据 + 引用`下一个节点的首地址`

- [x] 特性
- 空间上**不是连续的**
- 每存放一个值，都要多开销一个引用空间
- 传递一个链表，必须传递链表的根节点
- **每一个节点都认为自己是根结点**
- [x] 优点
- 只要内存足够大，就能存的下，不用担心空间碎片的问题
- 链表的添加和删除非常的容易
- [x] 缺点
- 查询速度慢.(查询某个位置)
- 链表的每一个节点都需要创建一个指向 next 的引用，浪费一些空间。**反之，当数据域存储的数据很多，从而存放引用的域占的比列就会变小**
- [x] 创建一个链表

```js
function Node(value) {
  this.value = value;
  this.next = null;
}

const a = new Node(1);
const b = new Node(2);
const c = new Node(3);

a.next = b;
b.next = c;
c.next = null;
```

---

### 线性数据结构的遍历

> 遍历：将集合中的每一个元素进行获取并查看

- [x] 数组

```js
function traverse(arr) {
  if (arr === null) return;
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
  }
}
```

- [x] 链表

> while(true) 在不直到循环次数的时候使用

```js
// 循环遍历
// root : 根节点
function traverse(root) {
  let temp = root;
  while (true) {
    if (temp != null) {
      console.log(temp.value);
    } else {
      break;
    }
    temp = temp.next;
  }
}

// 递归遍历

function recursion(root) {
  if (root == null) return;
  console.log(root.value);
  recursion(root.next);
}
```

---

### 链表逆置

> 先找到**最后一个节点**

```js
// 创建一个链表
function Node(value) {
  this.value = value;
  this.next = null;
}

const a = new Node(1);
const b = new Node(2);
const c = new Node(3);

a.next = b;
b.next = c;
c.next = null;

// 逆置

function inversion(root) {
  // 代表当前节点是倒数第二个节点
  if (root.next.next == null) {
    // 让最后一个节点指向自己
    root.next.next = root;
    return root.next;
  } else {
    const result = inversion(root.next);
    root.next.next = root;
    root.next = null;
    return result;
  }
}
```

### 栈

> 类比一个箱子，先放进去的，被压在下面。在 js 中的变量申明就是这种结构

- [x] 特性:FILO 先进后出

```js
function Stack() {
  this.arr = [];
  this.push = function (value) {
    this.arr.push(value);
  };
  this.pop = function () {
    return this.arr.pop();
  };
}
const stack = new Stack();
stack.push(1);
stack.push(2);
stack.push(3);
stack.pop();
```

### 队列

> pipe

- [x] 特性:FIFO 先进先出

```js
function Queue(){
   this.arr  = [];
    this.push = function (value){
    this.arr.push(value)
    }
    this.shift = function(){
    return this.arr.shift()
}

const q = new Queue()

q.push(1)
q.push(2)
q.push(3)
q.shift()

```

### 双向链表

> 两个指针域 + 数据域

- [x] 特性:两个指针域 + 数据域
- [x] 优点: 无论给出哪一个节点，都可以对所有的节点进行遍历
- [x] 缺点:多耗费一个引用的空间，而且构建双向链表比较复杂

---

## 二位数据结构

### 二维数组

> 数组中的每一个元素都是一个数组

```js
const arr = [[], []];
```

### 二维拓扑结构(图)

> 什么是拓扑结构？只研究节点之间关系的结构。不考虑各种物理因素，例如位置 大小 等

> 图:节点 + 边 。多个链表构成一个图

- [x] 描述这样的一个图

```js
function Node(value) {
  this.value = value;
  this.neigthbor = [];
}

const a = new Node("a");
const b = new Node("b");
const c = new Node("c");
const d = new Node("d");
const e = new Node("e");
const f = new Node("f");

a.neighbor.push(b);
a.neighbor.push(c);
a.neighbor.push(f);

b.neighbor.push(a);
b.neighbor.push(d);
b.neighbor.push(e);

c.neighbor.push(a);

d.neighbor.push(b);

e.neighbor.push(b);
```

#### 图的表示

> 点集合 和 边集合

```js
const max = 10000;
// 点集合
const pointSet = ["A", "B", "C", "D", "E"];
// 边集合 二维数组
const distance = [
  [0, 4, 7, max, max],
  [4, 0, 8, 6, max],
  [7, 8, 0, 5, max],
  [max, 6, 5, 0, 7],
  [max, max, max, 7, 0],
];
```

### 树形结构

> 树是图的一种

> 术名:有向无环图

- [x] 特性 : 有一个根节点且树形结构没有回路。
- [x] 叶子节点：没有子节点的节点
- [x] 节点：既不是根结点，又不是叶子节点的普通节点
- [x] 树的度：**拥有最多子节点的节点** 的 节点数。
- [x] 树的深度：树最深有几层，树的深度就为几

#### 二叉树

- [x] 特性：
- 树的度最大为 2 的树
- 每个节点都认为自己是根节点

- [x] 子树:二叉树中，每一个节点或叶子节点，都是一棵子树的根节点
- 左子数
- 右子树

#### 二叉树 - 满二叉树

- [x] 特性
- 所有的叶子节点都在最底层
- 每一个非叶子节点都有两个子节点

#### 二叉树 - 完全二叉树

- [x] 特性
- 所有的叶子节点都在最底层或者倒数第二层
- 所有的叶子节点 **都向左聚拢**

### 二叉树遍历

> 后序序列和中序序列,可以唯一确定一棵二叉树

- [x] **传递二叉树要传根节点**

- [x] 前序遍历(先根次序遍历)
  > 先打印当前的，再打印左子树 ，再打印右子树。根- 左 - 右

```js
// 一个节点：data  + left + right
function Node(value) {
  this.value = value;
  this.left = null;
  this.right = null;
}

function Preorder(root) {
  if (root == null) return;
  console.log(root.value);
  Preorder(root.left);
  Preorder(root.right);
}

function Inorder(root) {
  if (root == null) return;
  Inorder(root.left);
  console.log(root.value);
  Inorder(root.right);
}

function Postorder(root) {
  if (root == null) return;
  Postorder(root.left);
  Postorder(root.right);
  console.log(root.value);
}
```

- [x] 中序遍历(中根次序遍历)

  > 先打印左子树，再打印当前的 ，再打印右子树。左- 根 - 右

- [x] 后序遍历(后根次序遍历)

  > 先打印左子树，再打印右子树 ，再打印当前的。左- 右 - 根

- [x] 层序遍历
  > 从上到下 从左到右

```js
function levelOrder(root, depth) {
  const res = [];
  function traversal(root, depth) {
    if (root == null) return;
    if (!res[depth]) {
      res[depth] = [];
    }
    res[depth].push(root.value);
    traversal(root.left, depth + 1);
    traversal(root.right, depth + 1);
  }
  traversal(root, 0);
  return { result: res, depth: res.length };
}
const { result, depth } = levelOrder(a, 0);
console.log(result, depth);
```

### 还原二叉树

> 前中 --> 后序

- [x] 找出根节点
- [x] 找出根节点在中序中的位置
- [x] 分别找出中序和前序中的左子树 和 右子树 递归。

```js
const pre = ["a", "c", "f", "g", "b", "d", "e"];

const mid = ["f", "c", "g", "a", "d", "b", "e"];

const last = ["f", "g", "c", "d", "e", "b", "a"];

function Node(value) {
  this.value = value;
  this.left = null;
  this.right = null;
}
function fl(pre, mid) {
  if (
    pre == null ||
    mid == null ||
    pre.length == 0 ||
    mid.length == 0 ||
    mid.length != pre.length
  )
    return;

  // 拿到根节点 - 前序遍历的第一个节点是根节点
  const root = new Node(pre[0]);
  // 找到根节点在中序中的位置
  const index = mid.indexOf(root.value);

  const preLeft = pre.slice(1, 1 + index);

  const preRight = pre.slice(1 + index, pre.length);

  const midLeft = mid.slice(0, index);

  const midRight = mid.slice(index + 1, mid.length);

  root.left = fl(preLeft, midLeft);

  root.right = fl(preRight, midRight);

  return root;
}
```

> 后中 --> 前序

- [x] 找出根节点
- [x] 找出根节点在中序中的位置
- [x] 分别找出中序和前序中的左子树 和 右子树 递归。

```js
function midLast(mid, last) {
  if (
    last == null ||
    mid == null ||
    last.length == 0 ||
    mid.length == 0 ||
    mid.length != last.length
  )
    return;

  // 找出根节点 - 后序遍历的最后一个节点是根节点
  const [value] = last.slice(-1);
  const root = new Node(value);
  const index = mid.indexOf(value);

  const midLeft = mid.slice(0, index);
  const midRight = mid.slice(index + 1, mid.length);

  const lastLeft = last.slice(0, index);
  const lastRight = last.slice(index, last.length - 1);

  root.left = midLast(midLeft, lastLeft);
  root.right = midLast(midRight, lastRight);
  return root;
}
```

### 二叉树的 深搜 和 广搜

> 树的搜索 图的搜索 爬虫的逻辑 搜索引擎的爬虫算法

- [x] 深度优先搜索

> 更适合探索未知

> 对于二叉树来说，深度优先搜索，和前序遍历的顺序是一样的。**前序遍历 后序 中序遍历都属于深度优先遍历算**法。

```js
function Node(value) {
  this.value = value;
  this.left = null;
  this.right = null;
}

const a = new Node("a");
const b = new Node("b");
const c = new Node("c");
const d = new Node("d");
const e = new Node("e");
const f = new Node("f");
const g = new Node("g");

a.left = c;
a.right = b;
c.left = f;
c.right = g;
b.left = d;
b.right = e;

function deepSearch(root, target) {
  if (root == null) return false;
  if (root.value == target) return true;

  const left = deepSearch(root.left, target);
  const right = deepSearch(root.right, target);
  return left || right;
}
```

- [x] 广度优先搜索

> 更适合探索局域

> 一层一层进行搜索

```js
function broadSearch(rootList, target) {
  if (rootList == null || rootList.length == 0) return false;
  const childList = [];
  for (var i = 0; i < rootList.length; i++) {
    if (rootList[i] != null && rootList[i].value == target) return true;
    else {
      childList.push(rootList[i].left);
      childList.push(rootList[i].right);
    }
    return broadSearch(childList, target);
  }
}
```

### 二叉树的比较

> 二叉树的比较是所有树比较的基础

> 遇到二叉树比较的问题时，必须要确定左右两颗子数交换位置，还算不算同一棵二叉树，

```js
// 左右数互换不是同一棵树 - 判断二叉树是否相同

function compareTree(root1, root2) {
  if (root1 == root2) return true;
  if ((root1 != null && root2 == null) || (root2 != null && root1 == null))
    return false;
  if (root1.value != root2.value) return false;
  let leftBool = compareTree(root1.left, root2.left);

  let rightBool = compareTree(root1.right, root2.right);

  return rightBool && leftBool;
}

// 左右数互换是同一棵树 - 判断二叉树是否相同

function compareTree(root1, root2) {
  if (root1 == root2) return true;
  if ((root1 != null && root2 == null) || (root2 != null && root1 == null))
    return false;
  if (root1.value != root2.value) return false;
  let leftBool = compareTree(root1.left, root2.left);

  let rightBool = compareTree(root1.right, root2.right);

  const bool =
    compareTree(root1.right, root2.left) &&
    compareTree(root1.left, root2.right);
  return (rightBool && leftBool) || bool;
}
```

### 二叉树的 diff 算法

> 新增了什么 修改了什么 删除了什么

```js
// {type:'新增',origin:原来的节点, now:现在的节点}
const diff = [];

function diffTree(root1, root2, diffList) {
  if (root1 == root2) return diffList;
  if (root1 == null && root2 !== null) {
    diffList.push({ type: "新增", origin: null, now: root2 });
  } else if (root1 != null && root2 == null) {
    diffList.push({ type: "删除", origin: root1, now: null });
  } else if (root1.value != root2.valiue) {
    diffList.push({ type: "修改", origin: root1, now: root2 });
    diffTree(root1.left, root2.left, diffList);
    diffTree(root1.right, root2.right, diffList);
  } else {
    diffTree(root1.left, root2.left, diffList);
    diffTree(root1.right, root2.right, diffList);
  }
}
```

### 二叉搜索树

> 有排序效果的二叉树，**左子树的节点都比当前节点小，右节点都比当前节点大**

> 搜索性能取决于 二叉树的深度

```js
function Node(value) {
  this.value = value;
  this.left = null;
  this.right = null;
}
// 构建二叉搜索树
// 判断传入的值和当前值的大小关系
// 小则为左子树
// 大则为右子树
function addNode(root, num) {
  if (root == null) return;
  if (root.value == num) return;
  if (root.value < num) {
    if (root.right == null) root.right = new Node(num);
    else addNode(root.right, num);
  } else {
    if (root.left == null) root.left = new Node(num);
    else addNode(root.left, num);
  }
}
// 传入一个数组，输出一个二叉搜索树
// 将数组的第一位作为根节点
function buildSearchTree(arr) {
  if (arr == null || arr.length == 0) return null;
  const root = new Node(arr[0]);
  for (let i = 0; i < arr.length; i++) {
    addNode(root, arr[i]);
  }
  return root;
}

// 搜索二叉树
// 判断目标值 target  和当前值的关系 value
// target > value 则递归搜索右子树
// value > target 则递归搜索左子树
function search(root, target) {
  if (root == null) return false;
  if (root.value == target) return true;
  if (root.value < target) {
    return search(root.right, target);
  } else {
    return search(root.left, target);
  }
}
```

### 平衡二叉树

> 减少二叉搜索树的深度

- [x] 根节点的左子树与右子树的高度不能超过 1
- [x] 这棵二叉树的每一个子树都符合第一条

```js
function getDeep(root) {
  if (root == null) return 0;
  let leftDeep = getDeep(root.left);
  let rightDeep = getDeep(root.right);
  return Math.max(leftDeep, rightDeep) + 1;
}
// 判断一棵树 是不是平衡二叉树
// 计算 每棵树的左子树 和 右子树 判断深度是否相差 小于 1
function isBalance(root) {
  if (root == null) return true;
  let leftDeep = getDeep(root.left);
  let rightDeep = getDeep(root.right);
  if (Math.abs(leftDeep - rightDeep) > 1) {
    return false;
  } else {
    return isBalance(root.left) && isBalance(root.right);
  }
}
```

### 二叉树单旋

> 将一棵不平衡的二叉树 变成一棵平衡二叉树

- [x] 概念

```
旋转节点：不平衡的节点为旋转节点
新根：旋转之后称为根节点的节点
变化分支：父级节点发生变化的那个分支
不变分支：父级节点不变的那个分支
```

- [x] 左单旋
  > 左边浅 右边深

```js

function change(root){
    // 返回平衡之后的根节点
    if(isBalance(root)) return root;
    if(root.left != null){
       root.left =  change(root.left);
    }
    if(root.right != null){
      root.right =  change(root.right);
    }
    let leftDeep = getDeep(root.left)
    let rightDeep = getDeep(root.right)

    if(Math.abs(leftDeep - rightDeep)<2){
        return true;
    }else if(leftDeep > rightDeep){
        // 右单旋
        return rigthRotate(root)
    }else{
        // 左单旋
        return leftRotate(root)
    }
}

function rigthRotate(){
   // 找到新根
    var newRoot = root.left;
    // 找到变化分支
    var changeTree = root.left.right;
    // 当前旋转节点的左孩子为变化分支
    root.left = changeTree;
    // 新根的右孩子为旋转节点
    newRoot.right = root;
    // 返回新的根节点
    return newRoot;
}
function leftRotate(root){
1. 找到新根 - 右子树的根节点
const newRoot = root.right;
2. 找到变化分支 - 右子树的左子树
const changeTree = root.right.left;
3. 当前旋转节点的右孩子为变化分支
root.right = changeTress
4. 新根的左孩子为旋转节点
newRoot.left = root
5. 返回当前新的根节点
return newRoot
}
```

- [x] 右单旋

### 二叉树的双旋

> 变化分支，不可以是唯一的最深分支

> 比较变化分支 和 非变化分支的深度，如果变化分支深度大于非变化分支，则先进行单选

- [x] 左右双旋

  > 当要对某个节点进行左单旋时候，如果变化分支是唯一的最深分支，那么我们要对新根进行右单旋，然后再进行左单旋，
  > 这样的旋转叫做右左双旋

- [x] 右左双旋

  > 当要对某个节点进行右单旋时候，如果变化分支是唯一的最深分支，那么我们要对新根进行左单旋，然后再进行右单旋，
  > 这样的旋转叫做左右双旋

- [x] 左左双旋
  > 如果变化分支的高度比旋转节点的另一侧高度差距超过了 2，那么单旋之后依旧不平衡
- [x] 右右双旋

###

### 最小生成树

```js
const max = 10000;
// 点集合
const pointSet = ["A", "B", "C", "D", "E"];
// 边集合 二维数组
const distance = [
  [0, 4, 7, max, max],
  [4, 0, 8, 6, max],
  [7, 8, 0, 5, max],
  [max, 6, 5, 0, 7],
  [max, max, max, 7, 0],
];
```

> 联通 花费最小问题

#### 普利姆算法(加点法)

> 点与点之间 不断找花费最小的连法

- [x] 任选一个点作为起点
- [x] 找到以**当前选中的点**为起点，路径最短的边
- [x] 如果这个边的另一段 没有被联通，那么就连接
- [x] 如果这个边的另一段也早被连接，则看倒数第二短的边
- [x] 重复 2-4 直到所有的点连接，即所有点集合的数量和已经连接的点的数量相等

```js
const max = 1000;
const pointSet = ["A", "B", "c", "D", "E"];
const distance = [
  [0, 4, 7, max, max],
  [4, 0, 8, 6, max],
  [7, 8, 0, 5, max],
  [max, 6, 5, 0, 7],
  [max, max, max, 7, 0],
];

function Node(value) {
  this.value = value;
  this.neighbor = [];
}
// 拿到当前点在pointSet的位置
function getIndex(str) {
  for (let i = 0; i < pointSet.length; i++) {
    if (str == pointStr[i].value) return i;
    return -1;
  }
}
// nowPointSet 当前已经连接点集合
// 根据当前已经有的节点，来进行判断，获取到最短的点
function getMinDisNode(pointSet, distance, nowPointSet) {
  let fromNode = null;
  let minDisNode = null;
  let minDis = max;
  // 根据当前已有的这些点为起点，依次判断连接其他点点距离是多少
  for (let i = 0; i < nowPointSet.length; i++) {
    // 根据 点集 中的位置，找到对应 边集 中的位置
    const nowPointIndex = getIndex(nowPointSet[i].value);
    for (let j = 0; j < distance[nowPointIndex].length; j++) {
      const thisNode = pointSet[j];
      if (
        nowPointSet.indexOf(thisNode) < 0 &&
        distance[nowPointIndex][j] < minDis
      ) {
        fromNode = nowPointSet[i];
        minDisNode = thisNode;
        minDis = distance[nowPointIndex][j];
      }
    }
  }
  from.Node.push(minDisNode);
  minDisNode.Node.push(from);
  return minDisNode;
}
function prim(pointSet, distance, start) {
  const nowPointSet = []; //当前已经连接点集合
  nowPointSet.push(start);
  // 获取最小代价的边
  while (true) {
    const minDisNode = getMinDisNode(pointSet, distance, nowPointSet);
    nowPointSet.push(minDisNode);
    if (nowPointSet.length === pointSet.length) {
      break;
    }
  }
}

prim(pointSet, distance, "C");
```

#### 克鲁斯卡尔算法(加边法)

1. 选择最短的边进行连结
2. 要保证边连结的两端至少有一个点是新的点
3. 或者这个边是将两个部落进行连结的
4. 重复 1 - 3 直到所有的点都联通位置

```js
// 判断两个点能不能连接
function canLink(resultList, tempBegin, tempEnd){
    let beginIn = null;//记录起始点 所在部落 tempBegin
    let endIn = null;//记录终点 所在部落 tempEnd
    for (let i = 0 ; i < resultList.length ; i ++) {
        if (resultList[i].indexOf(tempBegin) > -1) {
            beginIn = resultList[i];
        }
        if (resultList[i].indexOf(tempEnd) > -1) {
            endIn = resultList[i];
        }
    }
    //两个点都是新的点（都不在任何部落里）——可以连接，产生新的部落
    // begin在A部落，end没有部落 —— A部落扩张一个村庄
    // end在A部落，begin没有部落 ——A部落扩张一个村庄
    // begin在A部落，end在B部落 ——将AB两个部落合并
    // begin和end在同一个部落，——不可以连接
    if (beginIn != null && endIn != null && beginIn == endIn) {
        return false;
    }
    return true;
}


function link(resultList, tempBegin, tempEnd){
    let beginIn = null;//记录起始点 所在部落 tempBegin
    let endIn = null;//记录终点 所在部落 tempEnd
    for (let i = 0 ; i < resultList.length ; i ++) {
        if (resultList[i].indexOf(tempBegin) > -1) {
            beginIn = resultList[i];
        }
        if (resultList[i].indexOf(tempEnd) > -1) {
            endIn = resultList[i];
        }
    }
    if (beginIn == null && endIn == null) {// 两个点都是新的点（都不在任何部落里）——可以连接，产生新的部落
        var newArr = [];
        newArr.push(tempBegin);
        newArr.push(tempEnd);
        resultList.push(newArr);
    } else if (beginIn != null && endIn == null) {// begin在A部落，end没有部落 —— A部落扩张一个村庄
        beginIn.push(tempEnd);
    } else if (beginIn == null && endIn != null) {// end在A部落，begin没有部落 ——A部落扩张一个村庄
        endIn.push(tempBegin);
    } else if (beginIn != null && endIn != null && beginIn != endIn) {// begin在A部落，end在B部落 ——将AB两个部落合并
        var allIn = beginIn.concat(endIn);
        var needRemove = resultList.indexOf(endIn);
        resultList.splice(needRemove, 1);
        needRemove = resultList.indexOf(beginIn);
        resultList.splice(needRemove, 1);
        resultList.push(allIn);
    }
    tempBegin.neighbor.push(tempEnd);
    tempEnd.neighbor.push(tempBegin);
}



function kruskal(pointSet,distance){
    let minDis = max;
    let begin = null;
    let end = null;

    // 得到 最短边的两个点
    for(let i = 0;i<distance.length,i++){
        const tempBegin = pointSet[i];//点集合 和    边集合是一一对应的
        for(let j = 0 ; j < distance[i].length,j++){
            const tempEnd = pointSet[j]
            if(i === j  && distance[i][j] < minDis && canLink(resultList, tempBegin, tempEnd)){
                    minDis = distance[i][j];
                    begin = tempBegin;
                    end = tempEnd;
            }
        }
    }
   // 连接这两个点
     link(resultList, begin, end);
}

kruskal(pointSet,distance)

```

### 234 树

> 一棵树，最多有 4 个叉

#### 背景

- [x] 树的层级越少，查找效率越高
- [x] 怎么样才能让二叉平衡排序树的层树变的更少
  > 如果不是二叉，层数会更少

#### 特点

- [x] 每个节点最多存储 3 个数据
- [x] 永远是平衡的
- [x] 子节点永远在最后一层

#### 改进

- [x] 希望能简化二叉树
- [x] 希望依旧保留多叉
- [x] 希望依旧单节点中存放多个值

### 红黑树

> 红 虚拟节点 黑 实际节点
