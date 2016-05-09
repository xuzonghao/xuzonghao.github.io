---
layout: post
title:  "使用php-mysql获取mysql数据库数据并展示"
date:   2016-05-10
categories: php
---



```
**mysql_query()显示返回所有结果集中所有信息**

<?php
header("Content-Type: text/html;charset=utf-8");
@mysql_connect("localhost", "root", "123qwe")
or die("database connect fail!");
@mysql_select_db("mydb")
or die("database not exist!");
mysql_query("set names utf8");			//不设置会造成显示乱码，可通过mysql的status命令查看编码格式
$myquery = @mysql_query("select * from userinfo")
or die("SQL exec not success！");
$rowscnt = mysql_num_rows($myquery);

echo "<table border=\"1\"><tr><th>id</th><th>姓名</th><th>性别</th><th>地址</th><th>邮件</th></tr>";

for($i=0; $i < $rowscnt; $i++){
	echo "<tr><td>" . mysql_result($myquery, $i, 0) . "</td>";
	echo "<td>" . mysql_result($myquery, $i, 1) . "</td>";
	echo "<td>" . mysql_result($myquery, $i, 2) . "</td>";
	echo "<td>" . mysql_result($myquery, $i, 3) . "</td>";
	echo "<td>" . mysql_result($myquery, $i, 4) . "</td></tr>";
}
echo "</table>";
mysql_close();
```

### 显示如下：
![php-mysql显示数据库数据](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/php/png/php-mysql-data.jpg?raw=true "php-mysql显示数据库数据")

#### by zonghao!