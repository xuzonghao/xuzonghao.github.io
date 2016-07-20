---
layout: post
title:  "Zabbix - LLD"
date:   2016-07-20
categories: Zabbix
---

什么是LLD？

```
英文Low-level discovery的缩写，可以翻译成自动发现。

我们要监控的对象如果是固定的，那直接添加一个item就可以了，但是如果不是固定的，那就需要用LLD。

举几个例子：
1、如果要监控CPU(s)使用率，这在Linux里是一个固定的值，添加一个item就可以。
如果要监控每个cpu的使用率，这就需要LLD了，因为每个机器的cpu个数都不一样，我们需要先发现cpu个数，在监控每个cpu的使用率。
2、每个机器的分区也不一样，需要用到LLD
3、每个交换机的网口个数也不一样，需要用到LLD

最后你会发现好像要监控的对象大部分都是不固定的，到处都是LLD。

Zabbix官方其实早就想到了这点，为我们做好了6个自动发现功能，真贴心
discovery of file systems;（vfs.fs.discovery）
discovery of network interfaces; (net.if.discovery)
discovery of CPUs and CPU cores; (system.cpu.discovery)
discovery of SNMP OIDs; (discovery[{#MACRO1}, oid1, {#MACRO2}, oid2, …,])
discovery using ODBC SQL queries; (db.odbc.discovery[<description>,<dsn>])
discovery of Windows services. (service.discovery)

这几个自动发现已经满足我们一些基本需求了，但是我们的需求是无穷无尽的，所以我们还需要自己构建一些自动发现。

在讲构建之前先来一点基础知识，Zabbix的自动发现可以处理json格式的数据，所以我们写的自动发现程序需要输出json格式。

先来看几个输出的例子：
```
![a](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/zabbix/png/a.jpg?raw=true "a")


```
为了方便理解，你可以把这个json数据看成excel里的三列，SNMPINDEX、IFDESCR、IFPHYSADDRESS。
再来看一个例子：
```
![a](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/zabbix/png/b.jpg?raw=true "a")

```
为了方便理解，你可以把这个json数据看成excel里的两列，HOST、COUNT。

看完这2个例子，就容易理解多了吧。
假如我们有一个excel，有1列的数据，也有2列的数据，也有3列的数据，按照json输出给zabbix就可以了。
```

# 几个真实案例：
## 案例一：每一行写了一个url的文本文件，用LLD监控是否可以？

```
当然可以，虽说系统带了Web scenarios，但是如果有5000行url，一个一个添加也会死人的。

假如我们的url.txt内容是

http://www.163.com/index.html
http://www.sohu.com/index.html
http://www.sina.com.cn/index.html
http://www.taobao.com/index.html
。。。省略5000行。。。
http://www.qq.com/index.html

很明显这相当于只有一列的excel，我们可以先建立一个模板，里面建立一个LLD，如果你想让哪个机器去访问这几个url，链接到这个模板就可以，如果多个机器链接到这个模板，相当于多机器监控这些url。如果机器够分布，够多，那岂不是可以监控多机房多链路了？

如果就一列，可以把所有数据放到一个array里，计算一下array的长度，除了最后一行特殊处理一下，前面的统一输出，因为json输出的最后少一个逗号，所以要特殊处理一下。不过我为了通用，并没这样用，哈哈，下面有贴我写的程序。
```

## 案例二、一个特殊需求，我们希望绑定HOST去访问url

```
http://www.163.com/index.html  192.168.99.2
http://www.sohu.com/index.html
http://www.sina.com.cn/index.html 192.168.99.5
http://www.taobao.com/index.html
。。。省略5000行。。。
http://www.qq.com/index.html 192.168.99.7 

这相当于excel里的两列，第一列URL，第二列HOST，所有我就写了一个shell，来构造出需要的json数据，这个简单修改一下，可以适用于任何列。
```

![a](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/zabbix/png/c.jpg?raw=true "a")


```
构造一个LLD发现程序就到这里了，我们可以触类旁通，想象力是无穷的，比如：
自动发现redis、mysql、http、oracle表空间，就是找出要监控的数据输出json数据，总之一句话，只要想发现，都可以自动发现。

最后就是zabbix_agentd.conf配置文件添加key。再写一个shell容许传三个参数（url、host、returncode）进去运行后得到返回值，web添加监控项，监控项里的key写web.monitor[{#URL},{#HOST},xxx]，对于url监控来说，xxx无非就是监控返回码、相应时间、下载速率，然后添加trigger、action等等，
```