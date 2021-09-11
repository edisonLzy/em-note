---

     title: mongodb
     date: 2021-09-11T03:10:10.061Z
     description: mongodb

---

[官网](https://docs.mongodb.com/manual/)

## 基本概念

- mongodb 属于 nosql 的文档型数据库，存储速度极快。
- 通常用于存储无关联的数据或者关联性不强的数据库
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

## 驱动

### mongodb

- 官方的驱动

### mongoose

- 安装

```shell
npm i mongoose
```

- 创建链接

1. mongoose.connect("协议://主机名称/数据库名称")
2. [spec](https://docs.mongodb.com/manual/reference/connection-string/)

```js
const mongoose = require("mongoose");
mongoose.set(key, value); // 创建链接之前 可以配置一些全局的行为
mongoose.connect("mongodb://localhost/test");
mongoose.connection.on("open", () => {
  // 链接成功触发
});
```

- 模型定义

1. 通过 schema 定义模型: [schema 的第二个参数](https://mongoosejs.com/docs/guide.html#options)
2. `模型名称`对应数据库中的 collection (`集合`)
3. [字段的约束](https://mongoosejs.com/docs/schematypes.html)
4. 更具带有索引的字段， 查询可以提高查询效率
5. sub-schema's: 可以复用的 schema，用于 `type`

```js
// 定义 schema
const userSchema = new mongoose.Schema({
   loginId: String // 可以是 js中基本包装类型
   loginId: {
       // 约束
       type: String,
       required: true,
       trim: true //写入数据的时候是否去除字符串的空格
       index: true //表示将字段设置为索引，用于提高查询效率
       unique: true // 特殊索引，唯一索引 和 index 取一即可
	 },
   pwd:{
        select: true // 表示查询用户的时候，查询结果不会包含该字段
   }
   address: {
      type : {
        province: String
        city: String
      }，
     _id:false //表示不会为该子对象生成 _id
   }
},{
  versionKey: false // 表示不会为每个文档生成 __v
})
// 定义 模型
module.exports = mongoose.model('模型名称',userSchema)
```

```js
const operationSchema = new mongoose.Schema({
  userid: {
    // 外键
    type: mongoose.Types.ObjectId,
  },
  extraInfo: {
    // 任意类型
    type: mongoose.Schema.Types.Mixed,
  },
});
```

## CURD

### 新增

- 通过 schema 定义 字段的索引 会反映到数据库中

- 通过面向对象的方式创建

```js
const User = require("./models");
const u = new User({
  // 字段
});
u.save(callback);
```

- 通过模型的 create 方法进行创建, 且可以传入多个对象。如果第二个参数是配置对象，则第一个参数需要是一个数组。

```js
User.create({
  // 字段
});
User.create(
  [
    {
      // 字段
    },
  ],
  op
);
```

- 可以通过模型的 validate 方法进行验证
- 不存在 schema 中定义的字段会被忽略

### 查询

### 修改

- 可以通过直接操作 create 方法返回的对象来 修改文档

```js
const res = await User.create(obj);
res.loginId = "xx";
res.save(); // 同步到 数据库中
```

## TODO

- Roto 3T
- 第三节

##
