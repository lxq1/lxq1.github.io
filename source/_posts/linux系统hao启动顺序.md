---
layout: post
title: linux系统hao启动顺序
categories: 
  - 软件
 
tags:
  - linux
  - hao
---

1. 启动 frp.sh  启动http和https内网穿透
2. 临时禁用SELinux
```
setenforce 0
```
3. docker 启用db数据库
```
docker start nextcloud_db 
```
4. docker 启用web
```
docker start nextcloud_web 
```
5. docker 启用onlyoffice

```
docker start onlyoffice 
```
5. 启用Nginx

```

/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

./nginx -s reload
```





