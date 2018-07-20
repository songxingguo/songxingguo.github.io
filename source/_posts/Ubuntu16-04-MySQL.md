title: ' Ubuntu 16.04 安装使用MySQL'
author: songxingguo
tags:
  - mysql
  - 'Ubuntu '
categories:
  - 操作系统
date: 2018-07-20 03:56:00
---
### 安装Mysql数据库

- 安装前先更新软件包列表
```
sudo apt update
```
- 删除Mysql
```
sudo apt-get remove mysql-*
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```
- 安装

  ```
  sudo apt-get install mysql-client mysql-server libmysqlclient-dev
  ```
  因为Ubuntu是16.04的，所以会默认安装5.7版本。
  <!-- more -->

- 安装完成后查看Mysql状态

  ```
   sudo service mysql status
  ```
- 启动服务

 ```
 sudo service mysql start
 ```
- 解决启动报错问题

  ```
  ls -ld /var/run/mysqld/
  mkdir -p /var/run/mysqld
  ls -ld /var/run/mysqld/
  sudo chown mysql.mysql /var/run/mysqld/
  sudo /etc/init.d/mysql start
  ```
- 登录Mysql数据库

  ```
  mysql -u root -p
  ```
- 重启Mysql
  ```
  /etc/init.d/mysql restart 
  ```
  
### 数据库操作

 - #### 登录Mysql数据库
 
   ```
   mysql -u root -p
   ```
   -u 表示选择登陆的用户名， -p 表示登陆的用户密码，上面命令输入之后会提示输入密码，此时输入密码就可以登录到mysql。

- #### 查看数据库
```
show databases; 
```
- #### 管理的数据库
```
use dbname; 
```
- #### 查看数据库表
```
show tables; 
```
- #### 查看表结构
```
desc tablename;
```
- #### 删除表
```
drop table [if exists] tablename; 
```
- #### 退出
```
exit; / quit; 
```
### 参考教程

[ubuntu16.04彻底卸载mysql并且重新安装mysql](https://www.cnblogs.com/xym4869/p/8781792.html)
[uUbuntu 16.04 安装使用MySQL](https://blog.csdn.net/vXueYing/article/details/52330180)
[Ubuntu16.04下安装MySQL及简单操作](https://blog.csdn.net/qq_29666899/article/details/79079488)
[Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock'](https://blog.csdn.net/heatdeath/article/details/78907563)
[Linux mysql停止失败的解决办法 Stopping MySQL database server mysqld](https://blog.csdn.net/blueskybluesoul/article/details/36658933)
