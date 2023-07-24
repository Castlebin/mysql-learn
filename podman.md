# 本地安装 podman 服务

https://podman.io/docs/installation


## Mac 系统手动安装 podman
1. 安装 podman
```bash
brew install podman
```

2. 启动并验证 podman 机器
```bash
podman machine init
podman machine start
```

3. 查看 podman 的安装信息
```bash
podman info
```

## 使用 podman 安装 MySQL
这里是用 percona:5.7.35 。将容器的 3306 端口映射到本地的 3305 端口。并且在容器中设置了 root 用户的密码为 $my_password （请改成自己的密码）。
```bash
podman run -itd --name percona-5.7 -p 3305:3306 -e MYSQL_ROOT_PASSWORD=$my_password percona:5.7.35
```

之后就可以开始愉快的在本地做实验了

## podman exec 进入容器
```bash
podman exec -it percona-5.7 bash
```
进入上面启动好的 percona-5.7 容器，打开它的 bash 终端。



## podman stop 停止容器
```bash
podman stop percona-5.7
```

## podman start 启动容器
```bash
podman start percona-5.7
```




