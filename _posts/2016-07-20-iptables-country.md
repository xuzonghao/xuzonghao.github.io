---
layout: post
title:  "使用iptables，按国别阻止网络流量"
date:   2016-07-20
categories: iptables
---



# 在Linux上按国别阻止网络流量

```
需要根据地理位置，有选择性地阻止或允许网络流量。比如说，你遇到了拒绝服务攻击，这些攻击主要源自在某一个国家注册的IP地址。在其他情况下，出于安全方面的原因，你又想要阻止外国来历不明SSH登录请求；或者贵公司对在线视频拥有发行权，因而只可以分发给某些国家；或者由于地域限制方面的公司政策，你需要防止本地主机将文档上传到非美国远程云存储系统。

所有这些场景都需要能够安装一个防火墙，可以按国别对流量进行过滤。有几种方法可以做到这一点。举例说，你可以使用TCP包装器(TCP wrapper)，针对个别应用程序(比如SSH、NFS和httpd)设置有条件的阻止。其缺点是，你想要保护的那个应用程序在开发当初必须支持TCP包装器。此外，TCP包装器并非普遍出现在不同的平台上(比如说，Arch Linux已停止对TCP包装器的支持)。另一种办法就是，利用基于国家的GeoIP信息来设置ipset，然后将它运用于iptables规则。后一种方法更有希望，因为基于iptables的过滤与应用程序无关，而且易于设置。

我在本教程中将介绍另一种基于iptables的GeoIP过滤机制，这种机制实施了xtables-addons。有些读者对它还不熟悉，所以有必要先介绍一下，xtables-addons是一套面向netfilter/iptables的扩展。xtables-addons内含了一个名为xt_geoip的模块，该模块扩展了netfilter/iptables的功能，可以根据来源/目的地国家，过滤、NAT或管理数据包。如果你想使用xt_geoip，不需要重新编译内核或iptables，只需要构建xtables-addons模块，并使用当前的内核构建环境(/lib/modules/`uname -r`/build)。也不需要重启。一旦你构建并安装好了xtables-addons，xt_geoip立即就可以与iptables结合使用。

至于xt_geoip和ipset之间的区别，官方来源(http://xtables-addons.sourceforge.net/geoip.php)提到，xt_geoip在内存占用空间方面少于ipset。不过在匹配速度方面，基于散列的ipset可能具有优势。

在本教程其余部分，我会演示如何使用iptables/xt_geoip，根据来源/目的地国家，阻止网络流量。

将Xtables-addons安装到Linux上

下面介绍如何编译xtables-addons，并将它安装到不同的Linux平台上。

想构建xtables-addons，你就需要先安装几个依赖程序包。

·将依赖程序包安装到Debian、Ubuntu或Linux Mint上

$ sudo apt-get install iptables-dev xtables-addons-common libtext-csv-xs-perl pkg-config

·将依赖程序包安装到CentOS、RHEL或Fedora上

CentOS/RHEL 6需要先安装EPEL软件库(面向perl-Text-CSV_XS)。

$ sudo yum install gcc-c++ make automake kernel-devel-`uname -r` wget unzip iptables-devel perl-Text-CSV_XS

编译和安装Xtables-addons

从官方网站(http://xtables-addons.sourceforge.net)下载最新的xtables-addons源代码，然后构建/安装它，如下所示。

$wget http://downloads.sourceforge.net/project/xtables-addons/Xtables-addons/xtables-addons
-2.10.tar.xz
$ tar xf xtables-addons-2.10.tar.xz
$ cd xtables-addons-2.10
$ ./configure
$ make
$ sudo make install

请注意：如果是默认情况下已启用SELinux的基于红帽的系统(CentOS、RHEL、Fedora)，有必要调整SELinux策略，如下所示。要不然，SELinux会阻止iptables装入xt_geoip模块。

$ sudo chcon -vR --user=system_u /lib/modules/$(uname -r)/extra/*.ko
$ sudo chcon -vR --type=lib_t /lib64/xtables/*.so

为Xtables-addons安装GeoIP数据库

下一步是安装GeoIP数据库，xt_geoip将用到该数据库，用于IP与国别映射。很方便的是，xtables-addons源程序包随带两个帮助脚本，可分别用来从MaxMind下载GeoIP数据库，并将它转换成xt_geoip可识别的二进制格式。这些脚本位于源程序包里面的geoip文件夹下面。按照下列说明，即可构建GeoIP数据库，并将它安装到你系统上。

$ cd geoip
$ ./xt_geoip_dl
$ ./xt_geoip_build GeoIPCountryWhois.csv
$ sudo mkdir -p /usr/share/xt_geoip
$ sudo cp -r {BE,LE} /usr/share/xt_geoip

据MaxMind声称，其GeoIP数据库的准确性达到99.8%，数据库更每月都更新。为了确保本地安装的GeoIP数据库内容最新，你就需要设置每月执行的计划任务，以便每月更新一次本地GeoIP数据库。
阻止来自或发往某个国家的网络流量
一旦xt_geoip模块和GeoIP数据库都已安装好，你就可以立即使用iptables命令中的geoip匹配选项。

$ sudo iptables -m geoip --src-cc country[,country...] --dst-cc country[,country...]

你想要阻止的国家使用两个字母ISO3166代码来指定，比如说US(美国)、CN(中国)、IN(印度)和FR(法国)。

比如说，如果你想阻止来自也门(YE)和赞比亚(ZM)的入站流量，下面这个iptables命令就能实现。

$ sudo iptables -I INPUT -m geoip --src-cc YE,ZM -j DROP

如果你想阻止发往中国(CN)的出站流量，只要运行下面这个命令。

$ sudo iptables -A OUTPUT -m geoip --dst-cc CN -j DROP

匹配条件也可以被“抵消”，只要将“!”放在“–src-cc”或“–dst-cc”的前面。比如说：

如果你想在服务器上阻止所有非美国的入站流量，可以运行这个命令：

$ sudo iptables -I INPUT -m geoip ! --src-cc US -j DROP
```

![a](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/png/iptables-country.jpg?raw=true "a")


```
针对Firewall-cmd用户

像CentOS/RHEL 7或Fedora这一些发行版已将iptables换成firewalld，作为默认防火墙服务器。在这类系统上，你同样可以利用xt_geoip，使用firewall-cmd阻止流量。上面三个例子可以用firewall-cmd来改写，如下所示。

$ sudo firewall-cmd --direct --add-rule ipv4 filter INPUT 0 -m geoip --src-cc YE,ZM -j DROP
$ sudo firewall-cmd --direct --add-rule ipv4 filter OUTPUT 0 -m geoip --dst-cc CN -j DROP
$ sudo firewall-cmd --direct --add-rule ipv4 filter INPUT 0 -m geoip ! --src-cc US -j DROP

结束语

我在本教程中介绍了iptables/xt_geoip，这是一种简单方法，可以根据来源/目的地国家，对网络数据包进行过滤。如果需要的话，可以将这件有用的武器部署到你的防火墙系统中。最后提醒一句，我应该提到：基于GeoIP的流量过滤并不是在你服务器上阻止某些国家的万无一失的方法。GeoIP数据库天生就不准确/不完整，如果使用VPN、Tor或任何受到危及的中继主机，就很容易欺骗来源/目的地国家。基于地域的过滤甚至会阻止本不该被禁止的合法流量。明白这个局限性后，再决定将它部署到你的生产环境中也不迟。
```