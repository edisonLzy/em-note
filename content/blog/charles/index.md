---

     title: charles
     date: 2021-08-29T02:22:00.987Z
     description: charles

---

- 开启 mac 抓包: Proxy - `macOS proxy`
- 禁止使用缓存: Tools - `no caching`
- 抓取 https:

1. Help - `SSL proxy` - install root certificate : 安装根证书 , 并设置信任

![截屏2021-08-27 下午5.55.28](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-27%20%E4%B8%8B%E5%8D%885.55.28.png)

2. Proxy-setting : 开启 enable transparent HTTP proxying

![截屏2021-08-27 下午6.00.01](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-27%20%E4%B8%8B%E5%8D%886.00.01.png)

3. ssl proxying setting: 添加需要抓包的地址

![截屏2021-08-27 下午6.01.05](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-27%20%E4%B8%8B%E5%8D%886.01.05.png)

- 断点的使用: 可用于更改请求的数据或者响应的数据

1. 开启断点: Proxy - enable Proxy

```shell
cmd + k
```

2. 右键选中想要打断点的接口或者点击 图标

![截屏2021-08-28 上午10.48.46](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-28%20%E4%B8%8A%E5%8D%8810.48.46.png)

2. Proxy - breaking setting

- 弱网环境模拟

1. 点击小乌龟
2. 开启弱网环境和设置参数: proxy - throttle setting

![截屏2021-08-28 上午11.04.18](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-28%20%E4%B8%8A%E5%8D%8811.04.18.png)

- 本地资源映射

1. 设置了映射关系的请求,直接使用本地的资源作为响应结果，而不会向服务器在请求。

1. 保存本次接口的响应信息到本地

![截屏2021-08-29 上午9.54.09](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-29%20%E4%B8%8A%E5%8D%889.54.09.png)

3. 设置本地资源映射关系: Tools - map local

![截屏2021-08-29 上午9.57.25](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-29%20%E4%B8%8A%E5%8D%889.57.25.png)

- 接口测试

1. 选中对应的接口点击 🖊️ 图标,即可修改请求的参数

   ![截屏2021-08-29 上午10.07.37](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-29%20%E4%B8%8A%E5%8D%8810.07.37.png)

## 注意

1. record setting 可能会不显示是抓的包![截屏2021-08-28 上午10.03.53](https://note-1300941899.cos.ap-nanjing.myqcloud.com/%E6%88%AA%E5%B1%8F2021-08-28%20%E4%B8%8A%E5%8D%8810.03.53.png)
