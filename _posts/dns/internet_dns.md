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


```