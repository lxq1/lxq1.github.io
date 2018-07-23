---
layout: post
title: GIT多人协同开发服务器搭建
categories: 
  - 软件
tags:
  - Linux
  - GIT
  - 教程
---


## 简介

### 工具

GIT是一款免费，开源的版本控制系统，用于敏捷高效管理大小项目。

### 环境

1. 腾讯云服务器centos6.8
2. GIT二进制包

### 目标

1. 根据GIT源码搭建GIT服务器
2. 利用ssh公钥管理多人协同开发




## 安装依赖包

```shell

//源码编译需要很多依赖包，提前安装好
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```



## 下载GIT

```shell
cd /usr/local/src  //选择一个下载目录
wget https://mirrors.edge.kernel.org/pub/software/scm/git/ 
```



## 解压和编译

```shell
tar -zvxf git-2.10.0.tar.gz
cd git-2.10.0
make all prefix=/usr/local/git
make install prefix=/usr/local/git
```


## 配置环境变量

```shell

echo 'export PATH=$PATH:/usr/local/git/bin' >> /etc/bashrc
source /etc/bashrc //生效
git --version //查看是否安装成功
```



## 创建GIT账户
```shell
useradd -m gituser
passwd gituser
su gituser
cd
mkdir .ssh

chmod 700 .ssh
// 把开发者的 SSH 公钥添加到这个用户的 authorized_keys 文件中

chmod 600 ~/.ssh/authorized_key
cat git.pub >> /home/git/.ssh/authorized_keys

```


## 初始化GIT

```shell

mkdir -p /data/repositories

cd /data/repositories/ && git init --bare test.git
```

   

## 配置用户权限

```shell
 chown -R gituser:gituser /data/repositories
 chmod 755 /data/repositories
```


## 限制权限不能登录服务器
```shell
    mkdir /home/gituser/git-shell-commands
    vi /etc/passwd
    gituser:x:500:500::/home/gituser:/usr/local/git/bin/git-shell

```



## 使用GIT

```shell
//建立自己的本地文件夹

cd ~ 

git init

git clone gituser@ip:/data/repositories/test.git

```



> [参考地址](https://www.cnblogs.com/feige1314/p/6955430.html)
> [大型github服务搭建](https://www.cnblogs.com/fly_dragon/p/8718614.html)