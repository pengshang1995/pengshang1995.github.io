---
title: Vagrant使用
date: 2018-01-26 16:50:16
categories:
  - Linux
tags:
  - Linux
---

## Vagrant常用命令
1. vagrant box list  查看目前已有的box
2. vagrant box add  新增一个box
3. vagrant box remove 删除指定的box
4. vagrant init 初始化配置vagrantfile
5. vagrant up 启动虚拟机
6. vagrant ssh ssh登录虚拟机
7. vagrant suspend 挂起虚拟机
8. vagrant reload 重启虚拟机
9. vagrant half 关闭虚拟机
10. vagrant status 查看虚拟机状态
11. vagrant destroy 删除虚拟机

## 对虚拟机的优化
> **替换源**
> -------
* sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak #备份源文件
* sudo vim /etc/apt/source.list #修改源
* sudo apt-get update #更新列表
* 源内容如下：

```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```


>安装Apache Nginx PHP
>-------
sudo apt-get install 对应名称
对应名称 -v 可以查看版本号



> Apache更改端口，将端口设置为8888
>-------
1. 修改 ports.conf 文件
2. curl -I 'http://127.0.0.1:8888'

>Mysql 安装
>-------
1. sudo apt-get install mysql-server #服务器端
2. 安装期间会提示输入为mysql设置root密码，我这边不操作，直接enter 不设置密码
3. sudo apt-get install mysql-client #客户端
4. mysql -uroot -p #测试连接库，上面安装服务端没有设置密码，这里直接enter进入

>php扩展 sudo apt-get install 名称
>-------
1. php5-mcrypt
2. php5-mysql
3. php5-gd

> 支持apache2的php
> -------
1. sudo apt-get install libapache2-mod-php5
2. 开启rewrite功能
3. sudo a2enmod rewrite
>支持nginx fastcgi
>-------
1. sudo apt-get install php5-cgi php5-fpm

>修改成9000端口 ，默认sock模式
>-------
1. cd /etc/php5/fpm/pool.d
2. sudo vim www.conf # search listen = 127.0.0.1:9000
3. sudo /etc/init.d/php5-fpm restart

##Vagrant高级知识
1. 端口转发 `config.vm.network "forwarded_port", guest: 8888, host: 8889`
2. 共享文件夹`config.vm.synced_folder "/Users/ps/www","/home/www",:nfs=>true`
3. 私有网络设置 `config.vm.network "private_network", ip: "192.168.33.10"`

## 虚拟机优化
1. 虚拟机名称 `vb.name = "ubuntu_ps"`
2. 虚拟机主机名 `config.vm.hostname = "ps"`
3. 配置虚拟机内存和CPU
    * `vb.memory = "1024"`
    * `vb.cpus = 2`

## 打包分发
1.打包
vagrant package --output  xxx.box
vagrant package --output  xxx.box --base 虚拟机名称








 




