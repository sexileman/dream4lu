---
title: CentOS 7.8 MySql5.6升级到5.7并且完成数据迁移
date: 2022-04-07 18:40:00
permalink: /pages/dhvv82/
sidebar: false
categories:
  - Linux
  - MySql
tags: 
  - 数据库
author: sexileman
---

### 一.准备工作

##### 1.将MySql5.6数据库的整个Data打包成tar.gz，可以使用如下命令查看

```mysql
show global variables like '%datadir%';
```

##### 2.可以看到我们的默认位置：

![image-20220407185745008](/Users/sexileman/Library/Application Support/typora-user-images/image-20220407185745008.png)

##### 3.然后去这个目录打包成tar.gz文件，我们这里将打包好的文件放在/usr/local/mysql文件夹下并命名为data.tar.gz，然后通过scp命令推送到我们升级的服务器上去

```sh
tar -zvcf /usr/local/mysql/data.tar.gz /var/lib/mysql
```

```sh
scp /usr/local/mysql/data.tar.gz ip:/usr/local/mysql
```

### 二.开始升级

##### 1.登录到MySql5.7所在的这台服务器上，先停止已经启动的mysql服务

```sh
service mysqld stop
```

##### 2.再找到MySql 5.7 Data所在位置，先将/var/lib/mysql整个文件夹备份一下，然后就可以删除掉了

```sh
rm -rf /var/lib/mysql
```

##### 3.解压已经拿到的文件，解压后的目录结构如下，将这个文件夹复制到/var/lib下,同时赋予权限

```sh
tar -xvf /user/local/mysql/data.tar.gz
```

![image-20220407191810526](/Users/sexileman/Library/Application Support/typora-user-images/image-20220407191810526.png)

```sh
cp -r /usr/local/mysql/var/lib/mysql /var/lib
```

```sh
chmod -R 777 /var/lib/mysql
```

##### 4.重启mysql服务

```sh
service mysqld start
```

##### 5.进入到/usr/bin目录下，升级数据库，使用如下命令，需要输入密码，这里root账号的密码是原来5.6版本中的密码

```sh
mysql_upgrade -uroot -p
```

##### 6.可以测试了