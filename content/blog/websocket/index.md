---

     title: websocket
     date: 2021-09-01T14:05:25.339Z
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

## 实现

- net 模块: 基于 TCP 传输层协议

```js
const net = require("net");
```

- crypto: 加密模块

```js
class Server extends EventEmitter {
  constructor(options) {
    super(options);
    this.options = options;
  }
}
```

- events: 实现发布订阅
- 握手过程实现

```js
const { EventEmitter } = require("events");
const net = require("net");
const { toAcceptKey, toHeaders } = require("./utils");
class Server extends EventEmitter {
  constructor(options) {
    super(options);
    this.options = options;
    //创建一个TCP服务器 每当服务器收到客户端连接后
    this.server = net.createServer(this.listener);
    this.server.listen(options.port || 9000);
  }
  listener = (socket) => {
    socket.setKeepAlive(true);
    socket.on("data", (chunk) => {
      // 接受到客户端请求到时候触发
      if (chunk.toString().match(/Upgrade: websocket/)) {
        // 说明是 http握手协议
        this.upgradeProtocol(socket, chunk.toString());
      } else {
        // socket通信协议
      }
    });
  };
  upgradeProtocol(socket, chunk) {
    //1. 解析请求信息: 起始行 \r\n 请求头 \r\n\r\n 请求体
    const rows = chunk.split("\r\n");
    //2. 获取 headers
    const headers = toHeaders(rows.slice(1, -2));
    //3. 重新计算 Sec-WebSocket-Key
    const wsKey = headers["Sec-WebSocket-Key"];
    const acceptKey = toAcceptKey(wsKey);
    //4. 组装响应体
    const response = [
      "HTTP/1.1 101 Switching Protocols",
      "Upgrade: websocket",
      "Connection: Upgrade",
      `Sec-WebSocket-Accept: ${acceptKey}`,
      "\r\n",
    ].join("\r\n");
    //5. 响应客户端
    socket.write(response);
  }
}
module.exports = {
  Server,
};
```

- 解析数据帧

1. FIN

```js
// chunk[0] 表示第一个字节
const FIN = (chunk[0] & 0b10000000) === 0b10000000;
```

2. opcode

```js
// 保留后面四个位
const opcode = chunk[0] & 0b00001111;
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

## TODO

1. Buffer.alloc
