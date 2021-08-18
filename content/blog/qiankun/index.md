#

---

     title: qiankun
     date: 2021-08-18T14:16:37.156Z
     description: qiankun

---

### 接入流程

- 创建主应用

```shell
create-react-app base
```

1. 注册子应用

```js
import { registerMicroApps, start } from 'qiankun';

const loader = (loading) => {
    console.log(loading)
}
// registerMicroApps([apps],{ 生命周期 })
registerMicroApps([
    {
        name: 'm-vue', // 应用名称
        entry: '//localhost:20000', // 子应用的服务地址
        container: '#container', // 子应用挂载的容器
        activeRule: '/vue', // 匹配到 /vue的时候,就会通过 fetch 加载子应用
        loader, // 加载状态
    },
    {
        name: 'm-react',
        entry: '//localhost:30000',
        container: '#container',
        activeRule: '/react',
        loader
    }
], {
    beforeLoad: () => {
        console.log('加载前')
    },
    beforeMount: () => {
        console.log('挂在前')
    },
    afterMount: () => {
        console.log('挂载后')
    },
    beforeUnmount: () => {
        console.log('销毁前')
    },
    afterUnmount: () => {
        console.log('销毁后')
    },
})
start({
    sandbox:{
        experimentalStyleIsolation:true // 启用 css-module实现样式隔离
        strictStyleIsolation:true // 启用 shadowDOM 实现样式隔离
    }
});

```

- 接入子应用

1.  设置 publicPath

2.  子应用需要允许跨域: 父应用通过 fetch 的方式请求子应用的资源,所以需要允许跨域

3.  子应用需要使用 umd 格式进行打包

4.  暴漏接入协议

5.  在 mount 的时候,执行子应用挂载逻辑,通过 props 获取 注册信息，可以获取子应用渲染的容器。

6.  通过 window.\_\_POWERD_BY_QIANKUN 内置变量，用于判断当前子应用是否是由父应用加载的。可用于让子应用单独运行

### 样式隔离

- 默认情况下,切换子应用它会使用动态加载样式

- 父子应用样式隔离:

1. 写法约束: BEM 规范
2. 工程化解决方案: CSS module 动态生成一个前缀

```js
start({
  sandbox: {},
});
```

3. shadowDOM: 注意生成的影子 DOM 中不会 body 标签,因此在 body 上面的样式会不生效

### js 沙箱

1. 切换应用的时候,还原 window

## TODO

1.  // 表示什么

## get

- 官方推荐的重写 cra 配置的包 https://www.npmjs.com/package/@rescripts/cli

1. 端口需要 .env 环境配置文件设置

```shell
PORT=30000
WEB_SOCKET_PORT=30000
```

2.  script 执行命令 需要通过 rescripts 执行

3.  每一个配置都是一个函数,需要返回更改后的 config 对象
