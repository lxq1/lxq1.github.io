---
title: Linux修改网络配置的几种方法
categories: 软件
tags:
- Linux
- network
---


```
vi /etc/sysconfig/network-scripts/ifcfg-eth0

修改

    ONBOOT=yes        // 计算机启动时是否激活网卡，此处取值yes

    BOOTPROTO=static               // 获取IP的方式，此处取值static

添加

    IPADDR=192.168.5.233                            //IP地址

    NETMASK=255.255.255.0                        //子网掩码

    GATEWAY=192.168.5.254                        //网关地址

    DNS1=8.8.8.8

    DNS2=114.114.114.114
service network restart重启网络服务

```
<!-- more --> 

CentOS7纯净版下配置网络：

1.安装完CentOS7后，root登录进入系统

2.输入ip a回车，查看网卡信息，并记录DEVICE和HWADDR供后续使用

3.输入vi /etc/sysconfig/network-scripts/ifcfg-eno16777736（DEVICE在这里用到）

4.编辑界面按“i”键 

修改     

    ONBOOT=yes     

    BOOTPROTO=static 

添加     

    HWADDR=第2步所做的记录    

    IPADDR=所设的ip，如：192.168.5.233   

    PREFIX=24  

    GATEWAY=192.168.5.254  

    DNS1=8.8.8.8    

    DNS2=114.114.114.114

5.按Esc键，输入:wq！保存退出

6.输入service network restart重启网络

7.输入ip a回车查看配置信息是否写入

8.ping本地网络、外网、域名看能否ping通

9.均能ping通后，输入yum install net-tools，可将ifconfig命令找回