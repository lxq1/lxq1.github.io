---
title: mysql学习进度1
categories: 软件
tags:
- mysql
---



## 显示所有的数据库



```

show databases;


```

1251错误

我的是8.0的版本，因为比较新的mysql采用新的保密方式所以旧的似乎不能用，改密码方式：

```

use mysql；
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '';

FLUSH PRIVILEGES; 
```

<!-- more --> 


这两天用docker搭了个mariadb碰到一些问题在此记录一下。

搭建步骤
```

1 docker pull 最新的 mariadb镜像

2. 用镜像建立mariadb的 container

docker run --name mdb1 -v /home/xx/docker_mdb1_data:/var/lib/mysql \

-p 13306:3306

-e MYSQL_ROOT_PASSWORD=

-e MYSQL_USER=xx

-e MYSQL_PASSWORD=

-e MYSQL_ROOT_HOST=%

-d mariadb

3.以上步骤完成后mariadb 的 container mdb1 就直接运行了。

在此踩了3个坑要注意：

1.建立container是要注意SEliunx的限制，

本地映射的文件夹 /home/xx/docker_mdb1_data必须关闭SELinux的保护：

chcon -Rt svirt_sandbox_file_t /home/xx/docker_mdb1_data

2.使用mysql的root连接container时出现

Access denied for user 'root'@'localhost' (using password: NO)

的错误，后续的坑也由此引发。在网上查找了一下可能有两个原因

一,MYSQL_ROOT_PASSWORD和MYSQL_PASSWORD最好不要相同。

二,要添加MYSQL_ROOT_HOST=%的环境变量

3.建立container后出现mysql相关系统错误时，比如Access denied for user 'root'@'localhost' (using password: NO)

重新建container的时候必须连外部映射的存储/home/xx/docker_mdb1_data全部删除干净。

sudo rm -rf /home/xx/docker_mdb1_data

```