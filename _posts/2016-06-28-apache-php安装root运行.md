---
layout: post
title:  "apache root用户启动，php"
date:   2016-06-28
categories: apache-php
---

# 编译安装

```
1、卸载系统默认安装的apr、apr-util
      rpm -e  --allmatches --nodeps apr-util
      rpm -e  --allmatches --nodeps apr
2、安装apr
      tar  zxvf   apr-1.4.6.tar.gz && cd  apr-1.4.6 && ./configure  --prefix=/usr/local/apr
      make && make install
      echo "/usr/local/apr/lib" >>/etc/ld.so.conf
      ldconfig
2、安装apr-util   
      tar   zxvf  apr-util-1.5.1.tar.gz  && cd  apr-util-1.5.1 && ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
      make && make install
3、安装pcre
      tar  zxvf   pcre-8.21.tar.gz && cd  pcre-8.21 && ./configure --prefix=/usr/local/pcre --with-apr=/usr/local/apr
      make && make install
4、安装httpd   
      tar zxvf   httpd-2.4.3.tar.gz  && cd  httpd-2.4.3
      cp -r /root/apr-1.4.6 /root/httpd-2.4.3/srclib/apr
      cp -r /root/apr-util-1.5.1 /root/httpd-2.4.3/srclib/apr-util
      修改Apache 源代码，在 include/http_config.h  头部中自行添加如下语句  
      #ifndef BIG_SECURITY_HOLE 
      #define BIG_SECURITY_HOLE 
      #endif
      ./configure --prefix=/usr/local/apache --with-mpm=prefork --with-apr=/usr/local/apr --with-included-apr --with-apr-util=/usr/local/apr-util --with-pcre=/usr/local/pcre --enable-so --enable-rewrite --enable-mods-shared=all
      make && make install
      cd    /usr/local/apache
6、编辑httpd.conf
       ......
       User root 
       Group root
       ......
7、重启apache
       /usr/local/apache/bin/apachectl restart
8、安装php
      tar xvf php-5.3.5.tar.gz && cd php-5.3.5
      ./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache/bin/apxs
      make && make install
      cp php.ini-development /usr/local/php/lib/php.ini
9、Apache与php结合，修改配置文件
      vim /usr/local/apache/conf/httpd.conf
      <IfModule dir_module>
         DirectoryIndex index.html index.htm default.htm default.html index.php index.php3 index.jsp index.cgi
      </IfModule>
```


# 简易安装

```
1、apache 2.4.3安装
主程序：
复制apache.tar.gz 到/usr/local/解压生成apache目录
配置httpd.conf：
AddType application/x-httpd-php .php .phtml .php3 .inc
模块：apachectl -t -D DUMP_MODULES


2、php5.4.3安装
主程序：
复制php.tar.gz 到/usr/local/解压生成php目录
模块：
php -m
Core ctype curl date dom ereg fileinfo filter gd hash iconv iplookup-php-static json libxml mysql mysqli pcre PDO pdo_mysql pdo_sqlite Phar posix Reflection session SimpleXML SPL SQLite sqlite3 standard tokenizer xml xmlreader xmlwriter zip zlib
复制php-modules.tar.gz 到/usr/lib64/解压生成php/modules模块目录和php/pear

3、加载PATH
# cat /root/.bash_profile 
IP=`ifconfig|grep "inet addr"|grep -v 127.0.0.1|awk -F':' '{print $2}'|awk '{print $1}'|tr -s "\n" "|"`
PS1="\n\e[1;36m[\e[m\e[1;32m\u\e[m\e[1;37m@\e[m\e[1;33m${IP}\e[m \e[1;31m\A\e[m \w\e[m\e[1;36m]\e[m\e[1;36m\e[m\n\\$ "

PATH=$PATH:$HOME/bin:/usr/local/php/bin:/usr/local/apache/bin

```

