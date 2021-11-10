---
title: 日志框架
tags: slf4j log4j2
typora-root-url: ../../..
---

#### 一、日志框架

[Slf4j](https://www.slf4j.org/)，简单日志门面（Simple Logging Facade for Java）为各种日志框架提供了统一的接口封装，包括java.util.logging、logback、Log4j等，使用户在部署时可以灵活配置自己想要的Logging APIs实现。

应用开发时，需要统一按照slf4j的API进行开发；部署时，选择不同的日志系统Jar包加入Java ClassPath中，即可自动转换到不同的日志框架上。

slf4j隐藏了具体的转换、适配细节，将应用和具体日志框架解耦，如果在类路径中没有发现绑定的日志实现，则默认使用NOP实现。

![slf4j-bindings](/images/slf4j-bindings.png)

slf4j unbound：**slf4j-api.jar**，默认会使用NOP方式实现；

slf4j NOP：**slf4j-api.jar** + **slf4j-nop.jar**，丢弃所有日志；

slf4j simple：**slf4j-api.jar** + **slf4j-simple.jar**，一个简单的日志实现；

slf4j + jdk：**slf4j-api.jar** + **slf4j-jdk14**，使用jdk官方日志；       

slf4j + logback：**slf4j-api.jar** + **logback-core.jar** + **logback-classic.jar**，logback实现；

slf4j + log4j2：**slf4j-api.jar** + **log4j-slf4j-impl.jar** + **log4j-api.jar** + **log4j-core.jar**，log4j2实现。



#### 二、配置文件（slf4j + log4j2）

##### 2.1 加载顺序

参考官网：<https://logging.apache.org/log4j/2.x/manual/configuration.html>

log4j2可以加载四种类型配置文件：json、yaml、xml、properties。加载优先级为classpath下的：

1）log4j2-test.properties

2）log4j2-test.yaml、log4j2-test.yml

3）log4j2-test.json、log4j2-test.jsn

4）log4j2-test.xml

5）log4j2.properties

6）log4j2.yaml、log4j2.yml

7）log4j2.json、log4j2.jsn

8）log4j2.xml

配置文件缺失时的默认配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Root level="error">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

##### 2.2 标签

**|Configuration**：配置文件root节点

|	**|status**：log4j本身的日志级别；

|	**|monitorinterval**：配置更新检测时间间隔（s）；

|	**|Appenders**：定义日志输入目的地，如console、file

|	|	**|Console**：日志输出到控制台

|	|	|	**|name**：appender名字；

|	|	|	**|target**：SYSTEM_OUT、SYSTEM_ERR；

|	|	|	|[**PatternLayout**](https://logging.apache.org/log4j/2.x/manual/layouts.html#PatternLayout)：输出格式

|	|	**|File**：日志输出到文件

|	|	|	**|name**：appender名字；

|	|	|	**|fileName**：文件名；

|	|	|	**|append**：是否追加方式；

|	|	|	**|PatternLayout**：输出格式

|	|	**|[RollingFile](http://logging.apache.org/log4j/2.x/manual/appenders.html#RollingFileAppender)**：日志输出到滚动文件，该文件超过指定时间、大小后，可以执行归档或者删除等操作

|	|	|	**|name**：appender名字；

|	|	|	**|fileName**：文件名；

|	|	|	**|filePattern**：归档时的文件名；

|	|	|	**|PatternLayout**：输出格式

|	|	|	**|Polices**：归档策略

|	|	|	|	**|CronTriggeringPolicy**：Cron表达式触发

|	|	|	|	**|OnStartupTriggeringPolicy**：JVM启动时触发

|	|	|	|	**|SizeBasedTriggeringPolicy**：基于文件大小

|	|	|	|	**|TimeBasedTriggeringPolicy**：基于时间

|	|	|	|	**|CompositeTriggeringPolicy**：多个触发策略的混合，如同时基于文件大小和时间

|	|	|	**|DefaultRolloverStrategy**：默认触发策略

|	|	|	|	**|max**：最大保留文件数，与%i配合；

|	**|Loggers**：定义logger

|	|	**|Root**：根logger，默认使用该logger进行日志输出

|	|	|	**|level**：日志输出级别，All < Trace < Debug < Info < Warn < Error < Fatal < OFF；

|	|	|	**|AppenderRef**：指向appender

|	|	**|Logger**：自定义logger

|	|	|	**|level**：日志输出级别，All < Trace < Debug < Info < Warn < Error < Fatal < OFF；

|	|	|	**|name**：指定该logger所适用的类或者类所在的包全路径，继承自Root节点；

|	|	|	**|AppenderRef**：指向appender。未指定时，使用root中的appender；指定后，指定的appender和root中的appender均输出日志，设置logger的additivity="false"后可只在指定的appender中进行输出

##### 2.3 样例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN" monitorInterval="30">
    <appenders>
        <console name="console" target="SYSTEM_OUT">
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </console>
        <File name="file" fileName="log/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>
        <RollingFile name="RollingFileInfo" fileName="${sys:user.home}/logs/info.log"
                     filePattern="${sys:user.home}/logs/$${date:yyyy-MM}/info-%d{yyyy-MM-dd}-%i.log">
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
        </RollingFile>
        <RollingFile name="RollingFileWarn" fileName="${sys:user.home}/logs/warn.log"
                     filePattern="${sys:user.home}/logs/$${date:yyyy-MM}/warn-%d{yyyy-MM-dd}-%i.log">
            <ThresholdFilter level="warn" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
            <DefaultRolloverStrategy max="20"/>
        </RollingFile>
        <RollingFile name="RollingFileError" fileName="${sys:user.home}/logs/error.log"
                     filePattern="${sys:user.home}/logs/$${date:yyyy-MM}/error-%d{yyyy-MM-dd}-%i.log">
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
        </RollingFile>
    </appenders>

    <loggers>
        <!--过滤掉spring和mybatis的一些无用的DEBUG信息-->
        <logger name="org.springframework" level="INFO"></logger>
        <logger name="org.mybatis" level="INFO"></logger>

        <root level="all">
            <appender-ref ref="Console"/>
            <appender-ref ref="RollingFileInfo"/>
            <appender-ref ref="RollingFileWarn"/>
            <appender-ref ref="RollingFileError"/>
        </root>
    </loggers>
</Configuration>
```

