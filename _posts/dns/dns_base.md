---
layout: post
title:  "Dns基础"
date:   2016-09-18
categories: Dns
---


```
CNAME记录不能和任何记录共存
acl.cernet（教育网）acl.cnc（联通）acl.tel（电信）acl.trusted（其他）

/usr/local/bind9/bin/named_reload.sh
/usr/local/bind9/sbin/rndc -s server_123_5 reload
/usr/local/bind9/sbin/rndc -s server_172_5 reload
/usr/local/bind9/sbin/rndc -s server_123_18 reload
/usr/local/bind9/sbin/rndc -s server_172_18 reload

/usr/local/bind9/sbin/rndc flush (全部清空缓存)
/usr/local/bind9/sbin/rndc flushname p.tanx.com  (清空域名缓存)

1GB带宽=100万PPS
```

```
基础命令
/usr/bin/dig @10.210.12.10 api.weibo.cn +time=2 +tries=5
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



```
PTR sarch
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