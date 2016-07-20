---
layout: post
title:  "MySQL 5.7 双主复制+keepalived"
date:   2016-07-20
categories: mysql
---


```
架构：
希望做双主复制+keepalived，架构大概如下图
主机A IP：192.168.1.2
主机B IP：192.168.1.3
VIP：192.168.1.4
```

# 一、首先安装MySQL 5.7

```
到下面的url下载你操作系统对应的yum包
http://dev.mysql.com/downloads/repo/yum/

运行下面两个命令安装
rpm -ivh mysql57-community-release-el6-8.noarch.rpm 
yum -y install mysql.x86_64 mysql-server.x86_64 mysql-devel.x86_64
```

# 二、做一些简单的优化工作
```
echo "noop" > /sys/block/nvme0n1/queue/scheduler
等等
```

# 三、配置/etc/my.cnf
核心工作也就是这个了，因为5.7的变化比较大，我简答列一些我的配置说明一下。

```
character_set_server=utf8
max_connections = 5000
max_connect_errors = 200000
transaction_isolation = READ-COMMITTED
explicit_defaults_for_timestamp = 1
tmp_table_size = 67108864
max_allowed_packet = 16777216
interactive_timeout = 57600  //老报超时，我就设置大了一点
wait_timeout = 57600  //老报超时，我就设置大了一点
read_buffer_size = 16777216
read_rnd_buffer_size = 33554432
sort_buffer_size = 134217728
join_buffer_size = 134217728
query_cache_size = 0
query_cache_type = 0

server-id = 1  //另外一台设置为2
auto_increment_increment=2
auto_increment_offset=1 //另外一台设置为2

binlog-do-db = nzabbix
binlog-do-db = szabbix
binlog-ignore-db = monitor_db
replicate-do-db = nzabbix //需要同步的数据
replicate-do-db = szabbix //需要同步的数据
replicate-ignore-db = mysql
replicate-ignore-db = sys
replicate-ignore-db = monitor_db  //用来看Mysql用的一个库
slave-skip-errors=all  //害怕遇到错误停止同步，忽略所有错误
sync_binlog = 0 //zabbix数据不是很重要，容许crash丢数据

master_info_repository = TABLE
relay_log_info_repository = TABLE

gtid_mode = on  //开启gtid，这是一个新特性，我们的复制是基于GTID的，所以打开
enforce_gtid_consistency = 1
binlog_gtid_simple_recovery = 1
log-slave-updates

# slave
slave-parallel-type = LOGICAL_CLOCK
slave-parallel-workers = 0  //日志老报一个错误，先关闭并行了
#slave-parallel-workers = 16

########innodb settings########
innodb_buffer_pool_size = 100G //内存的75%比较合适
innodb_buffer_pool_instances = 16
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_lru_scan_depth = 2000
innodb_lock_wait_timeout = 10000
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_flush_method = O_DIRECT
innodb_flush_neighbors = 1
innodb_log_file_size = 4G
innodb_log_buffer_size = 16777216
innodb_purge_threads = 4
innodb_large_prefix = 1
innodb_thread_concurrency = 64
innodb_print_all_deadlocks = 1
innodb_strict_mode = 1
innodb_sort_buffer_size = 67108864 
innodb_flush_log_at_trx_commit = 2
innodb_read_io_threads = 16
innodb_write_io_threads = 16 

[mysqld-5.7]
innodb_buffer_pool_dump_pct = 40
innodb_page_cleaners = 4
innodb_undo_log_truncate = 1
innodb_max_undo_log_size = 2G
innodb_purge_rseg_truncate_frequency = 128
binlog_gtid_simple_recovery=1
log_timestamps=system
transaction_write_set_extraction=MURMUR32
show_compatibility_56=on

比较关键的是GTID：

Gtid是从5.6开始推出的杀手级特性，通过GTID特性，极大的提升了主备切换的效率和一致性。

在MySQL5.7.5里引入了一个新的系统表GTID_EXECUTED：
表的描述参考官方文档：
http://dev.mysql.com/doc/refman/5.7/en/replication-gtids-concepts.html#replication-gtids-gtid-executed-table
对应worklog: http://dev.mysql.com/worklog/task/?id=6559
```

# 四、初始化目录

```
5.7已经不用mysql_install_db初始化了，已经改成下面这个命令了
mysqld --initialize --datadir=/data0/mysql/
chmod -R 755 /data0/mysql/
chown -R mysql.mysql /data0/mysql/
```

# 五、设置密码

```
5.7比较讨厌，初始密码不为空了，也可以说安全性更高了。
初始化密码在初始化日志里
grep "temporary password" error.log

如果你登录mysql想修改密码可能会报错
errROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

为了以后不麻烦，顺手降低密码策略。
mysql > SET GLOBAL  validate_password_policy='LOW'; 
mysql > alter user 'root'@'localhost' identified by 'your-password';
mysql > flush privileges; 

如果mysql和mysqladmin密码行带密码可能还会出现一个Warning
mysql -u root -p password 'your-password' 
mysql: [Warning] Using a password on the command line interface can be insecure.

解决方案有多种，可以参考：
http://www.quwenqing.com/read-247.html
其中的一种解决方案最靠谱
export MYSQL_PWD=password

但是这个错误提示非常讨厌，命令行没有参数可以关闭，我又不想在my.cnf里写密码。你会发现zabbix监控mysql的数据出来都是这个提示，我们还需要把mysql监控的shell里的命令行带密码的改成export这种形式。
```

# 六、设置数据同步

```
A和B上同时添加同步用户
use mysql;
create user 'repl'@'192.168.1.%' identified by 'your-password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.1.%'; 
flush privileges;

锁一下表，追平两台机器数据，然后解锁。

先看看GTID是否打开
mysql> show global variables like '%gtid%';
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| enforce_gtid_consistency | ON    |
| gtid_executed            |       |
| gtid_mode                | ON    |
| gtid_owned               |       |
| gtid_purged              |       |
+--------------------------+-------+
5 rows in set (0.10 sec)

说明gtid功能已启动。
GTID同步数据不用再记录对方的log文件和位置了，用master_auto_position=1就行，不过你用老的方法查看master的logfile和logpos，同步也是可以的。

在A和B上都运行
mysql> change master to master_host='192.168.1.2', master_user='repl',master_password='your-password',master_auto_position=1;
Query OK, 0 rows affected, 2 warnings (0.24 sec)
mysql> start slave;
Query OK, 0 rows affected, 1 warning (0.04 sec)
mysql> start slave;

在A和B上查看同步状态
show slave status\G
#表示同步的文件和位置
Master_Log_File: mysql-bin.000035
Read_Master_Log_Pos: 895597105
#显示下面表示工作正常
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
#表示当前同步的数据库
Replicate_Do_DB: nzabbix,szabbix

双主复制比较简单，总结一下就是：
安装MySQL->优化系统->优化配置my.cnf->初始化mysqld->添加同步用户->开始同步->查看同步状态。
```

# keepalived的安装和设置

```
keepalived安装比较简单
yum install keepalived

设置日志
cat /etc/sysconfig/keepalived
KEEPALIVED_OPTIONS="-D -d -S 0"

在rsyslog.conf最后添加
local0.*                                                /var/log/keepalived.log

重启rsyslog和keepalived

keepliaved的配置也很简单
global_defs {
   notification_email {
         dz@dz.com
   }
   router_id MySQL-ha85
}
 
vrrp_script check_run {
   script "/data0/keepalived_check_mysql.sh"     #监控MySQL的脚本
   interval 3
}
 
vrrp_sync_group VG1 {
    group {
          VI_1
    }
}
vrrp_instance VI_1 {
    state BACKUP 
    interface eth0                  #指定虚拟IP的网络接口
    virtual_router_id 85   #VRRP组名，两个节点的设置必须一样，以指明各个节点属于同一VRRP组
    priority 100                 #主节点的优先级（1-254之间），备用节点必须比主节点优先级低。
    advert_int 1                 #组播信息发送间隔，两个节点设置必须一样
   nopreempt  
    authentication {             #设置验证信息，两个节点必须一致
        auth_type PASS
        auth_pass 1111
    }
    track_script {
        check_run
    }
    virtual_ipaddress {
        192.168.1.4       #虚拟IP,对外提供MySQL服务的IP地址
    }
}

另外一台优先级设置底一点就可以，也可以设置为MASTER-BACKUP，看你自己的切换需求了。

/data0/keepalived_check_mysql.sh的作用是检查当前Mysql是否正常工作
我在里面做了一个写操作，来判断MySQL是否正常
 $MYSQL -h $MYSQL_HOST -u $MYSQL_USER -e "use monitor_db;UPDATE monitor_db SET CreateDate=NOW() WHERE ID=1;" >/dev/null 2>&1

我还写了一个程序放到cron里，每分钟运行一次，为啥要这么干？主要我一重启MySQL，就会发生自动杀掉keepalived，并且IP飘走，这时候如果MySQL重启好了，keepalived不能自己启动。程序如下：

if [ ! -z "$(/bin/ps aux|grep -v grep|grep 'mysqld --basedir')" ];then
        echo "mysqld is running"
        if [ -z "$(/bin/ps aux|grep -v grep|grep '/usr/sbin/keepalived ')" ];then
                echo "keepalived is not running"
                service keepalived restart
        fi
fi

就是判断如果MySQL还活着，就启动keepalived，不过这块代码有点简陋，理论上应该判断MySQL是否还活着，并且可以写数据，再启动keepalived。

这套MySQL双主复制+keepalived 架构其实可以适用于各种业务，大家可以灵活使用，扩展也很方便。
```