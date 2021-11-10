---
title: PyCharm 远程解释器
typora-root-url: ../../..
---

PyCharm可以通过配置远程解释器来运行或者调试远程代码，譬如树莓派上的代码。

PyCharm解释器是系统级的配置，可以不依赖项目进行配置。

### 1、配置路径

2019.1.2版本，配置路径：**Configure** -- **Preferences** -- **Project Interpreter** -- **add** -- **SSH Interpreter**

输入ssh的Host（IP地址）、用户名（Username）、密码（Password），并选择树莓派上的python解释器。

![PyCharm-remote-Interpreter](/images/PyCharm-remote-Interpreter.png)

![interpreter-config](/images/interpreter-config.png)



### 2、使用远程解释器

配置好远程解释器后，在项目级的配置中选择远程解释器，就可以在本地通过PyCharm对远程代码进行运行和调试了。

**Path mapping**：是本地文件和远程文件的映射。在调试和运行过程中，本地文件夹和远程文件夹会保持同步。

![interpreter-config1](/images/interpreter-config1.png)