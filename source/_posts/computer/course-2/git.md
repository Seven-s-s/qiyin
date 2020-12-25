---
title: Git
date: 2020/12/17 12:36
categories:
	- [计算机, 工具]
tags:
	- git
---



# 创建仓库

## 初始化仓库

使用当前目录作为仓库，进行初始化

> **git init**

执行完之后，当前目录会生成一个.git目录

## 拷贝

从git仓库拷贝项目

> **git clone <repo>**

克隆到指定的目录

> **git clone <repo> <directory>**

**repo：Git仓库**

**directory：本地目录**

## 配置

### 语法

> **git config**

### 显示当前的 git 配置信息

>**git config --list**
>
>**credential.helper=osxkeychain**
>**core.repositoryformatversion=0**
>**core.filemode=true**
>**core.bare=false**
>**core.logallrefupdates=true**
>**core.ignorecase=true**
>**core.precomposeunicode=true**

### 编辑 git 配置文件

> **git config -e    # 针对当前仓库** 
>
> **git config -e --global   # 针对系统上所有仓库**

### 设置提交代码时的用户信息

>**git config --global user.name "用户名"**
>**git config --global user.email  邮箱**

**如果去掉 --global 参数只对当前仓库有效**

# 基本指令

## 添加文件

### 添加一个或多个文件到暂存区

> **git add [file1] [file2] ...**

### 添加指定目录到暂存区，包括子目录

> **git add [dir]**

### 添加当前目录下的所有文件到暂存区

> **git add ***

## 查看添加文件状态

**查看在你上次提交之后是否有对文件进行再次修改。**

> **git status**

**通常使用 -s 参数来获得简短的输出结果**

> **git status -s**

## 提交文件

### 提交暂存区到本地仓库中

> **git commit -m "提交信息"**

### 提交暂存区的指定文件到仓库区

> **git commit [file1] [file2] ... -m "提交信息"**

## 删除文件

### 将文件从暂存区和工作区中删除

> **git rm <file>**

### 强制删除

**如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f，强行从暂存区和工作区中删除修改后的文件**

> **git rm -f <file>**

**如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录，换句话说，仅是从跟踪清单中删除**

> **git rm --cached <file>**

### 递归删除

**如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件**

>**git rm -r ***

## 移动文件

用于移动或重命名一个文件、目录或软连接。

> **git mv [file] [newfile]**

如果新但文件名已经存在，但还是要重命名它

> **git mv -f [file] [newfile]**

## 远程操作

### 下载远程代码并合并

> **git pull**

### 上传远程代码并合并

> **git push**

## 解决冲突

> **shift+!	输入:wq保存**

# 配置密钥

## 创建新的ssh key

> 输入 ssh-keygen -t rsa -C "youremail@youremail.com" 
>
> 执行这条命令会如上图提示文件保存路径，可以直接按Enter，
>
> 然后提示输入 passphrase（密码），输入两次（可以不输直接两次Enter），
>
> 然后会在 .ssh 目录生产两个文件：id_rsa和id_rsa.pub
>
> 用记事本打开.ssh目录下的id_rsa.pub文件，复制里面的内容，或者直接执行命令查看

## 查看密钥

> $ cat ~/.ssh/id_rsa.pub

或者之间点开文件查看

## 复制ssh key到github

> On the GitHub site Click “Settings” 
>
> Click “SSH and GPG Keys” 
>
> Click “New SSH key”

## 测试 ssh 链接 github

> 输入 ssh -T git@github.com
>
> 出现Successfully就表示可以了
