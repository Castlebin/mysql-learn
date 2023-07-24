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

现在这个时候，使用 MySQL 客户端连接 3305 端口，发现是连接不上的。因为MySQL 容器默认只允许本地连接。所以我们需要进入容器中，登录上 MySQL 服务，然后新建一个用户（不要修改 root 用户的权限！），并给它分配远程连接的权限。

## podman exec 进入容器
```bash
podman exec -it percona-5.7 bash
```
进入上面启动好的 percona-5.7 容器，打开它的 bash 终端。

## podman exec 进入容器中的 MySQL
```bash
mysql -uroot -p$my_password
```

## 查看当前的用户和权限
```sql
select host,user,plugin,authentication_string from mysql.user;
```
可以看到 root 用户当前是只允许 在 localhost 上登录的。

## 创建一个新的 MySQL 用户，并且给它分配权限
```sql
create user 'my_user'@'%' identified by '$my_password';
grant all privileges on *.* to 'my_user'@'%';
-- flush privileges;
```
或者
```sql
create user my_user;
ALTER USER 'my_user'@'%' IDENTIFIED WITH mysql_native_password BY '$my_password'
```

现在你就可以愉快的在本地用 MySQL 客户端连接 3305 端口了。


## podman 安装 MySQL 8.0  (其他同理，比如创建用户允许远程登录)
```bash
podman run -itd --name mysql-8 -p 3308:3306 -e MYSQL_ROOT_PASSWORD=$my_password mysql:8.0.32
```


## podman stop 停止容器
```bash
podman stop percona-5.7
```

## podman start 启动容器
```bash
podman start percona-5.7
```




