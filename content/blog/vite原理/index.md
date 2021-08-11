---

     title: vite原理
     date: 2021-08-11T13:16:24.527Z
     description: # vite原理

---

## 实现流程

- [x] 基于 Koa 启动一个 http 服务
- [x] 通过 process.cwd 获取 当前运行脚本的工作目录
- [x] 实现静态服务(koa static)，将请求结果放到 `ctx.body上面`
- [x] 解析 import 语法，重写路径(改写 main.js 中的资源路径)
- [x] 服务器端拦截`/@module`开头的请求,将其指向当前工作目录的`node_modules`中
- [x] 解析 vue 模块(创建 针对 vue 资源文件的映射表)

> 根据不同的引入路径,创建不同的 Vue 各个模块的映射表。

- [x] 解析 html `注入环境变量`
- [x] 解析.vue 文件

> 利用 compiler 模块 进行编译

## 基于 koa

> 1. 基于 koa 启动一个服务
> 2. koa 中的所有异步事件都应该是一个 promise

```js
const Koa = require("koa");
function createServer() {
  const app = new Koa();
  return app;
}
// 中间件注册
const static = require("koa-static");
// root:vite运行的目录
app.use(static(root));

app.use(async (ctx, next) => {
  ctx.response.is("js"); //判断文件是否是js文件
});
```

## es-module-lexer

> 解析 import 用法 成 ast 语法 🌲

```js
const { parse } = require("es-module-lexer");
```

## magic-string

> 字符串不变性?

```js
let magicString = new MagicString(source);
magicString.overwrite(s,e,newString)

magicString.toString()将处理结果转换为字符串
```

# todo

- [ ] 如何将流转换成字符串，(通过流来读取文件)

```js
// 通过 stream.on 监听data事件 和 end事件实现
```

- [ ] package.json 的 module 的作用
- [ ] 如何实现 可插拔的工具库
- [ ] js 中那些事件不会冒泡
- [ ] 渲染流程中的 render 函数如何调用的
- [ ] 如何利用 map 保证 一个 dom 只绑定一次事件
- [ ] open source https://opensource.guide/how-to-contribute/
- [ ] 再看 洋葱模型
- [ ] 热更新是如何实现的
- [ ] vue 模版编译原理

# get

1. import from 会默认发送一个 http 请求

2. `stream instanceOf Stream` 判断是不是流

3. $& 保留当前的匹配结果
