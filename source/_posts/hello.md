---
title: hello
date: 2018-07-18 21:39:29
tags:
---


# VMware-Linux虚拟机扩容

## 部署的环境

 **RHEL7**

## 1. 查看当前磁盘容量
```
[root@host-liu lixq]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root  4.0G  3.6G  457M  89% /
devtmpfs               905M     0  905M   0% /dev
tmpfs                  914M  148K  914M   1% /dev/shm
tmpfs                  914M  8.8M  905M   1% /run
tmpfs                  914M     0  914M   0% /sys/fs/cgroup
/dev/sda1              497M  119M  379M  24% /boot
/dev/sr0               3.5G  3.5G     0 100% /run/media/lixq/RHEL-7.0 Server.x86_64


```

## 2. 扩容VMware的磁盘容量

![VMware](_v_images/_vmware_1531377243_15578.png)


## 3. 查看分区数量和大小

```
 fdisk /dev/sda
 p ##查看分区数量
 n ##增加一个分区
     p ##设置主分区
     3 ##分区号
     扇区大小默认enter就可
 t ##修改分区类型
     3 ## 选择分区3
     8e ## 修改成LVM
 w ##保存分区
 reboot
```
## 4.格式化分区


```

[root@host-liu lixq]# mkfs.ext3 /dev/sda3
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
983040 inodes, 3932160 blocks
196608 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4026531840
120 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```
## 5.添加到LVM组

1. 创建物理卷 

```

[root@host-liu lixq]# pvcreate /dev/sda3
WARNING: ext3 signature detected on /dev/sda3 at offset 1080. Wipe it? [y/n] y
  Wiping ext3 signature on /dev/sda3.
  Physical volume "/dev/sda3" successfully created
```

2. 扩展卷组

```
[root@host-liu lixq]# vgextend /dev/rhel /dev/sda3
  Volume group "rhel" successfully extended
```
查看卷组
```
[root@host-liu lixq]# vgdisplay
  --- Volume group ---
  VG Name               rhel
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               19.50 GiB
  PE Size               4.00 MiB
  Total PE              4993
  Alloc PE / Size       1154 / 4.51 GiB
  Free  PE / Size       3839 / 15.00 GiB
  VG UUID               xC1oFk-D0AD-coM2-vU11-dYBu-4kry-vOtcrl
```

3. 扩展根分区

```
-l 块数量

[root@host-liu lixq]# lvextend -L +14G /dev/rhel/root
    Extending logical volume root to 18.01 GiB
    Logical volume root successfully resized
```

4. 刷新根分区的容量

```

 resize2fs /dev/rhel/root  ##centos-6使用这个
    xfs_growfs /dev/rhel/root ##7版本使用这个
```

## 6.扩容成功

```
[root@host-liu /]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root   18G  3.6G   15G  20% /
devtmpfs               905M     0  905M   0% /dev
tmpfs                  914M  148K  914M   1% /dev/shm
tmpfs                  914M  8.8M  905M   1% /run
tmpfs                  914M     0  914M   0% /sys/fs/cgroup
/dev/sda1              497M  119M  379M  24% /boot
/dev/sr0               3.5G  3.5G     0 100% /run/media/lixq/RHEL-7.0 Server.x86_64

```