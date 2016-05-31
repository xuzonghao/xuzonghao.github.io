---
layout: post
title:  "tomcat的配置和优化"
date:   2016-05-31
categories: tomcat
---




# tomcat的内存使用配置,最大连接数配置。

```
如何修改配置呢，在/tomcat的/bin/下面有个这个脚本文件。catailna.sh 脚本文件。 如果windows 是bat设置tomcat的使用内存，也其实就是设置jim的使用参数。
```

# 一.Tomcat内存优化

```
Tomcat内存优化主要是对 tomcat 启动参数优化，我们可以在 tomcat 的启动脚本 catalina.sh 中设置JAVA_OPTS 参数。

1.JAVA_OPTS参数说明

Java代码

 -server  启用jdk 的 server 版；
 -Xms      java虚拟机初始化时的最小内存；
 -Xmx      java虚拟机可使用的最大内存；
 -XX:PermSize    内存永久保留区域
 -XX:MaxPermSize   内存最大永久保留区域

设置Tomcat启动的初始内存 其初始空间(即-Xms)是物理内存的1/64，最大空间(-Xmx)是物理内存的1/4。可以利用JVM提供的-Xmn -Xms -Xmx等选项可

要加“m”说明是MB，否则就是KB了，在启动tomcat时会 报内存不足。

 -Xms：初始值  【初始化内存大小】
 -Xmx：最大值  【可以使用的最大内存】
 -Xmn：最小值

JVM堆的设置是指java程序运行过程中JVM可以调配使用的内存空间的设置.JVM在启动的时候会自动设置Heap size的值，其初始空间(即-Xms)是物理内存的1/64，最大空间(-Xmx)是物理内存的1/4。可以利用JVM提供的-Xmn -Xms -Xmx等选项可进行设置。Heap size 的大小是Young Generation 和Tenured Generaion 之和。

    提示：在JVM中如果98％的时间是用于GC且可用的Heap size 不足2％的时候将抛出此异常信息。

提示：Heap Size 最大不要超过可用物理内存的80％，一般的要将-Xms和-Xmx选项设置为相同，而-Xmn为1/4的-Xmx值。 这两个值的大小一般根据需要进行设置。初始化堆的大小执行了虚拟机在启动时向系统申请的内存的大小。一般而言，这个参数不重要。但是有的应用 程序在大负载的情况下会急剧地占用更多的内存，此时这个参数就是显得非常重要，如果虚拟机启动时设置使用的内存比较小而在这种情况下有许多对象进行初始 化，虚拟机就必须重复地增加内存来满足使用。由于这种原因，我们一般把-Xms和-Xmx设为一样大，而堆的最大值受限于系统使用的物理内存。一般使用数 据量较大的应用程序会使用持久对象，内存使用有可能迅速地增长。当应用程序需要的内存超出堆的最大值时虚拟机就会提示内存溢出，并且导致应用服务崩溃。因 此一般建议堆的最大值设置为可用内存的最大值的80%。

    如果系统花费很多的时间收集垃圾，请减小堆大小。一次完全的垃圾收集应该不超过 3-5 秒。如果垃圾收集成为瓶颈，那么需要指定代的大小，检查垃圾收集的详细输出，研究 垃圾收集参数对性能的影响。一般说来，你应该使用物理内存的 80% 作为堆大小。当增加处理器时，记得增加内存，因为分配可以并行进行，而垃圾收集不是并行的。

    在重启你的Tomcat服务器之后，这些配置的更改才会有效。

Windows下，在文件{tomcathome}/bin/catalina.bat，Unix下，在文件{tomcathome}/bin/catalina.sh的前面，增加如下设置：
服务器参数配置

tomcat默认： -Xms1024m -Xmx1024m -Xss1024K -XX:PermSize=128m -XX:MaxPermSize=256m

Java代码

JAVA_OPTS="-Djava.awt.headless=true -Dfile.encoding=UTF-8
-server -Xms2048m -Xmx2048m
-XX:NewSize=512m -XX:MaxNewSize=512m -XX:PermSize=512m
-XX:MaxPermSize=512m -XX:+DisableExplicitGC"

配置完成后可重启Tomcat ，通过以下命令进行查看配置是否生效：

1.首先查看Tomcat 进程号：

    ps -ef | grep tomcat

我们可以看到Tomcat 进程号是 9217 。

1.查看是否配置生效：

Xml代码

  sudo jmap  – heap 9217

我们可以看到MaxHeapSize 等参数已经生效。
```

# 二.Tomcat并发优化

```
1.Tomcat连接相关参数

在Tomcat 配置文件conf下面 server.xml 中的 配置中

在tomcat配置文件server.xml中的配置中，和连接数相关的参数有：

 minProcessors：最小空闲连接线程数，用于提高系统处理性能，默认值为10
 maxProcessors：最大连接线程数，即：并发处理的最大请求数，默认值为75
acceptCount：允许的最大连接数，应大于等于maxProcessors，默认值为100
enableLookups：是否反查域名，取值为：true或false。为了提高处理能力，应设置为false
connectionTimeout：网络连接超时，单位：毫秒。设置为0表示永不超时，这样设置有隐患的。通常可设置为30000毫秒。

1.参数说明
默认的tomcat 参数：

<Connector port=“8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />

修改：

<Connector port=“8080" protocol="HTTP/1.1"
             maxThreads="600"
             minSpareThreads="100"
             maxSpareThreads="500"
             acceptCount="700"
             connectionTimeout="20000"
            redirectPort="8443" />

这样设置以后，基本上没有再当机过。

maxThreads=“600" 表示最多同时处理600个连接     ///最大线程数
minSpareThreads=“100" 表示即使没有人使用也开这么多空线程等待   ///初始化时创建的线程数

maxSpareThreads=“500" 表示如果最多可以空500个线程，例如某时刻有505人访问，之后没有人访问了，则tomcat不会保留505个空线程，而是关闭505个空的。   ///一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。

acceptCount="700"//指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理

这里是http connector的优化，如果使用apache和tomcat做集群的负载均衡，并且使用ajp协议做apache和tomcat的协议转发，那么还需要优化ajp connector。

<Connector port="8009" protocol="AJP/1.3" maxThreads="600" minSpareThreads="100" maxSpareThreads="500" acceptCount="700"

connectionTimeout="20000" redirectPort="8443" />
```

# 报错error

```
一、Tomcat的JVM提示内存溢出

查看%TOMCAT_HOME%\logs文件夹下，日志文件是否有内存溢出错误

二、修改Tomcat的JVM

1、错误提示：java.lang.OutOfMemoryError: Java heap space

    Tomcat默认可以使用的内存为128MB，在较大型的应用项目中，这点内存是不够的，有可能导致系统无法运行。常见的问题是报Tomcat内存溢出错误，Out of Memory(系统内存不足)的异常，从而导致客户端显示500错误，一般调整Tomcat的使用内存即可解决此问题。

windows环境下修改

“%TOMCAT_HOME%\bin\catalina.bat”文件，在文件开头增加如下设置：JAVA_OPTS=-Xms2048m -Xmx2048m

Linux环境下修改

“%TOMCAT_HOME%\bin\catalina.sh”文件，在文件开头增加如下设置：JAVA_OPTS=-Xms2048m -Xmx2048m

其中，-Xms设置初始化内存大小，-Xmx设置可以使用的最大内存。

跟我上面那么设置就可以了。

2、错误提示：java.lang.OutOfMemoryError: PermGen space

原因：

 PermGen space的全称是Permanent Generation space,是指内存的永久保存区域，这块内存主要是被JVM存
放Class和Meta信息的,Class在被Loader时就会被放到PermGen space中，它和存放类实例(Instance)的Heap区域不同,GC(Garbage Collection)不会在主程序运行期对PermGen space进行清理，所以如果你的应用中有很CLASS的话,就很可能出现PermGen space错误，这种错误常见在web服务器对JSP进行pre compile的时候。如果你的WEB APP下都用了大量的第三方jar, 其大小超过了jvm默认的大小(4M)那么就会产生此错误信息了。

解决方法：

在catalina.bat的第一行增加：

set JAVA_OPTS=-Xms64m -Xmx256m -XX:PermSize=128M -XX:MaxNewSize=256m - XX:MaxPermSize=256m

在catalina.sh的第一行增加：

 JAVA_OPTS=-Xms64m -Xmx256m -XX:PermSize=128M -XX:MaxNewSize=256m  XX:MaxPermSize=256m

三、查看Tomcat的JVM内存

    Tomcat6中没有设置任何默认用户，因而需要手动往Tomcat6的conf文件夹下的tomcat-users.xml文件中
    添加用户。 如：

    <role rolename="manager"/>
      <user username="tomcat" password="tomcat" roles="manager"/>

注：添加完需要重启Tomcat6。

    访问http://localhost:8080/manager/status，输入上面添加的用户名和密码。

    然后在如下面的JVM下可以看到内存的使用情况。
```