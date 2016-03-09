### 拉取股票流入流出资金


<?php
header("Content-Type: text/html;charset=utf-8");
$scrFile="gupiao.csv";
$tagFile = dirname(realpath($file)) . DIRECTORY_SEPARATOR . php . DIRECTORY_SEPARATOR . $scrFile; 
$url="http://stockpage.10jqka.com.cn/spService/002424/Funds/realFunds";
if(($fh = fopen($tagFile,'wb')) === false){
  die($tagFile . "不可写 \n");
}else{
  echo $tagFile . "写入成功\n";
}


if(fwrite($fh, $string) === false){   //将$string写入资源$fh，并根据返回值给出不同提示信息，写入成功返回字符串字节数，写入失败返回false
  echo "写入文件：$tagFile ok \n";
  exit;
}else{
  echo "写入文件：$tagFile  成功. \n";
    }   



```
**jialei tigong**
curl --compressed http://stockpage.10jqka.com.cn/spService/002424/Funds/realFunds
来获取
"title":{
	"zlc":4148.77,
	"zlr":4279.77,
	"je":"97.78"
}
```



/*
if(($fn = fopen($url,'rb')) === false){
  die($url . "读取失败\n");
}
if($fh){
	 echo "从文件读取的内容为：\n";
	 while(!feof($fh)){
	 	$buffer = fgets($fh,50);
	 	echo $buffer;
} 
fclose($fh);
}