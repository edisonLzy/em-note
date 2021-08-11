---
title: Vite source
description: 'Vite source'
date: Wed Aug 11 2021 17:14:56 GMT+0800 (中国标准时间)
---


- getCurrentInstance 获取当前的组件实例
- vite 中的别名在 css 中的也是同样的解析规则

```html
<!-- module 开启 css module ,默认中通个 $style进行访问-->
<style module>
  .log {
    background-image: url(@/assets/xx.png);
  }
</style>
```

- css 预处理语言已默认支持
- eslint 静默模式检查

```shell
eslint --quite
```

- 基于 husky 的配置 githook

> https://www.npmjs.com/package/yorkie

- 插件开发

1. resolveId 用于自定义确认函数, 确定当前插件的工作范围(即需要处理那些模块)

```js
resolveId(source){
  // 不处理直接 返回 null
  return null
}
```

2. load 指定如何加载 某个模块(`id`)

```js
load(id){
  // 加载 vm模块时会返回 aa
  if(id ==='vm'){
    return `export default 'aa'`
  }
  return null
}
```

3. transform 将 load 加载的代码进一步处理

```js
// code 是代码块的内容
// id 是将来请求的url
transform(code,id){
    if(/vue&type=i18n/){
       return export default Comp=>{

    }
    }
    return null
}

<style>

 </style> 浏览器将会发出 xxxxvue&type=i18n的请求
```
