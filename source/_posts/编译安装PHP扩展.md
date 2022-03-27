---
title: 编译安装PHP扩展
tags:
  - Linux
  - PHP扩展
categories:
  - 编译安装
---
# 编译安装PHP扩展
1、下载PHP环境安装包 php-7.1.4
2、解压php安装包  
`tar -zxf php-7.1.4.tar.gz`
3、进入ext和你要安装的扩展的目录
4、执行phpize
`/usr/local/php7-bht/bin/phpize`
5、执行编译命令
 
```
./configure --with-php-config=/usr/local/php7-bht/bin/php-config --with-curl=DIR
```
**ps:** --with-curl 根据安装扩展不同更改

6、执行
 
```
    make && make install
```
7、然后会生成扩展名.so的文件
目录在extensions下

```
/usr/local/php7-bht/lib/php/extensions/no-debug-non-zts-20160303/
```
8、执行加载模块

```
/usr/local/php/bin/php -m |grep curl
```




