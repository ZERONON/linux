# shell习题170711-批量更改文件名

需求：

1. 找到/123目录下所有后缀名为.txt的文件

2. 批量修改.txt为.txt.bak

3. 把所有.bak文件打包压缩为123.tar.gz

4. 批量还原文件的名字，即把增加的.bak再删除

---

```bash
#!/bin/bash
##查找txt文件
find /123 -type f -name "*.txt" > /tmp/txt.list

##批量修改文件名
for f in `cat /tmp/txt.list`
do
    mv $f $f.bak
done

##创建一个目录，为了避免目录已经存在，所以要加一个复杂的后缀名
d=`date +%y%m%d%H%M%S`
mkdir /tmp/123_$d

##把.bak文件拷贝到/tmp/123_$d
for f in `cat /tmp/txt.list`
do
    cp $f.bak /tmp/123_$d
done

##打包压缩
cd /tmp/
tar czf 123.tar.gz 123_$d/

##还原
for f in `cat /tmp/txt.list`
do
    mv $f.bak $f
done
```



