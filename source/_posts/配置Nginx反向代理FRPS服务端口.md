---
title: 配置Nginx反向代理FRPS服务端口
categories: 软件
tags: 
- Nginx
- frp 
---


##  文章作用

**根据最后的效果图，我们可以看到省去了端口号访问，直接用域名访问内网中的项目，方便好记！**

##  Nginx简介   

1.  开放源代码的高性能HTTP服务器和反向代理服务器，同时支持IMAP和POP3代理

2. 高性能、高可用、丰富的功能模块，低资源占用

3. 高达50000个并发连接

##  FRP简介 

1. frp 是一个可用于内网穿透的高性能反向代理应用，支持 tcp, udp, http, https 协议.
2. frp为免费且开源项目，需要自己利用一台拥有公网IP机器搭建服务端，以部署内网穿透项目。
3. [GITHUB开源项目地址](https://github.com/fatedier/frp/)


<!-- more --> 
##  安装Nginx

> 采用源码安装 

> 安装目录： /usr/local/nginx

```
wget http://nginx.org/download/nginx-1.14.0.tar.gz

tar -xzf nginx-1.14.0.tar.gz -C /usr/src/

yum -y install gcc pcre pcre-devel openssl \
> openssl-devel gd gd-devel perl perl-ExtUtils-Embed

cd /usr/src/nginx-1.14.0

./configure --prefix=/usr/local/nginx --with-ipv6 --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gzip_static_module --with-http_perl_module --with-mail \
> --with-mail_ssl_module

make && make install 



```

##  启动脚本指令

```
/usr/local/nginx/sbin/nginx #启动主程序
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf #配置文件启动主程序
/usr/local/nginx/sbin/nginx -s stop #关闭主程序
/usr/local/nginx/sbin/nginx -s reload #关闭主程序

```

##  配置反向代理

###  Nginx配置文件

```
	#FRP测试
	server {
       listen 80;
       server_name **.***.***; # 填写你的域名
       location / {
           proxy_pass http://127.0.0.1:8088; # 填写frps的http端口
           proxy_redirect http://$host/ http://$http_host/;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header Host $host;
       }
	}
```

###  frpc的ini配置文件

```

[web01]
type = http
local_ip = 127.0.0.1
local_port = 1024 #填写本机的服务端口
use_encryption = false
use_compression = true
custom_domains = **.***.*** #填写你的域名

```

###  frps的ini配置文件
```
#subdomain_host = **.***.***
```


##  效果图

![](https://ws1.sinaimg.cn/large/640dde2dly1ftjrdkq3epj217u0lhq7a.jpg)


## 参考引用

> [frps服务端与nginx可共用80端口](https://www.nat.ee/96.html)
