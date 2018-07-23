---
layout: post
title: SELinux
categories: 
  - 软件
  
tags:
  - SELinux
  - linux
---

# SELinux


## 查看状态命令

```

sestatus -v        ##如果SELinux status参数为enabled即为开启状态

```

```

getenforce

```


## 关闭SELinux方法

1. 临时关闭，不需要重启
```
setenforce 0 #设置SELinux 成为permissive模式
setenforce 1 设置SELinux 成为enforcing模式

```

2. 永久关闭，需要重启
```
修改/etc/selinux/config 文件

将SELINUX=enforcing改为SELINUX=disabled

```