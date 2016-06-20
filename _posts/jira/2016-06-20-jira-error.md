
---
layout: post
title:  jira错误处理
date:   2016-06-20
categories: java
---



```
报错：
tail -f /data0/atlassian-jira-4.4-standalone/logs/catalina.out 
        at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:102)
        at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:109)
        at org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:554)
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:298)
        at org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:859)
        at org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:588)
        at org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:489)
        at java.lang.Thread.run(Thread.java:662)
Jun 20, 2016 7:04:10 AM org.apache.tomcat.util.net.JIoEndpoint createWorkerThread
INFO: Maximum number of threads (2048) created for connector with address null and port 80
```

```
更改最大连接数
        <Connector port="80"

                   maxThreads="2048"    //最大线程
                   minSpareThreads="300"	//最小空闲线程
                   maxSpareThreads="500"    //最大空闲线程
                   connectionTimeout="20000"  //连接超时时间

                   enableLookups="false"
                   maxHttpHeaderSize="8192"		//最大请求头大小
                   protocol="HTTP/1.1"
                   useBodyEncodingForURI="true"
                   redirectPort="8443"
                   acceptCount="2048"
                   disableUploadTimeout="true"/>

        <!--
```

```
[root@10.210.137.50|10.210.128.57| 08:40 ~]
# ps -ef |grep java				//查看tomcat的进程ID，记录ID号，假设进程ID为23976
root     15224 11620  0 08:40 pts/0    00:00:00 grep java
root     23976     1  0 04:06 ?        00:02:14 /usr/local/java/bin/java -Djava.util.logging.config.file=/data0/atlassian-jira-4.4-standalone/conf/logging.properties -XX:MaxPermSize=1000m -Xms100000m -Xmx100000m -Djava.awt.headless=true -Datlassian.standalone=JIRA -Dorg.apache.jasper.runtime.BodyContentImpl.LIMIT_BUFFER=true -Dmail.mime.decodeparameters=true -Xmn24000m -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:ParallelGCThreads=12 -XX:+CMSParallelRemarkEnabled -XX:PermSize=300m -server -XX:SurvivorRatio=1 -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=0 -XX:+UseFastAccessorMethods -XX:+PrintTenuringDistribution -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCApplicationStoppedTime -XX:+PrintGCApplicationConcurrentTime -Xloggc:/data0/gclogs/gc.log -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.endorsed.dirs=/data0/atlassian-jira-4.4-standalone/endorsed -classpath /data0/atlassian-jira-4.4-standalone/bin/bootstrap.jar -Dcatalina.base=/data0/atlassian-jira-4.4-standalone -Dcatalina.home=/data0/atlassian-jira-4.4-standalone -Djava.io.tmpdir=/data0/atlassian-jira-4.4-standalone/temp org.apache.catalina.startup.Bootstrap start
```

```
[root@10.210.137.50|10.210.128.57| 08:40 ~]
# lsof -p 23976|wc -l 		//查看当前进程id为23976的 文件操作数
2779



[root@10.210.137.50|10.210.128.57| 08:43 ~]
# ulimit -a 
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 1032073
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 900000   //将允许的最大文件数调整为65536
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 10240
cpu time               (seconds, -t) unlimited
max user processes              (-u) unlimited
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```


# jiraadmin管理账号无法登陆

```
select id, directory_name, active from cwd_directory;
ID    DIRECTORY_NAME          ACTIVE 
----- ----------------------- ------     
10102 AD win2k8               1 
1     JIRA Internal Directory 1     
10106 Active Directory server 1
3 row(s) in 0 ms


update cwd_directory set active=0 where id=10102
```


# 清空临时文件目录

```
rm -f /tmp/lucene*
rm -f /data0/atlassian-jira-4.4-standalone/temp/lucene*
/data0/atlassian-jira-4.4-standalone/bin/startup.sh 
```