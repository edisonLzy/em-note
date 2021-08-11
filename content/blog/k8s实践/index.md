# k8s实践

## 安装指定版本的k8s

> https://segmentfault.com/a/1190000039144214

## k8s 镜像无法拉取的问题

> ` coredns为例子`

```shell
# 1.  拉取镜像到本地
docker pull coredns/coredns:1.8.0
# 2.  打tag
docker tag coredns/coredns:1.8.0 registry.cn-hangzhou.aliyuncs.com/google_containers/coredns/coredns:v1.8.0
```

## kubeadm初始化警告”cgroupfs“解决

> 编辑docker配置文件/etc/docker/daemon.json

```json
"exec-opts": ["native.cgroupdriver=systemd"]
```

```shell
systemctl daemon-reload
systemctl restart docker
```

##  tc command not found

```shell
yum install -y iproute-tc
```

## yum包管理工具

> https://www.jianshu.com/p/140d36f04ca7
