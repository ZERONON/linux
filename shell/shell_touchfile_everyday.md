# shell习题170705-每日生成一个文件

请按照这样的日期格式（xxxx-xx-xx）每日生成一个文件，例如今天生成的文件为2017-07-05.log， 并且把磁盘的使用情况写到到这个文件中，（不用考虑cron，仅仅写脚本即可）

---

```bash
#! /bin/bash
d=`date +%F`
logfile=$d.log
df -h > $logfile
```



