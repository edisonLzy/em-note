---

     title: 微前端
     date: 2021-08-11T13:19:00.554Z
     description: 微前端

---

> 微前端就是将不同的功能按照不同的维度拆分成多个子应用。通过主应用来加载这些子应用。

## Why

**易于维护**

**与技术栈无关**

**为什么不是 iframe**

## How

### Single-spa

> 路由拦截

1. 创建子应用

   > 子引用 导出特定的钩子函数 bootstrap mount unmount

   1.1 安装 single-spa-vue

   ```js
   VUE项目安装 single-spa-vue 用于构建子应用的钩子函数
   ```

   1.2 改造入口文件

   ```js
   import Vue from "vue";
   import App from "./App.vue";
   import router from "./router";
   import singleSpaVue from "single-spa-vue";

   Vue.config.productionTip = false;

   const appOptions = {
     el: "#vue",
     router,
     render: (h) => h(App),
   };

   // 在非子应用中正常挂载应用
   if (!window.singleSpaNavigate) {
     delete appOptions.el;
     new Vue(appOptions).$mount("#app");
   }

   const app = singleSpaVue({
     Vue,
     appOptions: appOptions,
   });

   // 协议注入 导出特定 子应用启动钩子 供父应用调用
   export const bootstrap = app.bootstrap;
   export const mount = app.mount;
   export const unmount = app.unmount;
   ```

   1.3 将子应用打包成 lib

   ```js
   // vue.config.js
   module.exports = {
     configureWebpack: {
       output: {
         library: "singleVue",
         libraryTarget: "umd",
       },
       devServer: {
         port: 8888,
       },
     },
   };
   ```

   1.4 修改路由配置

   > 子应用特定的 路径前缀

   ```js
   const router = new VueRouter({
     mode: "history",
     base: "/vue",
     routes,
   });
   ```

2. 创建父应用

   2.1 安装 single-spa 用于注册子应用，并修改主文件

   ```js
   import Vue from "vue";
   import App from "./App.vue";
   import { registerApplication, start } from "single-spa";
   import router from "./router";
   Vue.config.productionTip = false;

   /*
    * runScript：一个promise同步方法。可以代替创建一个script标签，然后加载服务
    * */
   const runScript = async (url) => {
     return new Promise((resolve, reject) => {
       const script = document.createElement("script");
       script.src = url;
       script.onload = resolve;
       script.onerror = reject;
       document.head.append(script);
     });
   };

   // 注册子应用程序
   registerApplication(
     "vue-child",
     async () => {
       await runScript("http://localhost:8888/js/chunk-vendors.js");
       await runScript("http://localhost:8888/js/app.js");
       return window.singleVue;
     },
     (location) => location.pathname.startsWith("/vue")
   );

   start();

   new Vue({
     router,
     render: (h) => h(App),
   }).$mount("#app");
   ```

   2.2 子应用跳转抛出

   > Loading chunk about failed。在父应用加载子应用会 基于 父应用的 url 加载资源

```js
修改 子应用的publicPath,指定子应用资源从 指定的路径下取获取

publicPath:"http://localhost:8888",
```

3. 样式隔离

# todo

- [ ] webpack5 的联邦模块

- [ ] customEvent 事件通信

- [ ] shadow DOM 如何工作

  ```js
  attachShadown;
  ```

# get

1. 什么是 js 沙箱

**快照沙箱**

> 在应用沙箱挂载或卸载时记录快照，在切换时依据快照恢复环境 (无法支持多实例)

**代理沙箱**

> 基于 es6 proxy ，可以实现多应用沙箱

**nodejs 的沙箱**
