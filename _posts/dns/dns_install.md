---
layout: post
title:  "dns install"
date:   2016-09-23
categories: Dns
---


### bind 9.8.0 install 

```
EXAMPLE 10.210.12.10
**检查安装的模块**
./named -V
BIND 9.8.0 built with '--prefix=/usr/local/bind9' '--disable-openssl-version-check' '--enable-threads'
using OpenSSL version: OpenSSL 0.9.8e-rhel5 01 Jul 2008
using libxml2 version: 2.6.26

**安装bind需要的编译模块	安装openssl和libxml2**
yum install openssl-devel openssl gcc c+ make  libxml2 libxml2-devel -y


wget http://ftp.yz.yamagata-u.ac.jp/pub/network/isc/bind/9.8.0/bind-9.8.0.tar.gz

./configure '--prefix=/usr/local/bind9' '--disable-openssl-version-check' '--enable-threads'

make&&make install

**验证**
./named -V
BIND 9.8.0 built with '--prefix=/usr/local/bind9' '--disable-openssl-version-check' '--enable-threads'
using OpenSSL version: OpenSSL 1.0.1e 11 Feb 2013
using libxml2 version: 2.7.6

/usr/local/bind9/sbin/named -c /usr/local/bind9/etc/named.conf

**安装dig和nslookup**
yum install bind-utils 
```












