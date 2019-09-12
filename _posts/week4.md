---
type: photo
title: 一周总结week3
date: 2019-09-12 10:48:58
category: 周报
photos:
- https://www.gnu.org/software/gdb/images/archer.svg
tags:
- 工具
description: gdb 
---
## 利用gdb调试php源码
### 编译php源码
git仓库下载php源码
``` shell
-> git clone http://git.php.net/repository/php-src.git
-> cd php-src
~/php-src> git checkout PHP-7.2 origin/PHP-7.2
~/php-src> ./buildconf 
```
如果你执行buildconf出错，那么很可能是因为你的系统中没有autoconf这个工具，Mac下用homebrew工具安装即可。
``` shell
~/php-src> ./configure --disable-all --enable-debug --prefix=$HOME/myphp
~/php-src> make install
```
注意这里的prefix的参数必须为绝对路径，所以你不能写成~/myphp，另外我们这次编译只是为了调式，所以建议一定要设置prefix参数，要不然PHP会被安装到默认路径中，大多数时候是/usr/local/php中，这可能会造成一些没必要的污染。另外我们使用了两个选项，一个是--disable-all，这个表示禁止安装所有扩展（除了一个必须安装的），另外一个就是--enable-debug，这个选项表示以debug模式编译PHP源码，相当于gcc的-g选项，它会把调试信息编译进最终的二进制程序中。

### Mac OS X 10.5之后的版本安装gdb
```
brew install gdb
```
通过brew安装gdb后，并不能正常的工作，因为gdb需要控制其它进程来执行，这在Mac的最新版本中是不容许的，因此需要进行一系列的设置，[具体参见此文](https://sourceware.org/gdb/wiki/PermissionsDarwin#Create_a_certificate_in_the_System_Keychain)

### 使用gdb进行调试
hello.php
``` php
<?php
echo 'hello';
```
``` shell
~/myphp> gdb --args bin/php hello.php
(gdb) r
```
常用的gdb调试命令如下，也可以通过`man gdb`查看更多关于gdb的用法
* r->run执行
* b->break针对文件的函数设置断点
* c->continue继续执行
* n->next下一步
* list->针对当前行显示源码
* print->打印变量或表达式
