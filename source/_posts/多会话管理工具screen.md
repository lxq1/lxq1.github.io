---
layout: post
title: 多会话管理工具screen
categories: 
  - 软件
  
tags:

  - screen
---


## 需求

以前一直在用frp1.14版本的时候，使用xshell登录运行&命令后，退出后，也是可以稳定运行的，但是升级到frp1.19版本的时候，发现这个&在退出ssh的时候，后台也是直接退出的。

## 查找

从网上寻求后台进程方案。有三种。

| 1| 2 |3 |
|--- |---|---|
|&|nohup|screen|
|简单，容易失效|无法查看状态|需要安装包，功能强大|

## screen

最后决定采用screen后台进程方案。

[引用](http://man.linuxde.net/screen)

### 1.安装

[官方网站](http://www.gnu.org/software/screen/)

```
yum install screen

rpm -qa | grep screen #查看是否安装完毕
```
<!-- more --> 

### 2.使用

#### 选项

```

-A 　将所有的视窗都调整为目前终端机的大小。
-d <作业名称> 　将指定的screen作业离线。
-h <行数> 　指定视窗的缓冲区行数。
-m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r <作业名称> 　恢复离线的screen作业。
-R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s 　指定建立新视窗时，所要执行的shell。
-S <作业名称> 　指定screen作业的名称。
-v 　显示版本信息。
-x 　恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。
```
#### 创建对话

```
screen -S frpsocks5
```

#### 查看窗口

```
ctrl+a w #查看窗口，一般在xshell的标题栏
ctrl+a A #为当前窗口命名
```

#### 会话分离

```
ctrl+a d #会话分离
screen -ls
screen -r id #会话还原
```

#### 会话清除

```
screen -wipe
```
#### 会话关闭
```
ctrl+a k
```




