---
title: Redis
date: 2020/8/1 15:32
categories:
	- [计算机, 框架]
tags:
	- Redis
---

# 下载地址

> https://github.com/microsoftarchive/redis/tags

# 基本操作

## 信息添加

> 设置key，value数据
>
> set key value

## 获取值

> 根据key值查找对应的value，如果不存在，返回空（nil）;
>
> get key

## 清屏

> clear

## 帮助信息

> 获取帮助文档
>
> help 命令名称
>
> Help @组名

## 退出

> 退出客户端
>
> quit 或者 exit

# **数据类型**

> String、hash、list、set、sorted_set

## String

### 基本操作

1. 添加/修改数据

   >  set key value

2. 获取数据

   >  get key

3. 删除数据

   >  del key

4. 添加/几个多个数据

   > mset key1 value1 key2 value2 …

5. 获取多个数据

   > mget key1 key2 …

6. 获取数据字符个数

   >  strlen key

7. 追加信息到原始信息后部

   > append key value

### **拓展操作**

1. 设置数据增加指定范围的值

   > Incr key                   对于整数加一操作
   >
   > lncrby key increment         增加指定的整数
   >
   > Incrybyfloat key increment     增加指定的浮点数

2.  设置数值数据减少指定范围的值

   > decr key                  对于整数减一操作
   >
   > decryby key increment        减少指定的整数

3. 设置数据具有指定的生命周期

   >  setex key seconds value
   >
   > psetex key milliseconds value

4. key的设置约定

   > 数据库中的热点数据key命名惯例
   >
   > 表名：主键名：主键值：字段名
   >
   > eg：order:id:123:name

## hash

### **基本操作**

1. 添加/修改数据

   > H  set key field value

2.  获取数据

   > hget key field
   >
   > hgetall key

3. 删除数据

   > hdel key field1 {field2}

4. 添加/修改多个数据

   > hmset key field1 value1 field2 value2

5. 获取多个数据

   > hmget key field1 field2

6. 获取哈希表中的字段数量

   > hlen key

7. 获取哈希表中是否存在指定的字段

   > hexists key field

### **拓展操作**

1. 获取哈希表中所有的字段名或字段值

   >  hkeys key
   >
   > hvals key

2. 设置指定字段的数据增加指定范围的值

   > hincrby key field increment
   >
   > hincrbyfloat key field increment

## **list**

### **基本操作**

1. 添加/删除数据

   > lpush key value1 {valur2} …
   >
   > rpush key value2 {value2} …

2. 获取数据

   > lrange key start stop
   >
   > lindex key index
   >
   > llen key

3. 获取并移除数据

   > lpop key
   >
   > rpop key

### **拓展操作**

1. 规定时间内获取并移除数据

   > blpop key1 {key2} timeout
   >
   > brpop key1 {key2} timeout

2. 移除指定数据

   > lrem key count value

## set

### **基本操作**

1. 添加数据

   > sadd key member1 {member2}

2. 获取所有数据

   > smembers key

3. 删除数据

   > srem key member1 {member2}

4. 获取集合数据总数

   > scard key

5. 判断集合中是否包含指定数据

   > sismember key member

### **拓展操作**

1. 随机获取集合指定数量的数据

   > srandmember key {count}

2. 随机获取集合中的某个数据并将该数据移除集合

   > spop key

3. 求两个集合的交、并、差集

   > sinter key1 {key2}
   >
   > sunion key1 {key2}
   >
   > sdiff key1 {key2}

4. 求两个集合的交、并、差并存储到指定集合中

   > Sinterstore destination key1 {key2}
   >
   > sunionstore destination key1 {key2}
   >
   > sdiffstore destination key1 {key2}

5. 将指定数据从原始集合移动到目标集合中

   > smove source destination member

## **sorted_set**

### **基本操作**

1. 添加数据

   > zadd key score1 member1 {score2 member2}

2. 获取全部数据

   > zrange key start stop [WINTHSCORES]
   >
   > zrevrange key start stop {WINTHSCORES}

3. 删除数据

   > zrem key member {member …}

4. 按条件获取数据

   > zrangebyscore key min max {WINTHSCORES} {LIMIT}
   >
   > zrevrangebyscore key min max {WINTHSCORES}

5. 条件删除数据

   > zremrangebyrank key start stop
   >
   > zremrangebyscore key min max

6. 获取集合数量总数

   > Zcard key
   >
   > Zcount key min max

7. 集合交、并操作

   > Zinterstore destination numkeys key {key …}    Zunionstore destination numkeys key {key…}

### **拓展操作**

1. 获取数据对应的索引

   > Zrank key member
   >
   > Zrevrank key member

2. Score值获取与修改

   > Zscore key member
   >
   > Zincrby key increment member

3. 获取当前时间

   > Time

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

​       为key改名

​       rename key newkey

​       renamenx key newkey

​       对所有key排序

​       sort

​       其他key通用操作

​       help @generic

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

# **Jedis**

1. 创建连接

   > Jedis jedis = new Jedis(“127.0.0.1”,6379);

2. 添加元素

   > jedis.set(“str”,123);

3.  关闭连接

   > jedis.close();

# **持久化**

> RDB(记录数据)AOF(记录数据产生的过程)

## RDB

>save立即保存,如果时间过长会阻塞客服端指令
>
>bgsave 针对save阻塞问题的优化
>
>save配置
>
>save second changes

## AOF

> always(每次):零误差，性能低
>
> everysec(每秒)：准确性较高，性能较高，可能丢失一秒内的数据
>
> no(系统控制)：整体过程不可控
>
> aof重写（手动重写，自动重写）

# 事务

## 开事务

> multi

1. 对key’添加监视锁，在执行exec前如果key发生了变化，终止事务执行

   > watch key1 {key2…}

2. 取消对所有key的监视

   > unwatch

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
   >周期性抽查存储空间（随机抽查，重点抽查）

​    逐出算法

# 高级数据类型

> **Bitmaps**
>
> **HyperLogLog**
>
> **GEO**

# Redis集群