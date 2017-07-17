# shell习题170714-自动重启nginx服务

服务器上跑的是LNMP环境，近期总是有502现象。502为网站访问的状态码，200正常，502错误是nginx最为普通的错误状态码。由于502只是暂时的，并且只要一重启php-fpm服务则502消失，但不重启的话，则会一直持续很长时间。所以有必要写一个监控脚本，监控访问日志的状态码，一旦发生502，则自动重启一下php-fpm。

我们设定： 

1. access\_log  /data/log/access.log

2. 脚本死循环，每10s检测一次（假设每10s钟的日志条数为300左右）

3. 重启php-fpm的方法是  /etc/init.d/php-fpm restart

---

```bash
#! /bin/bash
log=/data/log/access.log
N=10
while :; do
    ##因为10秒钟大概产生300条日志
    tail -n 300 $log &gt; /tmp/log
    n_502=`grep -c ' 502"' /tmp/log`
    if [ $n_502 -ge $N ]; then
        ##记录系统的状态
        top -bn1 >/tmp/`date +%H%M%S`-top.log
        vmstat 1 5 >/tmp/`date +%H%M%S`-vm.log
        /etc/init.d/php-fpm restart 2>/dev/null
        ##重启php-fpm服务后，应先暂缓1分钟，而后继续每隔10s检测一次
        sleep 60
    fi    
    sleep 10
done
```



