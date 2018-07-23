---
layout: post
title: 更改rhel为centos源
categories: 
  - 软件

tags:
  - Linux

---



![](https://ws1.sinaimg.cn/large/640dde2dly1ftjpf1aldkj20vk02raab.jpg)

## 前言

虚拟机安装的RHEL 7 64版本，在使用yum安装软件时，提示错误，才知道原来redhat使用yum源是需要收费的。
为了继续学习，决定更改yum源。

## 卸载redhat原来自带的包

```
rpm -qa | grep yum 
查询所有yum包

rpm -qa|grep yum|xargs rpm -e --nodeps 
不管依赖关系直接卸载yum包

rpm -qa | grep yum
查询为空时，代表卸载成功

```

## 下载新的centos的包

查看当前系统的版本和位数
```
[root@host-liu yum.repos.d]# cat /etc/issue && arch
\S
Kernel \r on an \m

x86_64
[root@host-liu yum.repos.d]# cat /etc/system-release
Red Hat Enterprise Linux Server release 7.0 (Maipo)

```

打开地址 http://mirrors.163.com/centos/7/os/x86_64/Packages/


找到yum、yum-plugin-fastestmirror、yum-metadata-parser、python-urlgrabber这四个软件包下载。



> https://www.cnblogs.com/jym1/p/8088005.html


![](https://ws1.sinaimg.cn/large/640dde2dgy1ftjpk03ifdj20lu0hwjvs.jpg)



