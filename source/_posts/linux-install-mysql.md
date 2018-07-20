title: Linux安装Mysql数据库
author: songxingguo
tags:
  - Linux
  - mysql
categories:
  - 操作系统
date: 2018-06-06 12:14:00
---
### 安装Mysql数据库

1. wget 命令下载mysql压缩包（大约300M）

  ```
  wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz
  ```
2.解压压缩包
```
tar -zxvf mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz
```

```
rm -f mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz
```

```
mv mysql-5.7.11-linux-glibc2.5-x86_64/ mysql
```

```
groups mysql
```

```
groupadd mysql
```
```
useradd -r -g mysql mysql`
```

```
cd database/mysql-5.7
```
```
chown -R mysql:mysql ./
```
### 常用命令

- 查看Mysql服务
  ```
  lsof -i:3306
  ```
   或者
  ```
  service mysqld status
  ```

- 按名称查找
  ```
  find / -name mysql
  ```

- 删除所有查询的mysql
  ```
  find / -name mysql|xargs rm -rf
  ```