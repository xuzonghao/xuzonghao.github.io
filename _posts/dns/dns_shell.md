---
layout: post
title:  "dns shell"
date:   2016-09-23
categories: Dns
---


###　测试dig脚本

```
#!/bin/sh
function do_help()
{
        echo "cd $PWD && $0 dns_server_ip domain time tries"
        exit -1
}
name=$2
dns_srv_ip=$1
time_out=$3
tries=$4

if [ -z $name ];then
        name="www.weibo.com"
fi

if [ -z $time_out ];then
        time_out=2
fi

if [ -z $tries ];then
        tries=5
fi

if [ -z $dns_srv_ip ];then
        echo "You Must Input dns server ip."
        do_help
fi

/usr/bin/dig @$dns_srv_ip $name +time=$time_out +tries=$tries
if [ $? -ne 0 ];then
        exit -1
fi

exit 0
```


### PTR 反解预警脚本

```
#!/bin/bash

#use notice
#View log in `echo "$PWD"`/localDNS.csv,More than 1 is successful localDNS.csv,0 is failure!
#Add localdns in localDNS file!
#Add telphone in sms.sh!

while [ 1 ]; do

date=`/bin/date +%Y%m%d\/%H:%M:%S`
localDNS="`echo "$PWD"`/localDNS"

while read line
do
	IDC=`echo "$line"|awk -F"," '{print $1}'`
	localIP=`echo "$line"|awk -F"," '{print $2}'`
	PTR=`dig -x 14.17.44.35 @"$localIP" +noquestion +noauthority +noadditional +nostats +nocomments +time=2 |grep "PTR"`
	PTR_num=`dig -x 14.17.44.35 @"$localIP" +noquestion +noauthority +noadditional +nostats +nocomments |grep "PTR" |wc -l`
		if [[ "$PTR_num" > "0" ]]; then
			echo "$PTR"
			echo -e "$date"",""$line"",""$PTR"",""$PTR_num" >> localDNS.csv
		else
			echo -e "$date"",""$line"",""$PTR"",""$PTR_num" >> localDNS.csv
#mail
/usr/sbin/sendmail -t <<EOF
From:dnswarring@staff.com.cn
To:zonghao1@staff.com.cn
Cc:yong@staff..com.cn,jia@staff.com.cn
Subject:"$IDC" "$localIP" PTR failure
EOF
#SMS
./sms.sh "$line,PTR,Failure"
		fi
done < "$localDNS"

sleep 300s
done


#by zonghao1
```