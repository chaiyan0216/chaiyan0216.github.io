---
title: maven-release-plugin
tags: maven
---

[maven-release-plugin](https://maven.apache.org/maven-release/maven-release-plugin/)，是官方的插件，可以结合 ci 或者发布人员手工做一些自动化工作：发布到 maven 仓库、打 tag 和自动更新 git 仓库中 pom.xml 文件的版本号。



要使用该插件需要满足两个条件：

-   the `scm`-section with a `developerConnection`，在 pom.xml 文件中需要有 `<scm> <developerConnection>` 部分，需要这个是因为 release 插件要通过 scm clone 代码和 push pom.xml 的更新
-   the maven-release-plugin with a locked version

```xml
<scm>
    <developerConnection>scm:git:git@github.com:xxx/xxx.git</developerConnection>
</scm>
...
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-release-plugin</artifactId>
    <version>3.0.0-M4</version>
</plugin>
```



要发布的 maven 仓库地址由 `distributionManagement` 部分指定。

```xml
<distributionManagement>
    <repository>
        <id>xxx</id>
        <name>Releases</name>
        <url>http://xxx</url>
    </repository>
    <snapshotRepository>
        <id>xxx</id>
        <name>snapshots</name>
        <url>http://xxx</url>
    </snapshotRepository>
</distributionManagement>
```



通过：`mvn -B release:clean release:prepare release:perform` 来实现自动化发布，如果不需要进行 test 和生成 javadoc，可以指定参数：`-Dmaven.skip.test=true -Dmave.skip.javadoc=true`。发布过程实际进行的主要操作如下。

[release:clean](https://maven.apache.org/maven-release/maven-release-plugin/examples/clean-release.html)

-   删除发布描述文件（release.properties）
-   删除备份的 pom.xml

[release:prepare](https://maven.apache.org/maven-release/maven-release-plugin/examples/prepare-release.html)

-   生成 release.properties 文件
-   检查本地项目代码中是否有未提交的修改
-   检查依赖或插件是否有 SNAPSHOT 版本（allowTimestampedSnapshots 为 true 时跳过）
-   将 pom.xml version 从 x.y.z-SNAPSHOT 改为发布版本 x.y.z
-   更新 pom.xml scm 部分的 tag
-   执行 test（-Dmaven.skip.test=true 可以跳过）
-   提交 pom.xml 修改并 push 到 git 仓库
-   打 tag 并 push 到 git 仓库
-   将 pom.xml verson 从 x.y.z 改为 x.y.z+1-SNAPSHOT
-   提交 pom.xml 修改并再次 push 到 git 仓库

[release:preform](https://maven.apache.org/maven-release/maven-release-plugin/examples/perform-release.html)

-   依赖之前 prepare 阶段产生的 release.properties 文件
-   用 prepare 阶段打的 tag 从 scm url 检出代码
-   执行预定义的 maven goals（deploy 和 site-deploy）
-   release 完成后 release.properties 和其他相关的 release 文件都会被删除

