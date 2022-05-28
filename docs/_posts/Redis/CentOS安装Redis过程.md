---
title: CentOS安装Redis过程
date: 2022-05-28 17:33:25
permalink: /pages/jd3v22/
sidebar: false
categories:
- Linux
- Redis
  tags:
- Redis
  author: sexileman
---

### 一、先去Redis官网下载

###### 1.下载

```sh
wget https://github.com/redis/redis/archive/7.0.0.tar.gz
# 解压
tar -xzvf 7.0.0.tar.gz
```

<!-- more -->

### 二、安装Redis

###### 2.进入到刚才解压的目录，执行编译

`由于Redis是用C语言开发的，安装之前必须确认是否安装gcc环境(gcc -v),如果没有安装，可以执行以下命令安装`

```shell
yum -y install gcc
```

`注：这里安装时加了一个参数 PREFIX=/目录，这个参数是指定安装的目录，即最终会安装在/usr/local/redis/bin 目录下，如果不指定的话，会安装在系统默认的位置，一般是在/usr/local/bin 目录下`

```shell
# 编译
 make
# 安装
make install
```

####### 3.配置Redis相关

```shell
# 复制redis.conf到想要的目录
    cp /usr/local/redis/redis-7.0.0/redis.conf /etc/redis/6379.conf
# 编辑我们复制的这份redis.conf，将daemonize no改成yes
# 允许远程连接，注释掉 bind 127.0.0.1 -::1
# 设置redis密码 新增一行 requirepass xxxx设置密码
vi redis.conf
# 将redis的启动脚本复制一份放到/etc/init.d目录下
cp /usr/local/redis/redis-7.0.0/utils/redis_init_script /etc/init.d/redisd
# 使用vi编辑redisd文件，在第一行加入如下两行注释，保存退出
# chkconfig:   2345 90 10
# description:  Redis is a persistent key-value database
```

```shell
# 执行开机自启命令
chkconfig redisd on
# 开启 
service redisd start
# 关闭
service redisd stop
```

