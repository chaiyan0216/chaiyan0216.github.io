---
title: 可执行 Jar
tags: maven jar
---

有时候一个 Java 项目最终的成品需要是一个可执行 jar 而不是一个 lib，这就需要借助一些 maven 插件来完成。

下面列举一些常用的插件。



#### 1. maven-jar-plugin & maven-dependency-plugin

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
                <classpathPrefix>libs/</classpathPrefix>
                <mainClass>com.cy.App</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>prepare-package</phase>
            <goals>
                <goal>copy-dependencies</goal>
            </goals>
            <configuration>
                <outputDirectory>
                    ${project.build.directory}/libs
                </outputDirectory>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**maven-jar-plugin** 用来指定 jar 的 mainclass 和依赖的 classpath。

- addClasspath：是否在 mainfest 文件中添加 classpath
- classpathPrefix：依赖路径的前缀
- mainClass：入口类

**maven-dependency-plugin** 用来将全部或特定依赖复制到对应路径。

- outputDirectory：复制的目的路径

通过以上配置，执行 `mvn package` 即可打包好一个可执行 jar，同时也把它依赖的 jar 复制到了指定目录。



#### 2. maven-assembly-plugin

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>com.cy.App</mainClass>
                    </manifest>
                </archive>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**maven-assembly-plugin** 可以将其依赖的 jar 也一起打包到最终的可执行 jar 中，其最终会在 target 目录下生成两个 jar：xxx.jar 和 xxx-with-dependencies.jar。其中，xxx-with-dependencies.jar 是带有所有依赖的可执行 jar。



#### 3. maven-shade-plugin

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <executions>
        <execution>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <shadedArtifactAttached>true</shadedArtifactAttached>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>com.cy.App</mainClass>
                    </transformer>
                </transformers>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**maven-shade-plugin** 类似 assembly，也是在 target 下生成两个 jar：xxx.jar 和 xxx-shaded.jar，其中 xxx-shaded.jar 是包含所有依赖的可执行 jar。



#### 4. spring-boot-maven-plugin

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal>
            </goals>
            <configuration>
                <classifier>executable</classifier>
                <mainClass>com.cy.App</mainClass>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**spring-boot-maven-plugin** 同样是生成两个 jar：xxx.jar 和 xxx-classifier.jar，classifier 即是 pom 中定义的 executable 或着也能改成其它自定义后缀。

spring-boot-maven 由于增加了 spring boot 的一些依赖，相对上边的 assemble 和 shade 插件，最终生成的可执行 jar 会大一点，这个插件一般在 spring boot 项目中使用。



#### 5. 区别

[Difference between maven plugins](https://stackoverflow.com/a/43444019)

>1. `maven-jar-plugin`: This plugin provides the capability to build and sign JARs. But it just compiles the java files under `src/main/java` and `src/main/resources/`. It doesn't include the dependencies JAR files.
>2. `maven-assembly-plugin`: This plugin extracts all dependency JARs into raw classes and groups them together. It can also be used to build an executable JAR by specifying the main class. It works in project with less dependencies only; for large project with many dependencies, it will cause Java class names to conflict.
>3. `maven-shade-plugin`: It packages all dependencies into  one uber-JAR. It can also be used to build an executable JAR by specifying the main class. This plugin is particularly useful as it  merges content of specific files instead of overwriting them by [relocating classes](http://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html). This is needed when there are resource files that have the same name across the JARs and the plugin tries to package all the resource files together.

1. maven-jar-plugin：只编译 src/main/java 和 src/main/resources 下的文件，不包含依赖。
2. maven-assembly-plugin：把依赖的 jar 包提取为原始的 class 文件，但对于大项目会有类名冲突的问题。
3. maven-shade-plugin：解决了类名和资源文件冲突的问题，适用于大型工程。

