---
layout: post
title:  "使用XtraBackup进行热备、恢复、主从恢复"
date:   2016-04-26
categories: DBA
---


# 官网

```
http://www.percona.com/downloads/XtraBackup/
前面介绍mysqldump备份方式是采用逻辑备份，其最大的缺陷就是备份和恢复速度都慢，对于一个小于50G的数据库而言，这个速度还是能接受的，但如果数据库非常大，那再使用mysqldump备份就不太适合了。而使用lvm快照功能对数据库进行备份，可以实现几乎热备的功能，但备份过程较为复杂，不过现在倒是有个工具mylvmbackup可以实现自动化备份。
```


# 使用说明

```
InnoDB 有个商业的InnoDB Hotbackup，可以对InnoDB引擎的表实现在线热备。而 percona出品的Xtrabackup，是InnoDB Hotbackup的一个开源替代品，可以在线对InnoDB/XtraDB引擎的表进行物理备份。mysqldump支持在线备份，不过是逻辑备份，效率比较差。xtrabackup是开源的MySQL备份工具，物理备份，效率很不错。

Xtrabackup有两个主要的工具：xtrabackup、innobackupex，其中xtrabackup只能备份InnoDB和XtraDB两种数据表，innobackupex则封装了xtrabackup，同时可以备份MyISAM数据表。Xtrabackup做备份的时候不能备份表结构、触发器等等，智能纷纷.idb数据文件。另外innobackupex还不能完全支持增量备份，需要和xtrabackup结合起来实现全备的功能。

InnoDB可以不停止服务、MyISAM必须锁表停止写入
```



# mysql和XtraBackup版本对应信息

```
5.5 mysql, 使用2.0版本
5.0 mysql，使用1.6版本
同时根据系统版本使用XtraBackup版本
```


# 安装mysql

```
Mysql安装（要保证主从配置文件id的不同,在my.cnf里必须指定datadir=/usr/local/mysql/var）
yum install -y gcc gcc-c++ autoconf automake zlib-devel libxml2-devel ncurses-devel libmcrypt* libtool* cmake
/usr/sbin/useradd mysql -M -s /sbin/nologin 
mkdir /data1/mysql
tar xvf mysql-5.5.13.tar.gz && cd mysql-5.5.13
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS:STRING=utf8,gbk -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DENABLED_LOCAL_INFILE=1 -DMYSQL_TCP_PORT=3306 -DMYSQL_DATADIR=/data1/mysql
make && make install
初始化数据库
cd /usr/local/mysql/scripts
chmod +x mysql_install_db
./mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/var  			//初始化datadir目录
chown -R mysql:mysql /data1/mysql  		//配置目录权限
/usr/local/mysql/bin/mysqladmin -uroot password ""			//设置密码

**mysql编译完的安装包可以直接复制到从服务器上使用，但是需要删除datadir下数据并初始化数据且赋给相应权限**
```


# 安装XtraBackup

## rpm安装

```
yum -y install perl perl-devel libaio libaio-devel perl-Time-HiRes perl-DBD-MySQL
rpm -ivh percona-xtrabackup-2.2.10-1.el6.x86_64.rpm 		
```

## tar安装

```
tar xvf percona-xtrabackup-2.0.0.tar.gz && cd percona-xtrabackup-2.0.0   	//无需安装，直接使用
vim /root/.bash_profile +
PATH=$PATH:$HOME/bin:/usr/local/bin:/usr/local/java/bin:/usr/local/mysql/bin:/data1/jira_install/percona-xtrabackup-2.0.0/bin		//设置percona-xtrabackup-2.0.0环境变量
source /root/.bash_profile
```


# 全量备份、恢复

```
**备份**
mkdir  /data1/xtrabackup/ -p
innobackupex --user=root --password=** --defaults-file=/etc/my.cnf  /data1/xtrabackup
--defaults-file 是你要备份数据库启动时候指定的配置文件，在my.cnf里必须指定datadir=/usr/local/mysql/var
/data1/xtrabackup 是备份到的地方
完成后显示innobackupex: completed OK!
将备份完成的数据打包发送到备机

**恢复**
rsync -avzP 10.210.137.217::jira/data1/xtrabackup/2016-04-26_17-42-11 .
rm -rf datadir/*    //删除mysql数据目录下所有数据
mkdir var     //创建空目录
innobackupex --user=root --password=** --defaults-file=/etc/my.cnf  --copy-back /data1/xtrabackup/2016-04-20_17-26-24   
完成后显示innobackupex: completed OK!
恢复说明：
innobackupex --user=root --password --defaults-file=/etc/my.cnf  --apply-log /data/back_data/db/  
(--apply-log选项的命令是准备在一个备份上启动mysql服务)
innobackupex --user=root --password --defaults-file=/etc/my.cnf  --copy-back /data/back_data/db/  
(--copy-back 选项的命令从备份目录拷贝数据,索引,日志到my.cnf文件里规定的初始位置。)


/data1/jira_install/mysql-5.1.57/scripts/mysql_install_db --datadir=/usr/local/mysql/var 		//重置一下datadir
chown mysql.mysql * -R  //设定权限
/usr/local/mysql/bin/mysqld_safe &

测试mysql是否可以重启
mysqladmin shutdown -uroot && /usr/local/mysql/bin/mysqld_safe &
```


#单个数据库备份、恢复

```
**备份**
innobackupex --user=root --password=** --defaults-file=/etc/my.cnf --databases="jira4" /data1/xtrabackup
将备份完成的数据打包发送到备机


**恢复**
rsync -avzP 10.210.137.217::jira/data1/xtrabackup/2016-04-26_17-42-10 .
rm -rf datadir/*    //删除mysql数据目录下所有数据
mkdir var     //创建空目录
innobackupex --user=root --password=** --defaults-file=/etc/my.cnf  --copy-back --database="jira4" /data1/xtrabackup/2016-04-20_17-42-11

/data1/jira_install/mysql-5.1.57/scripts/mysql_install_db --datadir=/usr/local/mysql/var 		//重置一下datadir
chown mysql.mysql * -R  //设定权限
/usr/local/mysql/bin/mysqld_safe &

测试mysql是否可以重启
mysqladmin shutdown -uroot && /usr/local/mysql/bin/mysqld_safe &
```



# 查看使用xtrabackup备份信息

```
查看备份信息
# cat xtrabackup_checkpoints
backup_type = full-backuped
from_lsn = 0
to_lsn = 1607131
last_lsn = 1607131
compact = 0

最后看这个文件：
# cat /data1/2016-04-20_17-26-24/xtrabackup_binlog_info 
cat xtrabackup_binlog_info
mysql-bin.000017        41757311        jira4			//在从服务器上需要用到此信息

```



#主从设置

```
**从服务器my.cnf增加log更新选项**
log-slave-updates



**测试时候需要建立，正常情况无需此步骤操作**
mysql> CREATE DATABASE `backup_pool` DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;		//创建数据库，以UTF8模式
mysql> grant ALL on *.* to root@"%" identified by 'newpwd' WITH GRANT OPTION;		//设置库权限，开启账号权限传递功能
mysql> select user,host from mysql.user;											//查看用户和主机
mysql> show grants for root;   													//查看用户权限
mysql> show variables like '%storage_engine%';										//查看当前存储引擎
mysql> show processlist;														//显示当前运行的Query：
mysql> show master status;										//查看主数据库状态（position位置），记下file和position的值，从服务器设置需要用到
mysql> flush privileges;



**主开启相应权限**
mysql> GRANT REPLICATION SLAVE ON *.* to 'sync'@'10.210.137.50' identified by '';
mysql> select user,host from mysql.user;  //查看是否创建用户成功
mysql> flush privileges;					//刷新权限 



**重新设置同步信息**
mysql> slave stop;
mysql> reset slave;
mysql> show slave status\G



**从开启相应权限并设置同步**
mysql> change master to master_host='10.210.137.217',
master_user='sync', 
master_password='',
master_log_file='mysql-bin.000017', 
master_log_pos=41757311,
master_port=3306;
mysql> slave start;
mysql> flush privileges;
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.210.137.217
                  Master_User: sync
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000017
          Read_Master_Log_Pos: 45847279
               Relay_Log_File: localhost-relay-bin.000002
                Relay_Log_Pos: 4090219
        Relay_Master_Log_File: mysql-bin.000017
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 45847279
              Relay_Log_Space: 4090378
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
1 row in set (0.00 sec)


此时从开始自动追加同步信息



**查看追加binlog**
在主数据库上查看10.210.137.50
mysql> show processlist;
+------+------+---------------------+-------+-------------+-------+----------------------------------------------------------------+------------------+
| Id   | User | Host                | db    | Command     | Time  | State                                                          | Info             |
+------+------+---------------------+-------+-------------+-------+----------------------------------------------------------------+------------------+
| 1734 | root | localhost:34382     | jira4 | Sleep       |   348 |                                                                | NULL             |
| 1744 | root | localhost:34467     | jira4 | Sleep       |  6214 |                                                                | NULL             |
| 1745 | root | localhost:34474     | jira4 | Sleep       | 15847 |                                                                | NULL             |
| 1747 | root | localhost:34495     | jira4 | Sleep       |  6806 |                                                                | NULL             |
| 1780 | root | localhost:36711     | jira4 | Sleep       |    50 |                                                                | NULL             |
| 1782 | root | localhost:36713     | jira4 | Sleep       |  2390 |                                                                | NULL             |
| 1783 | root | localhost:36714     | jira4 | Sleep       |    17 |                                                                | NULL             |
| 1784 | root | localhost:36715     | jira4 | Sleep       | 11338 |                                                                | NULL             |
| 1884 | sync | 10.210.137.50:51355 | NULL  | Binlog Dump |   254 | Has sent all binlog to slave; waiting for binlog to be updated | NULL             |
| 1886 | root | localhost:43348     | NULL  | Query       |     0 | NULL                                                           | show processlist |
+------+------+---------------------+-------+-------------+-------+----------------------------------------------------------------+------------------+
10 rows in set (0.00 sec)

```



# 附上my.cnf

```
 cat  /etc/my.cnf |grep -v "^#"|grep -v "^$"
[mysql]
default-character-set=utf8
[client]
port            = 3306
socket          = /tmp/mysql.sock
default-character-set=utf8
[mysqld]
default-character-set=utf8
port            = 3306
socket          = /tmp/mysql.sock
skip-locking
key_buffer_size = 384M
max_allowed_packet = 100M
sort_buffer_size = 2M
read_buffer_size = 2M
read_rnd_buffer_size = 8M
myisam_sort_buffer_size = 64M
thread_cache_size = 8
query_cache_size = 32M
thread_concurrency = 24
datadir=/usr/local/mysql/var
log-bin=mysql-bin
server-id       = 11
binlog-do-db=jira_new_host
binlog-do-db=jira4
default-storage-engine=innodb
wait_timeout=31536000
interactive_timeout=31536000
default-character-set=utf8
innodb_data_home_dir = /usr/local/mysql/var/
innodb_data_file_path = ibdata1:2000M;ibdata2:10M:autoextend
innodb_log_group_home_dir = /usr/local/mysql/var/
innodb_buffer_pool_size = 3840M
innodb_additional_mem_pool_size = 2000M
innodb_log_file_size = 1000M
innodb_log_buffer_size = 8M
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
binlog_format=row
[mysqldump]
quick
max_allowed_packet = 16M
[mysql]
no-auto-rehash
[myisamchk]
key_buffer_size = 256M
sort_buffer_size = 256M
read_buffer = 2M
write_buffer = 2M
[mysqlhotcopy]
interactive-timeout
```
