---
title: mysql主从复制
categories:
  - Mysql
tags:
  - 主从复制
---
 

 
# mysql主从复制

-------


## 一、 配置主服务器

> 1. 编辑my.cnf文件 默认位置一般在/etc下

在[mysqld]的下面加入下面代码：


```
log-bin=mysql-bin
server-id=1
innodb_flush_log_at_trx_commit=1
sync_binlog=1
binlog-do-db=wordpress//表明备份哪个数据库
binlog_ignore_db=mysql //表明忽略mysql库的备份
```
> 2.重启mysql 

```
 service mysqld restart
```
> 3.连接mysql数据库 

```
    mysql -u root -p
```
> 4.在主服务器上创建用户并赋予"REPLICATION SLAVE"权限 x.x.x.x为 从属服务器ip

* 已授权的方式创建用户

 
```
    GRANT REPLICATION SLAVE
    -> ON *.*
    -> TO 'ps'@'192.168.199.118'
    -> IDENTIFIED BY '123456';
```

> 5.执行以下命令锁定数据库以防止写入数据。
   
```
 mysql>FLUSH TABLES WITH READ LOCK;
```
    
> 6.导出数据库备份文件

```
mysqldump -u root -p  --databases work  --lock-tables=false > all.sql
//lock-tables 是否锁定数据表 
//databases 数据库名
```

> 7.用scp命令传输数据库文件all.sql到从服务器

```
scp all.sql root@192.168.199.118:/root
```

* [注意:] scp命令使用时 主服务器和从服务器都要安装 openssh-clients

```
    yum install -y openssh-clients
    ssh -v //查看服务器上是否有openssh-clients
```
 
-------

> 8.连接mysql数据库 进入mysql命令行查看master状态

```
mysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000001 |      260 | work         | mysql            |
+------------------+----------+--------------+------------------+
```

> 9.解锁数据表

```
    mysql> UNLOCK TABLES;
```

## 二、 配置从属数据库
> 1.导入主数据库数据表

```
mysql -u root -p work < all.sql
```
* [注释]  < 导入 > 导出
 
> 2.编辑my.cnf,在[mysqld]下面加入

server-id=2

可以自己定义,保证唯一
 
> 3.登录mysql服务器，执行以下命令。

```
mysql> CHANGE MASTER TO
    -> MASTER_HOST='192.168.199.163',
    -> MASTER_USER='ps',
    -> MASTER_PASSWORD='123456',
    -> MASTER_PORT=3306,
    -> MASTER_LOG_FILE='mysql-bin.000001',
    -> MASTER_LOG_POS=260,
    -> MASTER_CONNECT_RETRY=10;
```
* [注意:] MASTER_HOST:主服务器的IP。
* MASTER_HOST:主服务器的IP。
* MASTER_USER：配置主服务器时建立的用户名
* MASTER_PASSWORD：用户密码
* MASTER_PORT：主服务器mysql端口，如果未曾修改，默认即可。

>4.启动slave进程。


```
    START SLAVE;//开启SLAVE进程
    show slave status\G //查看SLAVE进程状态
```

* [注意]连接不上mysql数据库 有可能是防火墙的原因 

```
service iptables stop
```

