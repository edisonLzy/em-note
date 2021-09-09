---

     title: mongodb
     date: 2021-09-09T14:28:26.300Z
     description: mongodb

---

[官网](https://docs.mongodb.com/manual/)

## 基本概念

- mongodb 属于 nosql 的文档型数据库，存储速度极快。
- 通常用于存储无关联的数据
- 数据格式丰富灵活，不同于 mysql 的列是固定的，从而导致其数据结构难以有效控制。
- mongodb 无需建表

### db: 数据库

- 类似于 mysql

### collection: 集合

- 类似于 mysql 中的 `table`

### document: 文档

- 类似于 mysql 中的 记录
- Primary key: 每一个 文档都有一个 `主键`
- filed: 文档中的字段

## 常见命令

- mongo 通过 `shell` 进行交互，而非 sql 语句。

### 查看数据库

- show dbs; : 查看所有数据库
- db; : 显示当前使用的数据库

1. Mongodb 中允许使用不存在的数据库,当向数据库中插入文档的时候，则会自动创建该数据库

- db.stats(): 查看数据库状态
- show collection; :查看数据库中的所有集合

### 切换数据库

- use 数据库名;

```shell
# 存在的数据库
use test;
# 不存在的数据库
use test11;
```

### 插入数据

- 关于 `_id`

1. 如果没有指定 `_id` ，则会自动添加一个字段作为`_id`作为 主键
2. 自动的主键是一个`ObjectId`对象
3. 使用该对象也可以把某个字符串还原成 ObjectId 对象，且该对象中还可以通过 `getStampTime`获取时间等信息

```shell
ObjectId('xx')
```

- db.collection.insertOne({})

```shell
db.user.insertOne({
 name: 'ee',
 age: 18
})
```

- db.collection.insertMany([])

```shell
db.user.insertMany([{
 name: 'ee',
 age: 18
}])
```

### 查询

- db.collection.find()

```shell
# 查询所有
db.user.find({})
```

### 修改

### 删除

## TODO

- Roto 3T
- 第三节

##
