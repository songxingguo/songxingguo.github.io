---
title: "Git指令速查\U0001F680"
urlname: bw0kwn12a6af5yg9
date: '2023-04-29 09:14:52 +0000'
tags: []
categories: []
---

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FojivBC2iy6WcdLcl88n9CGxYQje.jpeg)

> 文中 Git 相关术语都来源于中文版 Sourcetree。

<!--more-->

# Git 指令

## 全局指令

| **指令**                                                                                                                     | **描述**                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **版本信息**                                                                                                                 |                                                                                      |
| git --version                                                                                                                | git 版本                                                                             |
| **指令帮助**                                                                                                                 |                                                                                      |
| git [指令] -h                                                                                                                | 查看指令帮助信息                                                                     |
| git [指令] --help                                                                                                            | 打开指令详细帮助页面                                                                 |
| **查看日志：**                                                                                                               |                                                                                      |
| git log -n20                                                                                                                 | 查看日志(最近 20 条)，可省略 n 为-20；参数--graph 可视化显示分支关系                 |
| git log --follow [file]                                                                                                      | 显示某个文件的版本历史，包括文件改名                                                 |
| git log -n20 --graph                                                                                                         | 参数“--graph”可视化显示分支关系                                                      |
| git log --follow [file]                                                                                                      | 显示某个文件的版本历史                                                               |
| git reflog                                                                                                                   | 查看所有可用的历史版本记录（实际是 HEAD 变更记录），包含被回退的记录，常用来撤销回退 |
| git blame [file]                                                                                                             | 以列表形式查看指定文件的历史修改记录                                                 |
| **格式化日志**                                                                                                               |                                                                                      |
| git log -n20 --oneline                                                                                                       | 参数“--oneline”可以让日志输出更简洁（一行）                                          |
| git log --pretty=oneline                                                                                                     | 一行显示提交信息                                                                     |
| ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FuWCRNpZRL3DthGQmtXZ2PmEXWhC.png) |
| git log --pretty=format:"%h %s" --graph                                                                                      | 自定义输出格式                                                                       |
| ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fi3loaRpO70Ko7Lj8TKZX9StjUbr.png) |
| **筛选日志**                                                                                                                 |                                                                                      |
| git log --pretty=format:"%s" --since="2019-07-17 07:36pm" --until="2019-07-29 08:28pm"                                       | 查询指定时间区间的提交                                                               |
| git log --pretty=format:"%s" --since="2019-07-17 07:36pm" --until="2019-07-29 08:28pm" --grep="feat"                         | 查询包含指定字段的提交                                                               |
| **导出日志**                                                                                                                 |                                                                                      |
| git log > log.txt                                                                                                            | 导出 Git log 日志                                                                    |
| git log --pretty=format:"%s" --graph --since="2019-10-14 02 18 pm" --grep="feat" > log.txt                                   | 同上，日志格式化后导出日志                                                           |
| **创建仓库**                                                                                                                 |                                                                                      |
| git init [文件目录]                                                                                                          | 初始化创建 Git 仓库，如果不指定[文件目录]，则在当前目录创建                          |
| **Git 配置**                                                                                                                 |                                                                                      |
| git config --list                                                                                                            | 查看配置信息，包括系统（--system）+全局（--global）+项目（--local）配置              |
| git config --list --system                                                                                                   | 查看系统配置，全局（--global）、项目（--local）配置配置类似                          |
| git config --global user.name "名称"                                                                                         | 配置用户名                                                                           |
| git config --global user.email "邮箱"                                                                                        | 配置邮箱                                                                             |
| **查看文件**                                                                                                                 |                                                                                      |
| cat [file]                                                                                                                   | 读取一个文件，展示其文件内容                                                         |

## 本地操作

本地操作![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fi1V3kEfywqqGsGVyTMiHF2CCh2D.png)

### 指令-代码

| **指令**                | **描述**                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **状态**                |                                                                                        |
| git status              | 查看本地仓库状态，加参数-s 简洁模式                                                    |
| **添加到/移除暂存区**   |                                                                                        |
| git add [file1] [file2] | 添加文件到暂存区，包括修改的文件、新增的文件                                           |
| git add [dir]           | 同上，添加目录到暂存区，包括子目录                                                     |
| git add .               | 同上，添加所有修改、新增文件（未跟踪）到暂存区                                         |
| git rm [file]           | 删除工作区文件，并且将这次删除放入暂存区                                               |
| **提交到工作区**        |                                                                                        |
| git commit -m "说明"    | '说明' 提交变更，参数-m 设置提交的描述信息，应该正确提交，不带该参数会进入说明编辑模式 |
| git commit -a           | 参数-a，表示直接从工作区提交到版本库，略过了 git add 步骤，不包括新增的文件            |
| git commit [file]       | 提交暂存区的指定文件到仓库区                                                           |
| git commit --amend -m   | 使用一次新的 commit，替代上一次提交，会修改 commit 的 hash 值（id）                    |

### 指令-提交

| **指令**                  | **描述**                                                                                       |
| ------------------------- | ---------------------------------------------------------------------------------------------- |
| **拣选提交**              |                                                                                                |
| git cherry-pick [commit]  | 拣选提交，复制一个特定的提交到当前分支，而不管这个提交在哪个分支                               |
| **整理提交**              |                                                                                                |
| git rebase master         | 将当前分支变基合并到 master 分支， 注意 ⚠️ 只在从未推送至共用仓库的提交上执行变基命令。        |
| **重置提交**              |                                                                                                |
| git reset                 | 撤销暂存区状态，同 git reset HEAD，不影响工作区，默认使用混合重置(mixed)模式                   |
| git reset HEAD [file]     | 同上，指定文件 file，HEAD 可省略                                                               |
| git reset [commit]        | 回退到指定版本，清空暂存区，不影响工作区。工作区需要手动 git checkout 签出                     |
| git reset --soft [commit] | 移动分支 master、HEAD 到指定的版本，不影响暂存区、工作区，需手动 git checkout 签出更新         |
| git reset --hard HEAD     | 撤销工作区、暂存区的修改，用当前最新版                                                         |
| git reset --hard HEAD~    | 回退到上一个版本，并重置工作区、暂存区内容                                                     |
| git reset --hard [commit] | 回退到指定版本，并重置工作区、暂存区内容，举例：`git reset --hard 7678a31`                     |
| **撤销提交**              |                                                                                                |
| git revert [commit]       | 撤销一个提交，会用一个新的提交（原提交的逆向操作）来完成撤销操作，如果已 push 则重新 push 即可 |

### 指令-储藏

| **指令**                 | **描述**                                                                                |
| ------------------------ | --------------------------------------------------------------------------------------- |
| **查看暂存**             |                                                                                         |
| git stash list           | 查看所有被隐藏的内容列表                                                                |
| 储藏**代码**             |                                                                                         |
| git stash save "message" | 同 git stash，可以备注说明 message git stash apply 恢复被隐藏的文件，但是隐藏记录不删除 |
| git stash                | 把未提交内容隐藏起来，包括未暂存、已暂存。 等以后恢复现场后继续工作                     |
| **恢复代码**             |                                                                                         |
| git stash pop            | 恢复被隐藏的内容，同时删除隐藏记录                                                      |
| **删除**储藏             |                                                                                         |
| git stash drop           | 删除隐藏记录                                                                            |

## 远程交互

远程交互![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FtfIh_8MpJOKVw4wChaxrxqruY-m.png)

### 指令-仓库

| **指令**                      | **描述**                                                          |
| ----------------------------- | ----------------------------------------------------------------- |
| **查看/新建/修改/删除仓库**   |                                                                   |
| git remote -v                 | 查看所有远程仓库，不带参数-v 只显示名称                           |
| git remote show [remote]      | 显示某个远程仓库的信息                                            |
| git remote add [name] [url]   | 增加一个新的远程仓库，并命名                                      |
| git remote rename [old] [new] | 修改远程仓库名称                                                  |
| git remote rm [remote-name]   | 删除远程仓库                                                      |
| **克隆仓库**                  |                                                                   |
| git clone [git 地址]          | 从远程仓库克隆到本地（当前目录）                                  |
| **拉取仓库**                  |                                                                   |
| git pull [remote] [branch]    | 取回远程仓库的变化，并与本地版本合并                              |
| git pull                      | 同上，针对当前分支                                                |
| git pull --rebase             | 使用 rebase 的模式进行合并                                        |
| git fetch [remote]            | 获取远程仓库的所有变动到本地仓库，不会自动合并！需要手动合并      |
| **推送仓库**                  |                                                                   |
| git push                      | 推送当前分支到远程仓库                                            |
| git push [remote] [branch]    | 推送本地当前分支到远程仓库的指定分支                              |
| git push [remote] --force/-f  | 强行推送当前分支到远程仓库，即使有冲突，⚠️ 很危险！               |
| git push [remote] --all       | 推送所有分支到远程仓库                                            |
| git push –u                   | 参数–u 表示与远程分支建立关联，第一次执行的时候用，后面就不需要了 |

### 指令-分支

| **指令**                                                                                    | **描述**                                                                                         |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------------- | ------------------------------- | ---------------- |
| **分支查询**                                                                                |                                                                                                  |
| git branch                                                                                  | 列出所有本地分支，加参数-v 显示详细列表，下同                                                    |
| git branch -r                                                                               | 列出所有远程分支                                                                                 |
| git branch -a                                                                               | 列出所有本地分支和远程分支，用不同颜色区分                                                       |
| **分支新建**                                                                                |                                                                                                  |
| git branch [branch-name]                                                                    | 新建一个分支，但依然停留在当前分支                                                               |
| git branch [branch] [commit]                                                                | 新建一个分支，指向指定 commit id                                                                 |
| git branch --track [branch] [remote-branch]                                                 | 新建一个分支，与指定的远程分支建立关联                                                           |
| **分支切换**                                                                                |                                                                                                  |
| git checkout .                                                                              | 撤销工作区的（未暂存）修改，把暂存区恢复到工作区。不影响暂存区，如果没暂存，则撤销所有工作区修改 |
| git checkout [file]                                                                         | 同上，file 指定文件                                                                              |
| git checkout HEAD .                                                                         | 撤销工作区、暂存区的修改，用 HEAD 指向的当前分支最新版本替换工作区、暂存区                       |
| git checkout HEAD [file]                                                                    | 同上，file 指定文件                                                                              |
| git checkout -b dev                                                                         | 从当前分支创建并切换到 dev 分支                                                                  |
| git checkout -b feature1 dev                                                                | 从本地 dev 分支代码创建一个 feature1 分支，并切换到新分支                                        |
| git checkout -b hotfix remote hotfix                                                        | 从远端 remote 的 hotfix 分支创建本地 hotfix 分支                                                 |
| **分支切换：**✅switch：新的分支切换指令，切换功能和 checkout 一样，switch 只单纯的用于切换 |                                                                                                  |
| git switch master                                                                           | 切换到已有的 master 分支                                                                         |
| git switch -c dev                                                                           | 创建并切换到新的 dev 分支                                                                        |
| **分支合并**                                                                                |                                                                                                  |
| git merge [branch]                                                                          | 合并指定分支到当前分支                                                                           |
| git merge --no-ff dev                                                                       | 合并 dev 分支到当前分支，参数--no-ff 禁用快速合并模式                                            |
| **分支删除**                                                                                |                                                                                                  |
| git branch -d dev                                                                           | 删除 dev 分支，-D（大写）强制删除                                                                |
| git push origin --delete [branch-name]                                                      | 删除远程分支                                                                                     |
| git branch -r                                                                               | grep 'origin/dependabot'                                                                         | sed 's/origin\\///g' | xargs -I {} git push origin :{} | 批量删除远端分支 |
| **跟踪分支**                                                                                |                                                                                                  |
| git branch --set-upstream [branch] [remote-branch]                                          | 在现有分支与指定的远程分支之间建立跟踪关联：                                                     |

`git branch --set-upstream hotfix remote/hotfix git checkout [branch-name] `
切换到指定分支，并更新工作区 |

### 指令-标签

| **指令**                      | **描述**                                           |
| ----------------------------- | -------------------------------------------------- |
| **查看标签**                  |                                                    |
| git tag                       | 查看标签列表                                       |
| git tag -l 'a\*'              | 查看名称是“a”开头的标签列表，带查询参数            |
| git show [tagname]            | 查看标签信息                                       |
| **创建标签**                  |                                                    |
| git tag [tagname]             | 创建一个标签，默认标签是打在最新提交的 commit 上的 |
| git tag [tagname] <commit id> | 新建一个 tag 在指定 commit 上                      |
| git tag -a v5.1 -m'v5.1 版本' | 创建标签 v5.1.1039，-a 指定标签名，-m 指定说明文字 |
| **切换标签**                  |                                                    |
| git checkout v5.1.1039        | 切换标签，同切换分支                               |
| **推送标签**                  |                                                    |
| git push [remote] v5.1        | 推送标签，标签不会默认随代码推送推送到服务端       |
| git push [remote] --tags      | 提交所有 tag                                       |
| **删除标签**                  |                                                    |
| git tag -d [tagname]          | 删除本地标签                                       |

## 高级操作

### 指令-bisect

> git bisect 是一个很有用的命令，执行一个二分搜索，用来查找哪一次代码提交引入了错误。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FlyEWYZF1R8tq_oF5hPeYqRn9Mxn.png)

| **指令**                       | **描述**                                       |
| ------------------------------ | ---------------------------------------------- |
| git bisect start [终点] [起点] | 启动查错，举例：`git bisect start HEAD 4d83cf` |
| git bisect good                | 标识本次提交没有问题                           |
| git bisect bad                 | 标识本次提交有问题                             |
| git bisect reset               | 退出查错，回到最近一次的代码提交               |

### 指令-diff

| **指令**                 | **描述**                                                                                                                   |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| git diff                 | 查看暂存区和工作区的差异，举例`git diff 595d0dc11b6b34668b1620de2c29a313b024092c 1855846d6b49c2f4e3d7a05e4b9d76b9d435b4b9` |
| git diff [file]          | 同上，指定文件                                                                                                             |
| git diff --cached        | 查看已暂存的改动，就是暂存区与新版本 HEAD 进行比较                                                                         |
| git diff --staged        | 同上                                                                                                                       |
| git diff --cached [file] | 同上，指定文件                                                                                                             |
| git diff HEAD            | 查看已暂存的+未暂存的所有改动，就是与最新版本 HEAD 进行比较                                                                |
| git diff HEAD~           | 同上，与上一个版本比较。HEAD~表示上一个版本，HEAD~10 为最近第 10 个版本                                                    |
| git diff [id] [id]       | 查看两次提交之间的差异                                                                                                     |
| git diff [branch]        | 查看工作区和分支直接的差异                                                                                                 |

### 指令-submodule

> 子模块是链接；子树是复制的。

[使用子模块和子树来管理 Git 项目](https://zhuanlan.zhihu.com/p/143100657)

```bash
# 逆初始化模块，其中{MOD_NAME}为模块目录，执行后可发现模块目录被清空
git submodule deinit {MOD_NAME}
# 删除.gitmodules中记录的模块信息（--cached选项清除.git/modules中的缓存）
git rm --cached {MOD_NAME}
# 提交更改到代码库，可观察到'.gitmodules'内容发生变更
git commit -am "Remove a submodule."
```

```git
git submodule deinit -f path/to/submodule
rm -rf .git/modules/path/to/submodule
git rm -f path/to/submodule
```

[Git 删除子模块和远程分支](https://murphypei.github.io/blog/2018/09/git-delete-submodule)

# 最佳实践

## 使用原则

- 多建分支，没有什么是新建一个分支无法解决的。
- 多提交代码，方便后续如果有问题可以进行快速回滚。
- 善于使用变基命令整理没有推送的提交，让提交记录更加整洁。注意 ⚠️**只在从未推送至共用仓库的提交上执行变基命令**。

## 工作流

Git Flow![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fn2kcc6kKLjHaEHQ0Ge_YooUUnov.png)

# Git 练习场

- ，Git 在线练习。
- ，Git 学习网站，通过示例仓库，提供一系列 Git 的小练习，帮助用户掌握这个版本管理工具。

---

# 参考资料

[Git 入门图文教程(1.5W 字 40 图)🔥](https://www.yuque.com/kanding/ktech/ccgylqhnb94ug4bu?view=doc_embed)
[Git 常用指令集合 🔥🔥](https://www.yuque.com/kanding/ktech/ai3d3ky8f0dgixto?view=doc_embed)

[📖ProGit-Git 教程](https://www.yuque.com/kanding/ktech/ng8w19?view=doc_embed)

[git 使用学习总结](https://www.yuque.com/xavior.wx/blog/git-review?view=doc_embed&inner=b87ebc48)

---

最后，重学前端，将思考固定下来。

- 笔记和记忆力是提高开发效率的最好方法。
- 如果没有对旧事物进行大量练习，你不太可能发现新事物。
- 努力学习最感兴趣的东西。
  > ©️ 版权申明：版权所有@宋玉，本文内容仅供学习，欢迎指正、交流，转载请注明出处！[原文地址-语雀](https://www.yuque.com/songxingguo/devhints/bw0kwn12a6af5yg9)
