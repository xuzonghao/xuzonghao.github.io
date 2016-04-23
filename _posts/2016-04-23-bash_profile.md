---
layout: post
title:  ".bash_profile"
date:   2016-04-23
categories: linux kernel
---


# 设置字符提示栏为本机IP和时间，同时设置环境变量

```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

# [username@hostname /path/to/current/dir/]
IP=`ifconfig|grep "inet addr"|grep -v 127.0.0.1|awk -F':' '{print $2}'|awk '{print $1}'|tr -s "\n" "|"`
PS1="\n\e[1;36m[\e[m\e[1;32m\u\e[m\e[1;37m@\e[m\e[1;33m${IP}\e[m \e[1;31m\A\e[m \w\e[m\e[1;36m]\e[m\e[1;36m\e[m\n\\$ "


#PATH=$PATH:$HOME/bin
PATH=$PATH:$HOME/bin:/usr/local/bin:/usr/local/apache2/bin:/usr/local/php5/bin

export PATH
unset USERNAME
```