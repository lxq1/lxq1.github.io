---
layout: post
title: VSFTP启动失败-Failed to start Vsftpd ftp daemon
categories: 
  - 软件
tags:
  - Linux
  - 命令
---


## 错误如下
```
[root@host-liu vsftpd]# systemctl status vsftpd.service
vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; disabled)
   Active: failed (Result: exit-code) since Tue 2018-07-03 10:09:23 CST; 2min 3s ago
  Process: 2600 ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf (code=exited, status=2)

Jul 03 10:09:23 host-liu systemd[1]: vsftpd.service: control process exited, code=exited status=2
Jul 03 10:09:23 host-liu systemd[1]: Failed to start Vsftpd ftp daemon.
Jul 03 10:09:23 host-liu systemd[1]: Unit vsftpd.service entered failed state.

```

## 分析原因

1. 上网查找问题解决办法，**有人说listen改成NO**，但我的配置文件已经是NO了，所以这个方法不行，
2. 有人说端口占用问题，**看了一下端口**，21-20都是没服务占用，只能想其他办法了，
3. **关闭防火墙和SELinux**来测试最好，排除其他干扰因素。

## 解决办法

1. 一般启动错误肯定是配置文件有问题，我在配置虚拟用户模式的时候，出现了这个问题。
2. 最后清空所有配置文件，然后vi一行一行输入，在重启服务，发现可以用了，可能是自己复制粘贴网上代码的时候，编码格式问题导致了。
3. 如果大家有出现此问题的，可以试试清空配置文件，然后一行行添加下面最简单的配置文件参数来测试。

## 最简单的配置文件
```
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

## 虚拟用户配置文件
```

anonymous_enable=NO
local_enable=YES
guest_enable=YES
guest_username=virtual
allow_writeable_chroot=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd.vu
userlist_enable=YES
tcp_wrappers=YES
user_config_dir=/etc/vsftpd/vuser_dir
~                                     
```