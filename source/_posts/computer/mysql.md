---
title: MySQL
date: 2020/11/1
tags:
	- MySQL
---



# 创建数据表

## 通用命令

>**CREATE TABLE table_name (column_name column_type);**

## 例子

>  创建一个user表，字段为id,name,date

```sql
create table if not exists `user`(
`id` int unsigned auto_increment,
`name` varchar(10) not null,
`date` date,
primary key(`id`)
)engine=InnoDB default charset = utf8;
```

>**字段可为空设置为null，不能为空not null**
>
>    **AUTO_INCREMENT：设置为自增，一般用于主键**
>    
>    **PRIMARY KEY：用于定义列为主键**
>    
>**ENGINE 设置存储引擎，CHARSET 设置编码**



# 单表操作

## 查询语句

> **select * from  table_name**

## 插入语句

### 插入所有字段的数据

> **insert into table_name values()**

### 插入某些字段的数据

> **insert into table_name(field1,field2) values(value1,value2)**

## 更新语句

> **update table_name set field = value [,field1=value1]...**

## 条件语句

### 限制字段

> **where field = value**

### 限制条数

> **limit m,n**
>
> **m	从第几条数据开始**
>
> **n	限制条数**

### 升序排序

> **order by field**

### 降序排序

> **order by field desc**

### 查询条数

> **select count(*)**

### 分组查询

> **group by field**

## 模糊查询

```sql
select * from table_name WHERE field LIKE '%value';
```

> %：匹配零个或多个
> 
> %value，匹配以value结尾的所有值
>
> value%，匹配以value开头的所有值

## UNION查询

```sql
select * from table_name1
union select from table_name2
```



## CASE查询

### 用法

```sql
CASE case_value
	WHEN when_value THEN
		statement_list
	ELSE
		statement_list
END name;
```

### 例子

1. 建表sex

   |  id  | sex  |
   | :--: | :--: |
   |  1   |  1   |
   |  2   |  0   |
   |  3   |  1   |

2. 执行语句

   ```sql
   SELECT id,
   CASE sex
   	WHEN 0 THEN
   		'男'
   	WHEN 1 THEN
   		'女'
   	ELSE ''
   END sex
   FROM sex
   ```

3. 结果

   |  id  | sex  |
   | :--: | :--: |
   |  1   |  男  |
   |  2   |  女  |
   |  3   |  男  |

## 排序查询

> 使用order by语句进行排序，其中升序用asc，降序用desc，默认是升序。

### 例子

### 建表

> **student(id,name)**

### 查询学生表中姓名、学号，并以学号降序排序

```sql
select id,name from student order by id desc
```

### 查询学生表中前5名学生的姓名，学号，并以学号升序排列

```sql
select top 5 name,id from student order by id
```

# 多表操作

## 显示连接

### 内连接

**只查询在连接的表中能够有对应的记录**

> **select * from table_name1 as A inner join table_name2 as B on A.id = B.id**
>
> **as：将表设置为一个别名，也可不写**

### 外连接

#### 左连接

**table_name1，也就是基准表，用基准表的数据去匹配右表的数据，所以左表的记录是全部会查询出来的，如果右表没有记录对应的话就显示null**

>**select * from table_name1 as A left join table_name2 as B on A.id = B.id**

#### 右连接

**只是把left修改成了right，但是基准表变化了，是以右表的数据去匹配左表，所以左外连接能做到的查询，右外连接也能做到**

> **select * from table_name1 as A right join table_name2 as B on A.id = B.id**

### 例子

#### 建表

**user表**

| uid  | uname | iid  |
| :--: | :---: | :--: |
|  1   | 张三  |  1   |
|  2   | 李四  |  2   |
|  3   | 王五  |  3   |

**info表**

| iid  |    info    |
| :--: | :--------: |
|  1   | 张三的信息 |
|  2   | 李四的信息 |
|  4   | 某某的信息 |

#### 内连接

**执行语句**

```sql
SELECT A.uid,A.uname,B.info FROM user A
inner JOIN info B
ON A.iid = B.iid
```

**结果**

| uid  | uname |    info    |
| :--: | :---: | :--------: |
|  1   | 张三  | 张三的信息 |
|  2   | 李四  | 李四的信息 |

#### 左外连接

**执行语句**

```sql
SELECT A.uid,A.uname,B.info FROM user A
LEFT JOIN info B
ON A.iid = B.iid
```

**结果**

| uid  | uname |    info    |
| :--: | :---: | :--------: |
|  1   | 张三  | 张三的信息 |
|  2   | 李四  | 李四的信息 |
|  3   | 王五  |    null    |

#### 右外连接

**执行语句**

```mysql
SELECT A.uid,A.uname,B.info FROM user A
RIGHT JOIN info B
ON A.iid = B.iid
```

**结果**

| uid  | uname |    info    |
| :--: | :---: | :--------: |
|  1   | 张三  | 张三的信息 |
|  2   | 李四  | 李四的信息 |
| null | null  | 某某的信息 |

## 隐式内连接

> **select * from table_name1,table_name2 where table_name1.id = table_name2.id**

# 函数

## 字符串函数

### CONCAT

1. **概念**

   >  **合并多个字符串**

2. **用法**

   > **CONCAT**(s1,s2...sn)

3. **例子**

   >**在user表中搜索名字为zhangsan的数据**
   
   ```sql
   SELECT * FROM user WHERE name =  CONCAT("zhang","san")
   ```

### CONCAT_WS

1. **概念**

   > **合并多个字符串，并添加分隔符**

2. **用法**

   > **CONCAT_WS(x, s1,s2...sn)**

3. **例子**

   >**查询user表中用户生日为2020-11-5**
   
   ```sql
   SELECT * FROM user WHERE birth= CONCAT_WS("-",2020,11,5)
   ```

## 数字函数

## 日期函数

### ADDDATE

1. **概念**

   >**计算起始日期 d 加上 n 天的日期**

2. **用法**

   > **ADDDATE(d,n)**

3. **例子**

   >**查询未过期的商品(creatTime：生产时间，time：保质期)**
   
   ```sql
   SELECT * FROM goods WHERE ADDDATE(createTime, INTERVAL time DAY)>NOW()
   ```

### ADDTIME

1. **概念**

   > **n 是一个时间表达式，时间 t 加上时间表达式 n**

2. **用法**

   > **ADDTIME(t,n)**

3. **例子**

   加5秒

   ```sql
   SELECT ADDTIME('2020-11-11 11:11:11', 5);
   ```
   
   >2020-11-11 11:11:16 (秒)

   添加 2 小时, 10 分钟, 5 秒

   ```sql
   SELECT ADDTIME("2020-11-11 09:34:21", "2:10:5"); 
   ```
   
   >2020-11-11 11:44:26

### NOW

1. **概念**

   >**返回当前日期和时间**

2. **用法**

   > NOW()

## 高级函数

### IFNULL

1. **概念**

   >**如果 v1 的值不为 NULL，则返回 v1，否则返回 v2**

2. **用法**

   >**IFNULL(v1,v2)**

3. **例子**

   **查询user表中年龄，如果为null，则输出0**

   ```sql
   SELECT IFNULL(age,0) FROM user
   ```

### ISNULL

1. **概念**

   >**判断表达式是否为 NULL**

2. **用法**

   >  **ISNULL(expression)**

### GROUP_CONCAT

1. **概念**

   >**返回带有来自一个组的连接的非NULL值的字符串结果**
   >
   >**就是会计算哪些行属于同一组，将属于同一组的列显示出来**

2. **用法**

   > **GROUP_CONCAT()**

3. **例子**

   >student(sid,sname,tid),teacher(tid,tname)
   >
   >每一行tid对应所有的sname(默认是用，分隔)
   
   ```sql
   SELECT t.tname,GROUP_CONCAT(s.sid) from student s
   LEFT JOIN teacher t
   on s.tid = t.tid
   GROUP BY t.tid
   ```

# 例子

## 准备数据

> 1. 学生表
>
>    Student(sid,sname,sage,ssex)
>
>    sid：学生编号，sname：学生姓名，sage：学生年龄，ssex：学生性别
>
> 2. 课程表
>
>    Course(cid,cname,tid)
>
>    cid：课程编号，cname：课程名称，tid：教师编号
>
> 3. 教师表
>
>    Teacher(tid,tname)
>
>    tid：教师编号，tname：教师姓名
>
> 4. 成绩表
>
>    SC(sid,cid,score)
>
>    sid：学生编号，cid：课程编号，score：分数

创建测试数据

学生表Student

```sql
create table Student(sid varchar(10),sname varchar(10),sage datetime,ssex varchar(10))
insert into Student values('01' , '张三' , '1990-01-01 00:00:00.0000' , '男');
insert into Student values('02' , '李四' , '1990-12-21 00:00:00.0000' , '男');
insert into Student values('03' , '王五' , '1990-05-20 00:00:00.0000' , '男');
insert into Student values('04' , '赵六' , '1990-08-06 00:00:00.0000' , '男');
insert into Student values('05' , '李七' , '1991-12-01 00:00:00.0000' , '女');
insert into Student values('06' , '吴八' , '1992-03-01 00:00:00.0000' , '女');
insert into Student values('07' , '孙九' , '1989-07-01 00:00:00.0000' , '女');
insert into Student values('08' , '邓十' , '1990-01-20 00:00:00.0000' , '女');
```

课程表Course

```sql
create table Course(cid varchar(10),cname varchar(10),tid varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
```

教师表Teacher

```sql
create table Teacher(tid varchar(10),tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
```

成绩表SC

```sql
create table SC(sid varchar(10),cid varchar(10),score decimal(18,1));
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
```

## 例子

### 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

```sql
SELECT S.*,A.score score_01,B.score score_02 FROM 
((SELECT * FROM SC WHERE cid = '01') A
LEFT JOIN (SELECT * FROM SC WHERE cid = '02') B
ON A.sid = B.sid
LEFT JOIN Student S
ON A.sid = S.sid)
WHERE A.score > B.score
```

### 查询同时存在" 01 "课程和" 02 "课程的情况

```sql
SELECT * FROM (SELECT * FROM SC WHERE cid = '01') A
LEFT JOIN (SELECT * FROM SC WHERE cid = '02') B
ON A.sid = B.sid
WHERE B.cid IS NOT NULL
```

### 询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为null)

```sql
SELECT * FROM (SELECT * FROM SC WHERE cid = '01') A
LEFT JOIN (SELECT * FROM SC WHERE cid = '02') B
ON A.sid = B.sid
```

### 查询不存在" 01 "课程但存在" 02 "课程的情况

```sql
SELECT * FROM SC WHERE cid = '02' and sid NOT IN (SELECT sid FROM SC WHERE cid = '01')
```

### 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

```sql
SELECT A.sid,B.sname,A.score FROM
 (SELECT sid,AVG(score) score FROM SC GROUP BY sid) A
 LEFT JOIN Student B
 ON A.sid = B.sid
 WHERE A.score > 60
```

### 查询在 SC 表在成绩的学生信息

> **DISTINCT：去重**

```sql
SELECT * FROM student WHERE sid in (SELECT DISTINCT sid FROM SC)
```

### 查询所有同学学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为null)

```sql
SELECT B.sid,B.sname,A.count,A.total_score FROM 
(SELECT sid,COUNT(score) count,SUM(score) total_score FROM SC GROUP BY sid) A
RIGHT JOIN student B
ON A.sid = B.sid
```

### 查询「李」姓老师的数量

```sql
SELECT count(*) count FROM teacher WHERE tname like "李%"
```

### 查询学过「张三」老师授课的同学的信息

```sql
SELECT D.* FROM teacher A
LEFT JOIN course B
ON A.tid = B.tid
LEFT JOIN SC C
ON B.cid = C.cid
LEFT JOIN student D
on C.sid = D.sid
WHERE A.tname = '张三'
```

> 或者

```sql
SELECT * FROM student
WHERE sid in (SELECT DISTINCT sid FROM SC
WHERE cid = (SELECT cid FROM course
WHERE tid = (SELECT tid FROM teacher WHERE tname = "张三")))
```

### 查询没有学全所有课程的同学的信息

```sql
SELECT * FROM student WHERE sid in(
SELECT sid FROM SC GROUP BY sid HAVING COUNT(cid) < (
SELECT COUNT(*) FROM course))
```

### 待更新...

