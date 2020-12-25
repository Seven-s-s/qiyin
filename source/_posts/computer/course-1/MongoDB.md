---
title: MongoDB
date: 2020/12/23
categories:
	- [计算机,框架]
tags:
	- MongoDB
---

# 简介

* Mongodb 文档数据库，存储的是文档(Bson>json的二进制化)

> 特点：内部执行引擎为JS解释器，把文档存储成bson结构，在查询时，转化为就、JS对象，并可以通过熟悉的JS语法来操作

* Mongodb和传统数据库相比最大的不同

> 传统型数据库：结构化数据，定好了表结构后，每一行的内容必是符合表结构的，就是说-列的个数，类型都一样
>
> Mongodb文档型数据库：表下的每篇文档都可以有自己独特的结构(json对象都可以有自己独特的属性和值)

* 下载地址

> [**https://www.mongodb.com/download-center/community**](https://www.mongodb.com/download-center/community)

# Linux安装mongodb

1. 下载

> wget  https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel80-4.4.1.tgz 

2. 解压

> tar -zxvf mongodb-linux-x86_64-rhel80-4.4.1.tgz

3. 将解压包拷贝至指定目录

> mv mongodb-linux-x86_64-rhel80-4.4.1 /usr/local/mongodb

4. 创建数据存放目录与日志存放目录

> mkdir -p /usr/local/mongodb/data /usr/local/mongodb/logs

5. 启动mongodb服务

> /usr/local/mongodb/bin/mongod –dbpath=/usr/local/mongodb/data –logpath=/usr/local/mongodb/logs/mongodb.log –logappend –port=27017 --fork

6. 登录

> /usr/local/mongodb/bin/mongo

# 基本操作

## 通用操作

1. 查看数据库

> Show databases

2. 选择数据库

> Use 数据库名
>
> 隐式创建：在mongodb选择不存在的数据库不会报错，后期当该数据库有数据时，系统会自动创建

3. 查看集合

> Show collections

4. 创建集合

> db.createCollection(‘集合名’)

5. 删除集合

> db.集合名.drop()

## C增

> db.集合名.insert(JSON数据)
>
> 集合存在-则直接插入数据，集合不存在-隐式创建，创建时会有默认id，如需自定义，给插入的JSON数据增加_id键即可覆盖

增加多条数据

> (JSON数据)变为([JSON数据，JSON数据]）

快速插入多条数据

> 由于mongodb底层是JS引擎，所以支持部分JS语法
>
>  for(var i = 1; i < n; i++){
>
> ​	db.集合名.insert({name:"a"+i,age:i})
>
> }

## R查

1. 查询全部数据

> Db.集合名.find()

2. 只看name列

> db.集合名.find({},{name:1})

3. 除了name列

> db.集合名.find({},{name:0})
>
> 第一个{}不填表示查询所有

4. 查询固定数据

> db.集合名.find({name:"张三"})

5. 查询年龄大于5岁的数据

> db.集合名.find({age:{$gt:5}})

6. 查询5,8,10岁的数据

> db.集合名.find({age:{$in:[5,8,10]}})

7. 运算符

> $gt：大于
>
> $gte：大于等于
>
> $lt：小于
>
> $lte：小于等于
>
> $ne：不等于
>
> $in：in
>
> $nin：not in

## U改

> Db.集合名.update(条件，新数据，[是否新增，是否修改多条])

* 是否新增

> 指条件匹配不到数据则插入(true是插入，false否不插入默认)

* 是否修改多条

> 指将匹配成功的数据都修改(true是，false否默认)

* 修改张三的数据

> db.集合名.update({name:"zs1"},{$set:{name:"zs2"}})，不加set默认是替换而不是修改

* 将张三的年龄加1

> db.集合名.update({name:"zs1"},{$inc:{age:1}})

* 修改器

> $set：修改列值
>
> $inc：递增
>
> $rename：重命名列
>
> $unset：删除列

## D删

> db.集合名.remove({},[是否删除一条])
>
> ture：删除一条，默认false：全删除

# 拓展操作

1. 格式化数据pretty()

> db.集合名.find().pretty()

2. 排序

> db.集合名.find().sort(JSON数据)
>
> 键-就是要排序的字段的列/字段、值：1升序   -1降序

3. 分页

> db.集合名.find().skip(数字).limit(数字)
>
> skip跳过指定条数(可选)，limit限制查询的条数

4. 总结

> db.集合名.find()
>
> .pretty()格式化数据
>
> .sort({列:1/-1})排序
>
> .skip(数字)跳过指定条数
>
> .limit(数字)限制查询条数
>
> .count()统计总条数

# 聚合查询

```
Db.集合名.aggregate([
    {管道：{表达式}}
    ……
])
```

## 常用管道：

* $group

> 将集合中的文档进行分组，用于统计结果

* $match

> 过滤数据，只要输出符合条件的文档

* $sort

> 聚合数据进一步排序

* $skip

> 跳过指定文档数

* $limit

> 限制集合数据返回文档数

## 常用表达式

* $sum

> 总和  $sum：1同count表示统计

* $avg

> 平均

* $min

> 最小值

* $max

> 最大值

## 例子

1. 统计男生女生的总年龄：

```
db.集合名.aggregate([
    {
        $group:{
            _id:"$sex".
            result:{$sum:"$age"}
    	}
    }
])
```

2. 统计男生女生的总人数

```
db.集合名.aggregate([
    {
        $group:{
            _id:"$sex",
            num:{$sum:1}
        }
    }
])
```

3. 求学生总数和平均年龄

```
db.集合名.aggregate([
    {
        $group:{
            _id:null,
            Total_num:{$sum:1},
            Avg_age:{$avg:"age"}
        }
    }
])
```

3. 查询男生女生人数，按人数排序


```
db.集合名.aggregate([
    {
        $group:{
            _id:"$sex",
            num:{$sum:1}
        }
    },
    {
        $sort:{
            num:1//升序
        }
    }
])
```

#  索引

## 创建索引

> Db.集合名.createIndex(待创建索引的列，[额外选项])
>
> 待创建索引的列：{键：1，…，键：-1}
>
> 说明：1升序-1降序      例：{age：1}表示创建age索引并按照升序的方式存储
>
> 额外选项：设置索引的名称或唯一索引

## 删除索引

* 全部删除

> db.集合名.dropIndexes()

* 删除指定

> db.集合名.dropIndex(索引名)

## 查看索引

> Db.集合名.getIndexes()
>
> 显示出来的key：给哪个列设置了索引
>
> 显示出来的name：表示索引名称，默认系统生成，也可以自定义

## 给索引取名

> Db.集合名.createIndex({“键"：“值"}，{键：“名称"})

## 添加唯一索引

> Db.集合名.createIndex({“键"：“值"}，{unique：“键"})

## 分析索引

> Db.集合名.find().explain(“executionStats")

# 权限机制

## 创建账号

   ```
Use admin
db.createUser({
    "user":"admin",
    "pwd":"admin",
    "roles":[{
        role:"root",
        db:"admin"
    }]
}) 

   ```

## 角色

* 角色种类

> 超级用户角色：root
>
> 数据库用户角色：read、readwrite；
>
> 数据库管理角色：dbAdmin、userAdmin；
>
> 集群管理角色：clusterAdmin、clusterManager、ClusterMonitor、hostManager；
>
> 备份恢复角色：backup、restore；
>
> 所有数据库角色：readAnyDatabase、readWriterAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase；

* 角色说明

> root：只在admin数据库中可用。超级账号、超级权限；
>
> read：允许用户读取指定数据库；
>
> readWriter：允许用户读写指定数据库

## 开启验证模式

### 概念

> 指用户需要输入账号密码才能登录使用

### 操作步骤

1. 添加超级管理员

```sql
Use admin
db.createUser({
    "user":"admin",
    "pwd":"admin",
    "roles":[{
        role:"root",
        db:"admin"
    }]
}) 
```

2. 退出卸载服务

> Linux：mongod --shutdown
>
> Window：Mongod --remove

3. 重新安装需要输入账号密码的服务(注：在原安装命令基础上加上—auth即可)

> Linux：bin目录下
>
> ./mongod --dbpath=/usr/local/mongodb/data --logpath= /usr/local/mongodb/logs/mongodb.log --auth --logappend --port=27017 –fork
>
> Window：bin目录下
>
> mongod --install --dbpath (data路径) --logpath (log路径) –auth

4. 启动服务->登录测试

> Linux：bin目录下./mongo
>
> Windows：net start mongodb再输入mongo启动
>
> 直接启动show dbs看不到任何东西
>
> 方法1：mongo服务ip地址：端口号/数据库 -u 用户名 -p密码
>
> 方法2：先登录，选择数据库，输入db.auth(用户名，密码)

### 例子

> 添加用户shop1可以读shop数据库
>
> 添加用户shop2可以读写shop数据库
>
> 注意：必须在对应数据库创建用户

1. 准备测试数据

```
use shop
for(var i = 1 ;i <= 10;i++){
	db.goods.insert({
        "name":"goodsName"+i,
        "price":i
	})
}
```

2. 查看添加的数据

>db.goods.find()

3. 添加用户并设置权限

```
use shop
db.createUser({
    "user":"shop1",
    "pwd":"shop1",
    "roles":[{
    	role:"read",
    	db:"shop"
    }]
})
db.createUser({
    "user":"shop2",
    "pwd":"shop2",
    "roles":[{
    	role:"readWrite",
    	db:"shop"
    }]
})
```

4. 查看添加的用户

>use admin
>
>db.system.users.find().pretty()

5. 登入shop1账号

> mongo localhost:27017/shop -u shop1 -p shop1

6. 查看数据

> db.goods.find()

7. 尝试插入数据（会报错就对了）

```
db.goods.insert({
	"name":"goodsName11",
	"price":11
})
```

8. 登入shop2账号

> mongo localhost:27017/shop -u shop2 -p shop2

9. 查看数据

> db.goods.find()

10. 尝试插入数据（插入成功）

```
db.goods.insert({
	"name":"goodsName11",
	"price":11
})
```

# 备份还原

> 备份与还原都在bin目录下执行命令

## 备份语法

> mongodump -h -port -u -p -d -o
>
> -h：host 服务器IP地址(一般不写，默认本机)
>
> -port：   端口(一般不写，默认27017)
>
> -u：user  账号
>
> -p：pwd  密码
>
> -d：databse 数据库(数据库不写则导出全部)
>
> -o：open 备份到指定目录下

## 例子

* 备份所有数据

> mongodump -u admin -p admin -o 备份目录

* 备份指定数据

> mongodump -u shop1 -p shop1 -d shop -o 备份目录

## 还原语法

> mongorestore -h -port -u -p -d --drop 备份数据目录
>
> -d 不写则还原全部数据
>
> --drop 先删除数据库再导入

## 例子

* 还原所有数据

> 先删除两个数据库
>
> use test1，db.dropDatabse()，use test2()，db.dropDatabase()
>
> 查看剩下的数据库show dbs
>
> 还原数据mongorestore -u admin -p admin --drop
>
> 备份数据目录，重新登录查看效果

* 还原指定数据

> mongorestore -u shop2 -p shop2 -d shop –drop 备份目录

# Java连接mongodb

1. 查看mongodb占用的端口

> ps -ef | grep mongodb

2. 关掉mongodb

> kill -9 端口

3. 开启mongodb服务

> 传教mongo-start.sh文件并编辑
>
> ./mongod --dbpath=/usr/local/mongodb/data --logpath= /usr/local/mongodb/logs/mongodb.log --auth --logappend --port=27017 --bind_ip=0.0.0.0(设置为所有人可访问) --fork
>
> 将该文件设置为可执行文件,chmod +x mongo-start.sh
>
> 执行./mongo-start开启服务

4. 在java端连接测试

* 导入pom依赖

```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver</artifactId>
    <version>3.8.2</version>
</dependency>
```

* 未设置账号密码

```java
//创建连接
MongoClient client = new MongoClient(IP地址);

//获取操作的数据库
MongoDatabase test1 = client.getDatabase(数据库名);

//获取集合
MongoCollection<Document> collection = test1.getCollection(集合名);

//获取文档的内容
FindIterable<Document> documents = collection.find();

for (Document document : documents){
    System.out.println(document.get(字段));
}
client.close();
```

* 设置账号密码

```java
//设置服务器地址和端口号
ServerAddress serverAddress = new ServerAddress(IP地址, 端口号);

//设置用户名、数据库名称、密码
MongoCredential credential = MongoCredential.createCredential("admin", "admin", "admin".toCharArray());

//通过连接认证获取连接
MongoClientOptions build = MongoClientOptions.builder().build();

MongoClient client = new MongoClient(serverAddress, credential, build);

//获取操作的数据库
MongoDatabase test1 = client.getDatabase(数据库名);

//获取集合
MongoCollection<Document> collection = test1.getCollection(集合名);

//获取文档的内容
FindIterable<Document> documents = collection.find();

for (Document document : documents){
    System.out.println(document.get(字段));
}
client.close();
```

# SpringBoot连接MongoDB

## application.yml

* 不带密码

```yml
spring:
  data:
    mongodb:
      database: 数据库
      host: IP地址
```

* 密码访问

```yml
spring:
  data:
    mongodb:
      database: 数据库
      host: IP地址
      username: 用户名
      password: 密码
      authentication-database: 用户名绑定的数据库(admin)
```