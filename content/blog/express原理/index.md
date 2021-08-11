---

     title: express原理
     date: 2021-08-11T13:19:00.370Z
     description: express原理

---

![屏幕快照 2020-12-21 下午9.37.01](/Users/zhiyu/Desktop/屏幕快照 2020-12-21 下午 9.37.01.png)

## TODO

- [x] 中间件原理（next 方法）

> 1. 处理异步的情况,代码结偶
> 2. Router=>Layer=>Route=>Layer

- [x] express 路由如何实现

> 1. 每一次匹配的路径,就对应 Router 中 stack 属性的 layer

- [x] express 做了什么

> 1. 提供的路由系统
> 2. 封装了 http-sercer
> 3. 中间件机制

- [x] compose 函数对函数的要求

## 流程

### createApplication

**get**

> 注册路由信息

```js
1. 将路由规则 存储到数组中
```

**listen**

> 开启服务,匹配路由信息

```js
1. 通过 http创建服务（http.createServer）
2. 匹配 get等路由方法，注册的路由信息,并执行相应的handler

```

### Application

> 1. 应用类
> 2. 对外暴露接口
> 3. 启动服务
> 4. 处理路由匹配不到的请求(done 方法)

```js
1. 通过 http创建服务（http.createServer）
2. 匹配 get等路由方法，注册的路由信息,并执行相应的handler

```

### Router

> 1. 路由类
> 2. 路由拦截请求(get),注册路由信息(layer)到 stack。(创建 layer 和 Route 的关联关系)
> 3. 处理请求(handle),

### Layer

### Route

> 1. Dispatch 方法,

## 中间件

> 1. app.use(match,middlewares)
> 2. `/` 会匹配所有的路径，如果 match 是 `/`也可以省略
> 3. 一个中间件对应一层 layer,中间件的 route 是 undefined`这也是中间件的标示`

# get

1. require 一个 npm 包的查找顺序

> package.json 中的 main => index.js

2. 类对外暴漏接口，而不是属性
3. methods 库获取所有的请求方法
4. `res.end`只能返回 string 或者 buffer
