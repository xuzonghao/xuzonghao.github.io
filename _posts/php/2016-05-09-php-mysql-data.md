---
layout: post
title:  "使用php-mysql获取mysql数据库数据"
date:   2016-05-09
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
或


```
**逐行获取结果集中的记录**
使用mysql_result()效率低
array mysql_fetch_row(resource $resule)

<?php
header("Content-Type: text/html;charset=utf-8");
@mysql_connect("localhost", "root", "123qwe")
or die("database connect fail!");
@mysql_select_db("mydb")			//选择数据库
or die("database not exist!");
mysql_query("set names utf8");			//不设置会造成显示乱码，可通过mysql的status命令查看编码格式
$myquery = @mysql_query("select * from userinfo")		//执行sql语句
or die("SQL exec not success！");

echo "<table border=\"1\"><tr><th>id</th><th>姓名</th><th>性别</th><th>地址</th><th>邮件</th></tr>";

while ($row = mysql_fetch_row($myquery)) {
	echo "<tr><td>" . $row[0] . "</td>";
	echo "<td>" . $row[1] . "</td>";
	echo "<td>" . $row[2] . "</td>";
	echo "<td>" . $row[3] . "</td>";
	echo "<td>" . $row[4] . "</td></tr>";
}
echo "</table>";
mysql_close();
```



### 分页显示结果集中的数据

```
<?php
header("Content-Type: text/html;charset=utf-8");
@mysql_connect("localhost", "root", "123qwe")
or die("database connect fail!");
@mysql_select_db("mydb")			//选择数据库
or die("database not exist!");
mysql_query("set names utf8");			//不设置会造成显示乱码，可通过mysql的status命令查看编码格式
$myquery = @mysql_query("select * from userinfo")		//执行sql语句
or die("SQL exec not success！");
$page_size = 2;
$num_cnt = mysql_num_rows($myquery);
$page_cnt = ceil($num_cnt / $page_size);
if(isset($_GET['p'])){
	$page = $_GET['p'];
}else{
	$page = 1;
}
$query_start = ($page - 1) * $page_size;
$querysql = "select * from userinfo limit $query_start, $page_size";
$queryset = mysql_query($querysql);

echo "<table border=\"1\"><tr><th>id</th><th>姓名</th><th>性别</th><th>地址</th><th>邮件</th></tr>";

while ($row = mysql_fetch_array($queryset, MYSQL_BOTH)) {
	echo "<tr><td>" . $row[0] . "</td>";
	echo "<td>" . $row[1] . "</td>";
	echo "<td>" . $row["sex"] . "</td>";
	echo "<td>" . $row[3] . "</td>";
	echo "<td>" . $row["email"] . "</td></tr>";
}
echo "</table><br>";

$pager = "共 $page_cnt 页，跳转至第 ";
if($page_cnt > 1){
	for($i=1; $i <= $page_cnt; $i++){
		if($page == $i){
			$pager .= "<a href='?p=$i ' ><b>$i</b></a>";
		}else{
			$pager .= "<a href='?p=$i ' >$i</a>";
		}
	}
	echo $pager . " 页";
}
mysql_close();
```



### 显示如下：
![php-mysql显示数据库数据](https://github.com/xuzonghao/xuzonghao.github.io/blob/master/_posts/php/png/php-mysql-data.jpg?raw=true "php-mysql显示数据库数据")

#### by zonghao!