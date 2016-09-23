---
layout: post
title:  "Dns基础"
date:   2016-09-18
categories: Dns
---

### note

```
“.”绝对域名
顶级域名：
com商业组织    
edu教育
gov政府
mil军事
net基础设施，先同com
org非盈利组织
int国际组织
gTLD通用顶级域
```


### 概念

```
**递归**
解析器直接查询的名字服务器（本地名字服务器）却要不断地依照指示（referral）进行查询，直到得到结果。


**在权威名字服务器间选择**
BIND名字服务器使用一种称为“往返时间”（roundtrip time）或RTT的度量（metric）对同一个区的权威名字服务器进行选择。往返时间是指远程名字服务器响应查询的时间长度。每次BIND名字服务器向远程名字服务器发送查询时，都启动一个内部计时器。当它收到响应时就停止计时，记录下该远程名字服务器过了多长时间才响应。当名字服务器要选择向哪个名字服务器发送查询时，它就选择具有最小RTT的名字服务器。


**生存期:**
当然，名字服务器不能把数据永远放在缓存中。如果这样，即使权威名字服务器上的数据有了变动，它也绝不会知道。远程名字服务器还会使用这些缓存的数据。因此，包含这些数据的区的管理员要为数据设定一个生存期（time to live），简称TTL。生存期就是名字服务器允许数据在缓存中存放的时间。生存期一过，名字服务器就必须丢弃缓存的数据，并从权威名字服务器上获取新的数据。对于否定缓存数据也是一样；每隔一段时间，名字服务器也要清除否定回答，以防权威名字服务器上已经增加了新的数据。


**TTL**
TTL越长，查询你的区中信息的平均解析时间也就越短，因为数据能在缓存中存放更长的时间。缺点就是，如果名字服务器上的数据发生了变动，缓存中的信息将长时间地保持不一致。


**安全特性**
BIND 8和BIND 9支持关于查询、区传送和动态更新的访问控制表。BIND 4.9支持关于查询和区传送的访问控制表，更早版本的BIND根本不支持访问控制表。某些名字服务器，特别是运行在堡垒主机或其他安全性要求很高的主机上，可能需要这些特性。


**DNS 通知（NOTIFY）**
BIND 8和BIND 9支持区变动通知，它允许一个区的主名字服务器在序列号增加的时候通知该区的辅名字服务器。BIND 4不支持NOTIFY。


**增量区传送**
BIND 8.2.3和BIND 9支持增量区传送（incremental zone transfer），它允许辅名字服务器只要求从主服务器获得区变动的部分。这就使得区传送更快、更有效，对于大的、经常变动的区这个特性尤为重要。


**SOA**
SOA（start of authority，权威起始）记录
其中，mail addr字段是sina.com.cn的联系人的Internet地址。要将这个地址转换成Internet电子邮件地址格式，需要将地址中的第一个“.”变成“@”符号。于是freednsadmin.dnspod.com就变成了freednsadmin@dnspod.com


**区数据文件**
db.DOMAIN和db.ADDR文件统称为区数据文件（zone datafile）。还有其他一些区数据文件：db.cache和db.127.0.0，这些文件是系统开销（overhead）。
区数据文件中的大部分条目被称为DNS资源记录（resource record）。DNS查找是不区分大小写的。资源记录必须从一行的第一列开始。
SOA记录
指示该区的权威
NS记录
列出该区的一个名字服务器
其他记录
有关该区中主机的数据本章中讲到的其他记录包括：
A
名字到地址的映射
PTR
地址到名字的映射
CNAME
规范名字（相对于别名而言）


13个全球范围的“根服务器”（root-servers.net），除了当中的3个以外，其他都位于美国


**设置区默认TTL**
名字服务器在查询响应中提供这个TTL值，允许其他服务器将数据在缓存中存放TTL所指定的时间。如果你的数据不是经常变动或变动不大，可以考虑将TTL默认值设为几天。1周大概是使之有意义的最大值了。像1小时这样短的值也可以用，但是我们通常不建议使用小于1小时的值，这是考虑到由此引起的DNS流量。
（bind8.2以前版本配置文件第一个条目就是soa记录，bind8.2以后第一个条目是$TTL）


每一级域名长度的限制是63个字符
域名总长度则不能超过253个字符
树的深度不得超过127层
全限定域名 FQDN
递归查询，服务器必需回答目标IP与域名的映射关系。
迭代查询是，服务器收到一次迭代查询回复一次结果，这个结果不一定是目标IP与域名的映射关系，也可以是其它DNS服务器的地址
重指引：为解析查询做一件事：指引本地域名服务器到另一台主机来查询回答
生存期：（time to live）TTL，最大2天
域以soa记录必须有NS记录
SOA权威的开始
PTR反向解析
CNAME不能与任意记录类型并存，区本身不能做cname
动态域名不能做泛解析
权威域名服务器（Authority name server）：终结者域名服务器（只告知结果或该去哪找）: .（全球共有13个服务节点）
缓存（递归、迭代）域名服务器（Resolver）：苦力型域名服务器（一直找到权威域名服务器都说没有为止）
•缓存周期为TTL设定值（由权威域名服务器来决定）
•每次选择name server时根据RTT（Round-Trip-Time）值来决定选择结果
BIND8.2.3和9.1.0能抵御所有常见的攻击
BIND8和9支持关于查询、区传送和动态更新的访问控制表
SOA指示该区的权威
NS列出该区的一个名字服务器
A名字到地址的映射
PTR地址到名字的映射
CNAME规范名字（相当于别名而言）


**SOA记录**
SOA记录表示对该区数据而言，这个名字服务器就是最好的信息来源。根据这个SOA记录，我们的名字服务器就享有对区movie.edu的权威。每个db.DOMAIN和db.ADDR
文件都要有SOA记录。每个区数据文件中允许有一个也只允许有一个SOA记录。
我们把下面的SOA记录添加到db.movie.edu文件中：
movie.edu. IN SOA terminator.movie.edu. al.robocop.movie.edu. ( 
                         1        ; 序列号
                          3h       ; 3小时后刷新
                          1h       ; 1小时后重试
                          1w       ; 1周后期满
                          1h )     ; 否定缓存TTL为1小时
名字movie.edu.必须从文件的第一列开始。要确保这个名字是以“.”结尾。
IN表示Internet。
SOA后面的第一个名字（ternimator.movie.edu.）是movie.edu区的主名字服务器的名字。第二个名字（al.robocop.movie.edu.）是管理该区的人的电子邮件地址（如果你把第一个“.”换成“@”的话）。你经常会看到将root、postmaster或hostmaster作为电子邮件地址。名字服务器不会使用这个邮件地址 —— 它只对人有意义。


同主机表查找不同，对于一个名字，一次DNS查找可以返回多个地址。查找wormhole.movie.edu就会返回两个地址。如果查询者和名字服务器处于同一网络，为了获得更好的性能，一些名字服务器会把离查询者最近的地址放在回答的最前面。这个特性被称为“地址排序”。


**PTR记录**
下面我们要创建地址到名字的映射。db.192.249.49这个文件将网络192.249.249/24里的地址映射到主机名。这种映射使用的DNS资源记录是PTR（pointer 指针）记录。网络上每个网络接口都有一个这样的记录。（回忆一下在DNS中像查找名字那样查找地址。地址被反转过来，然后在后面加上in-addr.arpa。）
下面是我们为网络192.249.249/24添加的PTR记录：
1.249.249.192.in-addr.arpa.  IN PTR wormhole.movie.edu.
2.249.249.192.in-addr.arpa.  IN PTR robocop.movie.edu.
3.249.249.192.in-addr.arpa.  IN PTR terminator.movie.edu.
4.249.249.192.in-addr.arpa.  IN PTR diehard.movie.edu.


**回送地址**
名字服务器还需要一个额外的有关回送网络（loopback network）的db.ADDR文件，回送地址（loopback address）是一个特殊的地址，主机用它来将数据流导向自己。回送网络（通常）是127.0.0/24，而主机号（通常）是127.0.0.1。因此这个文件名就是db.127.0.0。
为什么名字服务器需要这个小文件呢？让我们先来想一下。没有人负责网络127.0.0/24，但是系统却用它来作为回送地址。既然没有人直接负责，那么使用它的每个人都要自己负责管理。你可能会忽略这个文件，而你的名字服务器仍能继续工作。但是如果查找127.0.0.1就会失败，因为所联系的根名字服务器本身并没有配置为将127.0.0.1映射到某个名字。你应该自己提供这个映射，那么就没有问题了。


**根线索（root hint）数据**
除了你的本地信息以外，名字服务器还需要知道负责根区的名字服务器在何处。这个信息只能从Internet主机ftp.rs.internic.net（198.41.0.6）那里查到。使用匿名FTP
从上面的domain子目录里下载named.root这个文件。（named.root就是我们前面称为db.cache的那个文件。在你下载后把它改名为db.cache就可以了。）
or
dig @a.root-servers.net . ns > named.root


**Bind注释**
在BIND 8和9中，你可以使用三种风格的注释：C语言风格、C＋＋风格和shell
风格：
/* 这是C语言风格的注释*/
// 这是C++风格的注释
# 这是shell风格的注释


配置文件中只能有一条option语句


符号 @ 
如果一个域名和起点相同,那么这个名字就可以写成“@”。这在区数据文件的SOA
记录中最常见。可以如下输入 SOA 记录:
@ IN SOA terminator.movie.edu. al.robocop.movie.edu. ( 1 ; 序列号
      3h ; 3 小时后刷新
      1h ; 1 小时后重试
      1w ;1周后期满
      1h) ;否定缓存TTL为1小时


**A记录可以有PTR**
EXAMPLE
$TTL    600
@       IN      SOA     movie.edu. zonghao.movie.edu.   (
                1
                3600
                7200
                3600
                60)
        IN      NS      10.210.128.191.
1       IN      PTR     a.movie.edu.
2       IN      PTR     b.movie.edu.
192 IN  PTR     a.movie.edu.
191 IN  PTR     moive.edu.


**重复最后一个名字**
如果一个资源记录名(从第一列开始)是一个空格或者制表符,那么就沿用上一个 记录的名字。如果一个名字有多个资源记录,你就可以使用这个方法。下面这个例 子就是一个名字有两个地址记录:
wormhole IN A 192.249.249.1 IN A 192.253.253.1
在第二个地址记录中,暗示wormhole 就是其对应的名字。即使对于不同类型的资源 记录,也可以使用这种捷径。


主机名中不允许使用下划线


**MX记录**
DNS用一种资源记录类型来实现增强的邮件路由
邮件收发器总是最新向优先级最小的邮件交换器发送邮件
plange.puntacana.dr. IN MX 1 listo.puntacana.dr. 
plange.puntacana.dr. IN MX 2 hep.puntacana.dr.


**SOA值**
movie.edu. IN SOA terminator.movie.edu. al.robocop.movie.edu. ( 
            1 ; 序列号   serial number可以用日期做序列号会更有意义
            3h ; 3小时候刷新   refresh告诉辅名字服务器相隔多久检查该区的数据是否是最新的
            1h ; 1小时后重试   retry辅名字服务器超过刷新时间后无法访问主机名字服务器，就会每隔一段时间重试ß
            1w ; 1周后期满    
            1h) ;否定缓存TTL为1小时
一周有608400秒
soa记录十一什么样的值完全取决于你的需要，通常，时间越长，名字服务器的负担就越轻，而变动传播的时间久越长


**loopback network 有几个NS就写几个**
EXAMPLE
$TTL 3600
@       IN      SOA     localhost. root.localhost.  (
                        1997022700 ; Serial
                        3600      ; Refresh
                        1800      ; Retry
                        604800    ; Expire
                        3600)     ; Minimum
                IN      NS      localhost.
1       IN      PTR    localhost.


**配置主机**
解析器允许多个nameserver（最多3个），默认超时时间是5秒，禁止使用回送地址
如果没有收到对查询响应或者超时了，那么解析器将按照他们的排列循序一次向这些名字服务器请求查询
tootsie: HOST name lookup failure，等到此信息出现需要75秒



```


### 解析器配置示例

```
resolv.conf
分为两种情况
1、主机有本地名字服务器
2、主机使用远程名字服务器

唯一解析器
cat /etc/resolv.conf
search weibo.com git.intra.sina.com.cn
nameserver 10.210.142.100
nameserver 8.8.8.8

本地名字服务器
cat /etc/resolv.conf
domain movie.edu
nameserver 0.0.0.0
nameserver 10.210.142.100
nameserver 10.210.128.191
options timeout:2
````


### 启动和说明

```
CNAME记录不能和任何记录共存
acl.cernet（教育网）acl.cnc（联通）acl.tel（电信）acl.trusted（其他）

/usr/local/bind9/bin/named_reload.sh
/usr/local/bind9/sbin/rndc -s server_123_5 reload
/usr/local/bind9/sbin/rndc -s server_172_5 reload
/usr/local/bind9/sbin/rndc -s server_123_18 reload
/usr/local/bind9/sbin/rndc -s server_172_18 reload

/usr/local/bind9/sbin/rndc flush (全部清空缓存)
/usr/local/bind9/sbin/rndc flushname p.tanx.com  (清空域名缓存)

1GB带宽=100万PPS
```




### 缓存的发返回记录

```
缓存
nx domain
no error

不缓存
REFUSED
service fused
timed out
```



### 命令

```
dig @10.210.12.10 api.weibo.cn +time=2 +tries=5
dig -x 14.17.44.35 @10.55.21.254 +noquestion +noauthority +noadditional +nostats +nocomments
dig @a.root-servers.net . ns > named.root
```