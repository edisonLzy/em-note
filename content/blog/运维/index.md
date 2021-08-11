---

     title: 运维
     date: 2021-08-11T13:16:24.873Z
     description: # 运维

##

---

## linux

### 命令

- [ ] tail

```shell
tail -f /var/log/nginx/access.log
```

- [ ] cat

## nginx

- [x] nginx 命令

```js
1. 重启nginx
nginx -s reload
2. 查看配置文件是否正确
nginx -t
```

- [x] 每一个 server 对应一个网站

```js
1. include
2. error_page 把服务器端错误 重定向到指定文件上面
3. log_format 指定日志格式
4. access_log 路径 日志格式
```

- [x] 变量

```js
1. $arg_参数名
2. $http_请求头
3. $sent_http_响应头
```

## Docker
