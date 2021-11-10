---
title: Java SPI
tags: spi
typora-root-url: ../../..
---

#### 概念

>   Service provider interface (SPI) is an API intended to be implemented or extended by a third party. It can be used to enable framework extension and replaceable components.

SPI 是需要被第三方实现或者扩展的接口，可以被用在框架的扩展和可替代组件上。

>   A service is a well-known set of interfaces and (usually abstract) classes. A service provider is a specific implementation of a service. The classes in a provider typically implement the interfaces and subclass the classes defined in the service itself. Service providers can be installed in an implementation of the Java platform in the form of extensions, that is, jar files placed into any of the usual extension directories. Providers can also be made available by adding them to the application's class path or by some other platform-specific means.

一个服务就是一组接口，一个服务 Provider 就是这组接口的特殊实现。服务 Provider 可以通过提供 jar 包到通常的扩展目录来实现，也可以通过添加 jar 包到应用程序的 class path 实现。



#### 理解

以上英文的说法稍稍绕了一点，SPI 可以从面向接口编程说起。

先从最简单的场景说起，假设有一个 service A 对外提供服务（API），然后有调用方 B 使用这个服务，那么 B 对 A 就有了一个依赖关系。当 A 发生接口变更时，如果被 B 使用到的接口更新了（加减了参数或者改了接口名），那么调用方 B 的代码也需要进行同步更新。这种情况在简单系统开发中并没有什么问题，但是当进行规模稍大一点的系统开发时，多个组件互相依赖，每个组件的更改都需要通知其他组件同步更新，这个复杂度随着组件数量的的增加呈几何增长。

因此，面向接口编程就被提出了。Service A 在对外提供服务前，首先根据调用方 B 的要求或者自己的功能点，对外提供一层抽象的接口（interface），这一层抽象的接口就像是 A 和 很多 B 之间的协议，服务方和调用方都按照协议的规定进行开发，服务方由 service provider 按照协议（接口）的定义进行具体的实现，而调用方则直接根据接口的定义进行使用。这样只要接口不变，那么调用方就无需更改。

再进一步讲，如果接口是调用方 B 提出来，但是由服务方 A 来实现，这就产生了依赖反转的现象。没有接口提出之前，调用方 B 直接依赖于服务提供方 A，B -> A；由 B 提出需要哪些接口后，A 的实现反而需要按照 B 提出的接口进行，也就是 A 的具体实现依赖于 B 的接口定义，A -> B。

再说到 java 的 SPI，它其实是`面向接口编程＋策略模式＋配置文件` 的组合，服务方 A 或者调用方 B 首先提出标准的服务接口，然后由不同的服务 Provider 进行实现，不同的服务 Provider 提供不同的 jar 包给调用方，调用方通过配置文件指定不同场景下调用服务的策略就可以实现启用、扩展、或者替换服务/框架的实现策略。JDBC加载不同类型数据库的驱动就是最典型的例子，还有SLF4J加载不同提供商的日志实现类也是类似的。

![img](/images/spi.png)



#### 约定

Java SPI 的应用需要遵循以下约定。

-   服务 Provider 提供了接口的一种具体实现后，需要在 jar 包 META-INF/services 目录下创建一个以“接口全限定名”命名的文本文件，文件的每一行都是一个具体实现类的全限定名，这可以通过在打 jar 包前在 resources 目录下对应文件夹内添加相应文件来实现
-   服务 Provider 实现好的 jar 包需要在调用方的 class path 中
-   调用方需要使用 java.util.ServiceLoder 动态加载实现模块，其通过扫描 class path 下所有 lib 中的 META-INF/services 目录实现类加载
-   服务 Provider 提供的实现类必须有一个无参构造方法



#### 示例

放在一个项目里进行 demo 了，没有拆分 jar 包。

-   resources 下方服务实现类的注册文件
-   com.cy.demo.spi.service 下是服务接口和对应的实现类
-   App 是调用方的模拟

<img src="/images/spi-demo.png" style="zoom:50%;" />



```java
// 服务实现类注册文件：com.cy.demo.spi.service.PayService
com.cy.demo.spi.service.impl.AliPay
com.cy.demo.spi.service.impl.WechatPay
```



```java
// 服务接口 PayService
package com.cy.demo.spi.service;
public interface PayService {
    void pay();
}

// 服务实现 AliPay
package com.cy.demo.spi.service.impl;
import com.cy.demo.spi.service.PayService;
public class AliPay implements PayService {
    @Override
    public void pay() {
        System.out.println("This is AliPay.");
    }
}

// 服务实现 WechatPay
package com.cy.demo.spi.service.impl;
import com.cy.demo.spi.service.PayService;
public class WechatPay implements PayService {
    @Override
    public void pay() {
        System.out.println("This is WechatPay.");
    }
}
```



```java
// 调用方
package com.cy.demo.spi;
import com.cy.demo.spi.service.PayService;
import java.util.ServiceLoader;
public class App {
    public static void main(String[] args) {
        ServiceLoader<PayService> loader = ServiceLoader.load(PayService.class);
        for (PayService payService : loader) {
            payService.pay();
        }
    }
}
```



```shell
# 运行结果
This is AliPay.
This is WechatPay.
```

