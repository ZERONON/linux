# shell习题170713-备份数据库

需求：

设计一个shell脚本来备份数据库，首先在本地服务器上保存一份数据，然后再远程拷贝一份，本地保存一周的数据，远程保存一个月。

假定，我们知道mysql root账号的密码，要备份的库为discuz，本地备份目录为/bak/mysql, 远程服务器ip为192.168.123.30，远程提供了一个rsync服务，备份的地址是 192.168.123.30::backup  . 写完脚本后，需要加入到cron中，每天凌晨3点执行。

---

```bash
#! /bin/bash
### backup mysql data
### Writen by Aming.
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/mysql/bin
d1=`data +%w`
d2=`date +%d`
pass="your_mysql_password"
bakdir=/bak/mysql
r_bakdir=192.168.123.30::backup

exec 1>/var/log/mysqlbak.log 2>/var/log/mysqlbak.log

echo "mysql backup begin at `date +"%F %T"`."
mysqldump -uroot -p$pass --default-character-set=gbk discuz >$bakdir/$d1.sql
rsync -az $bakdir/$d1.sql $r_bakdir/$d2.sql
echo "mysql backup end at `date +"%F %T"`."
```

然后加入cron

```bash
0 3 * * * /bin/bash /usr/local/sbin/mysqlbak.sh
```



