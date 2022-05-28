---
title: MySql8.0 Bundle RPM安装过程
date: 2022-05-28 14:33:25
permalink: /pages/sg6x2/
sidebar: false
categories:
  - Linux
  - MySql
tags: 
  - 数据库
author: sexileman
---

#### 一、清理旧的mysql环境

###### 1.查看当前安装的相关包

```sh
rpm -qa|grep -i mysql
# 也可能是
rpm -qa|grep -i mariadb
```

<!-- more -->

###### 2.卸载相关包及目录

```sh
rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
find / -name mysql
rm -rf /usr/share/mysql /usr/lib64/mysql /etc/selinux/targeted/active/modules/100/mysql
```

#### 二、安装及登录

###### 3.下载并解压tar安装包

```sh
# 使用wget下载
wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.29-1.el8.x86_64.rpm-bundle.tar
tar -xvf mysql-8.0.29-1.el8.x86_64.rpm-bundle.tar
```

###### 4.使用yum安装先行依赖包

```sh
yum -y install libaio
yum -y install perl
yum -y install net-tools
```

###### 5.按顺序安装rpm包

```sh
rpm -ivh mysql-community-common-8.0.29-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.29-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.29-1.el7.x86_64.rpm
rpm -ivh mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.29-1.el7.x86_64.rpm
```

###### 6.查看、启动、停止mysqld服务

```sh
systemctl status mysqld # 查看
systemctl start mysqld # 启动
systemctl stop mysqld # 停止
```

###### 7.第一次登录查找临时密码（必须先启动mysql服务）

```sh
# 查找临时密码
grep "temporary password" /var/log/mysqld.log
```

###### 8.使用临时密码登录，并设置新密码

```sh
# 使用临时密码登录如：OVgAh2Iwk2/r
mysql -uroot -p
```

```mysql
-- 设置密码
alter user 'root'@'localhost' identified by 'xxx';
flush privileges;
```

###### 9.设置远程登录

```mysql
create user mysql@'%' identified by 'xxx';
grant all privileges on *.* to mysql@'%' with grant option;
flush privileges;
```

