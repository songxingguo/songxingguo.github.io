title: Linux安装Mysql
author: songxingguo
date: 2018-06-06 12:14:46
tags:
---
查看Mysql服务
```
lsof -i:3306
```
或者
```
service mysqld status
```

按名称查找
```
find / -name mysql
```

删除所有查询的mysql
```
find / -name mysql|xargs rm -rf
```