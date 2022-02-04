---
title: 树莓派连接 Wi-Fi
tags: wpa2
---

树莓派 [官方文档](https://www.raspberrypi.org/documentation/) 提供了在没有外设的情况下 [开启 ssh](https://www.raspberrypi.org/documentation/remote-access/ssh/) 和 [连接 wifi](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) 的配置说明，但是没有说明如何连接 WPA2-Enterprise Wi-Fi，在 Raspberry Pi OS 界面上也无法直接连接（图标是灰色的）。



搜索到以下两篇文章，第二篇适用于我的情况。

[树莓派 3B+ 连接 WPA2 企业级加密的 WIFI](https://www.starky.ltd/2018/10/16/raspberry-pi-wifi-wpa2-eap/)

[树莓派链接WPA2-Enterprise（企业级加密）WIFI](https://pa.ci/134.html)



我的可用配置如下，reboot 后可以连接并正常访问网络。

```
## /etc/wpa_supplicant/wpa_supplicant.conf

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN

network={
	ssid="your_AP_name"
	key_mgmt=WPA-EAP
	eap=PEAP
	identity="your_username"
	password="password"
	phase1="peaplabel=auto pepver=auto"
	phase2="MSCHAPV2"
	proactive_key_caching=1
}
```

