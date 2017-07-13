# mysql远程备份、压缩、解压、导入一条命令搞定



需求： 需要把远程的mysql服务器A上的数据，导出再导入到另一台服务器B。  
其中B机器没有外网，所以只能通过中间一台有外网的机器C来操作。  
其中C机器和B机器内网是通的。

![](https://mmbiz.qpic.cn/mmbiz_jpg/ZTYjw6KickuDNPx3eWZ2DQr458F1lSP2ShVmOxgKb4RGVtZgiaZbGc6b6c3WdFYJIbMBE5EIL6Wqt5mn7topug6g/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)



思路就是： 先登录C机器，然后在C机器上执行命令：

```
ssh  user@A   "mysqldump -uroot --single-transaction  --master-data=2 -q --flush-logs --databases databasename |bzip2 -" |bzip2 -d |tee /data/1.sql |mysql -hB  -Pport -uusername -ppasswd
```

说明：

1. 命令中的A，B都为IP地址。

2. InnoDB 表在备份时，通常启用选项 --single-transaction 来保证备份的一致性，实际上它的工作原理是设定本次会话的隔离级别为：REPEATABLE READ，以确保本次会话\(dump\)时，不会看到其他会话已经提交了的数据。

3. master-data,使用此选项，可用于设立另一台服务器作为master的slave。它会使dump输出包含CHANGE MASTER TO语句，标记dump源的二进制日志坐标（文件名和位置）。在把load dump文件加载到slave之后，slave应当从该master坐标开始复制。如果选项赋值为2，那么CHANGE MASTER TO 语句会被写成一个SQL comment（注释），从而只提供信息，如果选项赋值为1，那么CHANGE MASTER TO语句不会被写成注释并且在dump被载入时生效。如果没有指定，默认值为1。

