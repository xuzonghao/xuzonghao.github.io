---
layout: post
title:  "使用XtraBackup对Mysql热备、恢复、主从恢复"
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
rsync -avzP 10.210.137.217::mysql/mysql .     //同步mysql安装程序，原来的mysql程序可以删除，
tar -izxvf *.tar.gz    //将同步的xtrabakcup压缩文件进行解压，注意：一定要加i参数，否则无法成功恢复
rm -rf datadir/*    //删除mysql数据目录下所有数据
mkdir var     //创建空目录，如果存在不需创建

innobackupex --user=root --apply-log /data1/xtrabackup/2016-04-20_17-26-24  
//然后将备份文件中的日志应用到备份文件中的数据文件上,如果不操作此步骤，会报错如下：
InnoDB: time but the database was not shut down
InnoDB: normally after that.
160430 23:08:21 [ERROR] Plugin 'InnoDB' init function returned error.
160430 23:08:21 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
160430 23:08:21 [ERROR] Unknown/unsupported table type: innodb
160430 23:08:21 [ERROR] Aborting
160430 23:08:21 [Note] /data0/mysql/libexec/mysqld: Shutdown complete
160430 23:08:21 mysqld_safe mysqld from pid file /usr/local/mysql/var/localhost.localdomain.pid ended

innobackupex --user=root --defaults-file=/etc/my.cnf  --copy-back /data1/xtrabackup/2016-04-20_17-26-24   
//一般不需要密码
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
innobackupex --user=root --password=** --defaults-file=/etc/my.cnf --databases=jira4 /data1/xtrabackup
将备份完成的数据打包发送到备机


**恢复**
rsync -avzP 10.210.137.217::mysql/mysql .     //同步mysql安装程序，原来的mysql程序可以删除，
tar -izxvf *.tar.gz    //将同步的xtrabakcup压缩文件进行解压，注意：一定要加i参数，否则无法成功恢复
rm -rf datadir/*    //删除mysql数据目录下所有数据
mkdir var     //创建空目录，如果存在不需创建

innobackupex --user=root --apply-log /data1/xtrabackup/2016-04-20_17-42-11
//然后将备份文件中的日志应用到备份文件中的数据文件上,如果不操作此步骤，会报错如下：
InnoDB: time but the database was not shut down
InnoDB: normally after that.
160430 23:08:21 [ERROR] Plugin 'InnoDB' init function returned error.
160430 23:08:21 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
160430 23:08:21 [ERROR] Unknown/unsupported table type: innodb
160430 23:08:21 [ERROR] Aborting
160430 23:08:21 [Note] /data0/mysql/libexec/mysqld: Shutdown complete
160430 23:08:21 mysqld_safe mysqld from pid file /usr/local/mysql/var/localhost.localdomain.pid ended

innobackupex --user=root --defaults-file=/etc/my.cnf  --copy-back --database=jira4 /data1/xtrabackup/2016-04-20_17-42-11    //执行之前保证mysql为启动状态且datadir目录为空，一般不需要密码，因为数据目录被删除

/data1/jira_install/mysql-5.1.57/scripts/mysql_install_db --datadir=/usr/local/mysql/var 		//重置一下datadir
chown mysql.mysql * -R  //设定权限
/usr/local/mysql/bin/mysqld_safe &

测试mysql是否可以重启
mysqladmin shutdown -uroot && /usr/local/mysql/bin/mysqld_safe &
```


# 测试mysql是否可以使用

```
/usr/local/mysql/bin/mysql -h   //如果报错如下，说明确实lib
/usr/local/mysql/bin/mysql: error while loading shared libraries: libtinfo.so.5: cannot open shared object file: No such file or directory
ln -s libtinfo.so.5.7 libtinfo.so.5
ll /lib64/libtinfo.so.5
lrwxrwxrwx 1 root root 15 Apr 27 13:18 /lib64/libtinfo.so.5 -> libtinfo.so.5.7
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
mysql-bin.000017        79904816        jira4			//在从服务器上需要用到此信息

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
master_log_pos=79904816,
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
log-slave-updates
```



# mysql主从知识

```
MySQL主从复制几个重要的启动选项
　　(1)　 log-slave-updates
　　log-slave-updates这个参数用来配置从服务器的更新是否写入二进制日志，这个选项默认是不打开的，但是，如果这个从服务器B是服务器A的从服务器，同时还作为服务器C的主服务器，那么就需要开发这个选项，这样它的从服务器C才能获得它的二进制日志进行同步操作
　　(2)　 master-connect-retry
　　master-connect-retry这个参数是用来设置在和主服务器连接丢失的时候，重试的时间间隔，默认是60秒
　　(3)　 read-only
　　read-only是用来限制普通用户对从数据库的更新操作，以确保从数据库的安全性，不过如果是超级用户依然可以对从数据库进行更新操作
　　(4)　 slave-skip-errors
　　在复制过程中，由于各种的原因，从服务器可能会遇到执行BINLOG中的SQL出错的情况，在默认情况下，服务器会停止复制进程，不再进行同步，等到用户自行来处理。
　　Slave-skip-errors的作用就是用来定义复制过程中从服务器可以自动跳过的错误号，当复制过程中遇到定义的错误号，就可以自动跳过，直接执行后面的SQL语句。
　　--slave-skip-errors=[err1,err2,…….|ALL]
　　但必须注意的是，启动这个参数，如果处理不当，很可能造成主从数据库的数据不同步，在应用中需要根据实际情况，如果对数据完整性要求不是很严格，那么这个选项确实可以减轻维护的成本

```



# 其他说明

## 进行备份

### 说明

```
innobackupex是我们要使用的备份工具；

xtrabackup是被封装在innobackupex之中的，innobackupex运行时需要调用它；

xtrabackup_51是xtrabackup运行时需要调用的工具；

tar4ibd是以tar流的形式产生备份时用来打包的工具。
```


### 完整备份

```
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --database=test /mysqlbackup/  
其中，--user指定连接数据库的用户名，--password指定连接数据库的密码，--defaults-file指定数据库的配置文件，innobackupex要从其中获取datadir等信息；--database指定要备份的数据库，这里指定的数据库只对MyISAM表和InnoDB表的表结构有效，对于InnoDB 数据来说都是全备（所有数据库中的InnoDB数据都进行了备份，不是只备份指定的数据库，恢复时也一样）；/mysqlbackup是备份文件的存放位置。
```

### 完整备份并打包

```
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --database=test --stream=tar /mysqlbackup > /mysqlbackup/dbbackup20110809.tar
--stream指定流的格式，目前只支持tar
```

### 完整备份并打包压缩

```
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --database=test --stream=tar /mysqlbackup/ | gzip /mysqlbackup/dbbackup20110809.tar.gz
```


### 完整备份到远程主机

```
innobackupex --user=root --password= MySQLPASSWORD --defaults-file=/etc/my.cnf --database=test --stream=tar /mysqlbackup | ssh root@remote-host cat ">"   /mysqlbackup/dbbackup20110809.tar
```


### 增量备份

```
innobackupex --user=root --password=MySQLPASSWORD --database=test --incremental --incremental-basedir=/mysqlbackup/2011-08-09_14-50-20/ /mysqlbackup/trn/
--incremental指明是增量备份，--incremental-basedir指定上次完整备份或者增量备份文件的位置。这里的增量备份其实只针对的是InnoDB，对于MyISAM来说，还是完整备份
```



## 进行恢复
 
### 完整备份恢复

```
在进行恢复前，如果完整备份在远程主机上，首先将完整备份复制到本地主机上，如果是tar包，则需要先解包，解包命令为：tar -izxvf dbbackup20110809.tar，这里必须使用-i参数。然后停止mysql数据库并删除欲恢复的数据库文件夹
service mysql stop  
rm /data0/mysql/var -rf

然后将备份文件中的日志应用到备份文件中的数据文件上
innobackupex --user=root --password=MySQLPASSWORD --apply-log /mysqlbackup/full/2011-08-09_14-50-20/ 
这里的--apply-log指明是将日志应用到数据文件上，完成之后将备份文件中的数据恢复到数据库中

innobackupex --user=root --password=MySQLPASSWORD --copy-back /mysqlbackup/full/2011-08-09_14-50-20/
这里的—copy-back指明是进行数据恢复。数据恢复完成之后，需要修改相关文件的权限mysql数据库才能正常启动

chown mysql:mysql -R * /data0/mysql/var
service mysql start

注意：虽然指定的是test数据库，但实际上如果其他数据库中也有InnoDB表，那么这些InnoDB表中的数据也会被恢复了。
```

### 增量备份恢复

```
增量备份恢复的步骤和完整备份恢复的步骤基本一致，只是应用日志的过程稍有不同。增量备份恢复时，是先将所有的增量备份挨个应用到完整备份的数据文件中，然后再将完整备份中的数据恢复到数据库中。

应用第一个增量备份
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --apply-log /mysqlbackup/full/2011-08-09_14-50-20/ --incremental-dir=/mysqlbackup/trn/2011-08-09_15-12-43/  

应用第二个增量备份
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --apply-log /mysqlbackup/full/2011-08-09_14-50-20/ --incremental-dir=/mysqlbackup/trn/2011-08-05_15-15-47/  


将完整备份中的数据恢复到数据库中
innobackupex --user=root --password=MySQLPASSWORD --defaults-file=/etc/my.cnf --copy-back /mysqlbackup/full/2011-08-05_14-50-20/  
其中，--incremental-dir指定要恢复的增量备份的位置
```


### 相关原理

```
完整备份的原理：

对于InnoDB，XtraBackup基于InnoDB的crash-recovery功能进行备份。

crash-recovery是这样的：InnoDB维护了一个redo log，又称为 transaction log，也叫事务日志，它包含了InnoDB数据的所有改动情况。InnoDB启动的时候先去检查datafile和transaction log，然后应用所有已提交的事务并回滚所有未提交的事务。

XtraBackup在备份的时候并不锁定表，而是一页一页地复制InnoDB的数据，与此同时，XtraBackup还有另外一个线程监视着transactions log，一旦log发生变化，就把变化过的log pages复制走(因为transactions log文件大小有限，写满之后，就会从头再开始写，新数据可能会覆盖到旧的数据，所以一旦变化就要立刻复制走）。在全部数据文件复制完成之后，停止复制logfile。

XtraBackup采用了其内置的InnoDB库以read-write模式打开InnoDB的数据文件，然后每次读写1MB(1MB/16KB=64page)的数据，一页一页地遍历，同时用InnoDB的buf_page_is_corrupted()函数检查此页的数据是否正常，如果正常则进行复制，如不正常则重新读取，最多重读10次，如果还是失败，则备份失败退出。复制transactions log的原理也是一样的，只不过每次读写512KB(512KB/16KB=32page)的数据。

由于XtraBackup其内置的InnoDB库打开文件的时候是rw的，所以运行XtraBackup的用户，必须对InnoDB的数据文件具有读写权限。

由于XtraBackup要从文件系统中复制大量的数据，所以它尽可能地使用posix_fadvise()，来告诉OS不要缓存读取到的数据(因为这些数据不会重用到了)，从而提升性能。如果要缓存的话，大量的数据会对OS的虚拟内存造成很大的压力，其它进程（如mysqld）很有可能会被swap出去，这样就出问题了。同时，XtraBackup在读取数据的时候还尽可能地预读。

由于不锁表，所以复制出来的数据是不一致的，数据的一致性是在恢复的时候使用crash-recovery进行实现的。

对于MyISAM,XtraBackup还是首先锁定所有的表，然后复制所有文件。

增量备份的原理：

在完整备份和增量备份文件中都有一个文件xtrabackup_checkpoints会记录备份完成时检查点的LSN。在进行新的增量备份时，XtraBackup会比较表空间中每页的LSN是否大于上次备份完成的LSN，如果是，则备份该页，并记录当前检查点的LSN。
```

