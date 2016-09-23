---
layout: post
title:  "dns conf"
date:   2016-09-23
categories: Dns
---


### bind set

```
rndc.conf 
key "rndc-key" {		//key语句定义rndc同named进行认证时用到的密钥.其语法与named.conf中的key语句相同,key语句包括algorithm和secret两个子句
        algorithm hmac-md5;	//配置分析器将接受任何串作为算法参数，当前只有字符串“hmac-md5”有意义.这个密钥是一个由RFC 3548所指定的base-64编码的字符串
        secret "E1VBAz8M7x2trwTQbwed8A==";
};

options {					//语句有三个子句default-server，default-key和default-port
        default-key "rndc-key";			//以一个密钥的名字作为其参数，密钥是在key语句中定义的
        default-server 127.0.0.1;		//需要一个主机名或IP地址参数,它表示一个通信的缺省服务器，如果服务器未在命令行中以-s选项指定的情况
        default-port 953;			//指定rndc用到的缺省端口， 它在命令行或server语句中没有指定端口的请款下生效
};

note:运行rndc-congen程序将会方便地为你创建一个rndc.conf文件，并且会显示你所需要添加到named.conf中的相关的controls语句.另外一个选择是，你可以运行rndc-confgen -a来建立一个rndc.key文件，就一点也不用修改named.conf了
```


### 设置slave服务器配置文件

```
zone "movie.edu" in { 
  type slave;
  file "bak.movie.edu";
  masters { 192.249.249.3; }; 
  };
验证slave是否同步成功
在var目录下看到bak.movie.edu,意味着slave服务器已经成功从主名字服务器哪里加载了区，并保存了一份备份拷贝
```


### 多个主名字服务器
```
masters { 192.249.249.3; 192.249.249.4; };
```


### 在bind9中通过rndc控制通道给名字服务器发送消息

```
注意：把named.conf和keys放入不能随便被人访问到的文件里会更保险一点
唯一支持算法是HMAC-MD5，该技术是使用快速MD5安全散列算法来进行身份认证
产生秘钥
echo "selbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselbooselboo" > test
/usr/local/bind9/sbin/rndc-confgen -r test > /usr/local/bind9/etc/rndc.conf
rndc.conf配置文件用来告诉rndc要用哪个认证秘钥，将里面的秘钥用于rndc.key里

使用bind9可以不指定端口
cat named.conf
include "/etc/namedb/rndc.key";
controls {
        inet * allow { any; } keys { "rndc-key"; };
};

如果你没有指定keys，在启动bind的时候会有如下报错
Jan 13 18:22:03 terminator named[13964]: type 'inet' control channel
has no 'keys' clause; control channel will be disabled

在keys子语句中指定秘钥必须在一个key语句中定义
cat /etc/namedb/rndc.key
key "rndc-key" {
        algorithm hmac-md5;
        secret "E1VBAz8M7x2trwTQbwed8A==";
};

使用rndc对多台bind进行控制，可以给每个名字服务器配置不同的秘钥
配置BIND服务器端（不与client在同一个服务器）
修改主配置文件
cat name.conf
include "/etc/namedb/rndc.key";

controls {
        inet * port 953 allow { any; } keys { "10.210.128.197-key"; };
};

修改rndc秘钥文件
cat /etc/namedb/rndc.key 
key "10.210.128.197-key" {
        algorithm hmac-md5;
        secret "Jt0sX2qgSCogMCrM+1ms1g==";
};


配置客户端(配置多个秘钥)
cat rndc.conf
key "rndc-key" {
        algorithm hmac-md5;
        secret "0wL9yzAUplEvD6sU18mi4w==";
};

key "10.210.128.197-key" {
        algorithm hmac-md5;
        secret "Jt0sX2qgSCogMCrM+1ms1g==";
};

server localhost {
        key "rndc-key";
};

server 10.210.128.197 {
        key "10.210.128.197-key";
};


使用客户端reload BIND服务端
reload本机BIND服务 
../sbin/rndc -s localhost reload
server reload successfu

reload  10.210.128.197 BIND服务
../sbin/rndc -s 10.210.128.197 reload
server reload successfu

如果没有给某个服务器分配秘钥，可使用-y选项命令指定它要使用哪个秘钥
../sbin/rndc -s 10.210.128.197 -y rndc-key reload

reload使用非默认端口
../sbin/rndc -s 10.210.128.197 -p 54 reload


注意：
BIND 9.0rndc只支持reload命令
BIND 9.1.0以后rndc支持多个区的reload
```



### 日志

```
在日志中主要有2个概念：通道（channel）和类别（category），通道指定了应该向哪里发送日志数据：是发给syslog，还是写在一个文件里。或者发送给named标准错误输出，还是发送到存储桶。
日志级别，过滤顺序
critical
error
warning
notice
info
debug[level]
dynamic
默认级别为info，看不到任何调试信息

用例
logging {
  channel my_syslog {
    syslog daemon;
    //调试信息不会发送到syslog，所以不必将严重性设置为debug或dynamic；
    //使用最低的syslog级别：info
    severity info;
  }
  channel my_file {
    file "log.msgs";
    //设置severity为dynamic以观察所有的调试信息
    severity  dynamic；
  }
}
```