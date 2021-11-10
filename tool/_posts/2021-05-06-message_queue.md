---
title: 消息队列
tags: rabbitmq
typora-root-url: ../..
---

[toc]

[消息队列](https://zh.wikipedia.org/wiki/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97)是一种进程间通信或同一进程的不同线程间的通信方式，直观一点的理解可以直接从字面进行理解，消息队列就是一个容纳消息的队列，producer 有新的消息就丢给队列，consumer 轮询队列进行消息的获取。

[高级消息队列协议](https://zh.wikipedia.org/wiki/%E9%AB%98%E7%BA%A7%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97%E5%8D%8F%E8%AE%AE)（Advanced Message Queuing Protocol AMQP）是面向消息中间件提供的开放的应用层协议。

消息队列有很多基于不同协议的开源实现：RabbitMQ（AMQP）、RocketMQ（自定协议）、Kafka（自定的基于TCP的二进制协议）。

- RabbitMQ：性能好、延时低、管理界面友好、社区活跃。但吞吐量较低，erlang 语言实现，较难扩展。
- RocketMQ：阿里出品，接口简单易用。
- Kafka：提供较少的核心功能、具备超高的吞吐量、极高的可用性和拓展性。但存在消息重复消费的缺点，适合于大数据实时计算和日志收集。

消息队列常见的应用场景：异步处理、应用解耦、流量削峰、消息通讯等。



一个模拟 RabbitMQ 的网站：[RabbitMQ Simulator](http://tryrabbitmq.com/)



#### AMQP 核心概念

- **Broker**：接收和分发 message 的应用，即 RabbitMQ 的 server。

- **Virtual Host**：出于多用户和安全因素设计的，把 AMQP 的基本组件划分到一个虚拟的分组中，类似于 namespace 概念。当多个不同的用户使用同一个RabbitMQ server 提供服务时，可以划分出多个 virtual host，每个用户在自己的 virtual host 创建 exchange/queue 等。

- **Connection**： publisher/consumer 和 broker 之间的 TCP 连接。断开连接的操作只会在 client 端进行，broker 不会断开连接，除非出现网络故障或 broker 服务出现问题。

- **Channel**：如果每一次访问 RabbitMQ server 都建立一个 connection，在消息量大的时候建立 TCP connection 的开销将是巨大的，效率也较低。Channel 是在 connection 内部建立的逻辑连接，channel 作为轻量级的 connection 极大减少了操作系统建立 TCP connection 的开销。

- **Message**：服务器和应用程序之间传送的数据，由 properties 和 body 组成。Properties 可以对 message 进行修饰，比如 message 的优先级、延迟等高级特性，body 就是 message 内容。

- **Queue**：消息最终被送到这里等待 consumer 取走。一个 message 可以被同时拷贝到多个 queue 中。

- **Exchange**： message 到达 broker 的第一站，根据分发规则，匹配查询表中的 routing key，分发消息到 queue 中去。常用的类型有：direct (point-to-point), topic (publish-subscribe) and fanout (multicast)。

- **Binding**： exchange 和 queue 之间的虚拟连接，binding 中可以包含 routing key。Binding 信息被保存到 exchange 中的查询表中，用于 message 的分发依据。

- **Routing Key**：单个路由规则，虚拟机可用它来确定如何路由一个 message。

  

**协议模型**

<img src="/images/amqp-arch1.png" style="zoom: 67%;" />

![](/images/amqp-arch2.png)



**消息流转**

<img src="/images/amqp-message-flow.jpg" style="zoom:67%;" />



#### Exchange 类型

主要有 3 种类型的 exchange。

- Direct Exchange（直连交换机）
- Topic Exchange（主题交换机）
- Fanout Exchange（扇型交换机）



##### Direct Exchange

消息传递时，Routing Key 必须完全匹配才会被队列接收，否则该消息会被抛弃。

<img src="/images/direct-exchange.png" style="zoom:40%;" />



##### Topic Exchange

模糊匹配的机制。

- \* 匹配一个单词
- \# 匹配零个或多个单词
- 发送到 topic exchange 的消息会被转发到所有关心 routing key 中指定 topic 的 queue 上
- 当特殊字符“*”（星号）和“＃”（哈希）未在绑定中使用时，主题交换机的行为就像直接交换机一样。

<img src="/images/topic-exchange.png" style="zoom:40%;" />

**例如**

![](/images/topic-exchange-example.png)

上图示例创建了三个绑定：Q1 绑定了绑定键 `*.orange.*`，Q2 绑定了`*.*.rabbit` 和 `lazy.#`。

- Q1 对所有 **orange** 感兴趣
- Q2 希望听到关于 **rabbit** 的一切，以及关于 **lazy** 的一切

那么消息经过 topic exchange 的路由结果将会是如下情况。

- quick.orange.rabbit 的消息会同时传递到两个队列
- lazy.orange.elephant 的消息也会同事传递到两个队列

- quick.orange.fox 只会传递到 Q1
- lazy.brown.fox 只会转到 Q2
- lazy.pink.rabbit 只会递到 Q2 一次，即使它匹配两个绑定
- quick.brown.fox 与任何绑定都不匹配，因此它将被丢弃
- lazy.orange.male.rabbit 虽然有四个单词，也会匹配最后一个绑定，并将被传递到 Q2



##### Fanout Exchange

名字比较形象了，像 fan 一样甩出消息到所有绑定的队列。

- 不处理 routing key，只需要简单的将队列绑定到交换机上
- 发送到 fanout exchange 的消息都会被转发到与该交换机绑定的所有队列上
- Fanout Exchange 转发消息是最快的

<img src="/images/fanout-exchange.png" style="zoom:40%;" />



#### RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) 是实现了高级消息队列协议（AMQP）的开源消息代理软件（亦称面向消息的中间件）。

RabbitMQ 服务器是用 [Erlang](https://www.erlang.org/) 语言编写的。



> RabbitMQ is a message broker: it accepts and forwards messages. You can think about it as a post office: when you put the mail that you want posting in a post box, you can be sure that Mr. or Ms. Mailperson will eventually deliver the mail to your recipient. In this analogy, RabbitMQ is a post box, a post office and a postman.
>
> The major difference between RabbitMQ and the post office is that it doesn't deal with paper, instead it accepts, stores and forwards binary blobs of data ‒ *messages*.



##### 安装

官网给了[安装指引](https://www.rabbitmq.com/download.html)，MacOS 下用 HomeBrew 比较简单方便，但 Linux 下的步骤稍显凌乱，这里整理下 Ubuntu 的安装步骤。

1. 添加 apt 仓库签名密钥

   ```shell
   ## Team RabbitMQ's main signing key
   sudo apt-key adv --keyserver "hkps://keys.openpgp.org" --recv-keys "0x0A9AF2115F4687BD29803A206B73A36E6026DFCA"
   ## Launchpad PPA that provides modern Erlang releases
   sudo apt-key adv --keyserver "keyserver.ubuntu.com" --recv-keys "F77F1EDA57EBB1CC"
   ## PackageCloud RabbitMQ repository
   curl -1sLf 'https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey' | apt-key add -
   ```

2. 安装 `apt-transport-https`，以便能够从 PackageCloud、Cloudsmith.io、Launchpad 下载 RabbitMQ 和 Erlang 的包文件

   ```shell
   sudo apt-get install apt-transport-https
   ```

3. 添加第三方软件源，将以下内容添加至 `/etc/apt/sources.list.d/rabbitmq.list`，需要注意不同的 Ubuntu 版本的 distribution name 有差别。

   - `focal` for Ubuntu 20.04
   - `bionic` for Ubuntu 18.04
   - `xenial` for Ubuntu 16.04
   - `buster` for Debian Buster

   ```shell
   # Source repository definition example.
   
   ## Provides modern Erlang/OTP releases
   ##
   ## "bionic" as distribution name should work for any reasonably recent Ubuntu or Debian release.
   ## See the release to distribution mapping table in RabbitMQ doc guides to learn more.
   deb http://ppa.launchpad.net/rabbitmq/rabbitmq-erlang/ubuntu bionic main
   deb-src http://ppa.launchpad.net/rabbitmq/rabbitmq-erlang/ubuntu bionic main
   
   ## Provides RabbitMQ
   ##
   ## "bionic" as distribution name should work for any reasonably recent Ubuntu or Debian release.
   ## See the release to distribution mapping table in RabbitMQ doc guides to learn more.
   deb https://packagecloud.io/rabbitmq/rabbitmq-server/ubuntu/ bionic main
   deb-src https://packagecloud.io/rabbitmq/rabbitmq-server/ubuntu/ bionic main
   ```

4. 安装 RabbitMQ 及其相关依赖的包

   ```shell
   ## Update package indices
   sudo apt-get update -y
   
   ## Install Erlang packages
   sudo apt-get install -y erlang-base \
                           erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets \
                           erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key \
                           erlang-runtime-tools erlang-snmp erlang-ssl \
                           erlang-syntax-tools erlang-tftp erlang-tools erlang-xmerl
   
   ## Install rabbitmq-server and its dependencies
   sudo apt-get install rabbitmq-server -y --fix-missing
   ```

   

##### 启停和界面

```shell
# 启动
sudo service rabbitmq-server start

# 停止
sudo service rabbitmq-server stop 

# 重启
sudo service rabbitmq-server restart 

# 查看当前状态
sudo service rabbitmq-server status 
```



RabbitMQ 提供了一个管理插件（rabbitmq_management），方便在 web 端管理 RabbitMQ，默认未开启需要在命令行开启该插件。开启插件并重启 RabbitMQ server 后，就可以通过浏览器进行管理，默认端口是 15672。

```shell
# 开启管理工具，然后重启服务即可通过 web 进行管理
sudo rabbitmq-plugins enable rabbitmq_management
```



默认有一个 guest/guest 的用户可以用来登录管理工具，同时也可以通过命令新建用户和赋予权限。

```shell
# 添加 admin/admin 用户
sudo rabbitmqctl add_user admin admin

# 赋予其管理员权限
sudo rabbitmqctl set_user_tags admin administrator 
```



##### 示例

官网的一些开发示例：[RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)

