---
title: 快捷指令
tags: mac
---

装了 OpenWrt 做旁路网关后，会遇到要在两套或者三套网络配置之间切换的情况，Mac 下有个自带的 `networksetup` 可以在命令行进行配置，但频繁切换的时候打那么一串命令也挺烦人。最后发现 Shortcuts（快捷指令）的 Run Shell Script 可以干这个事，以下是具体配置：

**配置1**：旁路网关

```shell
networksetup -setmanual Wi-Fi 192.168.31.3 255.255.255.0 192.168.31.2
networksetup -setdnsservers Wi-Fi 192.168.31.2
```

**配置2**：主路由

```shell
networksetup -setmanual Wi-Fi 192.168.31.3 255.255.255.0 192.168.31.1
networksetup -setdnsservers Wi-Fi 192.168.31.1
```

**配置3**：dhcp

```shell
networksetup -setdhcp Wi-Fi
networksetup -setdnsservers Wi-Fi "Empty"
```

配置添加好后可以让它们出现在 menu bar 上，这样直接在 menu bar 上用鼠标点点就能切换网络配置了，更懒的话还能在 spotlight 里找到并触发。



最后附上一些 url scheme，用法不写了，网上一搜一大把。

[bhagyas/app-urls](https://github.com/bhagyas/app-urls)：A long list of App URLs for iOS, macOS and Android

[常用 URL Schemes 收集](https://gist.github.com/zhuziyi1989/3f96a73c45a87778b560e44cb551ebd2)

