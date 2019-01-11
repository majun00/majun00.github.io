---
title: Git-实用手册
tags: 
- Git
categories:
- Git
---

### git 开发流程
1. 配置个人信息
`$ git config --global user.name "用户名"`
`$ git config --global user.email "邮箱"`

2. 配置ssh
`$ cd ~/.ssh` `$ ls` 检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件
`$ ssh-keygen` 创建一个 SSH key (`$ ssh-keygen -t rsa -C "your_email@example.com" `)
`$ cat ~/.ssh/id_rsa.pub` 查看私钥

3. 初始化项目
`$ git clone 地址` (或) `$ git init`
`$ git add .`
`$ git commit -m ''` (`$ git commit -a ' '`) (`$ git commit -am ' '`)
`$ git remote add origin 地址`
`$ git push origin -u master` 关联分支
`$ git pull` `$ git push`  直接调用

### git 实用指令
`$ git status` 查看当前状态 (`$ git status -s`) (`$ git diff`) (`$ git diff --cached`)
`$ git log --oneline` 查看日志 (`$ git log`) (`$ git log --oneline --graph`) (`$ git log --reverse --oneline`) (`$ git log --decorate`) (`$ git log --oneline --decorate --graph`)
`$ git reflog` 查看每一次切换版本的记录
`$ git reset --hard Head~0` (`$ git reset --hard 版本号`) 切换版本

`$ git branch` (`$ git branch -a`) 查看分支  
`$ git branch dev` 创建分支
`$ git branch -d dev` 删除分支
`$ git checkout dev` 切换分支
`$ git merge dev` 合并当前分支与dev分支

`$ git config --list` --system应用于该系统 --global应用于该用户
`$ touch .gitignore` 创建忽略文件
`$ git remote rm [别名] ` 删除别名
`$ git rm 文件` 删除文件 (`$ git rm --cached 文件`)

### git 其他命令
1. 当我们修改了很多文件，而不想每一个都add，想commit自动来提交本地修改，我们可以使用-a标识。git commit 命令的-a选项可将所有被修改或者已删除的且已经被git管理的文档提交到仓库中。千万注意，-a不会造成新文件被提交，只能修改。
`$ git commit -a -m "Changed some files"`

2. 如何你想从资源库中删除文件，我们使用rm。
`$ git rm file`

3. 创建一个叫做“feature_x”的分支, 并切换过去：`$ git checkout -b feature_x`, 使用如下命令预览差异：
`$ git diff <source_branch> <target_branch>`

4. 可以执行如下命令创建一个叫做 1.0.0 的标签：
`$ git tag 1.0.0 1b2e1d63ff`
1b2e1d63ff 是你想要标记的提交 ID 的前 10 位字符。

5. 可以使用如下命令替换掉本地改动：
`$ git checkout -- <filename>`
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

6. 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`$ git fetch origin`
`$ git reset --hard origin/master`

7. 内建的图形化 git：gitk
彩色的 git 输出：
`$ git config color.ui true`
显示历史记录时，每个提交的信息只显示一行：
`$ git config format.pretty oneline`
