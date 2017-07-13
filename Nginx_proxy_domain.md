# nginx代理一堆域名



需求背景：  内网有一台机器不能连外网，所以没有办法使用yum，考虑过使用iptables nat 转发上网，但因为一些原因，放弃使用。所以想到nginx代理，原理很简单。   
A 不能访问 1网站， B可以访问，A和B可以内网通信，所以可以让B作为A的代理。 并且可以限定访问的来源IP，配置文件如下：

```bash
server {
    listen       80;
    server_name aaa.com bbb.com  ccc.com ddd.com eee.com;

    location / {
        resolver 119.29.29.29;
        proxy_pass       http://$host;
        proxy_set_header Host      $host;
        proxy_set_header X-Real-IP $remote_addr;
        allow 192.168.5.0/24;
        deny all;
    }
}
```

  
说明：这里的119.29.29.29 为一个DNS的ip，用resolver来指定。

假如B机器内网ip为 192.168.5.11，只需要在A上加一条hosts

192.168.5.11 aaa.com bbb.com ccc.com ddd.com eee.com

