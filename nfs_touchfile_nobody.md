# NFS客户端创建文件属主显示nobody

相信使用CentOS6搭建NFS的朋友大多都遇到过如此问题，NFS服务搭建好后，在客户端挂载成功了，但是创建文件时，属主和属组却显示为nobody。  
这是NFS版本变化导致的，在centos 6版本中默认使用了nfs-v4版本，其提供了称为rpc.idmapd 的守护进程，并使用 /etc/idmapd.conf 的配置文件。当请求加载nfsv4 时，该守护进程将处理 UID 和 GID 映射。默认使用nis，没有nis它会自动映射成nobody用户。

解决此问题，并不难，下面提供两个办法：

1. 在客户端挂载NFS时，加一个参数"-o nfsvers=3"

2. 客户端和服务端分别打开/etc/idmapd.conf配置文件，把“\#Domain = local.domain.edu” 改为 “Domain = xxx.com” （这里的xxx.com,你随意定义吧），然后再重启rpcidmapd服务，service rpcidmapd restart



