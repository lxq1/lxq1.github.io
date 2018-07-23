---
title:
categories:
tags:
---
# 虚拟机全新Linux用源码安装Apache

安装方式可以采用源码安装或二进制数据包安装，源码安装是可以定制的一种安装方式，自由度比较高，满足企业对各种环境的不同需求。
源码安装需要大量的依赖软件包。

二进制数据包选择rpm包安装，简单快捷。

## 1. 下载软件包

采用wget联网下载以下的源码包

*  [apache](https://mirrors.tuna.tsinghua.edu.cn/apache//httpd/httpd-2.4.33.tar.gz)
* [apr](http://mirrors.hust.edu.cn/apache//apr/apr-1.6.3.tar.gz)
* [apr-util](https://mirrors.tuna.tsinghua.edu.cn/apache//apr/apr-util-1.6.1.tar.gz)


## 2. 安装软件

由于我们采用了源码安装Apache，所以安装Apache Http Server 之前需要安装很多依赖软件包，可以使用yum安装。

```
yum -y install gcc autoconf automake make \
> pcre pcre-devel openssl openssl-devel

还需要补充上error1的解决办法
```

![](https://ws1.sinaimg.cn/large/640dde2dly1ftjrebt1gvj20s40arjte.jpg)

```

[root@localhost ~]# tar -xzf httpd-2.4.33.tar.gz -C /usr/src/
[root@localhost ~]# tar -xzf apr-1.6.3.tar.gz -C /usr/src
[root@localhost ~]# tar -xzf apr-util-1.6.1.tar.gz -C /usr/src/

[root@localhost ~]# cd /usr/src/
[root@localhost src]# cd apr-1.6.3/
[root@localhost apr-1.6.3]# ./configure

[root@localhost apr-1.6.3]# make && make install

[root@localhost apr-util-1.6.1]# ./configure --with-apr=/usr/local/apr

[root@localhost apr-util-1.6.1]# make && make install 

提示缺少error1

[root@localhost src]# cd httpd-2.4.33/

[root@localhost httpd-2.4.33]# ./configure --prefix=/usr/local/apache2 --enable-so \
> --enable-ssl --enable-rewrite --with-mpm=worker --with-suexec-bin \
> --with-apr=/usr/local/apr/

[root@localhost httpd-2.4.33]# make && make install
提示error2

```

## 3. 启动软件

```
/usr/local/apache2/bin/apachectl start

```

* 配置防火墙

iptable -I  INPUT -P tcp --dport 80 -j ACCEPT

server iptables save 

* 查看端口是否启用

## 发生过的错误

### error1 缺少expat-devel包
```
xml/apr_xml.c:35:19: 致命错误：expat.h：没有那个文件或目录
 #include <expat.h>
                   ^
编译中断。
make[1]: *** [xml/apr_xml.lo] 错误 1
make[1]: 离开目录“/usr/src/apr-util-1.6.1”
make: *** [all-recursive] 错误 1
```
* 解决办法
yum install expat-devel

### error2 [htpasswd] 错误 
```

collect2: error: ld returned 1 exit status
make[2]: *** [htpasswd] 错误 1
make[2]: 离开目录“/usr/src/httpd-2.4.33/support”
make[1]: *** [all-recursive] 错误 1
make[1]: 离开目录“/usr/src/httpd-2.4.33/support”
make: *** [all-recursive] 错误 1



```


* 解决办法
```
cd /usr/src/
cp -r apr-1.6.3  /usr/src/httpd-2.4.33/srclib/apr
cd apr-1.6.3
./configure --prefix=/usr/local/apr
make && make install

cp -r apr-util-1.6.1  /usr/local/src/httpd-2.4.33/srclib/apr-util
cd /usr/src/apr-util-1.6.1
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
make && make install

cd /usr/src/httpd-2.4.33
./configure --with-included-apr --prefix=/usr/local/apache2 --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-so --enable-ssl --enable-rewrite --with-mpm=worker --with-suexec-bin 
make && make install
```


# 写一个相对完美点的全部安装代码

```

yum -y install gcc autoconf automake make \
> pcre pcre-devel openssl openssl-devel expat-devel

 tar -xzf httpd-2.4.33.tar.gz -C /usr/src/
 tar -xzf apr-1.6.3.tar.gz -C /usr/src
 tar -xzf apr-util-1.6.1.tar.gz -C /usr/src/

cd /usr/src/
cp -r apr-1.6.3  /usr/src/httpd-2.4.33/srclib/apr
cd apr-1.6.3
./configure --prefix=/usr/local/apr
make && make install

cp -r apr-util-1.6.1  /usr/src/httpd-2.4.33/srclib/apr-util
cd /usr/src/apr-util-1.6.1
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
make && make install

cd /usr/src/httpd-2.4.33
./configure --with-included-apr --prefix=/usr/local/apache2 --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-so --enable-ssl --enable-rewrite --with-mpm=worker --with-suexec-bin 
make && make install
```


