---

     title: Systemjs
     date: 2021-08-15T14:37:50.719Z
     description: Systemjs

---

## Systemjs

- [ ] 实现原理
- [x] 什么是增量迁移
- [ ] Systemjs 是什么

### 微前端解决的问题

1. 技术栈无关: 一个应用可以由不同的技术栈,不同的团队进行开发.

2. 子应用可以独立开发,独立部署

3. 实现增量迁移: 可以在老项目的基础之上再添加功能

### 子应用

1. 暴露接入协议: 包括 bootstrap , mount , unmount 等钩子，供父应用调用
2. 子应用直接可以相互通信
3. 子应用回打包成独立的模块,由父应用加载

### Systemjs

1. 通用的模块加载器,它能在浏览器上动态加载模块(子应用都会打包成模块)
2. 可以用于加载远程链接

## GET

1. webpack.config.js 是函数的情况,可用于获取 `env` 等变量信息

```js
module.exports = (env) => {
  // env === 'development'
  return {
    // 配置对象
  };
};
```

```shell
webpack --env development
```

2. AMD 依赖前置: 在加载特定模块的时候,需要先加载指定的依赖

3. 本地开发服务器

```shell
webpack serve
```

4. script 标签加载的文件,是先执行加载的文件代码还是先触发 `load事件`
