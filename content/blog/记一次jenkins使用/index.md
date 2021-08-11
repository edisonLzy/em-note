---

     title: 记一次jenkins使用
     date: 2021-08-11T13:11:26.990Z

---

## Got permission denied while trying to connect to the Docker daemon socket at

```shell
# 将jenkins添加到docker用户组
usermod -a -G docker jenkins
# 重启jenkins 和 docker
systemctl restart dockerd.service
systemctl restart jenkins.service
# 查看docker用户组是否有 jenkins 用户
cat /etc/group | grep docker
```
