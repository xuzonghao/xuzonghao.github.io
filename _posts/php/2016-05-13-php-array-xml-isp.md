---
layout: post
title:  "使用array处理xml获取isp"
date:   2016-05-13
categories: php
---



### 处理xml格式的中文

```xml
<?xml version="1.0" encoding="utf-8"?>
<isp>
	<entry>
		<id>1</id>
		<name>网通</name>
	</entry>
	<entry>
		<id>2</id>
		<name>中华电信</name>
	</entry>
	<entry>
		<id>3</id>
		<name>电信</name>
	</entry>
	<entry>
		<id>4</id>
		<name>移动</name>
	</entry>
	<entry>
		<id>5</id>
		<name>联通</name>
	</entry>
	<entry>
		<id>6</id>
		<name>吉通</name>
	</entry>
	<entry>
		<id>7</id>
		<name>铁通</name>
	</entry>
	<entry>
		<id>8</id>
		<name>广电</name>
	</entry>
	<entry>
		<id>9</id>
		<name>卫通</name>
	</entry>
	<entry>
		<id>10</id>
		<name>歌华</name>
	</entry>
	<entry>
		<id>11</id>
		<name>有线通</name>
	</entry>
	<entry>
		<id>12</id>
		<name>北电中国</name>
	</entry>
	<entry>
		<id>13</id>
		<name>珠江宽频</name>
	</entry>
	<entry>
		<id>14</id>
		<name>长城宽带</name>
	</entry>
	<entry>
		<id>15</id>
		<name>中海宽带</name>
	</entry>
	<entry>
		<id>16</id>
		<name>E家宽</name>
	</entry>
	<entry>
		<id>17</id>
		<name>石化宽带</name>
	</entry>
	<entry>
		<id>18</id>
		<name>油田宽带</name>
	</entry>
	<entry>
		<id>19</id>
		<name>视讯宽带</name>
	</entry>
	<entry>
		<id>20</id>
		<name>视通宽带</name>
	</entry>
	<entry>
		<id>21</id>
		<name>盈联宽带</name>
	</entry>
	<entry>
		<id>22</id>
		<name>盈通宽带</name>
	</entry>
	<entry>
		<id>23</id>
		<name>黔南宽带</name>
	</entry>
	<entry>
		<id>24</id>
		<name>长鸿宽带</name>
	</entry>
	<entry>
		<id>25</id>
		<name>赛尔宽带</name>
	</entry>
	<entry>
		<id>26</id>
		<name>邦联宽带</name>
	</entry>
	<entry>
		<id>27</id>
		<name>鹤山有线</name>
	</entry>
	<entry>
		<id>28</id>
		<name>聚友宽带</name>
	</entry>
	<entry>
		<id>29</id>
		<name>艾普宽带</name>
	</entry>
	<entry>
		<id>30</id>
		<name>航数宽网</name>
	</entry>
	<entry>
		<id>31</id>
		<name>西部宽影</name>
	</entry>
	<entry>
		<id>32</id>
		<name>蓝波宽带</name>
	</entry>
	<entry>
		<id>33</id>
		<name>无线宽频</name>
	</entry>
	<entry>
		<id>34</id>
		<name>方正宽带</name>
	</entry>
	<entry>
		<id>35</id>
		<name>星和宽带</name>
	</entry>
	<entry>
		<id>36</id>
		<name>家庭宽带</name>
	</entry>
	<entry>
		<id>37</id>
		<name>天柏宽网</name>
	</entry>
	<entry>
		<id>38</id>
		<name>天大宽带</name>
	</entry>
	<entry>
		<id>39</id>
		<name>华宇宽带</name>
	</entry>
	<entry>
		<id>40</id>
		<name>华数宽带</name>
	</entry>
	<entry>
		<id>41</id>
		<name>华通宽带</name>
	</entry>
	<entry>
		<id>42</id>
		<name>利达宽带</name>
	</entry>
	<entry>
		<id>43</id>
		<name>中海宽带</name>
	</entry>
	<entry>
		<id>44</id>
		<name>东风宽带</name>
	</entry>
	<entry>
		<id>45</id>
		<name>长庆通信</name>
	</entry>
	<entry>
		<id>46</id>
		<name>天威有线</name>
	</entry>
	<entry>
		<id>47</id>
		<name>新视宽频</name>
	</entry>
	<entry>
		<id>48</id>
		<name>亚太电信</name>
	</entry>
	<entry>
		<id>49</id>
		<name>远传电信</name>
	</entry>
	<entry>
		<id>50</id>
		<name>亚太宽频</name>
	</entry>
	<entry>
		<id>51</id>
		<name>东森宽频</name>
	</entry>
	<entry>
		<id>52</id>
		<name>Cyber</name>
	</entry>
	<entry>
		<id>53</id>
		<name>澳门电讯</name>
	</entry>
	<entry>
		<id>54</id>
		<name>有线宽带</name>
	</entry>
	<entry>
		<id>55</id>
		<name>香港宽频</name>
	</entry>
	<entry>
		<id>56</id>
		<name>城市电讯</name>
	</entry>
	<entry>
		<id>57</id>
		<name>新世界电讯</name>
	</entry>
	<entry>
		<id>58</id>
		<name>长丰宽带</name>
	</entry>
	<entry>
		<id>59</id>
		<name>宽带通</name>
	</entry>
	<entry>
		<id>60</id>
		<name>教育网</name>
	</entry>
	<entry>
		<id>61</id>
		<name>移通</name>
	</entry>
	<entry>
		<id>62</id>
		<name>东方在线</name>
	</entry>
	<entry>
		<id>63</id>
		<name>普天</name>
	</entry>
	<entry>
		<id>64</id>
		<name>世纪互联</name>
	</entry>
	<entry>
		<id>65</id>
		<name>网宿</name>
	</entry>
	<entry>
		<id>66</id>
		<name>电信通</name>
	</entry>
</isp>

```

### 方法一

```
<?php

//header("Content-Type: text/html;charset=utf-8");

error_reporting(0);
$filename = "isp.xml";
$handle = fopen($filename, "r");
$contents = fread($handle, filesize ($filename)); 
// $string = '<name>网通</name>';
$pattern = array("/[a-zA-Z0-9]+/","/[[:punct:]]/");
$replacement = "";
echo preg_replace($pattern, $replacement, $contents);
?>
```

### 方法二

```
<?php
$xml = "isp.xml";
if(file_exists($xml)){
	$xml = simplexml_load_file($xml);
	// print_r($xml);
	$xml = (array)$xml;
	// print_r($xml);
	foreach ($xml['entry'] as $iter) {
		echo $iter->name."\n";
	}
}else {
	exit('failed to open $xml');
}
?>
```


### result

```
网通
中华电信
电信
移动
联通
吉通
铁通
广电
卫通
歌华
有线通
北电中国
珠江宽频
长城宽带
中海宽带
E家宽
石化宽带
油田宽带
视讯宽带
视通宽带
盈联宽带
盈通宽带
黔南宽带
长鸿宽带
赛尔宽带
邦联宽带
鹤山有线
聚友宽带
艾普宽带
航数宽网
西部宽影
蓝波宽带
无线宽频
方正宽带
星和宽带
家庭宽带
天柏宽网
天大宽带
华宇宽带
华数宽带
华通宽带
利达宽带
中海宽带
东风宽带
长庆通信
天威有线
新视宽频
亚太电信
远传电信
亚太宽频
东森宽频
Cyber
澳门电讯
有线宽带
香港宽频
城市电讯
新世界电讯
长丰宽带
宽带通
教育网
移通
东方在线
普天
世纪互联
网宿
电信通
```