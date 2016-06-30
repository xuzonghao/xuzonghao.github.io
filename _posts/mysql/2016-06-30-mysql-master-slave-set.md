---
layout: post
title:  "mysql主从恢复"
date:   2016-06-30
categories: Mysql
---


# master端操作

```
databases:cfdb

备份需要同步的库：
mysqldump -uroot -p cfdb > /data0/zonghao1/cfdb.20160630

查看所有用户和密码
mysql -uroot -psina.com -e "select User,Host,password from mysql.user;"

查看使用同步账号的密码
mysql -uroot -psina.com -e "show grants for replica@10.13.8.44;""
GRANT REPLICATION SLAVE ON *.* TO 'replica'@'10.55.21.240' IDENTIFIED BY PASSWORD '*D36660B5249B066D7AC5A1A14CECB71D36944CBC'

查看postion和log位置
mysql -uroot -psina.com -e "show master status" 
mysql-bin.000028  171070510

```

# slave操作

```
将cfdb.20160630同步到本机
rsync -avzP master::cfdb/cfdb.20160630 /data0/zonghao1/

增加my.cnf增量追加log-slave-updates

创建数据库（如果存在不需要创建）
create database apms character set utf8; 

停止和初始化slave
slave stop;
reset slave;
flush privileges;

导入数据库
mysqldump -uroot -p cfdb < /data0/zonghao1/cfdb.20160630

杀掉mysql进程
pkill mysql

启动mysql
/usr/local/mysql/bin/mysqld_safe &

设置主从
change master to master_host='10.210.128.64',
master_user='replica', 
master_password='',
master_log_file='mysql-bin.000028', 
master_log_pos=171070510,
master_port=3306;
flush privileges;

开启slave
slave start;

查看同步状态
show slave status\G
```