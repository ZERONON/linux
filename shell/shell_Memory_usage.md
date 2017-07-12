# shell习题170707-统计内存使用

需求：

写一个脚本计算一下linux系统所有进程占用内存大小的和。（提示，使用ps或者top命令）

---

```bash
#! /bin/bash
sum=0
for mem in `ps aux |awk '{print $6}' |grep -v 'RSS' `
do
    sum=$[$sum+$mem]
done
echo "The total memory is $sum""k"
```



