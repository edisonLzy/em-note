#

---

     title: SingleSpa
     date: 2021-08-18T14:16:37.053Z
     description: SingleSpa

---

### 脚手架

- 安装

```shell
npm i create-single-spa -g
```

- 创建根应用

1. root 表示生成根应用

```shell
 create-single-spa  base
```

2. 通过 registerApplication 注册子应用

```js
import { registerApplication } from "";

registerApplication({
  activeWhen: [], // 匹配到路径 执行app方法,通过Systemjs 动态加载子应用
  app() {
    // 通过 SystemJS  加载子应用
    System.import("子应用的名称");
  },
  name,
  customProps, // 传递给子应用的数据
});
```

- 创建子应用

1. application / parcel 表示可以共用的模块或者应用

```shell
create-single-spa react-demo
create-single-spa vue-demo
```

2. vue 接入 可以使用 vue-cli-single-spa-plugin 改写入口文件，方便接入 主应用

3. 子应用的路由的 basename 需要注意

4. react 应用 接入的时候 需要 在主应用的 import-map 中 申明 react-dom 和 react 项目

5. vue 应用内置了 vue-router 不需要在主应用单独配置 externals,而 react-router 则需要

## TODO

- [] CSP 是什么
