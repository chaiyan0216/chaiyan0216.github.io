---
title: Linux 外接硬盘休眠
---

Linux 外接硬盘有时不会自动休眠，需要借助工具帮助其在空闲时休眠，以降低噪音、节约用电、延长寿命。



### hdparm

```shell
# 安装
$ pacman -S hdparm

# 查询硬盘信息
$ hdparm -I /dev/sdc
# 查询电源管理参数
$ hdparm -B /dev/sdc

# 设置 APM (Advanced Power Management) 参数
# 1-127 允许磁盘休眠，128-254 不允许，255 完全关闭电源管理功能
$ hdparm -B 127 /dev/sdc

# 设置多长时间后开始休眠，参数是 5 的倍数，比如 60*5 是 300 秒也就是 5 分钟
$ hdparm -S 60 /dev/sdc
```



### hd-idle

如果硬盘不支持 hdparm，可以使用 hd-idle

```shell
# 安装
$ pacman -S hd-idle

# 测试 10s 休眠
$ hd-idle -i 0 -a sda -i 10 -d

# 编辑 /etc/conf.d/hd-idle（不同发行版不一样）文件中的 HD_IDLE_OPTS 参数 
HD_IDLE_OPTS="-i 0 -a /dev/sda -i 600"

# 开机启动 hd-idle 服务
$ systemctl enable hd-idle.service
```

