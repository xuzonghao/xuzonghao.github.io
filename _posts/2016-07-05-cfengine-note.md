---
layout: post
title:  "cfengine note"
date:   2016-07-05
categories: cfengine
---

# cfengine安装（服务端和客户端都需要安装）

```
rpm -qa db4
yum install wget gcc make openssl openssl-devel db4-devel bzip2-devel gcc gcc-c++ flex pcre-devel -y
wget http://www.cfengine.org/downloads/cfengine-2.2.8.tar.gz &&  tar xvf cfengine-2.2.8.tar.gz && cd cfengine-2.2.8 
./configure --prefix=/usr/local/cfengine && make && make install 
mkdir -p /var/cfengine/inputs && mkdir /var/cfengine/outputs && mkdir /var/cfengine/bin
cp /usr/local/cfengine/sbin/cfagent /var/cfengine/bin && cp /usr/local/cfengine/sbin/cfenvd /var/cfengine/bin && cp /usr/local/cfengine/sbin/cfexecd /var/cfengine/bin
cp /usr/local/cfengine/sbin/cfservd /var/cfengine/bin

生成ppkeys
/usr/local/cfengine/sbin/cfkey

目录结构
/usr/local/sbin/							//cfengine的二进制文件
/usr/local/lib/								//cfengine的链接库文件
/usr/local/share/cfengine					//cfengine的配置文件example
/var/cfengine								//cfengine正常运行期间所使用的文件都位于这个目录，该目录安装之后并不存在，需要手动建立
/var/cfengine/bin							//重要的二进制文件都被复制到这里，以保证在调用这些二进制文件时都是可用的
/var/cfengine/inputs						//保存cfengine所需配置文件的标准位置，这个目录里将保存三个配置文件,update.conf,cfagent.conf和cfservd.conf
/var/cfengine/outputs						//保存每次cfexecd时的输出文件
/var/cfengine/ppkeys						//该目录用于保存系统的公钥和私钥以及其他系统的公钥
/var/cfengine/cengine.${servername}.runlog	//cfagent的每个字段的执行输出信息

mkdir /data1 && ln -s /data0/cfengine /data1/cfengine && ln -s /data0/cfengine /data1/local && ln -s /data0/cfengine/appfiles/safe_gate /data1/safe_gate	

工作方式
Cfengine服务器向其他服务器进行配置同步的方式，可以采用“拉”和“推”两种方式。
拉方式：
1)	服务器端运行cfservd进程（需要配置cfservd.conf），并准备一个正确的策略文件cfagent.conf供客户机下载。
2)	客户机使用cfagent或者通过cfexecd调用cfagent运行update.conf的配置，连接到服务器的cfserevd进程，下载cfagent.conf，并按照这个文件的策略来执行操作。

```


# 配置

```
服务端上传3个配置文件
update.conf 到 /usr/local/cfengine
cfagent.conf 到 /var/cfengine/inputs
cfservd.conf 到 /data0/cfengine/conf/

启动
 /usr/local/cfengine/sbin/cfservd -f -n /data0/cfengine/conf/cfservd.conf


客户端手动同步
update.conf 到 /usr/local/cfengine
/usr/local/cfengine/sbin/cfagent -qvv -f /usr/local/cfengine/update.conf

客户端守护执行
/var/cfengine/bin/cfexecd
```


# 排错

```
如果出现bad key问题

在服务端/var/cfengine/ppkeys删除root开头的key文件

```