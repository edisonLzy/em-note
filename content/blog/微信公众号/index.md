---

     title: 微信公众号
     date: 2021-09-18T14:19:44.135Z
     description: 微信公众号

---

- [] 实现 blog 文章自动同步到公众号

- [] 可以结合 ws 来实现消息推送给前端

1. 用户 - 微信服务器 - 我们的配置服务器 - 前端页面

## 客户信息

- 公众号平台设置 `自动回复`

1. 用户 - 微信服务器

2. 消息是固定的 , 需要先配置

- 配置服务器: 根据用户输入进行回复, 微信服务器只负责中转

1. 用户 - 微信服务器 - 配置服务器

2. 配置服务器地址 和 token (`在公众号后台进行配置`).

### co-wechat

- 负责处理微信服务器的请求的`npm包`

```js
import wechat from "co-wechat";
const conf = {
  token: "",
};
router.all(
  "/wechat",
  wechat(conf).middleware(async (message) => {
    // 接受到微信服务器之后的相应
    return "xxx ";
  })
);
```

- 原理

1. xml2js: 由于微信服务器传递数据的格式是 xml,所以需要使用此包将其解析成 js 或者包装 js 成 xml 发送给微信服务器

> https://www.npmjs.com/package/xml2js

2. koa-xml-body: 解析 xml-body  的 koa 中间件

3. 服务器和微信服务器的认证过程 和 https 验证完整性的逻辑一致。通过这个摘要算法以此来确定发送请求的微信服务器

## 服务器 API 调用

- 通过 API 的形式来,调用微信公众号后台的功能模块.

### 获取 access_token

- https://developers.weixin.qq.com/doc/offiaccount/Basic_Information/Get_access_token.html

```js
const tokenCache = {
  access_token: "",
  updateTime: Date.now(),
  expires_in: 7200,
};
get("/getToken", async (ctx) => {
  const url = `获取token的接口`; // 传入 appId 和 appSecret
  const res = await axios.get(url);
  // 更新 tokenCache
});
```

- 保证服务器端和微信服务器端通信安全

### co-wechat-api

- https://www.npmjs.com/package/co-wechat-api

- 更方便调用微信服务器的 API

```js
// 可以将 token保留至  redis 或者 mongo
const api = new WechatAPI(appid, appsecret, accesstoken的存储函数, 取出函数);
```

## GET

- 使用 测试号进行开发

1. http://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index

- ngrok 进行内网穿透 tunnel

1. 把内网的服务映射到外网(订阅信息的时候需要微信服务器调用我们的本地服务)

- koa 直接给 ctx.body 即完成了响应

- axios 请求拦截器可以取消请求吗?

## TODO

- 私服的搭建
