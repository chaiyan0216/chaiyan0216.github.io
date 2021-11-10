---
title: serialVersionUID 和 Runtime
---

关于 serialVersionUID、Runtime、ProcessBuilder 的三篇教程，serialVersionUID 和后两者没啥关系，不想开新帖了，就写在一起。

[What is the serialVersionUID? | Baeldung](https://www.baeldung.com/java-serial-version-uid)

[Java.lang.Runtime class in Java - GeeksforGeeks](https://www.geeksforgeeks.org/java-lang-runtime-class-in-java/)

[Guide to java.lang.ProcessBuilder API | Baeldung](https://www.baeldung.com/java-lang-processbuilder-api)



### Serial Version UID

> Simply put, we use the **serialVersionUID** attribute to remember versions of a **Serializable** class to verify that **a loaded class and the serialized object are compatible**.

简单来说：serialVersionUID 是用来校验**反序列化的类**和**内存里已加载的类**是否兼容的。

具体过程：

1) 类进行序列化时 serialVersionUID 会被写到文件中，如果没有这个值，虚拟机也会自动生成一个。

   > If *serialVersionUID* is not provided in a *Serializable* class, **the JVM will generate one automatically**. However, it is good practice to provide the serialVersionUID value and **update it after changes to the class so that we can have control over the serialization/deserialization process**.

2) 一个类经过序列化后，它会以一个文件的方式存在于磁盘或者网络中。经过一段时间之前的代码发生了更新，同一个类增加或者删除了某个属性。这时候如果开发人员认为跟更改之前不兼容了，那么就需要更新 serialVersionUID；如果认为依然兼容之前的类，那么开发人员可以保持 serialVersionUID  不变。

3) 类文件反序列化时，虚拟机检查文件中的 serialVersionUID，判断它是否与当前类的 serialVersionUID 一致，如果一致就说明序列化类的版本与当前类版本是一样的，可以反序列化成功，否则失败。

生成方式：

1. 默认的1L，比如：private static final long serialVersionUID = 1L;     

2. 根据类名、接口名、成员方法及属性等来生成一个64位的哈希字段，比如：private static final  long  serialVersionUID = xxxxL;



### Runtime

> Every Java application has a single instance of class Runtime that allows the application to interface with the environment in which the application is running. The current runtime can be obtained from the **getRuntime** method.

Runtime 是一个封装了 JVM 进程的类。每一个 Java 应用实际上都是启动了一个 JVM 进程，每一个 JVM 进程就是一个 Runtime 实例。Runtime 实例可以用来与所在运行环境进行交互，譬如：获取计算机资源、执行 shell 命令等。下边是教程中的几个例子：

```java
// 获取当前空闲内存
public class GFG
{
	public static void main(String[] args)
	{
		// get the current runtime assosiated with this process
		Runtime run = Runtime.getRuntime();
		// print the current free memory for this runtime
		System.out.println("" + run.freeMemory());
	}
}
```

```java
// linux 下打开 chrome
public class GFG
{
	public static void main(String[] args)
	{
		try
		{
			// create a process and execute google-chrome
			Process process = Runtime.getRuntime().exec("google-chrome");
			System.out.println("Google Chrome successfully started");
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
```

```java
// 程序退出时打印新的 Message
public class GFG
{
	// a class that extends thread that is to be called when program is exiting
	static class Message extends Thread
	{
		public void run()
		{
			System.out.println("Program exiting");
		}
	}
	public static void main(String[] args)
	{
		try
		{
			// register Message as shutdown hook
			Runtime.getRuntime().addShutdownHook(new Message());
			
			// cause thread to sleep for 3 seconds
			System.out.println("Waiting for 5 seconds...");
			Thread.sleep(5000);
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
```

```java
// 执行命令
public class GFG
{
	public static void main(String[] args)
	{
		try
		{
			String[] cmd = new String[2];
			cmd[0] = "atom";
			cmd[1] = "File.java";
			// create a process and execute cmdArray
			Process process = Runtime.getRuntime().exec(cmd);

			// print another message
			System.out.println("File.java opening in atom");
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
```

```java
// 在特定环境变量和目录下，执行命令
public class GFG
{
	public static void main(String[] args)
	{
		try
		{
			String[] cmd = new String[2];
			cmd[0] = "atom";
			cmd[1] = "File.java";
			// create a file with the working directory we wish
			File dir = new File("/home/saket/Desktop");
			// create a process and execute cmdArray
			Process process = Runtime.getRuntime().exec(cmd, null, dir);
			System.out.println("File.java opening.");
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
```



## ProcessBuilder

> The *ProcessBuilder* class provides methods for creating and configuring operating system processes.

ProcessBuilder 用于创建操作系统进程，提供了启动和管理进程（也就是应用程序）的方法。

> **Each ProcessBuilder instance allows us to manage a collection of process attributes**. We can then start a new *Process* with those given attributes.

ProcessBuilder 实例用来管理进程属性集，通过 start 方法使用这些属性去创建 Process，start 方法可以从同一 ProcessBuilder 实例重复调用，以利用相同的或相关的属性创建新的子进程。

Runtime 的 exec 方法就是通过调用 ProcessBuilder API 实现的。

一些 API：

- ```java
  ProcessBuilder(String... command)
  ```

  > To create a new process builder with the specified operating system program and arguments, we can use this convenient constructor.

  构造函数

  

- ```java
  directory(File directory)
  ```

  > We can override the default working directory of the current process by calling the *directory* method and passing a *File* object. **By default, the current working directory is set to the value returned by the user.dir system property**.

  工作目录，默认是 user.dir

  

- ```java
  environment()
  ```

  > If we want to get the current environment variables, we can simply call the *environment* method. It returns us a copy of the current process environment using *System.getenv()* but as a *Map*.

  获取环境变量

  

- ```java
  inheritIO()
  ```

  > If we want to specify that the source and destination for our subprocess standard I/O should be the same as that of the current Java process, we can use the *inheritIO* method.

  子进程使用当前 Java 进程相同的 IO
  
  

- ```java
  redirectInput(File file), redirectOutput(File file), redirectError(File file)
  ```

  > When we want to redirect the process builder’s standard input, output and error destination to a file, we have these three similar redirect methods at our disposal.

  重定向输入、输出、错误的重定向
  
  

- ```java
  start()
  ```

  > Last but not least, to start a new process with what we've configured, we simply call *start()*.
  
  启动进程



一个例子：

```java
@Test
public void givenProcessBuilder_whenModifyWorkingDir_thenSuccess() 
  throws IOException, InterruptedException {
    ProcessBuilder processBuilder = new ProcessBuilder("/bin/sh", "-c", "ls");

    processBuilder.directory(new File("src"));
    Process process = processBuilder.start();

    List<String> results = readOutput(process.getInputStream());
    assertThat("Results should not be empty", results, is(not(empty())));
    assertThat("Results should contain directory listing: ", results, contains("main", "test"));

    int exitCode = process.waitFor();
    assertEquals("No errors should be detected", 0, exitCode);
}
```

