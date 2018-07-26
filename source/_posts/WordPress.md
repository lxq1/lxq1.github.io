---
title: wordpress搭建
tags: 
- wordpress
categories:
- 软件
---

# WordPress
wordpress_db:
```
    image: mysql:latest
    container_name: wordpress_db
    restart: always
    ports:
    - 33033:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
    environment:
        MYSQL_ROOT_PASSWORD: 
        MYSQL_DATABASE: WORDPRESS

        MYSQL_USER: 

        MYSQL_PASSWORD: 


```
docker 运行命令
```
docker run  --name wordpress_db -v /mysql/data:/var/lib/mysql \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD= \
-e MYSQL_DATABASE: wordpress \
-e MYSQL_USER=lxq \
-e MYSQL_PASSWORD=lxq1234 \
-e MYSQL_ROOT_HOST=% \
-d mysql

```

<!-- more --> 
wordpress_php:
```
    image: wordpress:fpm-alpine
    container_name: wordpress_php
    restart: always
    environment:
        WORDPRESS_DB_HOST: wordpress_db:3306

        WORDPRESS_DB_NAME: WORDPRESS

        WORDPRESS_DB_USER: lxq

        WORDPRESS_DB_PASSWORD: 

    volumes:

        - ./wordpress:/var/www/html

    links:
        - wordpress_db
        


```
docker 运行命令
```
docker run --name wordpress4 -v /wordpress4:/var/www/html \
-p 8888:80 \
-d wordpress:latest \


--link wordpress_db \
-e WORDPRESS_DB_HOST: wordpress_db:3306 \
-e WORDPRESS_DB_NAME: wordpress \
-e WORDPRESS_DB_USER: lxq \
-e WORDPRESS_DB_PASSWORD: 

fpm-alpine

```