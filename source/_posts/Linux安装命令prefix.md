---
title: Linux安装命令prefix
categories: 软件
tags:
- linux
- prefix
---



摘自：https://zhidao.baidu.com/question/535223201.html


不指定prefix，则可执行文件默认放在/usr /local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc。其它的资源文件放在/usr /local/share。你要卸载这个程序，要么在原来的make目录下用一次make uninstall（前提是make文件指定过uninstall）,要么去上述目录里面把相关的文件一个个手工删掉。
指定prefix，直接删掉一个文件夹就够了。

-----

摘自 ：http://baike.haosou.com/doc/3909580-4103422.html

linux安装软件采用源码安装灵活自由，适用于不同的平台，维护也十分方便。

源码的安装一般由3个步骤组成:

配置(configure)

编译(make)

安装(make install)

安装方法：

具体的安装方法一般作者都会给出文档，这里说明配置(configure)的prefix选项

以安装supersparrow-0.0.0为例，我们打算把他安装到目录 /usr/local/supersparrow,于是在supersparrow-0.0.0目录执行带选项的脚本

./configure –prefix=/usr/local/supersparrow

执行成功后再编译、安装(make，make install);安装完成将自动生成目录supersparrow,而且该软件任何的文档都被复制到这个目录。为什么要指定这个安装目录?是为了以后的维护方便，假如没有用这个选项，安装过程结束后，该软件所需的软件被复制到不同的系统目录下，很难弄清楚到底复制了那些文档、都复制到哪里去了-基本上是一塌糊涂。

用了-prefix选项的另一个好处是卸载软件或移植软件。当某个安装的软件不再需要时，只须简单的删除该安装目录，就能够把软件卸载得干干净净;移植软件只需拷贝整个目录到另外一个机器即可(相同的操作系统)。

一个小选项有这么方便的作用，建议在实际工作中多多使用。