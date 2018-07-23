---
layout: post
title: GITHUB使用socks5代理SSH
categories: 
  - 软件

tags:
  - github
  - socks5
  - 教程
---

## 简介
### 需求
1. github是非常不错的代码托管平台，相信没有开发者不知道他。
2. 由于特殊原因，github在国内访问速度非常非常慢，基本上不超过20k/s，天啦噜~

### 目标
1. 通过利用socks5代理提升github访问速度。
### 环境
1. Windows 7系统
2. github 客户端
3. shadowsocks软件
## 配置

### github-http-socks5配置文件
```java
// C:\Users\lixq\.gitconfig

[user]
	name = github-name
	email = github-email
[http]
	proxy = socks5://127.0.0.1:1080
```


### github-ssh-socks5配置文件

```java
// C:\Users\当前用户\.ssh\config

Host github.com
	
	ProxyCommand connect -H 127.0.0.1:1080 %h 22
```

## 效果



