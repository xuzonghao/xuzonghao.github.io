---
layout: post
title:  "mysql OneProxy轻量级读写分离"
date:   2016-07-20
categories: mysql
---


# 读写分离架构

![a](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/mysql/png/oneproxy_read_write_split_arch.jpg?raw=true "a")


# 配置文件

```
export ONEPROXY_HOME=/data/oneproxy

# valgrind --leak-check=full --show-reachable=yes \
${ONEPROXY_HOME}/oneproxy --keepalive --proxy-address=:3307 \
  --proxy-master-addresses=172.30.12.4:3306@users \
  --proxy-slave-addresses=172.30.12.5:3306@users \
  --proxy-slave-addresses=172.30.12.6:3306@users \
  --proxy-user-list=test/1378F6CC3A8E8A43CA388193FBED5405982FBBD3@test \
  --proxy-group-policy=users:read_slave \
  --log-file=${ONEPROXY_HOME}/oneproxy.log \
  --pid-file=${ONEPROXY_HOME}/oneproxy.pid

  重新启动OneProxy中间件，并且执行一些查询命令，也可以随便关闭主库或从库来进行高可用的测试。对于流量分配策略（“proxy-group-policy”），还有几个不同的策略可以选择， 主要区别在于主库是否参与读流量，以及是否将所有的查询语句都发送到备库。下面是对这些策略的简单介绍：

“read_slave”，将所有只读流量路由到备库进行服务，使用round-lobbin的策略，主库将不承担读流量，除非没有备库可以提供服务。
“read_balance”，将只读流量平均地路由到所有的节点，包括主库，特别为一主一备的情况设计。
“big_slave”，同“read_slave”，区别是只路由复杂的SQL到备库。
“big_balance”，同“read_balance”，区别是只路由复杂的SQL到备库。
只读流量指的是在事务外（后端开启自动提交情况下，不在显式事务内）的所有“SELECT”、“SHOW”等关键字开头的命令，所有在显式事务（比如使用“start transaction”命令启动的事务）内的语句都被认为是写操作。如果要强制一个查询语句走主库，可以在SQL语句中增加“/* master */”注释来强制走主库。

select /* master */ * from ...;
同样在没有启用读写分离的策略（默认值为“master_only”）时，也可以使用“/* slave */”注释来告知某个SQL可以走从库查询。也可以使和“proxy-force-master”选项来强制涉及某个表的所有SQL都走主库，如下所示：

export ONEPROXY_HOME=/data/oneproxy

# valgrind --leak-check=full --show-reachable=yes \
${ONEPROXY_HOME}/oneproxy --keepalive --proxy-address=:3307 \
  --proxy-master-addresses=172.30.12.4:3306@users \
  --proxy-slave-addresses=172.30.12.5:3306@users \
  --proxy-slave-addresses=172.30.12.6:3306@users \
  --proxy-user-list=test/1378F6CC3A8E8A43CA388193FBED5405982FBBD3@test \
  --proxy-force-master=<table name> \
  --proxy-group-policy=users:read_failover \
  --log-file=${ONEPROXY_HOME}/oneproxy.log \
  --pid-file=${ONEPROXY_HOME}/oneproxy.pid
在OneProxy中哪些SQL语句是复杂查询，受“big_slave”选项的路由控制呢？下列情况都会被OneProxy自动识别为复杂查询，无需应用程序指定。

没有Where条件的SQL语句。
有排序“order by”或“group by”子句的SQL语句。
有“distinct”关键字的语句，比如“select distinct …”。
有汇总函数，如“avg”或“sum”等汇总函数。
有子查询或者多个表关联的语句。
OneProxy可以实时检测备库和主库之间的复制时延，并可以根据设定的最大主从时延来自动踢出延时太久的节点，以免应用程序读到太旧的数据。启用此功能时，只要你停掉备库的复制，就可以安心地去做备份了，不会有生产流量进来打扰备份工作，也没需要采取复杂的措施来隔离生产流量；在备份结束后，只需要再次开启复制，就可以了，只要追上主库，OneProxy会自动分配流量。在OneProxy也可以按比例调节后端流量分配。
```

