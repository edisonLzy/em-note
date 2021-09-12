---

     title: mongodb
     date: 2021-09-12T12:28:25.772Z
     description: mongodb

---

[官 网](https://docs.mongodb.com/manual/)

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

### 备份

```shell
# -d 备份的数据库
# -o 备份输出的目录
mongodump -d test -o ./
```

### 恢复

```shell
# -d 恢复数据库的名字
mongorestore -d test ./backup/test
```

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
const cursor = db.user.find({})
```

- 返回值是一个 游标对象 , 通过 `next` 方法来 迭代 得到所有的查询结果

1. count
2. size

```js
const cursor = db.user.find({});
cursor.next(); // 获取 下一条 文档
cursor.hasNext(); // 判断 是否还能 向下移动
cursor.skip(15); // 跳过指定 文档数量
cursor.limit(2); // 获取两条 文档
cursor.sort({
  age: 1, // 按照年龄 升序排序
}); // 1 表示升序 -1 表示降序
```

- 条件

1. 是否正则进行模糊查询

```js
db.collection.find({
  name: "xxx",
});

db.collection.find({
  name: /7$/,
  age: /^王/,
});
```

2. 操作符: $开头

```js
{
  $or: [
    {
      name: /7$/,
    },
    {
      age: /^王/,
    },
  ];
}
```

3. 查询子对象

```js
db.user.find({
  "loves.0": "music", // 查询 爱好的第一项是 music的文档
});

db.user.find({
  "address.city": "chengdu", // 查询 城市是 chengdu的数据
});
```

- 投影: find 的第二个参数

1. 相当于 select
2. 除了 \_id 外，其他的字段不能混合编写

```js
find(
  {},
  {
    age: 1,
    "address.city": 1,
  }
);

find(
  {},
  {
    age: 1,
    name: 0,
  }
); // error
```

### 修改

- [update](https://docs.mongodb.com/manual/tutorial/update-documents-with-aggregation-pipeline/)

- db.<collection>.updateOne(filter,update,[loptions])
- db.<collection>.updateMany(filter,update,[loptions])
- update 的写法

1. $rename: 表示修改数据库 字段的名称

```js
{
  $rename: {
    name: "fullname"; // 将 name属性改成 fullname
  }
}
```

2. $unset: 去掉某一些属性

```js
{
  $unset: {
    age: "","address.province":"" // 将 age属性和 address中的province属性删除
  }
}
```

3. $set: 更新字段,如果不存在该字段 则会增加该字段

```js
{
  $set: {
    name: "xxx";
  }
}
```

- 针对于数组的操作

1. 添加一项: $addToSet 不存在则添加 而 $push 则都会添加
2. 添加多项: $each
3. 删除:$pull
4. $表示查询条件匹配的项

### 删除

- db.<collection>.deleteOne
- db.<collection>.deleteMany

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

- 模型定义: 通过模型间接操作 数据库

1. 通过 schema 定义模型: [schema 的第二个参数](https://mongoosejs.com/docs/guide.html#options)
2. `模型名称`对应数据库中的 collection (`集合`)
3. [字段的约束](https://mongoosejs.com/docs/schematypes.html)
4. 根据带有索引的字段， 查询可以提高查询效率
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

- 通过 schema 定义 `字段的索引` 会反映到数据库中

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
- 添加多条数据，建议使用 inertMany()

```js
await User.insertMany(users);
```

### 查询

- <model>.findById(id,{投影})
- <model>.findOne(id,{投影})
- <model>.find(id,{投影})

```js
const documentQuery = User.find({
  age: {},
})
  .skip()
  .limit()
  .sort();

await documentQuery; // 真正执行查询
```

- count 得到的是当前结果的数量
- 查询\_id 的时候，使用字符串即可
- 投影可以写成字符串

```js
await User.find(
  {
    age: {},
  },
  "-address -age"
); // 查询结果不包含 address 和 age
```

- populate 支持关联查询

1. populate('字段名称',头像)

```js
await Operation.find().limit(1).populate("userid"); // userid 是opertion 模型中的字段名称
```

### 修改

- 可以通过直接操作 create 方法返回的对象来 修改文档

```js
const res = await User.create(obj);
res.loginId = "xx";
res.array.push("xx", "yy");
res.save(); // 同步到 数据库中
```

- updateOne  和 updateMany: 默认情况下 更新操作不会触发验证

```js
updateOne(filter, update, {
  runValidators: true, // 更新操作也需要验证
});
```

### 删除

- <model>.deleteOne(filter)
- <model>.deleteMany(filter)

## 索引

- 类似目录,可以快速的定位内容
- 典型的空间换时间的算法
- 在查询带索引的数据的时候，直接查询数据的索引就行

### 创建索引(原生)

- db.<collection>.createIndex(keys,[opts])

```shell
# 1表示升序
db.user.createIndex({
  age: 1
},[opts])
```

1. opts.backgorund: 是否在后台进行索引的创建

- 查询当前集合中的索引

```shell
db.user.getIndexes()
```

- 删除所有的索引

```shell
db.user.dropIndexes()
```

- 删除指定的索引

```shell
db.user.dropIndex('索引名称')
```

- 最佳实践

1. 数据量巨大的时候使用
2. 查询和排序字段使用索引
3. 尽量避免在程序运行过程中频繁的创建和删除索引

## 虚拟属性

```js
new Schema({
  name: String,
  lastName: String,
  fullName: {
    virtual: true,
    get() {
      return this.name + this.lastName;
    },
  },
});
```

## 模型方法

- 可以在实例中调用

```js
const userSchema = new Schema({
  name: String,
  lastName: String,
  fullName: {
    virtual: true,
    get() {
      return this.name + this.lastName;
    },
  },
});

userSchema.methods.print = function () {
  conosole.log(this.name);
};
```

## 静态方法

- 通过 模型直接调用

```js
const userSchema = new Schema({
  name: String,
  lastName: String,
  fullName: {
    virtual: true,
    get() {
      return this.name + this.lastName;
    },
  },
});

userSchema.static("get", async function () {
  // this 表示该模型的实例
  const count = await this.countDocuments(filter);
  return await this.find({});
});
```

## 并发管理

- mongoose 默认 只对 数组进行版本管理，每一次对数组的修改都会更新`__v` 用于记录版本

## TODO

- Roto 3T
- 第三节
- plugin

## get

1. Robo 3T : cmd + r 重新查询
