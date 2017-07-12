# 安装pure-ftp

### 使用yum安装pure-ftp

```bash
yum install -y epel-release
yum install pure-ftpd
```

### 配置文件

配置文件路径：/etc/pure-ftpd/pure-ftpd.conf

```bash
ChrootEveryone              yes//锁定所有用户到家目录中
BrokenClientsCompatibility  no        
MaxClientsNumber            50//最大的客户端数量
Daemonize                   yes
MaxClientsPerIP             8//同一个IP允许8个链接
VerboseLog                  no
DisplayDotFiles             no//不显示隐藏文件
AnonymousOnly               no//只允许匿名用户
NoAnonymous                 yes//不允许匿名用户
SyslogFacility              ftp
DontResolve                 yes//禁止反向解析
MaxIdleTime                 15//最大空闲时间15分钟
PureDB                      /etc/pure-ftpd/pureftpd.pdb
LimitRecursion              3136 8//最多显示3136个文件，最深8层目录
AnonymousCanCreateDirs      no//是否允许匿名用户创建目录
MaxLoad                     4//最多可下载的数量
AntiWarez                   yes
Umask                       133:022//问价和目录的umask
MinUID                      100//用户ID至少要大于100才能登陆
AllowUserFXP                no//是否允许用户使用FXP协议
AllowAnonymousFXP           no//是否允许匿名用户使用FXP协议
ProhibitDotFilesWrite       no//是否允许使用点文件
ProhibitDotFilesRead        no//是否允许读取点文件
AutoRename                  no
AnonymousCantUpload         yes//不允许匿名用户上传
PIDFile                     /var/run/pure-ftpd.pid
MaxDiskUsage                99//磁盘最大利用率
CustomerProof               yes
ForcePassiveIP              47.52.xx.xx//强制绑定IP
PassivePortRange            60000 60100//被动模式端口设置
```

阿里云ECS网卡只绑定内网IP，所以需要在配置文件里强制绑定外网IP，`ForcePassiveIP`

### 新建用户

```bash
useradd -s /sbin/nologin ftpuser//创建普通用户
pure-pw useradd puser -u ftpuser -d /data/ftp -m
//pure-pw useradd 虚拟用户名 –u 系统用户名 –d FTP目录 –m（把用户密码加入pureftpd.pdb，不需要重启FTP）
pure-pw list//查看用户列表
pure-pw userdel [username]//删除用户
chown -R ftpuser:ftpuser /data/ftp
```

### 启动测试

```bash
/usr/sbin/pure-config.pl /etc/pure-ftpd/pure-ftpd.conf//或者service pure-ftpd start

//Running: /usr/local/pureftpd/sbin/pure-ftpd -A -c50 -B -C8 -E -fftp -H -I10 -lpuredb:/usr/local/pureftpd/etc/pureftpd.pdb -L2000:8 -s -U133:022 -u1000 -i -Z-4
注：如果出现running说明启动成功。
```

使用lftp测试

```bash
yum install -y lftp //安装lftp

[root@docsss ~]# lftp test@127.0.0.1
口令:
lftp test@127.0.0.1:~> ls
drwxr-xr-x    2 1001       ftpuser            4096 Jul 10 18:17 down
drwxr-xr-x    2 1001       ftpuser            4096 Jul 10 18:17 up
```

使用ftp测试

```bash
ftp> open 47.52.x.x
连接到 47.52.x.x。
220---------- Welcome to Pure-FTPd [privsep] [TLS] ----------
220-You are user number 1 of 50 allowed.
220-Local time is now 11:50. Server port: 21.
220-IPv6 connections are also welcome on this server.
220 You will be disconnected after 15 minutes of inactivity.
200 OK, UTF-8 enabled
用户(47.52.x.x:(none)): puser
331 User puser OK. Password required
密码:
230 OK. Current directory is /
ftp> ls
200 PORT command successful
150 Connecting to port 22937
down
up
226 2 matches total
ftp: 收到 13 字节，用时 0.00秒 3.25千字节/秒。
```



