---

     title: websocket
     date: 2021-08-31T14:04:03.753Z
     description: websocket

---

[课件](http://www.zhufengpeixun.com/strong/html/98.websocket.html)

## 连接建立

- 升级过程

1. 请求升级(http 协议)

```text
Connection:Upgrade
Upgrade: websocket
```

- Accept 的计算

1. 利用 sha1 计算出摘要

```js
const crypto = require("crypto");
const code = "";
function toAcceptKey(wsKey) {
  return crypto
    .createHash("sha1")
    .update(wsKey + code)
    .digest("base64");
}
```

- 数据帧格式

1. websocket 的最小传输单位是帧,由一个或者多个帧组成一条完整的消息

## 实战

- 安装 ws

```shell
npm i ws -D
```

- 服务端编写

1. socket:套接字

```js
const { Server } = require("ws");
const wsServer = new Server({
  port: 8888,
});
wsServer.on("connection", (socket) => {
  // 监听客户端发过来的消息
  socket.on("message", (msg) => {
    // 发送消息给客户端的消息
    socket.send(msg);
  });
});
```

- 客户端

```js
const socket = new WebSocket("ws://localhost:8888"); // 链接服务器
// 当连接打开或者建立后出发回调
socket.onopen = function () {
  // 发送消息给服务端
  socket.send("server");
};
// 监听服务器发送来的消息
socket.onmessage = function (event) {
  console.log(event.data);
};
function send() {}
```

## GET

1. 1 个字节 8 个位
2. 按位异或: 相同为 0,不相同为 1
3. FIN:1 表示最后一个分片
4. opcode: 表示 数据的类型
5. Payload length:
6. 大端序和小端序

```js
const buffer = Buffer.from([0b00000001, 0b00000000]);
```
