---
title: Wordpress错误提示:Cookies被阻止或您的浏览器不支持
tags:
- wordpress
- error
categories:
- 软件
---


## 出现问题

自己刚刚装了wordpress没两天，就出现这个问题，导致后台登录不上。

![](https://ws1.sinaimg.cn/large/640dde2dgy1ftjr50xnwuj20cd0cc3yt.jpg)

## 分析问题

###  升级版本？
1.  百度查了一下，有的说升级版本，但是刚刚装的最新的版本啊，应该不是这个问题。

### 设置浏览器安全性？

1. 打开cookie设置，发现已经设置好了安全性了，应该也不是这个问题。

![](https://ws1.sinaimg.cn/large/640dde2dgy1ftjr59nxdnj20m009paak.jpg)

### 插件问题？

1. 进入云主机后台，将plugins的文件夹重新命名其他的，重新刷新浏览器，结果成功进入后台！

 ![](https://ws1.sinaimg.cn/large/640dde2dly1ftjr6pnz8zj209n04u3yg.jpg)

## 解决问题

1. 将plugins的文件夹重命名后，进入到后台，然后在改回原来的名字即可。
