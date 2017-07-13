# shell习题170710-设计监控脚本

设计一个脚本，监控远程的一台机器\(假设ip为123.23.11.21\)的存活状态，当发现宕机时发一封邮件给你自己。

提示：

1. 你可以使用ping命令   ping -c10 123.23.11.21

2. 发邮件脚本可以参考 [https://coding.net/u/aminglinux/p/aminglinux-book/git/blob/master/D22Z/mail.py](https://coding.net/u/aminglinux/p/aminglinux-book/git/blob/master/D22Z/mail.py)

3. 脚本可以搞成死循环，每隔30s检测一次

---

```bash
#!/bin/bash
ip=123.23.11.21
ma=abc@139.com

while 1
do
        ping -c10 $ip >/dev/null 2>/dev/null
        if [ $? != "0" ]
        then
                python /usr/local/sbin/mail.py $ma "$ip down" "$ip is down,plese check."
                #假设mail.py已经编写并设置好了
        fi
        sleep 30
done
```



