---
title:
categories:
tags:
---
# nextcloud


## 先下载镜像

## 关闭selinux


## yml配置文件

```


nextcloud-db:

    image: mariadb
    container_name: nextcloud_db
    ports:
    - 33034:3306
    volumes:
        - /cloud/mysql/data:/var/lib/mysql

    environment:

        - MYSQL_ROOT_PASSWORD=
        - MYSQL_DATABASE=nextcloud
        - MYSQL_USER=lxq
        - MYSQL_PASSWORD=
        - MYSQL_ROOT_HOST=%

nextcloud_web:

    image: wonderfall/nextcloud
    container_name: nextcloud_web
    environment:
        - UID=1000
        - GID=1000
        - UPLOAD_MAX_SIZE=10G
        - APC_SHM_SIZE=128M
        - OPCACHE_MEM_SIZE=128
        - CRON_PERIOD=15m
        - TZ=Aisa/Shanghai
        - ADMIN_USER=admin
        - ADMIN_PASSWORD=
        - DOMAIN=pan.haoguangfu.cn
        - DB_TYPE=mysql
        - DB_NAME=nextcloud
        - DB_USER=lxq
        - DB_PASSWORD=
        - DB_HOST=nextcloud-db:3306

    volumes:

        - /cloud/nextcloud/data:/data

        - /cloud/nextcloud/config:/config

        - /cloud/nextcloud/apps:/apps2

        - /cloud/nextcloud/themes:/nextcloud/themes
    expose:
      - 8888
    links:

      - nextcloud-db

 	
```

## 重建文件索引

```

cp -r /aria2/data/_dl/.  /root/nextcloud/data/lixq/files/video/down

rm -rf /aria2/data/_dl/*

docker exec -it nextcloud_web sh

occ files:scan --path="/lixq/files/video" ##新文件名必须是英文或数字，乱码的话不显示
```

> https://www.orgleaf.com/2400.html

