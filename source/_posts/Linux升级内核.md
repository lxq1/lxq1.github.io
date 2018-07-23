---
layout: post
title: Linux升级内核
categories: 
  - 软件
  
tags:
  - 内核
  - linux
---
# 升级内核


## 查看当前系统版本

```

[root@VM_0_10_centos ~]# more /etc/issue
CentOS release 6.9 (Final)
Kernel \r on an \m

[root@VM_0_10_centos ~]# uname -a
Linux VM_0_10_centos 2.6.32-642.6.2.el6.x86_64 #1 SMP Wed Oct 26 06:52:09 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

```

## 导入public key

```

[root@VM_0_10_centos ~]# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org


```

## 安装ELRepo到CentOS
```
[root@VM_0_10_centos ~]# rpm -Uvh http://119.163.198.36:1024/elrepo-release-6-8.el6.elrepo.noarch.rpm

```

## 安装 kernel-lt（lt=long-term）

```
[root@VM_0_10_centos ~]# yum --enablerepo=elrepo-kernel install kernel-lt -y

```


![](https://ws1.sinaimg.cn/large/640dde2dgy1ftjpo1l9pnj20qj0d9jsu.jpg)


## 编辑grub.conf文件，修改Grub引导顺序
[root@localhost ~]# vim /etc/grub.conf