---
title: Linux
date: 2020/10/1 12:47
tags:
	- Linux
---




# 常用命令

## 查看当前下的路径

> pwd

## 列出所有文件

ls

>**-l	以列表的样式列出所有文件**
>
>**-a	显示隐藏文件**
>
>**-l -h或者-lh	显示文件大小**
>
>***.txt	查找以txt结尾的文件**
>
>**a.*	查找以a开头的文件**

​	*	0个或任意多个字符

​	？	一个字符

​	[abcd]	abcd中的一个字符

​	[a-z]	a到z中的一个字符

## 进入文件

cd

> ​	**..	返回上一级**
>
> ​	**-	在两个目录来回切换**
>
> ​	**~	返回根目录**

## 自动补全

tab

## 上下键

获取上次的命令，获取下次的命令

## 清屏

> **clear**

## 查看IP地址

> **ip addr**

## 端口号

### 查询被占用的端口号

> **ps aux**

### 查询某个应用占用的端口号

> **ps -ef | grep 应用名**

# 文件操作

## 创建文件

**touch**

> **文件名	创建文件**	
>
> **.文件名	创建隐藏文件**

## 创建文件夹

**mkdir**

## 删除

**rm**

> **-d或-r 文件夹名	删除文件夹**

## 移动

**mv**

> **mv a b	将a文件移动到b文件夹下**
>
> **mv a ./b	将a文件移动到当前目录并改名**

## **复制**

**cp**

>**cp a b	将a文件复制到b文件夹下**

## 编辑

**vi**

> **i	进行编辑**
>
> **q	推出程序**
>
> **w	保存文件**
>
> **按下ESC，输入:wq保存**

## 查看文件

tail/cat

## 设置为可执行文件

以ch为后缀的文件

> chmod +x 文件

# 管理员操作

> **su	切换到超级用户**
>
> **reboot	重启**
>
> **halt	关机**

## 修改密码

> passwd

# 下载命令

## 下载vim编辑器

> **yum -y install vim***

## 下载jdk1.8

> **yum install java-1.8.0-openjdk* -y**

## 下载Git

>**yum install git**

# 虚拟机

## 共享文件

创建一个共享文件夹，该文件在虚拟机上mnt/hgfs文件夹下

如果/mnt目录下有hgfs，但hgfs下没有共享文件，输入**vmware-hgfsclient**时却能看到共享文件夹名称，那下载工具

> **yum install -y open-vm-tools-devel**

然后执行以下命令就ok了

> **vmhgfs-fuse .host:/ /mnt/hgfs**

# 云服务器

## 持久运行springboot项目

命令

> nohup java -jar xxx.jar > system.log 2>&1 &

nohub一般形式为如下:

> nohub command &
>
> 但是当你退出账户时，仍然会停止对应的进程。
>
> 所以这就需要你在后面添加 2>&1 &(相当于正常退出，仍保持命令在后台运行)
>
> 上面这个command正好对上java -jar blog.jar > system.log
>
> “>” 输出重定向，通常用于输出日志

ps -a可以查看Java程序运行的进程号，使用kill -9 进程号可以杀死进程。
