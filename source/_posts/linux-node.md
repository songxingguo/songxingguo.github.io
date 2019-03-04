title: Linux配置Node环境
author: songxingguo
tags:
  - Node
  - Linux
categories:
  - 操作系统
date: 2018-06-06 10:56:00
---
配置node环境
---

1. wget命令下载Node.js安装包（下载到当前目录）。
```bash
 wget https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz
```
2. 解压文件（解压到当前目录）。
```bash
tar xvf node-v6.9.5-linux-x64.tar.xz
```
3. 创建软链接，使node和npm命令全局有效。
```bash
ln -s /root/node-v6.9.5-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v6.9.5-linux-x64/bin/npm /usr/local/bin/npm
```
4. 查看node、npm版本。
```bash
node -v
npm -v
```
<!-- more-->

---
nvm管理Node版本
---

- 列出Node.js的所有版本。
```bash
nvm list-remote
```
- 运行 nvm ls 查看已安装Node.js版本，当前使用的版本为v6.9.5。返回结果如下所示。
```bash
 nvm ls
 ```
- NVM的更多操作请参考帮助文档：
```bash
nvm help
```
升级 Node 版本
---
```bash
sudo npm cache clean -f
sudo npm install -g n
npm view node versions
sudo n 10.14.2
```
或者

```bash
sudo npm cache clean -f
sudo npm install -g n
npm view node versions
sudo n stable
```

重新安装依赖
---
```bash
rm -rf node_modules
rm package-lock.json
npm cache clear --force
npm install
```
npm 镜像
---
```bash
npm config set registry https://registry.npm.taobao.org
```
[淘宝 npm 官网](http://npm.taobao.org/)