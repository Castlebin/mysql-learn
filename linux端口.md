## Centos开放指定端口命令

- 1、开启防火墙

``` bash
sudo systemctl start firewalld
```

- 2、开放指定端口 [6666]

```bash
sudo firewall-cmd --zone=public --add-port=6666/tcp --permanent
```

--add-port=portid[-portid]/protocol
命令含义：
--zone #作用域
--add-port=6666/tcp #添加端口，格式为：端口/通讯协议
--permanent #永久生效，没有此参数重启后失效

- 3、重启防火墙

```
sudo firewall-cmd --reload
```

- 4、查看端口号
netstat -nltp //查看当前所有tcp端口·  
netstat -nltp |grep 6666 //查看所有1935端口使用情况·

CentOS默认开放的本地端口范围
系统本地开放端口的范围：（默认30000多到60000多）

[root@linux2 ~]# vim /etc/sysctl.conf
net.ipv4.ip_local_port_range = 10240 65000 #建议不要小于10000 ，因为本机很可能会有类似如8080这样的服务
