---
title: Docker 中运行 OpenWrt 旁路网关
---

树莓派里直刷 OpenWrt 当旁路网关用时，资源使用率长期不到 5% 比较浪费，OpenWrt 也不太能胜任通用 Linux 的功能，通过 Docker 的方式运行 OpenWrt 可以完美解决这个问题，树莓派依然可以保留通用 Linux 能力。

Docker Image：[sulinggg/openwrt](https://hub.docker.com/r/sulinggg/openwrt)

详细说明：https://mlapp.cn/376.html

我是在 4B + Manjaro 上跑的，其它发行版应该差别不大。



### 1. 网卡混杂

> 混杂模式是指一台机器的网卡能够**接收所有经过它的数据流**，而不论其目的地址是否是它。
>
> 一般计算机网卡都工作在非混杂模式下，此时网卡只接受来自网络端口的目的地址指向自己的数据。 
>
> 当网卡工作在混杂模式下时，网卡将来自接口的所有数据都捕获并交给相应的驱动程序。

以 Docker 方式运行 OpenWrt 时，树莓派网卡不仅要接收它自己的数据，还要接收流向 OpenWrt 的数据，因此需要打开网卡混杂模式。

Manajro 下临时打开：`sudo ip link set eth0 promisc on`，这种方式重启后失效。

永久打开：[Promiscuous mode](https://wiki.archlinux.org/title/Network_configuration#Promiscuous_mode)，添加文件 */etc/systemd/system/promiscuous@.service*，增加以下内容。

```
[Unit]
Description=Set %i interface in promiscuous mode
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/bin/ip link set dev %i promisc on
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

通过命令 `systemctl enable promiscuous@eth0.service` 开启。（不同网卡请修改 **eth0**）



### 2. 安装 Docker

Manjaro 下安装软件太简单。

```shell
# 安装
$ sudo pacman -S docker
# 开机启动
$ systemctl enable docker.service
# 启动
$ systemctl start docker.service
# 添加用户到 docker 组，username 改成自己，需要注销或者重启下生效
$ sudo usermod -aG docker [username]
# 测试 hello world
$ docker run hello-world
Hello from Docker!
...
```



### 3. 创建 Docker 网络

```shell
# 创建
$ docker network create -d macvlan --subnet=192.168.0.0/24 --gateway=192.168.0.1 -o parent=eth0 macnet
# 查看
$ docker network ls
```

**192.168.0.x** 取决于树莓派所在子网，要与树莓派在同一子网。

**eth0** 是网卡；**macnet** 是创建的网络名，可自取。



### 4. 启动 Docker 容器

先拉取镜像，不同型号 tag 不一样，4B 是 rpi4

```shell
# 拉取
$ docker pull registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4
# 查看
$ docker images
```

启动容器：

```shell
# 启动
$ docker run --restart always --name openwrt -d --network macnet --privileged -v /home/panda/Shares:/mnt/Shares registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4 /sbin/init
# 查看
$ docker ps -a
```

`--restart always` 表示容器退出时始终重启，使服务尽量保持始终可用；

`--name openwrt` 容器的名称；

`-d` 运行在 Daemon 模式；

`--network macnet` 容器加入 `macnet`网络；

`--privileged ` 运行在特权模式下；

`-v /home/panda/Shares:/mnt/Shares` 挂载 Manjaro 的目录到容器（可选）；

`registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4` 为 Docker 镜像名，因容器托管在阿里云 Docker 镜像仓库内，所以在镜像名中含有阿里云仓库信息；

`/sbin/init` 定义容器启动后执行的命令。



### 5. 配置 Docker 容器

进入容器：

```shell
# openwrt 是启动时定义的名称
$ docker exec -it openwrt bash
```

更改配置：

```shell
$ vim /etc/config/network
```

```
config interface 'lan'        
    option type 'bridge' // 删掉，不然国内网站打不开
    option ifname 'eth0'
    option proto 'static'
    option netmask '255.255.255.0'
    option ip6assign '60'
    option ipaddr '192.168.0.100' // 修改为树莓派子网
    option gateway '192.168.0.1' // 修改为树莓派子网
    option broadcast '192.168.0.255' // 修改为树莓派子网
    option dns '192.168.0.1' // 修改为树莓派子网
```

重启网络：

```shell
$ /etc/init.d/network restart
```



### 6. 登录

浏览器输入 http://192.168.0.100 进入控制面板。

用户名/密码：root/password。

Samba 共享如果需要加密，模板中**注释掉** `invalid users = root`，同时命令行中运行 `smbpasswd -a root` 为 root 用户添加访问密码。