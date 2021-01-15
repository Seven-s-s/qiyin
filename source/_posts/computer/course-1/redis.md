---
title: Redis
date: 2020/8/1 15:32
categories:
	- [计算机, 框架]
tags:
	- Redis
---

# Redis简介

## Redis是什么

> Redis ( Remote Dictionary Server )，即远程字典服务!
>
> 是一个开源的使用ANSlC语言编写、支持网络、可基于内存亦可持久化的日志型、Key-value数据库，并提供多种语言的APl。
>
> redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。免费和开源!是当下最热门的NoSQL技术之一!也被人们称之为结构化数据库!

## Redis能干嘛

> 1、内存存储、持久化，内存中是断电即失、所以说持久化很重要( rdb、aof )
>
> 2、效率高，可以用于高速缓存
>
> 3、发布订阅系统
>
> 4、地图信息分析
>
> 5、计时器、计数器（浏览量!)
>
> ......

## 特性

> 1、多样的数据类型
>
> 2、持久化
>
> 3、集群
>
> 4、事务
>
> ......

# 安装Redis

## Windows下安装

1. 下载安装包

> https://github.com/microsoftarchive/redis/tags

2. 解压

![redis](redis.assets/redis.png)

3. 开启redis，双击运行服务（redis-server）
4. 使用redid客户端来连接redis（redis-cli）
5. 测试

> set name zhangsan
>
> get name

## Linux下安装

1. 下载安装包（Redis-5.0.8.tar.gz）
2. 解压Redis的安装包！程序/opt
3. 进入解压后的文件，可以看到redis的配置文件
4. 基本的环境安装

> yum install gcc-c++
>
> make
>
> make install

5. redis的默认安装路径 `usr/local/bin`
6. 将redis配置文件，复制到当前目录下
7. redis默认不是后台启动的，修改配置文件

> vim redis.conf
>
> 将daemonize改为yes

8. 启动Redis服务

> redis-server ./redis.conf
>
> redis-cli -p 6379

9. 测试

> set name zhangsan
>
> get name

10. 查看redis的进程是否开启

> ps -ef|grep redis

11. 如何关闭redis服务

> shutdown

# 基础知识

> redis默认16个数据库
>
> 默认使用的是第0个

1. 切换数据库

> select 6

2. 查看数据库大小

> DBSIZE

3. 查看所有的key

> keys *

4. 清除当前数据库

> flushdb

5. 清除全部的数据库

> FLUSHALL

## Redis是单线程的

> 明白Redis是很快的，官方表示，Redis是基于内存操作，CPU不是Redis性能瓶颈，Redis的瓶颈是根据机器的内存和网络带宽，既然可以使用单线程来实现，就使用单线程了!所有就使用了单线程了!
>
> Redis是C语言写的，官方提供的数据为100000+的QPS，完全不比同样是使用key-vale的Memecache差!Redis为什么单线程还这么快?

## Redis 为什么单线程还这么快?

> 1、误区1:高性能的服务器一定是多线程的?
>
> 2、误区2∶多线程（CPU上下文会切换!)一定比单线程效率高!先去CPU>内存>硬盘的速度要有所了解!
>
> 核心: redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程(CPU上下文会切换∶耗时的操作! !! )，对于内存系统来说，如果没有上下文切换效率就是最高的!多次读写都是在一个CPU上的，在内存情况下，这个就是最佳的方案!

# 五大数据类型

> String、hash、list、set、sorted_set

## Redis-Key

```bash
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name zhangsan 	# set key
OK
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set age 1
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> EXISTS name			# 判断当前key是否存在
(integer) 1
127.0.0.1:6379> EXISTS name1
(integer) 0
127.0.0.1:6379> move name 1 		# 移除当前的key
(integer) 1
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name lisi
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> clear 				# 清屏
127.0.0.1:6379> get name
"lisi"
127.0.0.1:6379> EXPIRE name 10 		# 设置过期时间，单位是秒
(integer) 1
127.0.0.1:6379> ttl name 			# 查看当前剩余时间
(integer) 8
127.0.0.1:6379> ttl name
(integer) 3
127.0.0.1:6379> ttl name
(integer) 0
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> type age 			# 查看当前key的类型
string
```

## String（字符串）

### 基本操作

```bash
127.0.0.1:6379> set key1 v1      	# 设置值
OK
127.0.0.1:6379> get key1   		 	# 获取值
"v1"
127.0.0.1:6379> keys * 				# 获得所有的key
1) "key1"
127.0.0.1:6379> EXISTS key1			# 判断某个key是否存在
(integer) 1
127.0.0.1:6379> APPEND key1 "hello"	# 追加字符串，如果当前key不重载，相当于set key
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> STRLEN key1			# 获取字符串长度
(integer) 7
127.0.0.1:6379> APPEND key1 ",zhangsan"
(integer) 16
127.0.0.1:6379> STRLEN key1
(integer) 16
127.0.0.1:6379> get key1
"v1hello,zhangsan"
```

### 自增、自减

```bash
127.0.0.1:6379> set i 0				# 初始浏览量0
OK
127.0.0.1:6379> get i
"0"
127.0.0.1:6379> incr i				# 自增1 浏览量+1
(integer) 1
127.0.0.1:6379> incr i
(integer) 2
127.0.0.1:6379> get i
"2"
127.0.0.1:6379> decr i				#自减1 浏览就-1
(integer) 1
127.0.0.1:6379> decr i
(integer) 0
127.0.0.1:6379> decr i
(integer) -1
127.0.0.1:6379> get i
"-1"
127.0.0.1:6379> incrby i 10			# 可以设置部长，指定增量 
(integer) 9
127.0.0.1:6379> incrby i 10
(integer) 19
127.0.0.1:6379> decrby i 5
(integer) 14
```

### 字符串范围

```bash
127.0.0.1:6379> set key1 "hello,zhangsan"	# 设置key1
OK
127.0.0.1:6379> get key1
"hello,zhangsan"
127.0.0.1:6379> getrange key1 0 3			# 截取字符串 [0.3]
"hell"
127.0.0.1:6379> getrange key1 0 -1			#截取全部字符串，和get key一样
"hello,zhangsan"
```

### 替换

```bash
127.0.0.1:6379>  set key2 zhangsan
OK
127.0.0.1:6379> get key2
"zhangsan"
127.0.0.1:6379> setrange key2 1 xx	# 替换指定位置
(integer) 8
127.0.0.1:6379> get key2
"zxxngsan"
```

### 设置过期时间

> setex(set with expire)	设置货期时间
>
> setnx(set if not exists)	不存在设置（在分布式锁中常常使用）

```bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> setex key 30 "hello"	# 设置key的值为hello，30秒后过期
OK
127.0.0.1:6379> ttl key
(integer) 19
127.0.0.1:6379> get key
"hello"
127.0.0.1:6379> setnx mykey "redis"		# 如果没有可以不存在，创建mykey
(integer) 1	
127.0.0.1:6379> keys *
1) "mykey"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "mongoDB"	# 如果mykey存在，创建失败
(integer) 0
127.0.0.1:6379> get mykey
"redis"
```

### 批量设置获取

```bash
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3	# 同时设置多个值
OK
127.0.0.1:6379> mget k1 k2 k3			# 同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4		# msetnx是一个原子性的操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get k4
(nil)
```

### 对象

> set user:1 {name:zhangsan,age:2}	设置一个user:1对象 值为json字符串来保存一个对象
>
> 这里key是一个巧妙的设计：user:{id}:{filed}，如此设置在Redis是完全ok的

```bash
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "2"
```

### 先get后set

```bash
127.0.0.1:6379> getset db redis		# 如果不存在，则返回nil
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb	# 如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
```

### 使用场景

* 计数器
* 统计多单位数量
* 粉丝数
* 对象缓存存储

## List（列表）

> redis里面，可以把list玩成栈、队列、阻塞队列！
>
> 所有的list命令都是l开头的

### 基本操作

```bash
127.0.0.1:6379> lpush list one	# 将一个值或者多个值，插入到列表的头部（左）
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> lrange list 0 1
1) "three"
2) "two"
127.0.0.1:6379> rpush list right	# 将一个值或者多个值，插入到列表的尾部（右）
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
```

### 移除

```bash
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"
127.0.0.1:6379> lpop list	# 移除list的第一个元素
"three"
127.0.0.1:6379> rpop list	# 移除list的最后一个元素
"right"
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
```

### 获取某个值

```bash
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> lindex list 1	# 通过下标获取list中的某一个值
"one"
127.0.0.1:6379> lindex list 0
"two"
```

### 获取长度

```bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> lpush list one
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> llen list	# 返回列表的长度
(integer) 3
```

### 移除指定值

```bash
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> lrem list 1 one		# 移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> lrem list 1 three
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> lrem list 2 three
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "two"
```

### 截取指定长度

```bash
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379>  rpush mylist "hello1"
(integer) 2
127.0.0.1:6379>  rpush mylist "hello2"
(integer) 3
127.0.0.1:6379>  rpush mylist "hello3"
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2	# 通过下标截取指定的长度，这个list已经被改变了，只剩下截取的元素
OK
127.0.0.1:6379> lrange mylist 0 -1
1) "hello1"
2) "hello2"
```

### rpoplpush

> 移除列表的最后一个元素，将它移动到新的列表中

```bash
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "hello1"
(integer) 2
127.0.0.1:6379> rpush mylist "hello2"
(integer) 3
127.0.0.1:6379> rpoplpush mylist otherlist	
"hello2"
127.0.0.1:6379> lrange mylist 0 -1		# 查看原来的列表
1) "hello"
2) "hello1"
127.0.0.1:6379> lrange otherlist 0 -1	# 查看目标列表
1) "hello2"
```

### lset

> 将列表中指定下标的值替换为另一个值，更新操作

```bash
127.0.0.1:6379> exists list			# 判断这个列表是否存在
(integer) 0
127.0.0.1:6379> lset list 0 item	# 如果不存在列表，去更新会报错
(error) ERR no such key
127.0.0.1:6379> lpush list value1
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "value1"
127.0.0.1:6379> lset list 0 item	# 如果存在，更新当前下标的值
OK
127.0.0.1:6379> lrange list 0 -1
1) "item"
127.0.0.1:6379> lset list 1 ohter	# 如果不存在，则会报错
(error) ERR index out of range
```

### 插入值

> 将某个具体的value插入到列表某个元素的前面或者后面

```bash
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "world"
(integer) 2
127.0.0.1:6379> linsert mylist before "world" "other"
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
127.0.0.1:6379> linsert mylist after world new
(integer) 4
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "other"
3) "world"
4) "new"
```

### 总结

* 它实际上是一个链表，before node after，left，right都可以插入值
* 如果key不存在，创建新的链表
* 如果key存在，创建内容
* 如果移除了所有值，空链表，也代表不存在
* 在两边插入或者改动值，效率最高！中间元素，相对来说效率低！
* 消息排队、消息队列、栈

## Set（集合）

> 不可重复的集合

### 基本操作

```bash
127.0.0.1:6379> sadd set hello		# set集合中添加元素
(integer) 1
127.0.0.1:6379> sadd set zhangsan
(integer) 1
127.0.0.1:6379> sadd set world
(integer) 1
127.0.0.1:6379> smembers set		# 查看指定set的所有值
1) "world"
2) "hello"
3) "zhangsan"
127.0.0.1:6379> sismember set hello # 判断某一个值是否在set集合中
(integer) 1
127.0.0.1:6379> sismember set age
(integer) 0
```

### 获取个数

```bash
127.0.0.1:6379> scard set # 获取set集合中的内容元素个数
(integer) 3
```

### 移除

```bash
127.0.0.1:6379> srem set hello	# 移除set集合中的指定元素
(integer) 1
127.0.0.1:6379> scard set
(integer) 2
127.0.0.1:6379> smembers set
1) "world"
2) "zhangsan"
```

### 随机获取元素

```bash
127.0.0.1:6379> sadd set a
(integer) 1
127.0.0.1:6379> sadd set b
(integer) 1
127.0.0.1:6379> srandmember set		# 随机获取一个元素
"world"
127.0.0.1:6379> srandmember set 2	# 随机获取指定个数的元素
1) "b"
2) "a"
127.0.0.1:6379> srandmember set 2
1) "zhangsan"
2) "a"
127.0.0.1:6379> srandmember set 2
1) "world"
2) "zhangsan"
```

### 随机删除值

```bash
127.0.0.1:6379> smembers set
1) "world"
2) "b"
3) "a"
4) "zhangsan"
127.0.0.1:6379> spop set	# 随机删除一些set集合中的元素
"world"
127.0.0.1:6379> spop set
"zhangsan"
127.0.0.1:6379> smembers set
1) "b"
2) "a"
```

### 移动

```bash
127.0.0.1:6379> sadd myset hello
(integer) 1
127.0.0.1:6379> sadd myset world
(integer) 1
127.0.0.1:6379> sadd myset zhangsan
(integer) 1
127.0.0.1:6379> sadd myset2 hello2
(integer) 1
127.0.0.1:6379> smove myset myset2 zhangsan	# 将一个指定的值，移动到另外一个set集合中
(integer) 1
127.0.0.1:6379> smembers myset
1) "world"
2) "hello"
127.0.0.1:6379> smembers myset2
1) "hello2"
2) "zhangsan"
```

### 交并差

```bash
127.0.0.1:6379> sadd key1 a
(integer) 1
127.0.0.1:6379> sadd key1 b
(integer) 1
127.0.0.1:6379> sadd key1 c
(integer) 1
127.0.0.1:6379> sadd key2 c
(integer) 1
127.0.0.1:6379> sadd key2 d
(integer) 1
127.0.0.1:6379> sadd key2 e
(integer) 1
127.0.0.1:6379> sdiff key1 key2		# 差集
1) "b"
2) "a"
127.0.0.1:6379> sinter key1 key2	# 交集 共同好友可以这样实现
1) "c"
127.0.0.1:6379> sunion key1 key2	# 并集
1) "b"
2) "a"
3) "c"
4) "e"
5) "d"
```

## Hash（哈希）

> Map集合，key-map！这个值是一个map集合！本质和String类型没有太大区别，还是一个简单的key-value

### 基本操作

```bash
127.0.0.1:6379> hset myhash field1 zhangsan		# set一个具体的key-value
(integer) 1
127.0.0.1:6379> hget myhash field1				# 获取一个字段值
"zhangsan"
127.0.0.1:6379> hmset myhash field1 hello field2 world	# set多个key-value
OK
127.0.0.1:6379> hmget myhash field1 field2		# 获取多个字段值
1) "hello"
2) "world"
127.0.0.1:6379> hgetall myhash					# 获取全部的数据
1) "field1"
2) "hello"
3) "field2"
4) "world"
127.0.0.1:6379> hdel myhash field1			# s删除指定key字段，对应的value值也就消失了
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
```

### 长度

```bash
127.0.0.1:6379> hmset myhash field1 hello field2 world
OK
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "world"
3) "field1"
4) "hello"
127.0.0.1:6379> hlen myhash		# 获取hash表的字段数量
(integer) 2
```

### 是否存在

```bash
127.0.0.1:6379> hexists myhash field1
(integer) 1
127.0.0.1:6379> hexists myhash field3
(integer) 0
```

### 获取field和value

```bash
127.0.0.1:6379> hkeys myhash
1) "field2"
2) "field1"
127.0.0.1:6379> hvals myhash
1) "world"
2) "hello"
```

### 增减

```bash
127.0.0.1:6379> hset myhash field3 5	# 指定增量
(integer) 1
127.0.0.1:6379> hincrby myhash field3 1
(integer) 6
127.0.0.1:6379> hincrby myhash field3 -1
(integer) 5
127.0.0.1:6379> hsetnx myhash field4 hello	# 如果不存在则可以设值
(integer) 1
127.0.0.1:6379> hsetnx myhash field4 world	# 如果存在则不能设值
(integer) 0
```

### 对象

```bash
127.0.0.1:6379> hset user:1 name zhangsan
(integer) 1
127.0.0.1:6379> hget user:1 name
"zhangsan"
```

> hash变更的数据user name age，尤其是用户信息之类的，经常变动的信息！hash更适合于对象的存储，Strin更适合于字符串的存储

## Zset（有序集合）

> 在set的基础上，增加一个值，setk1 v1，zset k1 score1 v1

```bash
127.0.0.1:6379> zadd myset 1 one	# 添加一个值
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three	# 添加多个值
(integer) 2
127.0.0.1:6379> zrange myset 0 -1
1) "one"
2) "two"
3) "three"

```

### 排序

```bash
127.0.0.1:6379> zadd salary 2500 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 5000 lisi
(integer) 1
127.0.0.1:6379> zadd salary 500 wangwu
(integer) 1
127.0.0.1:6379> zrangebyscore salary -inf +inf	# 显示全部用户，从小到大
1) "wangwu"
2) "zhangsan"
3) "lisi"
127.0.0.1:6379> zrangebyscore salary -inf +inf withscores	# 显示全部的用户，并且附带score
1) "wangwu"
2) "500"
3) "zhangsan"
4) "2500"
5) "lisi"
6) "5000"
127.0.0.1:6379> zrangebyscore salary -inf 2500 withscores	# 显示工资小于2500员工的升序排序
1) "wangwu"
2) "500"
3) "zhangsan"
4) "2500"
127.0.0.1:6379> zrevrange salary 0 -1	# # 显示全部用户，从大到小
1) "lisi"
2) "zhangsan"
3) "wangwu"
```

### 移除

```bash
127.0.0.1:6379> zrem salary zhangsan
(integer) 1
```

### 统计个数

```bash
127.0.0.1:6379> zcard salary
(integer) 2
```

### zcount

```bash
127.0.0.1:6379> zadd myset 1 hello
(integer) 1
127.0.0.1:6379> zadd myset 2 world 3 zhangsan
(integer) 2
127.0.0.1:6379> zcount myset 1 3	# 获取指定区间的成员数量
(integer) 3
127.0.0.1:6379> zcount myset 1 2
(integer) 2
```

### 使用场景

* 存储班级成绩表，工资表排序
* 重要消息，带权重进行判断
* 排行榜应用实现

# 三种特殊数据类型

## geospatital地理位置

> 朋友的定位，附件的人，打车距离计算

### geoadd

> 添加地理位置
>
> 规则：两级无法直接添加，我们一般会下载城市数据，直接通过java程序一次性导入
>
> 有效的经度从-180度到180度。
>
> 有效的纬度从-85.05112878度到85.05112878度。
>
> 当坐标位置超出上述指定范围时,该命令将会返回一个错误。
>
> 参数 key 值（纬度、经度、名称）

```bash
127.0.0.1:6379> geoadd china:city 116.40 39.900 beijin
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqin
(integer) 1
127.0.0.1:6379> geoadd china:city 114.05 22.52 shenzhen 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 3
```

### getpos

> 获取当前定位，一定是一个坐标值

```bash
127.0.0.1:6379> geopos china:city beijin
1) 1) "116.39999896287918"
   2) "39.900000091670925"
127.0.0.1:6379> geopos china:city beijin chongqin
1) 1) "116.39999896287918"
   2) "39.900000091670925"
2) 1) "106.49999767541885"
   2) "29.529999579006592"
```

### geodist

> 两人之间的距离单位:
>
> m表示单位为米
>
> km表示单位为千米
>
> mi表示单位为英里
>
> ft表示单位为英尺。

```bash
127.0.0.1:6379> geodist china:city beijin shanghai
"1067378.7564"
127.0.0.1:6379> geodist china:city beijin shanghai km
"1067.3788"		# 查看北京到上海的直线距离
127.0.0.1:6379> geodist china:city beijin chongqin km
"1464.0708"		# 查看北京到重庆的直线距离
```

### georadius

> 已给定的经纬度为中心，找出某一半径内的元素

```bash
127.0.0.1:6379> georadius china:city 110 30 1000 km	# 以100，30这个经纬度为中心，寻找方圆1000km内的城市
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
127.0.0.1:6379> georadius china:city 110 30 500 km
1) "chongqin"
2) "xian"
127.0.0.1:6379>  georadius china:city 110 30 500 km withdist	# 显示到中间距离的位置
1) 1) "chongqin"
   2) "341.9374"
2) 1) "xian"
   2) "483.8340"
127.0.0.1:6379>  georadius china:city 110 30 500 km withcoord	# 显示他人的定位信息
1) 1) "chongqin"
   2) 1) "106.49999767541885"
      2) "29.529999579006592"
2) 1) "xian"
   2) 1) "108.96000176668167"
      2) "34.2599996441893"
127.0.0.1:6379>  georadius china:city 110 30 500 km withdist withcoord count 1	# 筛选出指定的结果
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885"
      2) "29.529999579006592"
127.0.0.1:6379>  georadius china:city 110 30 500 km withdist withcoord count 2
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885"
      2) "29.529999579006592"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167"
      2) "34.2599996441893"
```

### georadiusbymember

> 找出位于指定元素周围的其他元素

```bash
127.0.0.1:6379> georadiusbymember china:city beijin 1000 km
1) "beijin"
2) "xian"
127.0.0.1:6379> georadiusbymember china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"
```

### geohash

> 返回11个字符的geohash字符串
>
> 将二维的经纬度转换成一维的字符串，如果两个字符串越接近，那么距离越近

```bash
127.0.0.1:6379> geohash china:city beijin chongqin
1) "wx4fbxxfke0"
2) "wm5xzrybty0"
```

### 用zset操作geo

> geo底层的实现原理其实就是zset，可以使用zset命令来操作geo

```bash
127.0.0.1:6379> zrange china:city 0 -1	# 查看地图中的全部元素
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijin"
127.0.0.1:6379> zrem china:city beijin	# 移除指定的元素
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
```

## hyperloglog

### 什么是基数

> A{1,3,5,7,8,7}
>
> B{1,3,5,7,8}
>
> 基础（不重复的元素） = 5，可以接收误差

### 简介

> Redis 2.8.9版本就更新了Hyperloglog数据结构!
>
> Redis Hyperloglog基数统计的算法!
>
> 优点∶占用的内存是固定，2*64不同的元素的技术，只需要废12KB内存!如果要从内存角度来比较的话Hyperloglog首选!网页的UV(一个人访问一个网站多次，但是还是算作一个人!)
>
> 传统的方式，set保存用户的id，然后就可以统计set中的元素数量作为标准判断!
>
> 这个方式如果保存大量的用户id，就会比较麻烦!我们的目的是为了计数，而不是保存用户id ;0.81%错误率!统计UV任务，可以忽略不计的!

### 测试使用

```bash
127.0.0.1:6379> pfadd mykey a b c d e f g h i j	# 创建第一组元素 mykey
(integer) 1
127.0.0.1:6379> pfcount mykey	# 统计mykey元素的基数数量
(integer) 10
127.0.0.1:6379> pfadd mykey2 i j z x c v b n m	# 创建第二组元素mykey2
(integer) 1
127.0.0.1:6379> pfcount mykey2
(integer) 9
127.0.0.1:6379> pfmerge mykey3 mykey mykey2	# 合并两组mykey mykey2 => mykey3并集
OK
127.0.0.1:6379> pfcount mykey3
(integer) 15
```

> 如果允许容错，那么一定可以使用hyperloglog
>
> 如果不需要容错，就是要set或者自己的数据类型即可

## bitmaps

位存储

> 统计用户信息，活跃，不活跃!登录、未登录!打卡，365打卡!两个状态的，都可以使用Bitmaps !
>
> Bitmaps位图，数据结构!都是操作二进制位来进行记录，就只有0和1两个状态!
>
> 365天=365 bit1字节=8bit46个字节左右!

使用bitmap来记录周一到周日的打卡!

周一∶1周二:0周三:0 周四∶1 ......

```bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
```

查看某一天是否打卡

```bash
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

统计操作，统计打卡天数

```bash
127.0.0.1:6379> bitcount sign	# 统计这周的打卡记录，就可以看到是否有全勤
(integer) 3
```



# 通用操作

## Key通用操作

### **基本操作**

1. 删除指定key

   > del key

2. 获取key是否存在

   > exists key

3. 获取Key的类型

   > type key

###     **拓展操作**

1. 为指定的key设置有效期

   > expire key seconds
   >
   > pexpire key milliseconds
   >
   > expireat key timestamp
   >
   > pexpireat key milliseconds-timestamp

2. 获取key的有效时间

   > ttl key
   >
   > pttl key

3. 切换key从时效性转化为永久性

   > persist key

4. 查询key

   > keys pattern

5. 例子

   > keys *      查询所有
   >
   > keys it*     查询所有以it开头
   >
   > keys *it     查询所有以it结尾
   >
   > keys ??it    查询所有前面两个为任意字符，后面以it结尾
   >
   > keys user:?   查询所有以user:开头，最后一个字符任意
   >
   > keys u[st]er:1 查询所有以u开头，以er:1结尾，中间包含一个字母,s或t 

###     **其他操作**

为key改名

> rename key newkey
>
> renamenx key newkey

对所有key排序

> sort

其他key通用操作

> help @generic

## 数据库通用操作

### 基本操作

1. 切换数据库

   > delect index

2. 其他操作

   > quit
   >
   > ping
   >
   > echo message

### 相关操作

1. 数据移动

   > move key db

2. 数据清除

   > dbsize
   >
   > flushdb
   >
   > flushall

# 事务

> Redis事务本质:一组命令的集合!一个事务中的所有命令都会被序列化，在事务执行过程的中，会按照顺序执行!一次性、顺序性、排他性!执行一些列的命令!
>
> **Redis事务没有没有隔离级别的概念!**
>
> 所有的命令在事务中，并没有直接被执行!只有发起执行命令的时候才会执行! Exec
>
> **Redis单条命令式保存原子性的，但是事务不保证原子性!**

## redis事务

* 开启事务（multi）
* 命令入队（...）
* 执行事务（exec）

## 正常执行事务

```bash
127.0.0.1:6379> multi		# 开启事务
OK
127.0.0.1:6379> set k1 v1	# 命令入队
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec		# 执行事务
1) OK
2) OK
3) "v2"
4) OK
```

## 放弃事务

```bash
127.0.0.1:6379> multi		# 开启事务
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> discard		# 取消事务
OK
127.0.0.1:6379> get k4		# 事务队列中的命令都不会执行
(nil)
```

## 编译型异常

> 代码有问题或者命令有错，事务中所有的命令都不会执行

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> getset k3	# 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> set k5 v5
QUEUED
127.0.0.1:6379> exec		# 执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k5		# 所有的命令都不会执行
(nil)
```

## 运行时异常

> 如果事务队列中存在与语法性，那么执行命令的时候，其他命令式可以正常执行，错误命令抛出异常

```bash
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr k1		# 会执行的时候失败
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k3
QUEUED
127.0.0.1:6379> exec
1) (error) ERR value is not an integer or out of range	# 虽然第一条命令报错了，但是依旧正常执行
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
```

## 监控

悲观锁

* 很悲观，认为什么时候都会出问题，无论做什么都会加锁!

乐观锁

* 很乐观，认为什么时候都不会出问题，所以不会上锁!更新数据的时候去判断一下，在此期间是否有人修改过这个数据
* 获取version
* 更新的时候比较version

正常执行成功

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money		# 监视money对象
OK
127.0.0.1:6379> multi			# 事务正常结束，数据期间没有发生变动，这个时候正常执行
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
```

多线程修改值

> 使用watch可以当作redis的乐观锁操作

```bash
127.0.0.1:6379> watch money		# 监视money对象
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec			# 执行之前，另外一个线程，修改了我们的值，这个时候，就会导致事务失败
(nil)
```

> 打开另一个客户端

```bash
127.0.0.1:6379> get money
"80"
127.0.0.1:6379> set money 1000
OK
```

如果修改失败，获取最新的值

```bash
127.0.0.1:6379> unwatch			# 如果发现事务执行失败，就先解锁
OK
127.0.0.1:6379> watch money		# 获取最新的值，再次监视，sekect version
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec			# 比对监视的值是否发生了变化，如果没有变化，那么可能执行成功，如果变化了，就执行失败
1) (integer) 990
2) (integer) 30
```

## 分布式锁

使用setnx设置一个公共锁

> setnx lock-key value
>
> 有值返回设置失败，无值返回设置成功
>
> 对于返回设置成功的，拥有控制权，进行下一步的业务操作
>
> 对于返回设置失败的，不具有控制权，排队或等待

释放锁

> del lock-key

解决死锁

分布式锁改良

> 使用expire为锁key添加时间限定，到时不释放，放弃锁
>
> expire lock-name second
>
> pexpire lock-name milliseconds

# Jedis（Java操作Redis）

> 什么是Jedis是Redis官方推荐的java连接开发工具!使用lava 操作Redis中间件!如果你要使用java操作redis，那么一定要对Jedis十分的熟悉!

1. 导入对应的依赖

```xml
<dependencies>
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.2.0</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```

2. 编码测试

* 连接数据库
* 操作命令
* 断开连接

```java
public class Test {
    public static void main(String[] args) {
        //new Jedis()对象
        Jedis jedis = new Jedis("127.0.0.1",6379);
        //Jedis所有命令都是之前用过的名令
        System.out.println(jedis.ping());
        //输出PONG，表示连接成功
        jedis.close();
    }
}
```

## String

> 修改中间的test1()，测试每个方法

```java
static Jedis jedis = new Jedis("127.0.0.1", 6379);
public static void main(java.lang.String[] args) {
    System.out.println("操作前的数据");
    print();
    test1();
    System.out.println("操作后的数据");
    print();
    jedis.close();
}

//添加修改单个值
public static void test1(){
    jedis.set("1","1");
    jedis.set("2","2");
}
//删除单个值
public static void test2(){
    jedis.del("1");
    //jedis.del("2");
}
//添加修改多个值
public static void test3(){
    jedis.mset("text1","text1","text2","text2","text3","text3");
}
//删除多个值
public static void test4(){
    jedis.del("text1","text2");
    //jedis.del("text1","text2","text3");
}
//获取多个数据
public static void test5(){
    System.out.println(jedis.mget("text1","text2"));
}
//获取字符个数
public static void test6(){
    System.out.println(jedis.strlen("text1"));
}
//追加字符串到原始字符串后面
public static void test7(){
    jedis.append("text1","text1");
}

//设置数据增加指定范围的值
public static void test8(){
    jedis.incr("1");//加一操作
    //jedis.incrBy("1",10);//增加指定的整数
    //jedis.incrByFloat("1",3.5);//增加指定的浮点数
    //jedis.incrByFloat("1",-3.5);//减少指定的浮点数
}
//设置数据减少指定范围的值
public static void test9(){
    jedis.decr("1");//减一操作
    //jedis.decrBy("1",10);//增加指定的整数
}
//设置数据具有指定的生命周期
public static void test10(){
    jedis.setex("3",5,"3");//设置指定的秒数
    //jedis.psetex("4",10000,"4");//设置指定的毫秒数
}


//输出所有键值
static void print(){
    for (String key:jedis.keys("*")){
        System.out.println(key+" "+jedis.get(key));
    }
}
```

## List

```java
static Jedis jedis = new Jedis("127.0.0.1",6379);
public static void main(String[] args) {
    System.out.println("操作前的数据");
    print();
    test1();
    System.out.println("操作后的数据");
    print();
    jedis.close();
}

//添加、修改数据
public static void test1(){
    jedis.lpush("list","1","2","3");//从左边添加多条数据
    //jedis.rpush("list","4","5","6");//从右边添加多条数据
}
//获取并移除数据
public static void test2(){
    String value = jedis.lpop("list");//从左边获取并移除
    //String value = jedis.rpop("list");//从右边获取并移除数据
}
//获取索引为n的值
public static void test3(){
    System.out.println(jedis.lindex("list",1));
}
//获取长度
public static void test4(){
    System.out.println(jedis.llen("list"));
}
//移除指定数据
public static void test5(){
    jedis.lrem("list",1,"1");//删除n条值为1的数据
}

//输出数据
public static void print(){
    System.out.println(jedis.lrange("list",0,-1));//从左边出所有数据
}
```

## Set

```java
static Jedis jedis = new Jedis("127.0.0.1",6379);

public static void main(String[] args) {
    System.out.println("操作前的数据");
    print();
    test1();
    System.out.println("操作后的数据");
    print();
    jedis.close();
}

//添加、修改数据
public static void test1(){
    jedis.sadd("set","1","2","4","5");
    //jedis.sadd("set1","1","2","3","6","7");
}
//删除数据
public static void test2(){
    jedis.srem("set","1");//可删除多条数据
}
//获取集合总数
public static void test3(){
    System.out.println(jedis.scard("set"));
}
//判断集合是否存在该元素
public static void test4(){
    System.out.println(jedis.sismember("set","1"));
}

//随机获取集合的数据
public static void test5(){
    //System.out.println(jedis.srandmember("set"));
    System.out.println(jedis.srandmember("set",2));//随机获取两个
}
//随机获取集合的某个数据并移除
public static void test6(){
    System.out.println(jedis.spop("set"));
}
//求两个集合的交并差
public static void test7(){
    System.out.println(jedis.sinter("set","set1"));//交
    System.out.println(jedis.sunion("set","set1"));//并
    System.out.println(jedis.sdiff("set","set1"));//差
}
//求两个集合的交并差并储存到指定的集合中
public static void test8(){
    jedis.sinterstore("set01","set","set1");
    jedis.sunionstore("set02","set","set1");
    jedis.sdiffstore("set03","set","set1");
}
//将指定数据从原始集合移动到目标集合中
public static void test9(){
    jedis.smove("set","set1","5");
}

static void print(){
    System.out.println(jedis.smembers("set"));
    //System.out.println(jedis.smembers("set1"));
    /*System.out.println(jedis.smembers("set01"));
    System.out.println(jedis.smembers("set02"));
    System.out.println(jedis.smembers("set03"));*/
}
```

## Hash

```java
static Jedis jedis = new Jedis("127.0.0.1",6379);

public static void main(String[] args) {
    System.out.println("操作前的数据");
    print();
    test1();
    System.out.println("操作后的数据");
    print();
    jedis.close();
}

//添加修改数据
public static void test1(){
    jedis.hset("hash","1","11");
    jedis.hset("hash","2","22");
}
//获取所有的值
public static void test2(){
    Map<String, String> hash = jedis.hgetAll("hash");
    System.out.println(hash);
}
//删除数据
public static void test3(){
    jedis.hdel("hash","1");//删除一条数据
    //jedis.hdel("hash","1","2");//删除多条数据
}
//添加修改多条数据
public static void test4(){
    Map<String,String> map = new HashMap<>();
    map.put("1","11");
    map.put("2","22");
    jedis.hmset("hash",map);
}
//获取多个数据
public static void test5(){
    System.out.println(jedis.hmget("hash","1","2"));
}
//获取表中的字段数量
public static void test6(){
    System.out.println(jedis.hlen("hash"));
}
//判断表中是否存在该字段
public static void test7(){
    System.out.println(jedis.hexists("hash","1"));
}

//获取表中的所有字段名或字段值
public static void test8(){
    System.out.println(jedis.hkeys("hash"));//字段名
    System.out.println(jedis.hvals("hash"));//字段值
}
//设置字段的数据增加指定范围的值
public static void test9(){
    jedis.hincrBy("hash","1",10);
    //jedis.hincrByFloat("hash","1",10.5);
}

//输出所有hash字段名、字段值
static void print(){
    for (String key:jedis.hkeys("hash")){
        System.out.println(key+" "+jedis.hget("hash",key));
    }
}
```

## Zset

```java
static Jedis jedis = new Jedis("127.0.0.1",6379);

public static void main(String[] args) {
    System.out.println("操作前的数据");
    print();
    test1();
    System.out.println("操作后的数据");
    print();
    jedis.close();
}

//添加数据
public static void test1(){
    //jedis.zadd("zsort",10,"1");//添加一条数据
    Map<String,Double> map = new HashMap<>();
    map.put("1",20.0);
    map.put("2",10.0);
    map.put("3",50.0);
    map.put("4",60.0);
    map.put("5",40.0);
    jedis.zadd("zsort",map);//添加多条数据
}
//删除数据
public static void test2(){
    jedis.zrem("zsort","1","2");
}
//根据条件获取数据
public static void test3(){
    System.out.println(jedis.zrangeByScore("zsort", 0, 50));
}
//按条件删除数据
public static void test4(){
    jedis.zremrangeByRank("zsort",0,1);//删除索引0到1的值
    //jedis.zremrangeByScore("zsort",10,20);//删除score为10到20的值
}
//获取集合总数
public static void test5(){
    System.out.println(jedis.zcard("zsort"));
    //System.out.println(jedis.zcount("zsort",10,50));//根据score获取数量
}

//获取数据的索引
public static void test6(){
    System.out.println(jedis.zrank("zsort", "1"));
    System.out.println(jedis.zrevrank("zsort","1"));
}
//score的值获取与修改
public static void test7(){
    System.out.println(jedis.zscore("zsort", "1"));
    jedis.zincrby("zsort",100,"1");//将1的socre添加100
}
//获取当前时间
public static void test8(){
    System.out.println(jedis.time());
}


public static void print(){
    System.out.println(jedis.zrange("zsort",0,-1));//从小到大
    //System.out.println(jedis.zrevrange("zort",0,-1));//从大到小
}
```

## 事务

```java
Jedis jedis = new Jedis("127.0.0.1",6379);

JSONObject jsonObject = new JSONObject();
jsonObject.put("hello","world");
jsonObject.put("name","zhangsan");
//开启事务
Transaction multi = jedis.multi();
String jsonString = jsonObject.toJSONString();
try {
    multi.set("user1",jsonString);
    multi.set("user2",jsonString);

    //先正常执行一次，查看效果，清空数据库之后将下列注释取消查看效果
    //代码抛出异常，执行失败
    //int i = 1/0;

    //执行事务
    multi.exec();
}catch (Exception e) {
    //放弃事务
    multi.discard();
    e.printStackTrace();
}finally {
    System.out.println(jedis.get("user1"));
    System.out.println(jedis.get("user2"));
    //关闭连接
    jedis.close();
}
```

# 整合SpringBoot

> SpringBoot操作数据: spring-data jpa jdbc mongodb redis !
>
> SpringData也是和SpringBoot齐名的项目!
>
> 说明︰在SpringBoot2.x之后，原来使用的jedis被替换为了lettuce?
>
> jedis :采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全的，使用jedis pool连接池!更像BIO模式
>
> lettuce :采用netty，实例可以再多个线程中进行共享，不存在线程不安全的情况!可以减少线程数据了，更像NIO模式

1. 导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

2. 配置连接

```properties
#Springboot所有的配置类，都有一个自动配置类 RedisAutoConfiguration
#自动配置类都会绑定一个properties配置文件  RedisProperties

#配置Redis
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

3. 测试

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        //redisTemplate 操作不同的数据类型，api和我们的指令一样
        //opsForValue   操作字符串   类似String
        //opsForList    操作List     类似List
        //opsForSet
        //opsForHash
        //opsForZset
        //opsForGeo
        //opsForHyperLogLog

        //我们常用的方法都可以直接通过redisTemplate操作，比如事务和基本的CRUD

        //获取redis的连接对象
//        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
//        connection.flushDb();
//        connection.flushAll();
        redisTemplate.opsForValue().set("mykey","zhangsan");
        System.out.println(redisTemplate.opsForValue().get("mykey"));
    }

}
```

4. 查看键

> keys *
>
> 发现在mykey前面一串奇怪的字符串
>
> 需要序列化

## 序列化

1. 编写配置类

```java
@Configuration
public class RedisConfig {
    //编写我们自己的redisTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        //位方便开发，一般采用<String,Object>
        RedisTemplate<String,Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectJackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        //String序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        //key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        //hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        //value序列化方式采用Jackson
        template.setValueSerializer(objectJackson2JsonRedisSerializer);
        //hash的value序列化方式采用Jackson
        template.setHashValueSerializer(objectJackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

2. 测试

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    @Qualifier("redisTemplate")//需要指向自己配置的模板
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        redisTemplate.opsForValue().set("mykey","zhangsan");
        System.out.println(redisTemplate.opsForValue().get("mykey"));
    }
```

## 保存对象

1. 新建User类

```java
@Component
@AllArgsConstructor
@NoArgsConstructor
@Data
//在企业中，我们所有的pojo都会序列化
public class User implements Serializable {
    private String name;
    private int age;
}
```

2. 测试

```java
@Test
void test() throws JsonProcessingException {
    //真实的开发一般使用json来传递对象
    User user = new User("zhangsan", 6);
    //String jsonUser = new ObjectMapper().writeValueAsString(user);
	//redisTemplate.opsForValue().set("user",jsonUser);
    redisTemplate.opsForValue().set("user",user);//User对象不实现序列化，运行会报错，或者使用上面注释的代码
    System.out.println(redisTemplate.opsForValue().get("user"));
}
```

# Config配置

## 网络

```bash
bind 127.0.0.1		# 绑定的ip
protected-mode yes	# 保护模式
port 6379			# 端口号
```

## 通用general

```bash
daemonize yes					# 以守护进程的方式运行，默认是no，我们需要自己开启yes
pidfile /var/run/redis_6379.pid	# 如果以后台的方式运行，我们需要指定一个pid文件
loglevel notice
loginfile "" 		# 日志的文件位置
databases 16 		# 数据库的数量，默认是16个数据库
always-show-logo	# 是否总是显示logo
```

## 快照

```bash
save 900 1		# 如果900s内，如果至少有一个key进行修改，我们进行持久化操作
save 300 10		# 如果300s内，如果至少有十个key进行修改，我们进行持久化操作
save 60 10000	# 如果60s内，如果至少有一万个key进行修改，我们进行持久化操作

stop-writes-on-bgsave-error yes	# 持久化如果出错，是否需要继续工作
rdbcompression yes	# 是否压缩rdb文件，需要消耗一些CPU资源
rdbchecksum yes		# 保存rdb文件的时候，进行错误的检查校验
dir ./				# rdb文件保存的目录
```

## replication复制



## security

```bash
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> config get requirepass			# 获取redis密码
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass "123456"	# 设置redis密码
OK
127.0.0.1:6379> config get requirepass			# 所有命令都没有权限
(error) NOAUTH Authentication required.
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456						# 使用密码进行登录
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123456"
```

## 限制clients

```bash
maxclients 10000				# 设置能连接上redis的最大客户端的数量
maxmemory <bytes>				# redis配置最大的内存容量
maxmemory-policy noeviction		# 内存达到上限之后的处理策略
1、volatile-1ru:只对设置了过期时间的key进行LRU（默认值)
2、a77keys-7ru :删除1ru算法的key
3、volatile-random:随机删除即将过期key
4、a7lkeys-random:随机删缭
5、 volatile-ttl :删除即将过期的
6、noeviction :永不过期，返回错误
```

## append only模式 aof配置

```bash
appendonly no	# 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分情况下，rdb完全够用
appendfilename "appendonly.aof"	# 持久化文件的名字

#appendfsync always		# 每次修改都会sync，消耗性能
appendfsync everysec	# 每秒执行一次，可能会丢失这1s的数据
#appendfsync no			# 不执行sync，操作系统自己同步数据，数据最快
```



# 持久化

> Redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化功能!

## RDB

> 在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。Redis会单独创建( fork )一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何I0操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。
>
> 默认是RDB，一般不修改这个配置

### 触发机制

1. save的规则满足的情况下，会自动触发rdb规则
2. 执行flushdb命令，也会触发rdb规则
3. 退出redis，也会产生rdb文件

备份就自动生成一个dump.rdb文件

### 恢复rdb文件

1. 只需要将rdb文件放在redis启动目录就可以，redis启动的时候会自动检查dump.rdb恢复其中的数据
2. 查看需要存在的位置

```bash
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin"	# 如果在这个目录下不存在dump.rdb文件，启动就会自动恢复其中的数据
```

### 优点

> 1、适合大规模的数据恢复!
>
> 2、对数据的完整性要不高!

### 缺点

> 1、需要一定的时间间隔进程操作!如果redis意外宕机了，这个最后一次修改数据就没有的了!
>
> 2、fork进程的时候，会占用一定的内容空间!!

## AOF

> 将我们的所有命令都记录下来，history，恢复的时候就把这个文件全部在执行一遍!

### 是什么

> 该日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来(读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

aop保存的是appendonly.aof

默认是不开启的，我们需要手动进行配置!我们只需要将appendoaly改为yes就开启了aof !

重启，redis就可以生效了!

如果这个aof 文件有错位，这时候redis是启动不起来的吗，我们需要修复这个aof文件

redis给我们提供了一个工具redis-check-aof --fix

如果文件正常，重启就可以直接恢复了

### 优点

> 1、每一次修改都同步，文件的完整会更加好!
>
> 2、每秒同步一次，可能会丢失一秒的数据3、从不同步，效率最高的!

### 缺点

> 1、相对于数据文件来说，aof远远大于rdb，修复的速度也比 rdb慢!
>
> 2、Aof运行效率也要比rdb 慢，所以我们redis默认的配置就是rdb持久化!

### 扩展

> 1、RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储
>
> 2、AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。
>
> 3、**只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化**
>
> 4、同时开启两种持久化方式
>
> * 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
> * RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢?作者建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份），快速重启，而且不会有AOF可能潘在的Bug，留着作为一个万一的手段。
>
> 5、性能建议
>
> * 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 9001这条规则。
> * 如果Enable AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的lO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOFrewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
> * 如果不启用AOF，仅靠主-从复制实现高可用性也可以，能省掉一大笔IO，也减少了重写时带来的系统波动.代价是如果主/从同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个主/从中的rdb文件，载入较新的那个，微博就是这种架构。

# 主从复制

> 主从复制，是指将一台Redis服务器的数据I，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower);数据的复制是单向的，只能由主节点到从节点。Master以写为主，Slave以读为主。
>
> 默认情况下，每台Redis服务器都是主节点;且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。主从**复制的作用主要包括︰**
>
> 1、数据冗余︰主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
>
> 2、故障恢复∶当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复;实际上是一种服务的冗余。
>
> 3、负载均衡︰在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载;尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
>
> 4、高可用基石︰除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。
>
> **一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的，原因如下：**
>
> 1、从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大;
>
> 2、从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，单台Redis最大使用内存不应该超过20G。

## 环境配置

只配置从库，不用配置主库

```bash
127.0.0.1:6379> info replication
# Replication
role:master			# 角色
connected_slaves:0	# 没有从机
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

复制三个配置文件，然后修改配置信息

* 端口号
* pid名字
* log文件名字
* dump.rdb名字

修改完毕之后，启动三个reids服务器，通过进程信息查看ps -ef|grep redis

## 一主二从

默认情况下，每台redis服务器都是主节点，一般情况只用配置从机

认老大！一主（79）二从（80，81）

```bash
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379	# SLAVEOF host 6379 找谁当主机
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1 #可以的看到主机的信息master_port:6379
master_link_status:up
master_last_io_seconds_ago : 3
master_sync_in_progress:0
slave_rep1_offset: 14
s1ave_priority: 100
slave_read_on1y: 1
connected_slaves :o
master_replid:a81be8dd257636b2d3e7a9f595e69d73ff03774e
master_rep7id2:000000000000000000000000000000000000000o
master_rep1_offset: 14
second_rep1_offset:-1
rep1_backlog_active:1
repl_backlog_size:1048576
rep1_backlog_first_byte_offset: 1
rep1_backlog_histlen : 14
```

在主机中查看

```bash
127.0.0.1:6379> info replication
# Replication
role:master			
connected_slaves:1	# 多了从机的配置
slave0:ip=127.0.0.1,port=6380,state=on1ine,offset=42,1ag=1	# 多了从机的信息
master_rep7id:a81be8dd257636b2d3e7a9f595e69d73ff03774e
master_rep1id2:000000000000000000000000000000000000000o
master_rep1_offset:42
second_rep1_offset:-1
rep1_back1og_active:1
rep1_back1og_size:1048576
rep1_backlog_first_byte_offset: 1
rep1_backlog_histlen :42
```

如果两个都配置了，就会有两个从机
真实的从主配置应该在配置文件中配置，这样的话是永久的，我们这里使用的是命令，暂时的!
主机可以写，从机不能写只能读!主机中的所有信息和数据，都会自动被从机保存!

```bash
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set k1 v1
OK
```

从机只能读取内容

```bash
127.0.0.1:6381> keys *
( empty list or set)
127.0.0.1:6381> keys *
1)"k1"
127.0.0.1:6381> get k1
"v1"
127.0.0.1:6381> set k2 v2
(error) READONLY You can't write against a read only replica.
```

测试∶主机断开连接，从机依旧连接到主机的，但是没有写操作，这个时候，主机如果回来了，从机依旧可以直接获取到主机写的信息!

如果是使用命令行，来配置的主从，这个时候如果重启了，就会变回主机!只要变为从机，立马就会从主机中获取值!

## 复制原理

> Slave启动成功连接到master后会发送一个sync命令
>
> Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，**master将传送整个数据文件到slave，并完成一次完全同步。**
>
> 全量复制:而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
>
> 增量复制:Master继续将新的所有收集到的修改命令依次传给slave，完成同步
>
> 但是只要是重新连接master，一次完全同步(全量复制)将被自动执行

如果主机断开了连接，我们可以使用SLAVEOF no one让自己变成主机!其他的节点就可以手动连接到最新的这个主节点(手动）!如果这个时候老大修复了，那就重新连接!

## 哨兵模式

### 概念

> 主从切换技术的方法是∶当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了sentinel (哨兵）架构来解决这个问题。
>
> 谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库。
>
> 哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

![redis_2](../redis_2.png)

这里的哨兵有两个作用

* 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
* 当哨兵监测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

![redis](redis.assets/redis-1610257177186.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为主观下线。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为客观下线。

### 测试

1、配置哨兵配置文件sentinel.conf

```bash
# sentinel monitor 被监控的名称 host port 1
sentinel monitor myredis 127.0.0.1 6379 1
```

数字1，代表主机挂了，slave投票让谁接替成为主机，票数最多的，将会成为主机

2、启动哨兵

> redis-sentinel config/sentinel.config

如果Master节点断开了，这个时候就会从从机中随机选择一个服务器（里面有一个投票算法）

### 哨兵模式

优点

* 哨兵集群，基于主从复制模式，所有的主从配置优点，它全有
* 主从可以切换，故障可以转移，系统的可用性就会更好
* 3、哨兵模式就是主从模式的升级，手动到自动，更加健壮!缺点∶

缺点

* Redis 不好啊在线扩容的，集群容量一旦到达上限，在线扩容就十分麻烦!
* 实现哨兵模式的配置其实是很麻烦的，里面有很多选择!

### 全部配置

```bash
# Example sentine1.conf

#哨兵sentine1实例运行的端口默认26379
port 26379

#哨兵sentine1的工作目录
dir /tmp

#哨兵sentine1监控的redis主节点的 ip port
# master-name可以自己命名的主节点名字只能由字母A-z、数字0-9、这三个字符".-_"组成。
# quorum配置多少个sentine1哨兵统一认为master主节点失联那么这时客观上认为主节点失联了
# sentinel monitor <master-name> <ip><redis-port> <quorum>
sentine1 monitor mymaster 127.0.0.1 6379 2

#当在Redis实例中开启了requirepass foobared 授权密码这样所有连接Redis实例的客户端都要提供密码
#设置哨兵sentine1连接主从的密码注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentine1 auth-pass mymaster MysUPER--secret-0123passwOrd

# 指定多少毫秒之后主节点没有应答哨兵sentine1此时哨兵主观上认为主节点下线默认30秒
# sentine1 down-after-mi1liseconds <master-name> <mi11iseconds>
sentinel down-after-mi77iseconds mymaster 30000

#这个配置项指定了在发生failover主备切换时最多可以有多少个s1ave同时对新的master进行同步，这个数字越小，完成failover所需的时间就越长，但是如果这个数字越大，就意味着越多的s1ave因为replication而不可用。可以通过将这个值设为1来保证每次只有一个s1ave 处于不能处理命令请求的状态。
# sentine1 para7le1-syncs <master-name> <nums1aves>
sentine1 paralle1-syncs mymaster 1

#故障转移的超时时间 failover-timeout可以用在以下这些方面:#1．同一个sentine1对同一个master两次failover之间的间隔时间。
#2．当一个slave从一个错误的master那里同步数据开始计算时间。直到s7ave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按para11e7-syncs所配置的规则来了
#默认三分钟
# sentinel failover-timeout <master-name> <mil7iseconds>
sentine7 failover-timeout mypaster 180000

# SCRIPTS EXECUTION
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。#对于脚本的运行结果有以下规则:
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚木的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。

#通知型脚本:当sentine1有任何警告级别的事件发生时〈比如说redis实例的主观失效和客观失效等等)，将会去调用这个脚本，这时这个脚本应该通过邮件，SNS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果sentine1.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentine1无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
sentinel notification-script mymaster /var/redis/notify.sh

#客户端重新配置主节点参数脚本
#当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
#以下参数将会在调用脚本时传给脚本:# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
#目前<state>总是“failover”
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
#目前<state>总是“failover”，
# <role>是“7eader”或者"observer”中的一个。
#参数 from-ip，from-port，to-ip，to-port是用来和旧的master和新的master(即旧的s7ave)通信的
#这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
```

# 缓存穿透和雪崩

> Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。
>
> 另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案

## 缓存穿透

### 概念

> 缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

### 解决方案

布隆过滤器

> 布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力;

缓存空对象

> 当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源;

缓存空对象存在的问题

> 1、如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键;
>
> 2、即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

## 缓存击穿

### 概念

> 这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。
>
> 当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。

### 解决方案

设置热点数据永不过期

> 从缓存层面来看，没有设置过期时间，所以不会出现热点key过期后产生的问题。

加互斥锁

> 分布式锁:使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

## 缓存雪崩

### 概念

> 缓存雪崩，是指在某一个时间段，缓存集中过期失效。Redis宕机!
>
> 产生雪崩的原因之一，比如在写本女 的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。
>
> 其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

### 解决方案

redis高可用

> 这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。

限流降级

> 这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

数据预热

> 数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。

# 删除策略

1. 定时删除

   > 优点：节约内存，到时就删除，快速释放不必要的内存占用
   >
   > 缺点：cpu压力大，无论cpu次数负载量多高，均占用cpu
   >
   > 拿cpu换内存

2. 惰性删除

    >优点：节约cpu性能，发现必须删除的时候才删除
    >
    >缺点：内存压力大，出行长期占用内存的数据
    >
    >用存储空间换cpu性能

3. 定期删除

   >Cpu性能占用设置有峰值，检测频度可自定义设置
   >
   >内存压力不是很大，长期占用内存的类数据会被持续清理
   >
   >周期性抽查存储空间（随机抽查，重点抽查



