---
layout: post
title:  "使用array处理xml获取city"
date:   2016-05-13
categories: php
---



```
**array使用$xml['countries']形式**
**object和class使用$iter->regions形式**
<?php
$xml = "regions.xml";
if(file_exists($xml)){
	$xml = simplexml_load_file($xml);
	// print_r($xml);
	$xml = (array)$xml;
	// print_r($xml);
	// print_r($xml['countries']);die();
	foreach ($xml['countries'] as $iter) {
		// print_r($iter->regions);
		foreach ($iter->regions as $city) {
			echo $city->en."\n";
			}
		}
}else {
	exit('failed to open $xml');
}
?>
```