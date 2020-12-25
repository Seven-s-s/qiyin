---
title: 数据库面试题
date: 2020/12/17 12:04
categories:
	- [计算机, 面试, 面试题]
tags:
	- 面试题
---



## 数据库的分类

> **非关系数据库：mysql、oracle、sqlserver等**
>
> **关系型数据库：redis、memcache、mongodb、hahdoop等**
>
> **redis：键值对数据库**
>
> **mongodb：文档数据库**

## 数据库三范式

范式就是规范，就是在关系型数据库设计表时要遵循的规范

要想满足第二范式就必须满足第一范式，要想满足第三范式就必须满足第二范式

>**第一范式：要求属性具有原子性，不可再分解**
>
>**第二范式：每一行必须被唯一标识（主键）**
>
>**第三范式：任何字段不能由其他字段派生出来，要求字段没有冗余（外键）**

## 事务四个基本特性

事务是并发控制的单位，是用户定义的一个操作序列，这些操作要么都做要么都不做，是一个不可分割的工作单位

> 1. **原子性（A）**
>
>    **一个事务要么完整执行，要么就不执行**
>
> 2. **一致性（C）**
>
>    **底层数据存储的完整性**
>
> 3. **隔离性（I）**
>
>    **事务必须在不干扰其他进程或事务的前提下独立完成**
>
> 4. **持久性（D）**
>
>    **某个事务的执行过程中，对数据所做的所有改动都必须再事务成功结束前保存至某种物理存储设备**

## 事务的隔离级别

> **脏读：A查询B修改后问提交的数据，当B回滚时，A查询的数据是无效的**
>
> **不可重复读：A在第一次查询用户甲的信息，B将用户甲的信息修改并提交；A再次读取用户甲的信息，A两次获取的信息不同则称为“不可重复读”**
>
> **幻读：A查询用户数量时，当B新增或删除用户时，A再次获取用户数量时，两次数量不一致，则称为“幻读”**
>
> **注意：“不可重复读”与“幻读”的区别在于，不可重复读强调的是数据信息的改变，幻读强调的是数量上的改变**
>
> |          隔离级别          | 脏读 | 不可重复读 | 幻读 |
> | :------------------------: | :--: | :--------: | :--: |
> | 读未提交(Read Uncommitted) |  ×   |     ×      |  ×   |
> |  读已提交(Read Committed)  |  √   |     ×      |  ×   |
> |  可重复读(Repeated Read)   |  √   |     √      |  ×   |
> |    串行化(Serializable)    |  √   |     √      |  √   |
>
> **以上四种隔离级别，串行化的级别最高，读未提交的级别最低，级别越高，效率越低**
>
> **MySQL支持以上四种隔离级别，默认的隔离级别是可重复度**
>
> **Oracle数据库只支持串行化和读已提交 ，默认是读已提交；**

## Mysql最大的默认连接数

> **100**

为什么需要最大连接数？

>**特定服务器上面数据库最多只能支持一定数目同时连接**

## Mysql分页？Oracle分页

为什么需要分页？

> **当有很多数据，一个页面不可能显示为所有数据，需要进行分段显示**

mysql使用了limit关键字来限制查询条数

oracle使用了rownum三层嵌套循环

## 对JDBC的理解

> **他就是Java与数据库建立连接的桥梁或插件，用Java代码就能操作数据库的增删查改、存储过程、事务等**

## 写一个简单的JDBC程序

### 操作步骤

> 1. **加载驱动(com.mysql.jdbc.Driver)**
> 2. **获取参数(DriverManager.getConnection(url,username,password))**
> 3. **设置参数(Statement PrepareStatement)**
> 4. **执行(execute)**

### 例子

```java
Class.forName("com.mysql.cj.jdbc.Driver");
String url = "jdbc:mysql://localhost:3306/user?serverTimezone=GMT";
String username = "root";
String password = "123456";
Connection connection = DriverManager.getConnection(url,username,password);
String sql = "select * from user";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
ResultSet resultSet = preparedStatement.executeQuery();
while (resultSet.next()){
    System.out.println(resultSet.getObject(1)+" "+resultSet.getObject(2));
}
connection.close();
preparedStatement.close();
resultSet.close();
```

## PreparedStatement相比于statement的好处

> 1. **PreparedStatement是预编译的，比statement快**
> 2. **代码的可读性和可维护性高**
> 3. **可以防SQL注入**

## 数据库连接池的作用

> 1. **限定数据库的连接个数，进行统一的连接管理**
> 2. **节约资源**
> 3. **加快响应速度**

## 数据库优化