---
title: SleepWatcher
---

Mac 之前遇到了合盖后掉电比较快的问题，可能是跟代理或者 wi-fi 有关，没有具体查问题的原因，之后发现了 SleepWatcher 这个工具。[SleepWatcher](https://www.bernhard-baehr.de) 可以在 Mac 睡眠、唤醒以及空闲的时候通过脚本执行一些定制的命令。

**安装**

直接通过 brew 就能安装，安装完后也是通过 brew 启动 service。

```
$ brew install sleepwatcher
$ brew services start sleepwatcher
```

通过 brew 安装完 sleepwatcher 后，terminal 里提示说明文档是在：/usr/local/opt/sleepwatcher/ReadMe.rtf，里边也是说怎么升级和安装的。

只需要睡眠和唤醒后执行一些命令，那直接到下一步准备脚本就可以了。如果有更高级的需求，man 一下。

**脚本**

在 **home** 目录下分别创建 **.sleep** 和 **.wakeup** 文件，并赋予可执行权限就可以了。

至于脚本的内容，就根据自己需求来，譬如下边是睡眠时关闭某个 app 和 wi-fi，唤醒时再打开。

```
# .sleep
#! /usr/bin/env zsh

osascript -e 'quit app "xxx"'
networksetup -setairportpower en0 off
```

```
# .wakeup
#! /usr/bin/env zsh

networksetup -setairportpower en0 on
osascript -e 'open app "xxx"'
```

