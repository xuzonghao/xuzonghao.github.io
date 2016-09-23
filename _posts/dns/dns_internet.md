---
layout: post
title:  "内网dns"
date:   2016-04-09
categories: Dns
---



```
**检查进程**
* * * * * (cd /root; ./named_ck.sh > /dev/null 2>&1)

named_ck.sh（检查脚本）
UNAME=`uname`
DATE=`date '+%Y%m%d%H%M%S'`
NO=`ps -ef |grep /usr/local/bind9/sbin/named |grep -v grep  |wc -l`
if [ $NO -eq 0 ] ;then
/usr/local/bind9/sbin/named -c /usr/local/bind9/etc/named.conf
echo "$DATE  ! named   restart ! "  >> named_ck.log
fi
```



set system
```
cat /etc/sysconfig/network
GATEWAY=10.210.200.100

cat .bashrc
ulimit -i 127424

list:ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 127424
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 10240
cpu time               (seconds, -t) unlimited
max user processes              (-u) 22960
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited

cat /etc/resolv.conf
nameserver 10.210.12.10
options timeout:2 
```



bind set
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