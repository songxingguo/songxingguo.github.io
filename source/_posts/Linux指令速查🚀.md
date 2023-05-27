---
title: "Linux指令速查\U0001F680"
urlname: vtheqvmkw27cx4k8
date: '2023-05-27 05:51:14 +0000'
tags: []
categories: []
---

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fi3TCMSeF32kRYmI8qEfGc16lXNU.jpeg)

# 指令速查

## 指令-帮助

| **指令** | **描述**               |
| -------- | ---------------------- |
| `help`   |                        |
| `man`    | 查看帮助命令           |
| `man ls` | 查看 ls 命令的帮助文档 |

## 指令-目录

| **指令** | **描述**                               | **选项** |
| -------- | -------------------------------------- | -------- |
| `ls`     | 列出目录及文件名，英文全拼：list files |

- ls -d ：只列出目录本身
- ls -a 列出目录所有文件，包含以.开始的隐藏文件
- ls -A 列出除.及..的其它文件
- ls -r 反序排列
- ls -t 以文件修改时间排序
- ls -S 以文件大小排序
- ls -h 以易读大小显示
- ls -l 除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来
  |
  | `ls | sed "s:^:`pwd`/:"` | 列出文件绝对路径（不包含隐藏文件） | |
  | `ls -l t*` | 列出当前目录中所有以"t"开头的目录的详细内容 | |
  | `ls -lrS` | 按大小反序显示文件详细信息 | |
  | `ls -lhrt` | 按易读方式按时间反序排序，并显示文件详细信息 | |
  | `cd` [相对路径或绝对路径] | 切换目录，英文全拼：change directory |
- **绝对路径**：
  路径的写法，由根目录 / 写起，例如： /usr/share/doc 这个目录。
- **相对路径**：
  路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成： cd ../man 这就是相对路径的写法。
- **~** 也表示为 home 目录 的意思， **.** 则是表示目前所在的目录， **..** 则表示目前目录位置的上一层目录。
  |
  | `cd /` | 进入要目录 | |
  | `cd ~` | 进入 "home" 目录 | |
  | `cd -` | 进入上一次工作路径 | |
  | `cd !$` | 把上个命令的参数作为 cd 参数使用 | |
  | `pwd` | 显示目前的目录，英文全拼：print work directory |
- -P ：显示出确实的路径，而非使用链接 (link) 路径。
  |
  | `pwd -P` | 查看软链接的实际路径 | |
  | `mkdir` [-mp] 目录名称 | 创建一个新的目录，英文全拼：make directory |
- -m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～
- -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！
  |
  | `mkdir t` | 当前工作目录下创建名为 t 的文件夹 | |
  | `mkdir -p /tmp/test/t1/t` | 在 tmp 目录下创建路径为 test/t1/t 的目录 | |
  | `rmdir` [-p] 目录名称 | 删除一个空的目录，英文全拼：remove directory |
- -p ：从该目录起，一次删除多级空目录
  |
  | `rmdir -p parent/child/child11` | 当 parent 子目录被删除后使它也成为空目录的话，则顺便一并删除 | |
  | `cp` | 复制文件或目录，英文全拼：copy file |
- -a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
- -d：若来源档为链接档的属性(link file)，则复制链接档属性而非文件本身；
- -f：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
- -i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
- -l：进行硬式链接(hard link)的链接档创建，而非复制文件本身；
- -p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
- -r：递归持续复制，用於目录的复制行为；(常用)
- -s：复制成为符号链接档 (symbolic link)，亦即『捷径』文件；
- -u：若 destination 比 source 旧才升级 destination ！
  |
  | `cp -ai a.txt test` | 复制 a.txt 到 test 目录下，保持原文件时间，如果原文件存在提示是否覆盖。 | |
  | `cp -s a.txt link_a.txt` | 为 a.txt 建立一个链接（快捷方式） | |
  | `rm [选项] 文件…` | 删除文件或目录，英文全拼：remove |
- -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
- -i ：互动模式，在删除前会询问使用者是否动作
- -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！
  |
  | `rm -i *.log` | 删除任何 .log 文件，删除前逐一询问确认 | |
  | `rm -rf test` | 删除 test 子目录及子目录中所有档案删除，并且不用一一确认 | |
  | `rm -- -f*` | 删除以 -f 开头的文件 | |
  | `mv` | 移动文件与目录，或修改文件与目录的名称，英文全拼：move file |
- -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
- -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
- -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
  |
  | `mv test.log test1.txt` | 将文件 test.log 重命名为 test1.txt | |
  | `mv llog1.txt log2.txt log3.txt /test3` | 将文件 log1.txt,log2.txt,log3.txt 移动到根的 test3 目录中 | |
  | `mv -i log1.txt log2.txt` | 将文件 file1 改名为 file2，如果 file2 已经存在，则询问是否覆盖 | |
  | `mv * ../` | 移动当前文件夹下的所有文件到上一级目录 | |

## 指令-文件

| **指令**                                                                                         | **描述** | **选项** |
| ------------------------------------------------------------------------------------------------ | -------- | -------- |
| **文件创建&修改**                                                                                |          |          |
| `touch` [-acfm][-d<日期时间>][-r<参考文件或目录>] [-t<日期时间>][--help][--version][文件或目录…] |

- 用于修改文件或者目录的时间属性，包括存取时间和更改时间。
- 若文件不存在，系统会建立一个新的文件。
  |
- a 改变档案的读取时间记录。
- m 改变档案的修改时间记录。
- c 假如目的档案不存在，不会建立新的档案。与 --no-create 的效果一样。
- f 不使用，是为了与其他 unix 系统的相容性而保留。
- r 使用参考档的时间记录，与 --file 的效果一样。
- d 设定时间与日期，可以使用各种不同的格式。
- t 设定档案的时间记录，格式与 date 指令相同。
- --no-create 不会建立新档案。
- --help 列出指令格式。
- --version 列出版本讯息。
  |
  | **文件编辑** | | |
  | `vim` [filename] | 启动 vi/vim，进行编辑。具体查看：[Vim 编辑器](#Ow1Ed)。 | |
  | **文件查看** | | |
  | `cat` [-AbEnTv] | 由第一行开始显示文件内容 |
- -A ：相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
- -b ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
- -E ：将结尾的断行字节 $ 显示出来；
- -n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
- -T ：将 [tab] 按键以 ^I 显示出来；
- -v ：列出一些看不出来的特殊字符
  |
  | `cat file1 file2 > file` | 将几个文件合并为一个文件 | |
  | `cat > filename` | 从键盘创建一个文件，只能创建新文件，不能编辑已有文。 | |
  | `cat filename` | 一次显示整个文件 | |
  | `tac` | 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！ | |
  | `nl` [-bnw] 文件 | 显示的时候，顺道输出行号！ |
- -b ：指定行号指定的方式，主要有两种：
  -b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
  -b t ：如果有空行，空的那一行不要列出行号(默认值)；
- -n ：列出行号表示的方法，主要有三种：
  -n ln ：行号在荧幕的最左方显示；
  -n rn ：行号在自己栏位的最右方显示，且不加 0 ；
  -n rz ：行号在自己栏位的最右方显示，且加 0 ；
- -w ：行号栏位的占用的位数。
  |
  | `more` | 一页一页的显示文件内容 |
- 空白键 (space)：代表向下翻一页；
- Enter：代表向下翻『一行』；
- /字串：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f ：立刻显示出档名以及目前显示的行数；
- q ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。
  |
  | `more +3 text.txt` | 显示文件中从第 3 行起的内容 | |
  | `ls -l | more -5` | 在所列出文件目录详细信息，借助管道使每次显示 5 行，按空格显示下 5 行。 | |
  | `less` | 与 more 类似，但是比 more 更好的是，他可以往前翻页！ |
- 空白键 ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup] ：向上翻动一页；
- /字串 ：向下搜寻『字串』的功能；
- ?字串 ：向上搜寻『字串』的功能；
- n：重复前一个搜寻 (与 / 或 ? 有关！)
- N：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q：离开 less 这个程序；
  |
  | `ps -aux | less -N` | ps 查看进程信息并通过 less 分页显示 | |
  | `less 1.log 2.log` | 查看多个文件 | |
  | `head` [-n number] 文件 | 只看头几行，默认显示前面 10 行 |
- -n ：后面接数字，代表显示几行的意思
  |
  | `head 1.log -n 20` | 显示 1.log 文件中前 20 行 | |
  | `head -c 20 log2014.log` | 显示 1.log 文件前 20 字节 | |
  | `head -n -10 t.log` | 显示 t.log 最后 10 行 | |
  | `tail` [-n number] 文件 | 只看尾巴几行 |
- -n ：后面接数字，代表显示几行的意思
- -f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c 才会结束 tail 的侦测
  |
  | `ping 127.0.0.1 > ping.log &` | 循环读取逐渐增加的文件内容 | |
  | `tail -f ping.log` | 后台运行：可使用 jobs -l 查看，也可使用 fg 将其移到前台运行。 | |
  | **文件对比** | | |
  | `diff` | 文件对比 | |
  | `diff -w name_list.txt name_list_new.txt` | 比较的时候忽略空白符 | |

## 指令-查找

| **指令** | **描述**                             | **选项** |
| -------- | ------------------------------------ | -------- |
| `find`   | 在文件树中查找文件，并作出相应的处理 |

- -name 按照文件名查找文件
- -perm 按文件权限查找文件
- -user 按文件属主查找文件
- -group 按照文件所属的组来查找文件。
- -type 查找某一类型的文件，诸如：
  - b - 块设备文件
  - d - 目录
  - c - 字符设备文件
  - l - 符号链接文件
  - p - 管道文件
  - f - 普通文件
- -size n :[c] 查找文件长度为 n 块文件，带有 c 时表文件字节大小
- -amin n 查找系统中最后 N 分钟访问的文件
- -atime n 查找系统中最后 n\*24 小时访问的文件
- -cmin n 查找系统中最后 N 分钟被改变文件状态的文件
- -ctime n 查找系统中最后 n\*24 小时被改变文件状态的文件
- -mmin n 查找系统中最后 N 分钟被改变文件数据的文件
- -mtime n 查找系统中最后 n\*24 小时被改变文件数据的文件
- (用减号-来限定更改时间在距今 n 日以内的文件，而用加号+来限定更改时间在距今 n 日以前的文件。 )
- -maxdepth n 最大查找目录深度
- -prune 选项来指出需要忽略的目录。在使用-prune 选项时要当心，因为如果你同时使用了-depth 选项，那么-prune 选项就会被 find 命令忽略
- -newer 如果希望查找更改时间比某个文件新但比另一个文件旧的所有文件，可以使用-newer 选项
  |
  | `find -atime -2` | 查找 48 小时内修改过的文件 | |
  | `find ./ -name '*.log'` | 在当前目录查找 以 .log 结尾的文件。 **.** 代表当前目录 | |
  | `find -size +1000c` | 查找大于 1K 的文件 | |
  | `find . -f -name 'passwd*' -exec grep "pkg" {} \\` | 当前目录下查找文件名以 passwd 开头，内容包含 "pkg" 字符的文件 | |
  | `grep` | 文本搜索命令 |
- -A n --after-context 显示匹配字符后 n 行
- -B n --before-context 显示匹配字符前 n 行
- -C n --context 显示匹配字符前后 n 行
- -c --count 计算符合样式的列数
- -i 忽略大小写
- -l 只列出文件内容符合指定的样式的文件名称
- -f 从文件中读取关键词
- -n 显示匹配内容的所在文件中行数
- -R 递归查找文件夹
  |
  | `ps -ef | grep svn` | 查找指定进程 | |
  | `ps -ef | grep svn -c` | 查找指定进程个数 | |
  | `cat test1.txt | grep -f key.log` | 从文件中读取关键词 | |
  | `which` | 查看可执行文件的位置 | |
  | `which cd` | 查看 cd，显示不存在，因为 cd 是内建命令，而 which 查找显示是 PATH 中的命令。 | |
  | `locate` | 配合数据库查看文件位置 |
- -l num（要显示的行数）
- -f 将特定的档案系统排除在外，如将 proc 排除在外
- -r 使用正则运算式做为寻找条件
  |

## 指令-系统管理

| **指令**     | **描述**             | **选项** |
| ------------ | -------------------- | -------- |
| **磁盘信息** |                      |          |
| `df`         | 显示磁盘空间使用情况 |

- -a 全部文件系统列表
- -h 以方便阅读的方式显示信息
- -i 显示 inode 信息
- -k 区块为 1024 字节
- -l 只显示本地磁盘
- -T 列出文件系统类型
  |
  | `df -l` | 显示磁盘使用情况 | |
  | `df -haT` | 以易读方式列出所有文件系统及其类型 | |
  | `du [选项] [文件]` | 对文件和目录磁盘使用的空间的查看 |
- -a 显示目录中所有文件大小
- -k 以 KB 为单位显示文件大小
- -m 以 MB 为单位显示文件大小
- -g 以 GB 为单位显示文件大小
- -h 以易读方式显示文件大小
- -s 仅显示总计
- -c 或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和
  |
  | `du -h scf/` | 以易读方式显示文件夹内及子文件夹大小 | |
  | `du -ah scf/` | 以易读方式显示文件夹内所有文件大小 | |
  | 进程管理 | | |
  | `ps` | 查看当前运行的进程状态，一次性查看，如果需要动态连续结果使用 top |
- -A 显示所有进程
- a 显示所有进程
- -a 显示同一终端下所有进程
- c 显示进程真实名称
- e 显示环境变量
- f 显示进程间的关系
- r 显示当前终端运行的进程
- -aux 显示所有包含其它使用的进程
  |
  | `ps -ef` | 显示当前所有进程 | |
  | `ps-ef | grep java` | 显示当前所有 java 相关进程 | |
  | `top` | 正在执行的进程的相关信息，包括进程 ID、内存占用率、CPU 占用率等 |
- -c 显示完整的进程命令
- -s 保密模式
- -p <进程号> 指定进程显示
- -n <次数>循环显示次数
  |
  | `top -u oracle` | 显示某个特定用户的进程，可以使用-u 选项 | |
  | `kill` | 杀死进程。 |
- -l 信号，若果不加信号的编号参数，则使用“-l”参数会列出全部的信号名称
- -a 当处理当前进程时，不限制命令名和进程号的对应关系
- -p 指定 kill 命令只打印相关进程的进程号，而不发送任何信号
- -s 指定发送信号
- -u 指定用户
  |
  | `kill -9 $(ps -ef | grep pro1)` | 先使用 ps 查找进程 pro1，然后用 kill 杀掉 | |
  | `kill -s 9 27810` | 杀死进程号为 27810 的进程，强制终止，系统资源无法回收。 | |
  | shutdown | 关机与重启 | |
  | `shutdown -h now` | 立刻关机 | |
  | `shutdown -r -t 60` | 60 秒后重启 | |
  | `shutdown -r now` | 重启(1) | |
  | `reboot` | 重启(2) | |

## 指令-**网络通讯**

| **指令**                | **描述**                           |
| ----------------------- | ---------------------------------- | -------------- |
| `ifconfig`              | 显示网络设备情况                   |
| `netstat`               | 显示网络相关信                     |
| netstat -a              | 列出所有端口                       |
| netstat -tunlp          | grep 端口号                        | 查看进程端口号 |
| `ping`                  |                                    |
| `ping -b 192.168.120.1` |                                    |
| `ping -c 5 gmail.com`   | ping 一个远程主机，只发 5 个数据包 |
| `dig`                   |                                    |

| `curl`
[官方文档](https://curl.se/docs/) | 用来请求 Web 服务器。 |
| [Homebrew](https://brew.sh/index_zh-cn.html) | MacOS 软件包管理器 |
| **软件下载与安装** | |
| `/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)")` | 安装`Homebrew` |
| `brew install wget` | 安装`wget` |
| `wget`
[官方文档](https://www.gnu.org/software/wget/) | wget 是一个使用 HTTP,HTTPS,FTP 和 FTPS 协议来下载文件的免费软件 |
| `wget http://prdownloads.sourceforge.net/sourceforge/nagios/nagios-3.2.1.tar.gz` | 使用 wget 从网上下载软件、音乐、视频 |
| `wget -O taglist.zip http://www.vim.org/scripts/download_script.php?src_id=7701` | 下载文件并以指定的文件名保存文件 |
| `wget -b http://www.baidu.com/index.html` | 使用 wget -b 后台下载 |
| `tail -f wget-log` | 查看下载进度 |
| `ftp` | 连接 ftp 服务器并下载多个文件 |
| `ftp IP/hostname` | |

## 指令-备份压缩

| **指令**                            | **描述**                 |
| ----------------------------------- | ------------------------ |
| `tar`                               | 压缩或解压               |
| `tar cvf archive_name.tar dirname/` | 压缩文件                 |
| `tar xvf archive_name.tar`          | 解压文件                 |
| `tar tvf archive_name.tar`          | 查看 tar 文件            |
| `gzip`                              | 压缩或解压               |
| `gzip test.txt`                     | 创建一个\*.gz 的压缩文件 |
| `gzip -d test.txt.gz`               | 解压\*.gz 文件           |
| `gzip -l *.gz`                      | 显示压缩的比率           |

# 常用软件

| **指令**                                                                                                                                                                                | **描述**                         |     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- | --- |
| **mysql**                                                                                                                                                                               |                                  |     |
| **安装与连接**                                                                                                                                                                          |                                  |     |
| `wget [http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz](http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz)` | 下载 mysql                       |     |
| `tar -zxvf mysql-5.7.11-linux-glibc2.5-x86_64.tar.gz`                                                                                                                                   | 解压 mysql 压缩包                |     |
| `mysql -u root -p -h 192.168.1.2`                                                                                                                                                       | 连接一个远程数据库，需要输入密码 |     |
| `mysql -u root -p`                                                                                                                                                                      | 连接本地数据库                   |     |
| `lsof -i:3306`                                                                                                                                                                          | 查看 Mysql 服务                  |     |
| `service mysql status`                                                                                                                                                                  | 查看数据库状态                   |     |
| **数据库操作**                                                                                                                                                                          |                                  |     |
| `show databases`                                                                                                                                                                        | 查看数据库                       |     |
| `use dbname`                                                                                                                                                                            | 管理数据库                       |     |
| `show tables`                                                                                                                                                                           | 查看数据库表                     |     |
| `desc tablename`                                                                                                                                                                        | 查看表结构                       |     |
| `drop table [if exists] tablename;`                                                                                                                                                     | 删除表                           |     |
| [**ssh**](https://wangdoc.com/ssh/)                                                                                                                                                     | **登录远程服务器**               |     |
| `yum install ssh`                                                                                                                                                                       | 安装 SSH                         |     |
| `service sshd start`                                                                                                                                                                    | 启动 SSH                         |     |
| `service sshd restart`                                                                                                                                                                  | 重启 SSH                         |     |
| `ssh root@127.0.0.1 -p22`                                                                                                                                                               | 登录远程服务器                   |

- -p 后面是端口号
- root 是服务器用户名
- 127.0.0.1 是服务器 ip
  |
  | `$ ssh-keygen -t rsa -C "your_email@example.com"` | 生成密钥 | |
  | `cat ~/.ssh/id_rsa.pub` | 查看公钥 | |
  | `ssh -T git@github.com` | 测试 SSH 是否生效 | |
  | ssh-agent | 密钥管理器 | |
  | ```bash
  ssh-add ~/.ssh/id_rsa_github
  ssh-add ~/.ssh/id_rsa_gitlab

````
 | 使用ssh-add将私钥交给ssh-agent保管，其他程序需要身份验证的时候可以将验证申请交给ssh-agent来完成整个认证过程。 |  |
| **node** |  |  |
| ` wget [https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz](https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz)` | 下载node |  |
| `tar xvf node-v6.9.5-linux-x64.tar.xz` | 解压到当前目录 |  |
| `ln -s /root/node-v6.9.5-linux-x64/bin/node /usr/local/bin/node`
`ln -s /root/node-v6.9.5-linux-x64/bin/npm /usr/local/bin/npm` | 创建软链接，使node和npm命令全局有效 |  |
| **nvm** | **Node版本管理器** |  |
| `nvm ls` | 列出Node.js的所有版本。 |  |
| `nvm use v4.9.1` | 切换node版本 |  |
| **npm** | **Node包管理器** |  |
| ```bash
sudo npm cache clean -f
sudo npm install -g n
npm view node versions
sudo n 10.14.2
````

或```bash
sudo npm cache clean -f
sudo npm install -g n
npm view node versions
sudo n stable

````
 | 升级 Node 版本 |  |
| ```bash
rm -rf node_modules
rm package-lock.json
npm cache clear --force
npm install
````

| 重新安装依赖 | |
| `npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org)` | 配置淘宝镜像 | |

## 文本编辑器 vi/vim

Vim 是从 vi 发展出来的一个文本编辑器。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

| **指令**                | **描述**                                            |
| ----------------------- | --------------------------------------------------- |
| **命令模式**            | 用户刚刚启动 vi/vim，便进入了命令模式。             |
| `i`                     | 切换到输入模式，以输入字符。                        |
| `x `                    | 删除当前光标所在处的字符。                          |
| `:`                     | 切换到底线命令模式，以在最底一行输入命令。          |
| **输入模式**            | 在命令模式下按下 i 就进入了输入模式。               |
| 字符按键以及 Shift 组合 | 输入字符                                            |
| ENTER                   | 回车键，换行                                        |
| BACK SPACE              | 退格键，删除光标前一个字符                          |
| DEL                     | 删除键，删除光标后一个字符                          |
| 方向键                  | 在文本中移动光标                                    |
| HOME/END                | 移动光标到行首/行尾                                 |
| Page Up/Page Down       | 上/下翻页                                           |
| Insert                  | 切换光标为输入/替换模式，光标将变成竖线/下划线      |
| ESC                     | 退出输入模式，切换到命令模式                        |
| **底线命令模式**        | 在命令模式下按下:（英文冒号）就进入了底线命令模式。 |
| `q `                    | 退出程序                                            |
| `w`                     | 保存文件                                            |

[史上最全 Vim 快捷键键位图（入门到进阶） | 菜鸟教程](https://www.runoob.com/w3cnote/all-vim-cheatsheat.html)

# 快捷键

| **快捷键**     | **描述**                     |
| -------------- | ---------------------------- |
| `Ctrl + a `    | 光标到开头                   |
| `Ctrl + c `    | 中断当前程序                 |
| `Ctrl + d `    | 退出当前窗口或当前用户       |
| `Ctrl + e`     | 光标到结尾                   |
| `Ctrl + l `    | 清屏 相当与 clear            |
| `Ctrl + u`     | 剪切、删除（光标以前的）内容 |
| `Ctrl + k`     | 剪切、删除（光标以后的）内容 |
| `Ctrl + r `    | 查找（最近用过的命令）       |
| `tab`          | 所有路径以及补全命令         |
| `Ctrl+shift+c` | 命令行复制内容               |
| `Ctrl+shift+v` | 命令行粘贴内容               |
| `Ctrl + q `    | 取消屏幕锁定                 |
| `Ctrl + s `    | 执行屏幕锁定                 |

# 参考资料

[命令行常用工具的替代品 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2022/01/cli-alternative-tools.html)
[45 个常用 Linux 命令，让你轻松玩转 Linux！ - 掘金](https://juejin.cn/post/6844903930166509581#heading-140)
[Linux 命令大全 | 菜鸟教程](https://www.runoob.com/linux/linux-command-manual.html)
[Linux 常用命令学习 | 菜鸟教程](https://www.runoob.com/w3cnote/linux-common-command-2.html)
[Linux 常用命令详细大全（面试常考）-阿里云开发者社区](https://developer.aliyun.com/article/842453)
[50 个最常用的 Unix/Linux 命令 - GongYong](https://gywbd.github.io/posts/2014/8/50-linux-commands.html)
[Curl 使用指南](https://www.liuxing.io/blog/curl/)
[Homebrew 安装、使用、升级和卸载](https://blog.devhitao.com/2020/01/18/homebrew-usage/)
[报错：Failed to upgrade Homebrew Portable Ruby](https://www.jianshu.com/p/53a7d11a7250)
