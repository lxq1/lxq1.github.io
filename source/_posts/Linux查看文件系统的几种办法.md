---
layout: post
title: Linux查看文件系统的几种办法
categories: 
  - 软件
tags:
  - Linux
---


1. fdisk -l

只能列出硬盘的分区表，容量大小以及分区类型，但是看不到文件系统类型。

2. df -h

用来查看文件系统磁盘空间使用量的，但是df命令只会列出已经挂载的文件系统信息，对于没有挂载的文件系统是查看不到的。

3. parted 

命令比较强大了， 可以对大于2T的磁盘设备进行分区，已经创建GPT分区。

例如查看/dev/sda的文件系统类型，可以按照如下步骤查看
```
parted /dev/sda

      print list
```