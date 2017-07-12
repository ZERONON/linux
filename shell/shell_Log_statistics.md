# shell习题170706-统计日志

有日志1.log，内容如下：

日志片段：

```bash
112.111.12.248 - [25/Sep/2013:16:08:31 +0800]formula-x.haotui.com "/seccode.php?update=0.5593110133088248" 200"http://formula-x.haotui.com/registerbbs.php" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1;)"
61.147.76.51 - [25/Sep/2013:16:08:31 +0800]xyzdiy.5d6d.com "/attachment.php?aid=4554&k=9ce51e2c376bc861603c7689d97c04a1&t=1334564048&fid=9&sid=zgohwYoLZq2qPW233ZIRsJiUeu22XqE8f49jY9mouRSoE71" 301"http://xyzdiy.5d6d.com/thread-1435-1-23.html" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)"
```

要求： 统计出每个IP的访问量有多少？

---

```bash
awk '{print $1}' 1.log |sort -n|uniq -c |sort -n}' 1.log |sort -n|uniq -c |sort -n
```



