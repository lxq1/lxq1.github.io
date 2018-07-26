---
title: docker学习进度1
tags: 
- linux
- docker
categories: 学习
---


CentOS Docker 安装

> [参考地址](http://www.runoob.com/docker/centos-docker-install.html)

## 前提条件

目前，CentOS 仅发行版本中的内核支持 Docker。

Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。

Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。

## 查询版本

<!-- more --> 
```
cat /etc/redhat-release 
```

## 使用 yum 安装

```
uname -r ##查看内核版本是否符合要求

```

## 安装 Docker
```
yum -y install docker-io

service docker start    #启动 Docker 后台服务

```



## 安装Compose

```

curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

docker-compose up -d #运行yml

```

## 测试docker

```
docker run hello-world

```

由于本地没有hello-world这个镜像，所以会下载一个hello-world的镜像，并在容器内运行。

## 镜像加速

鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：http://hub-mirror.c.163.com

```
#重启
service docker restart 
```
## Docker Hello World
Docker 允许你在容器内运行应用程序， 使用 docker run 命令来在容器内运行一个应用程序。

```
docker run ubuntu:15.10 /bin/echo "Hello world" #Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。
```



## 运行交互式的容器
我们通过docker的两个参数 -i -t，让docker运行的容器实现"对话"的能力

```
docker run -i -t ubuntu:15.10 /bin/bash ##泓奥云
```

```

docker run centos:6.8 /bin/echo "hello world" ##腾讯云

```


各个参数解析：

* -t:在新容器内指定一个伪终端或终端。

* -i:允许你对容器内的标准输入 (STDIN) 进行交互。

我们尝试在容器中运行命令 cat /proc/version和ls分别查看当前系统的版本信息和当前目录下的文件列表。

我们可以通过运行exit命令或者使用CTRL+D来退出容器。


## 启动容器

泓奥云
```
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
腾讯云
```
docker run -d centos:6.8 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

在容器内使用docker logs命令，查看容器内的标准输出

```
docker logs ** #填写docker id

```

## 停止容器

```

docker stop ** #填写docker id
```

## 运行一个web应用

接下来让我们尝试使用 docker 构建一个 web 应用程序。

我们将在docker容器中运行一个 Python Flask 应用来运行一个web应用。

```

docker pull training/webapp

docker run -d -P training/webapp python app.py


```

Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 32769 上。

这时我们可以通过浏览器访问WEB应用



## 进入容器

```

docker exec -it <docker_name> /bin/bash
```
其中，/bin/bash有可能是/bin/sh，因为不一定所有的docker都安装了shell


## 卸载docker

```

rpm -e docker-io
```
卸载Docker后,/var/lib/docker/目录下会保留原Docker的镜像,网络,存储卷等文件. 如果需要全新安装Docker,需要删除/var/lib/docker/目录

```
rm -fr /var/lib/docker/

```

## 卸载镜像

1. 删除前需要保证容器是停止的  stop

2. 需要注意删除镜像和容器的命令不一样。 docker rmi ID  ,其中 容器(rm)  和 镜像(rmi)

3. 顺序需要先删除容器

```
docker rmi 镜像id


```

## 修改已经存在的docker容器的映射端口

1. 停止容器
2. 停止docker
3. 进入    /var/lib/docker/containers/ 容器ID的文件夹
4. 修改config.v2.json和hostconfig.json
5. 启动docker
6. 启动容器



