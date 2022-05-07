---
title: 魅族 MX4 刷 Ubuntu Touch
typora-root-url: ../..
---

偶然看到魅族 MX4 能刷 Ubuntu Touch：[Meizu MX4](https://devices.ubuntu-touch.io/device/arale/)，正好手里有个压箱底的 MX4，参考下边两篇文章刷成功了，除了 root 其它步骤都比较顺利。

[MX4刷入Ubuntu Touch的经历](https://www.csdn.net/tags/MtTaEgwsNTMyNjUyLWJsb2cO0O0O.html)

[初试魅族MX4安装ubuntu touch系统](https://blog.csdn.net/m0_60329953/article/details/119534022)



## 1. 工具

[Flyme 4.5.7 固件](https://bbs.meizu.cn/thread-6918814-1-1.html)

[Flyme 4.2.8.2A 固件](https://www.flyme.com/firmwarelist-6.html#3)

魅族工具箱：https://pan.baidu.com/s/1cRAOdJtKO_UrWLPOJlNR9g，提取码：nrtu



## 2. 降级和 Root

刷机关键点在于能解锁 bootloader，手里的 MX4 系统版本是 Flyme 6.3.0.2A，无法直接解锁 bootloader，需要降级到 Flyme 4.2.8.2A 才能解锁。

由于 Flyme 6.3.0.2A 无法直接降到 Flyme 4.2.8.2A，还需要先刷一个中间版本 Flyme 4.5.7 过渡。

具体降级过程如下：

a. 在 Flyme 6.3.0.2A 系统先获取 root 权限，Flyme 系统是带这个功能的。

b. 下载 Flyme 4.5.7 固件（update.zip），拷贝到手机存储的根目录。

c. 关闭手机，然后同时按住 **电源和音量+** 启动手机，启动后会弹出刷机界面，同时**选择升级系统和清除数据**，等待刷机完成。

d. 系统再次启动后就是 Flyme 4.5.7 了，再下载 Flyme 4.2.8.2A（update.zip），拷贝到手机存储的根目录。

e. 再次关闭手机，然后同时按住 **电源和音量+** 启动手机，同时**选择升级系统和清除数据**，等待刷机完成。

f. 系统再次启动后就是 Flyme 4.2.8.2A 了。

刷完 Flyme 4.2.8.2A 之后获取 root 权限是最费劲的一步，本来 Flyme 系统是带这个功能的，但可能因为 4.2.8.2A 版本年代太久远了，中间换过服务器地址，导致使用系统自带的 root 功能无效，只能通过第三方工具来 root。我用的是 KingRoot 4.62，试了很久才最终 root 成功。



## 3. 解锁 Bootloader

手机安装魅族工具箱里的：BL-unlock/unlock.apk，启动 app 解锁即可。



## 4. 刷第三方 Recovery

刷第三方 Recovery 不是用来刷镜像，而是用来调节 cache 分区大小和格式化 cache 分区。

因为 MX4 原来的 cache 分区只有 117MB，而 ubports installer 在安装时无法将镜像文件 push 到 cache，会导致安装失败。

魅族工具箱里的脚本是 Window bat，我的不是 Window 系统，需要分步手工执行命令。

刷入过程如下：

a. 电脑上安装 adb 和 fastboot 工具，命令行输入 `adb kill-server` 和 `adb start-server`。

b. 手机打开 usb 调试功能，连接到电脑上。

c. 电脑命令行输入 `adb devices` 看到有设备连接上才行。

d. 电脑命令行输入 `fastboot flash recovery BL-unlock/philz-mx4.img` 刷入 philz 版本的 recovery。



## 5. 调整分区

参考视频 [Meizu MX4 Resize partitions System, Cache, Userdata](https://www.bilibili.com/video/BV1Lt4y167aX/) 进行分区的调整，需要注意的是视频里是 32G 的，国内的好像都是 16G 的。

通过 adb shell 调用 parted 工具，调整 cache 分区大小，具体过程如下：

a. `adb shell` -- 进入 shell

b. `parted /dev/block/mmcblk0` -- 确定分区对像

c. `unit MB` -- MB为单位

d. `print` -- 查看分区

e. `rm 16` -- 删掉16分区

f. `rm 15` -- 删掉15分区

g. `rm 14` -- 删掉14分区

h. `mkpartfs system ext2 629 2677` -- 重建分区

i. `name 14 system` -- 命名 14 分区为 system

j. `mkpartfs cache ext4 2677 4725` -- 重建分区

k. `name 15 cache` -- 命名 15 分区为 cache

l. `mkpartfs userdata ext4 4725 15617` -- 重建分区

m. `name 16 userdata` -- 命名 16 分区为 userdata

n. 重启 recovery，在手机界面上找到 clean for install a New ROM，wipe all，重新格式化 cache，system 等为 ext4 格式

o. 最后再次打开分区表，确认分区大小和格式没问题



## 6. Ubports Installer 刷机

这步没什么需要说明的，下载安装好 installer，连接手机按界面提示操作即可，基本是傻瓜式操作。



## 7. 启动后配置

**sshd**

- `sudo android-gadget-service enable ssh`，手机端打开 ssh 服务
- `mkdir -pm700 ~/.ssh`，手机端创建目录
- `nc -l 1234 > ~/.ssh/authorized_keys`，手机端等待客户端公钥
- `nc 192.0.2.1 1234 < ~/.ssh/id_rsa.pub`，电脑端发送公钥，ip 要改成自己手机的 ip
- `ssh phablet@192.0.2.1`，电脑端连接，ip 要改成自己手机的 ip

**w:由于文件系统为只读，因而无法使用文件锁 /var/lib/dpkg/lock**

去除手机只读模式：`sudo mount -o remount,rw /`

**/var/cache/apt/archives/ 上没有足够的可用空间**

在某个空间大的分区建立一个目录，然后把 /var/cache/apt/archives 换成指向那个目录的软链接

```shell
mkdir -p "$HOME/debs/partial"
sudo rm -rf /var/cache/apt/archives
sudo ln -s "$HOME/debs" /var/cache/apt/archives
```

