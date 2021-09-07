---

     title: 数据结构
     date: 2021-09-07T14:13:23.834Z
     description: 数据结构

---

## 定义

- 数据结构研究的是数据如何在计算机中进行组织和存储，使得我们可以高效的获取数据或者修改数据
- 大量的算法,都是已数据结构为基石
- 数据结构研究的是 内层中的 `增删改查`

## 分类

![截屏2021-08-29 下午9.32.02](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-29%20%E4%B8%8B%E5%8D%889.32.02.png)

## 数组

- java 中申明一个数组

```java
int[] arr = new int[10]

int[] scores = new int[]{100,99,66}
```

- addLast 添加元素

1. 给数组的 size 赋值即可

```java
public void addLast(int e){
   add(size, e)
}
// O(1)
public void addFirst(int e){
   add(0, e)
}
```

- 给指定位置添加元素

1. 将指定位置之后的所有元素向后面移动

```java
// O(n * 1/2) === O(n)
public void add(int index, int e){
  if(size == data.length){
     throw new IllegalArgumentException('xxx')
  }
  if(index < 0 ||  index > size){
     //保证 数组是紧密的
     throw new IllegalArgumentException('xxx')
  }
  for(int i = size - 1; i >= index ; i --){
     data[i + 1] = data[i];
  }
     data[size++] = e;
}
```

- 查询元素

1. 通过私有化变量的方式,做一些合法的拦截操作

```java
public int get(int index){
  if(index < 0 || index >= size){
     throw new Error('xx')
  }
  return data[index]
}
```

- 修改元素

```java
// O(1)
public int set(int index,int e){
  if(index < 0 || index >= size){
     throw new Error('xx')
  }
  return data[index]
}
```

- contains: 是否包含某个元素

```java
public int contains(int e){

}
```

- find: 查找某个元素的索引
- remove(int index)

1. 将指定索引后面的元素。向前移动 1 位即可

2. size --

```java
// O(n * 1/2) === O(n)
public int remove(int index){
    if(index < 0 || index >= size){
     throw new Error('xx')
  }
  int ret = data[index];
  for(int = index + 1 ; i < size ; i ++){
    data[i - 1] = data[i]
  }
  size --;
  return ret
}
```

### 特点

- 快速查询: 知道索引的时候,复杂度为 O(1)
- 不可变性(js 做了自动扩容的优化)

### 动态数组

- 新建一个新的数组,将老数组中的每一项赋值到 新的数组中
- 将指向老数组的指针指向新的数组, java 会自动回收老数组

### resize 的复杂度分析

- 均摊复杂度：addLast 和 removeLast 均为 O(1)
- 复杂度震荡: 使用 Lazy 策略解决复杂度震荡

## GET

1. Java 数组开辟的空间默认值为 0
2. 为什么数组的查询速度快
