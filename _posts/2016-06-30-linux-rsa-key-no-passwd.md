---
layout: post
title:  "linux系统使用rsa无密码验证"
date:   2016-06-30
categories: linux
---

yum -y install openssh-clients

# 原理

```
SSH无密码验证的原理
 Master作为客户端，要实现无密码公钥认证，连接到服务器Salve上时，需要在Master上生成一个密钥对，包括一个公钥和一个私钥，而后将公钥复制到所有的Salve上。当Master通过SSH链接到Salve上时，Salve会生成一个随机数并用Master的公钥对随机数进行加密，并发送给Master。Master收到加密数之后再用私钥解密，并将解密数回传给Salve，Salve确认解密数无误之后就允许Master进行连接了。这就是一个公钥认证过程，期间不需要手工输入密码，重要的过程是将Master上产生的公钥复制到Salve上。
```


# a服务器

```
vim /etc/ssh/sshd_config 
RSAAuthentication yes # 启用 RSA 认证
PubkeyAuthentication yes # 启用公钥私钥配对认证方式
AuthorizedKeysFile .ssh/authorized_keys # 公钥文件路径
重启SSH服务： service sshd restart 

ssh-keygen -t rsa  
输入后，会提示创建.ssh/id_rsa、id_rsa.pub的文件，其中第一个为密钥，第二个为公钥。过程中会要求输入密码，为了ssh访问过程无须密码，可以直接回车 。
cat id_rsa.pub >> ~/.ssh/authorized_keys  && chmod 600 authorized_keys      

查看本机是否可以SSH无需密码登录： ssh localhost ,以上证明本机登录成功

ssh-keygen:生成秘钥
其中：
  -t指定算法
  -f 指定生成秘钥路径
  -N 指定密码
```


# b服务器

```
将公钥复制到被管理机器下的.ssh目录下（先确保存在这个目录）
mkdir .ssh && chmod 700 .ssh && cd .ssh
将管理机id_rsa.pub传输到本机.ssh下
cat id_rsa.pub >> ~/.ssh/authorized_keys 
chmod 600 authorized_keys && cd ..
```

# 主机验证

```
ssh 10.210.128.193 
```


# 注意

```
注意几点：
  1> SSH的配置文件一定要修改，而且修改后要重启
  2> 认证文件一定要采用追加方式：cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  3> authorized_keys文件的权限一定要修改为600
  4> .ssh的文件如果是手动创建的话权限一定要修改为700 
```