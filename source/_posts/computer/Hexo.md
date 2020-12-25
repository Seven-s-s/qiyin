---
title: hexo的使用
sticky: true
date: 2020/12/16
tags: hexo
comment:
	true
---

## 安装Hexo

### 第一步安装Nodejs

1. 下载

   > wget https://nodejs.org/dist/v12.18.1/node-v12.18.1-linux-x64.tar.xz

2. 解压

   > tar xf node-v12.18.1-linux-x64.tar.xz

3. 进入解压目录

   > cd node-v12.18.1-linux-x64 

4. 执行node命令，查看版本

   > ./bin/node -v 

5. 修改文件的名字

   > mv node-v12.18.1-linux-x64 nodejs

6. 映射配置全局node

   > ln -s /node的路径/bin/node /usr/local/bin/
   >
   > ln -s /node的路径/bin/npm /usr/local/bin/

7. 配置环境变量

   > vi /etc/profile
   >
   > 在最后一行添加
   >
   > export NODE_HOME=node的路径
   >
   > export PATH=$PATH:$NODE_HOME/bin 
   >
   > export NODE_PATH=$NODE_HOME/lib/node_modules

8. 执行命令 source /etc/profile 及在当前控制台更新

### 第二部安装Git

> yum install git

### 第三步安装hexo

> ```
> npm install -g hexo-cli
> ```

## 基本操作

新建一个文件，用来写博客的，在该文件下初始化

> hexo init

初始化模板

> hexo generate

运行该模板

> hexo server

## 基于宝塔面板一键部署hexo博客

### 创建git用户

1. 创建git用户

   > adduser git

2. 获取权限

   > chmod 740 /etc/sudoers
   >
   > vim /etc/sudoers

3. 按 `i` 键进入文件的编辑模式，按向下键找到如下字段

   > root    ALL=(ALL)       ALL

4. 在其后面增加一句

   > git     ALL=(ALL)       ALL

5. 按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`:`，然后输入`wq`回车）

   > chmod 400 /etc/sudoers

### 配置密钥

1. 创建密钥

   > 一般存放在`c/用户/.ssh`下。

2. 将`id_rsa.pub`里面的密钥复制,在服务器运行下面命令，创建.ssh文件夹

   > su git
   >
   > mkdir ~/.ssh

3. 创建`.ssh/authorized_keys`文件，打开`authorized_keys`文件并将刚才在本地机器复制的内容拷贝其中并保存

   > vim ~/.ssh/authorized_keys

4. 按`i`进入编辑模式粘贴完按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`:`，然后输入`wq`回车）

   * 修改权限

     >chmod 755 ~ 
     >
     >chmod 700 ~/.ssh
     >
     >chmod 600 ~/.ssh/authorized_keys
   
   * 测试本地连接服务器（在本地电脑git bash here）
   
     > //yourIp为远程服务器的ip地址
     >
     > ssh -v git@yourIp     //yourIp为你的服务器ip
   
   * 如果出现Welcome to xxx则表示连接成功

### 创建git仓库

- 切换到root用户，创建一个目录用于存储网站的根目录

  > su root

- 创建网站的根目录

  > mkdir /home/hexo

- 给予权限

  > chown git:git -R /home/hexo

### 自动化部署

* 获取root权限

  > su root

* 建立git仓库

  > cd /home/git
  >
  > git init --bare blog.git

* 修改blog.git权限

  > chown git:git -R blog.git

* 在 `/home/hexo/blog.git` 下，有一个自动生成的 `hooks` 文件夹，我们创建一个新的 `git` 钩子 `post-receive`，用于自动部署。

  > vim blog.git/hooks/post-receive

* 按 `i` 键进入文件的编辑模式，在该文件中添加两行代码（将下边的代码粘贴进去)，指定 Git 的工作树（源代码）和 Git 目录

  >  *#!/bin/bash*
  >
  >  git --work-tree=/home/hexo --git-dir=/home/git/blog.git checkout -f 

* 按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`：`，然后输入`wq`回车）

* 修改文件权限，使得其可执行

  > chmod +x /home/git/blog.git/hooks/post-receive

### 添加网站

点击添加网站**→**添加站点

> 输入域名+根目录（/home/hexo）

### 配置本地Hexo

* 博客根目录_config下增加

  > deploy:
  >
  > ​	type: git
  >
  > ​    repo: root@***(服务器ip,内网外网都行):/home/git/blog.git    #仓库地址
  >
  > ​    branch: master    #分支

* 部署

  > hexo clean
  >
  > hexo g
  >
  > hexo d

- 输入`hexo d`的时候，如果出现ERROR Deployer not found: git，输入以下命令

  > npm install --save hexo-deployer-git

- 如果出现`bash: git-receive-pack: command not found`

  > sudo ln -s /usr/local/git/bin/git-receive-pack  /usr/bin/git-receive-pack

* 访问服务器ip，看有没有成功