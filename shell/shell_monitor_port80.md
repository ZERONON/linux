# shell习题170712-监控80端口

需求：

写一个脚本，判断本机的80端口是否开启着，如果开启着什么都不做，如果发现端口不存在，那么重启一下httpd服务，并发邮件通知你自己。脚本写好后，可以每一分钟执行一次，也可以写一个死循环的脚本，30s检测一次。

---

```bash
#! /bin/bash
mail=123@123.com
if netstat -lnp |grep ':80' |grep -q 'LISTEN'; then
    exit
else
    /usr/local/apache2/bin/apachectl restart >/dev/null 2> /dev/null
    python mail.py $mail "check_80" "The 80 port is down."
    n=`ps aux |grep httpd|grep -cv grep`
    if [ $n -eq 0 ]; then
        /usr/local/apache2/bin/apachectl start 2>/tmp/apache_start.err
    fi
    if [ -s /tmp/apache_start.err ]; then
        python mail.py  $mail  'apache_start_error'   `cat /tmp/apache_start.err`
    fi
fi
```



