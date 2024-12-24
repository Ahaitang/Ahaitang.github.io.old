+++
title = 'NOSQL期末复习——Redis'
date = 2024-12-22T01:06:20+08:00
weight=5
categories = ['AHUCM', 'NOSQL', '期末复习','Redis']
images = 'post/avatar.png'
+++
# Redis
## Reids 简介
### Redis是什么？
Redis（Remote Dictionary Server）是一种开源的高性能键值对数据库。它支持多种数据结构，如字符串、哈希表、列表、集合、有序集合等。Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启时可以再次加载进行使用。Redis支持主从复制，可以实现读写分离，提高系统的可靠性。Redis支持数据备份，可以对数据进行备份，防止数据丢失。Redis支持多种编程语言的客户端，如Java、Python、C、C++、PHP、JavaScript、Ruby等。

### Redis的优点有哪些？
1. 性能高：Redis的性能高于关系型数据库。
2. 内存存储：Redis支持内存存储，可以支持高并发访问。
3. 持久化：Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启时可以再次加载进行使用。
4. 主从复制：Redis支持主从复制，可以实现读写分离，提高系统的可靠性。            
5. 多数据结构：Redis支持多种数据结构，如字符串、哈希表、列表、集合、有序集合等。
6. 多编程语言支持：Redis支持多种编程语言的客户端，如Java、Python、C、C++、PHP、JavaScript、Ruby等。

### Redis的缺点有哪些？            
1. 功能不全：Redis不支持完整的关系型数据库的功能，如SQL。            
2. 单线程模型：Redis是单线程模型，不能充分利用多核CPU。            
3. 弱一致性：Redis的一致性是弱一致性，不能保证数据的强一致性。            
4. 缺乏事务支持：Redis不支持事务，只能通过客户端来实现事务功能。            
5. 缺乏ACID保证：Redis不支持ACID保证，不能保证数据的完整性、一致性、隔离性、持久性。

### Redis的应用场景有哪些？
1. 缓存：Redis可以用作缓存，提高系统的响应速度。
2. 计数器：Redis可以用作计数器，比如用户访问计数、商品销量等。            
3. 排行榜：Redis可以用作排行榜，比如热门商品排行榜、实时点赞排行榜等。            
4. 发布/订阅：Redis可以用作发布/订阅系统，比如实时消息通知、实时聊天系统等。            
5. 地理位置：Redis可以用作地理位置信息存储，比如用户的经纬度信息、商品的地理位置信息等。


## Redis 的操作命令
### 通用命令
- 【`ping`】沟通命令，查看状态，返回 PONG
```
root@zimoqiufeng-virtual-machine:/usr/local/redis/redis-7.0.15# redis-cli
127.0.0.1:6379> ping
PONG
```
- 【`dbsize`】查看当前数据库中key的数目
```
127.0.0.1:6379> dbsize
(integer) 1
```
- 【`select db`】redis 默认十六个库，切换库命令
```
127.0.0.1:6379> select 1
OK
```
- 【`flushdb`】删除当前库的数据
```
127.0.0.1:6379[1]> set guanwanli guanwanli
OK
127.0.0.1:6379[1]> dbsize
(integer) 1
127.0.0.1:6379[1]> flushdb
OK
127.0.0.1:6379[1]> dbsize
(integer) 0
```
- 【`quit` or `exit` 】退出当前 Redis 连接
```
quit
```
- 【`redis-cli`】登录自带 redis 命令行客户端
```
redis-cli
```

### Key 的操作命令
- 【`keys pattern`】 查看 key
>通配符:  
>\- ：表示 0-多个字符 ，例如：keys \* 查询所有的 key , \* 表示 0 或多个字符  
>? ：表示单个字符，例如：wo?d , 匹配 word , wood
```
127.0.0.1:6379[1]> keys *
1) "woood"
2) "guanwanli"
3) "wood"
4) "word"

1) 127.0.0.1:6379[1]> keys wo?d
2) "wood"
3) "word"

127.0.0.1:6379[1]> keys w*******d
1) "woood"
2) "wood"
3) "word"
```
- 【`exists key\[key…]`】 判断 key 是否存在
```
exists key
exists key1 key2
127.0.0.1:6379[1]> exists word wood hello
(integer) 2
```
- 【`expire key seconds`】设置 key 的生存时间，超过时间，key 自动删除。单位是秒
```
127.0.0.1:6379[1]> expire wood 3
(integer) 1
127.0.0.1:6379[1]> keys *
1) "woood"
2) "guanwanli"
3) "word"
```
- 【`ttl key`】以秒为单位，返回key的剩余生存时间; 返回值 -1: 没有设置key的生存时间，key永不过期 ;  -2：key不存在
```
127.0.0.1:6379[1]> expire woood 100
(integer) 1
127.0.0.1:6379[1]> ttl woood
(integer) 94
```
- 【`type key`】查看key 所存储值的数据类型返回值：字符串表示的数据类型
> - none (key 不存在)
> - string(字符串)
> - list(列表)
> - set(集合)
> - zset(有序集)
> - hash(哈希表)
```
127.0.0.1:6379[1]> type word
string
```


- 【`del key \[key`】删除指定存在的 key ，不存在的 key 忽略。返回值：数字，删除的 key 的数量
```
127.0.0.1:6379[1]> del redis
(integer) 0
127.0.0.1:6379[1]> del word
(integer) 1
```

## Redis 的数据类型
### 字符串类型的 value 操作命令
- 【`set key value`】将字符串值 value 设置到 key 中，已经存在的 key 设置新的 value，会覆盖原来的值。
```
127.0.0.1:6379[1]> set GitCode GitCode
OK
127.0.0.1:6379[1]> set InsCode InsCode
OK
127.0.0.1:6379[1]> set Blog blog
OK
```
- 【`get key`】 获取 key 中设置的字符串值
```
127.0.0.1:6379[1]> get GitCode
"GitCode"
```
- 【`incr key`】将 key 中储存的数字值加 1，如果 key 不存在，则 key 的值先被初始化为 0 再执行 incr 操作（只能对数字类型的数据操作）
```
127.0.0.1:6379[1]> set index 1
OK
127.0.0.1:6379[1]> incr index
(integer) 2
127.0.0.1:6379[1]> get index
"2"
```
- 【decr key】将 key 中储存的数字值减1，如果 key 不存在，则么 key 的值先被初始化为 0 再执行 decr 操作（只能对数字类型的数据操作）
```
127.0.0.1:6379[1]> decr index
(integer) 1
127.0.0.1:6379[1]> decr myindex
(integer) -1
```
- 【`append key value`】 如果 key 存在， 则将 value 追加到 key 原来旧值的末尾如果 key 不存在， 则将再key 设置值为 value，返回值：追加字符串之后的总长度
```
127.0.0.1:6379[1]> get GitCode
"GitCode"
127.0.0.1:6379[1]> append GitCode 13
(integer) 9
127.0.0.1:6379[1]> get GitCode
"GitCode13"
```
- 【`strlen key`】返回 key 所储存的字符串值的长度返回值：如果key存在，返回字符串值的长度 key 不存在，返回0

```
127.0.0.1:6379[1]> set str "hello world"
OK
127.0.0.1:6379[1]> strlen str
(integer) 11
```
- 【`getrange key start end`】获取 key 中字符串值从 start 开始 到 end 结束 的子字符串,包括 start 和 end,负数表示从字符串的末尾开始， -1 表示最后一个字符返回值：截取的子字符串。
```
127.0.0.1:6379[1]> set str "hello world"
OK
127.0.0.1:6379[1]> getrange str 0 5
"hello"
127.0.0.1:6379[1]> getrange str -3 -1
"rld"
```
- 【`setrange key offset value`】将 value 值插入到 key 字符串值指定偏移量处，原字符串值被覆盖。返回值：插入操作之后，字符串的长度。
```
127.0.0.1:6379[1]> set str "hello world"
OK
127.0.0.1:6379[1]> setrange str 6 "redis"
(integer) 12
127.0.0.1:6379[1]> get str
"hello redis"
```
- 【`mset key value \[key value]`】同时设置多个 key-value 对，返回值：OK
```
127.0.0.1:6379[1]> mset name "guanwanli" age 25
OK
```
- 【`mget key \[key]`】获取所有(一个或多个)给定 key 所对应的值。返回值：一个列表，包含所有给定 key 的值。
```
127.0.0.1:6379[1]> mget name age
1) "guanwanli"
2) "25"
```
- 【`msetnx key value \[key value]`】 同时设置多个 key-value 对，当且仅当所有给定 key 都不存在。返回值：OK，表示设置成功；如果给定 key 已经存在，则不进行任何操作。
```
127.0.0.1:6379[1]> msetnx name "guanwanli" age 25
(integer) 1
```
### 哈希类型的value操作命令
- 【`hset hash 表的 key field value`】哈希类型field（域 ）和 value 的隐射表，value分为field和value，hset可将key中的值设置为value，如果 key 不存在，则新建 hash 表，执行赋值，如果有 field ,则覆盖值。
返回值：  
如果 field 是 hash 表中新 field，且设置值成功，返回 1  
如果 field 已经存在，旧值覆盖新值，返回 0
```
127.0.0.1:6379[1]> hset user1 name zhangsan
(integer) 1
127.0.0.1:6379[1]> hset user1 name lisi
(integer) 0
```
- 【`hget key field`】获取 hash 表中指定 key 的 value 值。
> 返回值：field 域的值，如果 key 不存在或者 field 不存在返回 nil
```
127.0.0.1:6379[1]> hget user1 name
"zhangsan"
127.0.0.1:6379[1]> hget user1 age
(nil)
```
- 【`hgetall key`】获取 hash 表中指定 key 的所有 field-value 值。
> 返回值：一个包含所有 field-value 对的表，表的长度是 field 数量。
```
127.0.0.1:6379[1]> hgetall user1
1) "name"
2) "zhangsan"
```
- 【`hmset key field value \[field value]`】同时设置多个 field-value 对，返回值：OK
```
127.0.0.1:6379[1]> hmset user1 age 25 email <EMAIL>
OK
```
- 【`hmget key field \[field]`】获取 hash 表中指定 key 的多个 field 域的值。
> 返回值：一个包含所有 field 域的值的表，表的长度是 field 数量。
```
127.0.0.1:6379[1]> hmget user1 name age email
1) "zhangsan"
2) "25"
3) "<EMAIL>"
```
- 【`hdel key field \[field]`】删除 hash 表中指定 key 的指定 field 域，返回值：被删除的 field 数量。
```
127.0.0.1:6379[1]> hdel user1 age
(integer) 1
127.0.0.1:6379[1]> hgetall user1
1) "name"
2) "zhangsan"
3) "email"
4) "<EMAIL>"
```
- 【`hkeys key`】获取 hash 表中指定 key 的所有 field 域。
> 返回值：一个包含所有 field 域的表。
```
127.0.0.1:6379[1]> hkeys user1
1) "name"
2) "email"
```
- 【`hvals key`】获取 hash 表中指定 key 的所有 value 值。
> 返回值：一个包含所有 value 值的表。
```
127.0.0.1:6379[1]> hvals user1
1) "zhangsan"
2) "<EMAIL>"
```
- 【`hexists key field`】查看 hash 表中指定 key 的指定 field 域是否存在。
> 返回值：1 存在，0 不存在。
```
127.0.0.1:6379[1]> hexists user1 name
(integer) 1
127.0.0.1:6379[1]> hexists user1 age
(integer) 0
```
- 【`hlen key`】获取 hash 表中指定 key 的 field 数量。
> 返回值：field 数量。
```
127.0.0.1:6379[1]> hlen user1
(integer) 2
```
- 【`hincrby key field increment`】将 hash 表中指定 key 的指定 field 域的值加上增量 increment。
> 返回值：增量后的 field 域的值。
```
127.0.0.1:6379[1]> hincrby user1 age 1
(integer) 26
127.0.0.1:6379[1]> hincrby user1 age 10
(integer) 36
```
### 列表类型的value操作命令
- 【`lpush key value1 \[value2]`】将一个或多个值 value 插入到列表 key 的表头，返回值：插入操作之后，列表的长度。
```
127.0.0.1:6379[1]> lpush mylist 1 2 3
(integer) 3
```
- 【`rpush key value1 \[value2]`】将一个或多个值 value 插入到列表 key 的表尾，返回值：插入操作之后，列表的长度。
```
127.0.0.1:6379[1]> rpush mylist 4 5 6
(integer) 6
```
- 【`llen key`】获取列表 key 的长度。
> 返回值：列表的长度。
```
127.0.0.1:6379[1]> llen mylist
(integer) 6
```
- 【`lrange key start end`】获取列表 key 中指定区间内的元素，区间以偏移量 start 和 end 指定。 -1 表示列表的最后一个元素， start ，stop 超出列表的范围不会出现错误。
> 返回值：一个包含指定区间内元素的列表。
```
127.0.0.1:6379[1]> lrange mylist 0 3
1) "1"
2) "2"
3) "3"
4) "4"
```
- 【`lindex key index`】通过索引获取列表中的元素。
> 返回值：指定索引的元素。
```
127.0.0.1:6379[1]> lindex mylist 2
"3"
```
- 【`lset key index value`】通过索引设置列表元素的值。
> 返回值：OK。
```
127.0.0.1:6379[1]> lset mylist 2 9
OK
127.0.0.1:6379[1]> lrange mylist 0 3
1) "1"
2) "2"
3) "9"
4) "4"
```
- 【`lpop key`】移除并返回列表 key 的头元素。
> 返回值：列表的头元素。
```
127.0.0.1:6379[1]> lpop mylist
"1"
```
- 【`rpop key`】移除并返回列表 key 的尾元素。
> 返回值：列表的尾元素。
```
127.0.0.1:6379[1]> rpop mylist
"4"
```
- 【`lrem key count value`】移除列表中与参数 value 相等的元素
> count >0 ，从列表的左侧向右开始移除； count < 0 从列表的尾部开始移除；count = 0 移除表中所有与 value 相等的值。     
> 返回值：被移除元素的数量。
```
127.0.0.1:6379[1]> lrem mylist 1 2
(integer) 1
127.0.0.1:6379[1]> lrange mylist 0 -1
1) "1"
2) "9"
3) "4"
```
- 【`linsert key before|after pivot value`】值 value 插入到列表 key 当中位于值 pivot 之前或之后的位置。key 不存在，pivot 不在列表中，不执行任何操作。
> 返回值：命令执行成功，返回新列表的长度。没有找到 pivot 返回 -1， key 不存在返回 0
```
127.0.0.1:6379[1]> linsert mylist before 9 8
(integer) 4
127.0.0.1:6379[1]> lrange mylist 0 -1
1) "1"
2) "8"
3) "9"
4) "4"
```
### 集合类型的value操作命令
- 【`sadd key member1 \[member2]`】将一个或多个 member 元素加入到集合 key 当中，已经存在于集合的 member 元素将被忽略。
> 返回值：被成功添加的元素的数量。
```
127.0.0.1:6379[1]> sadd myset 1 2 3
(integer) 3
```
- 【`smembers key`】返回集合 key 中的所有成员。
> 返回值：一个包含集合中所有成员的表。
```
127.0.0.1:6379[1]> smembers myset
1) "1"
2) "2"
3) "3"
```
- 【`scard key`】返回集合 key 的基数(集合中元素的数量)。
> 返回值：集合的基数。
```
127.0.0.1:6379[1]> scard myset
(integer) 3
```
- 【`sismember key member`】判断 member 元素是否是集合 key 的成员。
> 返回值：1 元素是集合的成员，0 元素不是集合的成员。
```
127.0.0.1:6379[1]> sismember myset 2
(integer) 1
127.0.0.1:6379[1]> sismember myset 4
(integer) 0
```
- 【`srem key member1 \[member2]`】移除集合 key 中的一个或多个 member 元素，不存在的 member 元素会被忽略。
> 返回值：被成功移除的元素的数量。
```
127.0.0.1:6379[1]> srem myset 2
(integer) 1
127.0.0.1:6379[1]> smembers myset
1) "1"
2) "3"
```
- 【`spop key`】随机移除并返回集合 key 中的一个元素。
> 返回值：被移除的元素。
```
127.0.0.1:6379[1]> spop myset
"1"
127.0.0.1:6379[1]> smembers myset
1) "3"
```
- 【`srandmember key [count]`】随机返回集合 key 中的 count 个元素。
> 返回值：一个包含 count 个随机元素的表。
```
127.0.0.1:6379[1]> srandmember myset
"3"
127.0.0.1:6379[1]> srandmember myset 2
1) "1"
2) "3"
```
- 【`sinter key1 \[key2]`】返回给定所有集合的交集。
> 返回值：一个包含交集元素的表。
```
127.0.0.1:6379[1]> sadd set1 1 2 3
(integer) 3
127.0.0.1:6379[1]> sadd set2 2 3 4
(integer) 3
127.0.0.1:6379[1]> sinter set1 set2
1) "2"
2) "3"
```
- 【`sinterstore destination key1 \[key2]`】将给定集合的交集存储在 destination 集合中。
> 返回值：destination 集合的基数。
```
127.0.0.1:6379[1]> sinterstore set3 set1 set2
(integer) 2
127.0.0.1:6379[1]> smembers set3
1) "2"
2) "3"
```
- 【`sunion key1 \[key2]`】返回给定所有集合的并集。
> 返回值：一个包含并集元素的表。
```
127.0.0.1:6379[1]> sunion set1 set2
1) "1"
2) "2"
3) "3"
4) "4"
```
- 【`sunionstore destination key1 \[key2]`】将给定集合的并集存储在 destination 集合中。
> 返回值：destination 集合的基数。
```
127.0.0.1:6379[1]> sunionstore set4 set1 set2
(integer) 4
127.0.0.1:6379[1]> smembers set4
1) "1"
2) "2"
3) "3"
4) "4"
```
- 【`sdiff key1 \[key2]`】返回给定所有集合的差集。
> 返回值：一个包含差集元素的表。
```
127.0.0.1:6379[1]> sdiff set1 set2
1) "1"
```
- 【`sdiffstore destination key1 \[key2]`】将给定集合的差集存储在 destination 集合中。
> 返回值：destination 集合的基数。
```
127.0.0.1:6379[1]> sdiffstore set5 set1 set2
(integer) 1
127.0.0.1:6379[1]> smembers set5
1) "1"
``` 
### 有序集合类型的value操作命令
- 【`zadd key score1 member1 \[score2 member2]`】将一个或多个成员 member 及其分数 score 加入到有序集 key 当中。
> 返回值：被成功添加的元素的数量。
```
127.0.0.1:6379[1]> zadd myzset 1 one 2 two 3 three
(integer) 3
```
- 【`zrange key start end \[withscores]`】通过索引区间返回有序集 key 中，指定区间内的成员。
>tart， stop 都是从 0 开始。0 是第一个元素，1 是第二个元素。以 -1 表示最后一个成员，-2 表示倒数第二个成员。使用WITHSCORES 选项让 score 和 value 一同返回。  
> 返回值：一个包含指定区间内的成员的列表。
```
127.0.0.1:6379[1]> zrange myzset 0 -1
1) "one"
2) "two"
3) "three"
```
- 【`zrevrange key start end \[withscores]`】通过索引区间返回有序集 key 中，指定区间内的成员。
>tart， stop 都是从 0 开始。0 是第一个元素，1 是第二个元素。以 -1 表示最后一个成员，-2 表示倒数第二个成员。使用WITHSCORES 选项让 score 和 value 一同返回。  
> 返回值：一个包含指定区间内的成员的列表。
```
127.0.0.1:6379[1]> zrevrange myzset 0 -1
1) "three"
2) "two"
3) "one"
``` 
- 【`zrangebyscore key min max \[withscores]`】返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。
> 返回值：一个包含指定区间内的成员的列表。
```
127.0.0.1:6379[1]> zrangebyscore myzset 1 2
1) "one"
2) "two"
```
- 【`zrevrangebyscore key max min \[withscores]`】返回有序集 key 中，所有 score 值介于 max 和 min 之间(包括等于 max 或 min )的成员。有序集成员按 score 值递减(从大到小)次序排列。
> 返回值：一个包含指定区间内的成员的列表。
```
127.0.0.1:6379[1]> zrevrangebyscore myzset 2 1
1) "two"
2) "one"
```
- 【`zcard key`】返回有序集 key 的基数。
> 返回值：有序集的基数。
```
127.0.0.1:6379[1]> zcard myzset
(integer) 3
```
- 【`zscore key member`】返回有序集 key 中，成员 member 的 score 值。
> 返回值：成员的 score 值。
```
127.0.0.1:6379[1]> zscore myzset one
"1"
```
- 【`zrem key member1 \[member2]`】移除有序集 key 中的一个或多个成员，不存在的成员将被忽略。
> 返回值：被成功移除的元素的数量。
```
127.0.0.1:6379[1]> zrem myzset two
(integer) 1
127.0.0.1:6379[1]> zrange myzset 0 -1
1) "one"
2) "three"
```
- 【`zremrangebyrank key start stop`】移除有序集 key 中，指定排名(rank)区间内的所有成员。
> 返回值：被移除成员的数量。
```
127.0.0.1:6379[1]> zremrangebyrank myzset 0 1
(integer) 2
127.0.0.1:6379[1]> zrange myzset 0 -1
1) "three"
```
- 【`zremrangebyscore key min max`】移除有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。
> 返回值：被移除成员的数量。
```
127.0.0.1:6379[1]> zremrangebyscore myzset 1 2
(integer) 1
127.0.0.1:6379[1]> zrange myzset 0 -1
1) "three"
```
- 【`zcount key min max`】返回有序集 key 中， score 值在 min 和 max 之间(包括等于 min 或 max )的成员的数量。
> 返回值：指定范围内，成员的数量。
```
127.0.0.1:6379[1]> zcount myzset 1 2
(integer) 2
```
- 【`zunionstore destination numkeys key1 \[key2]`】计算给定的一个或多个有序集的并集，并存储在新的有序集合 destination 中。
> 返回值：destination 集合的基数。
```
127.0.0.1:6379[1]> zadd zset1 1 one 2 two 3 three
(integer) 3
127.0.0.1:6379[1]> zadd zset2 2 two 3 three 4 four
(integer) 4
127.0.0.1:6379[1]> zunionstore myzset 2 zset1 zset2
(integer) 4
127.0.0.1:6379[1]> zrange myzset 0 -1 withscores
1) "one"
2) "1"
3) "two"
4) "2"
5) "three"
6) "3"
7) "four"
8) "4"
```
- 【`zinterstore destination numkeys key1 \[key2]`】计算给定的一个或多个有序集的交集，并存储在新的有序集合 destination 中。
> 返回值：destination 集合的基数。
```
127.0.0.1:6379[1]> zinterstore myzset 2 zset1 zset2
(integer) 2
127.0.0.1:6379[1]> zrange myzset 0 -1 withscores
1) "two"
2) "2"
3) "three"
4) "3"
``` 

### Jedis 
#### 引入 JAR 包
```xml
<dependency>  
	<groupId>redis.clients</groupId>  
	<artifactId>jedis</artifactId>  
	<version>5.1.0</version>  
</dependency>  
  
<dependency>  
	<groupId>com.alibaba</groupId>  
	<artifactId>fastjson</artifactId>  
	<version>1.2.62</version>  
</dependency>
```

#### 连接数据库
```JAVA
// 创建Jedis对象  
Jedis jedis = new Jedis("localhost", 6379);
```
#### 对数据类型进行操作
```JAVA
/*  
todo: String  
*/  
public void redisStr(Jedis jedis) {  
// 字符串操作  
jedis.set("name", "admin");  
System.out.println("取值：" + jedis.get("name"));  
System.out.println("----------");  
  
// 取旧值 赋新值  
String oldName = jedis.getSet("name", "zhangSan");  
System.out.println("旧值:" + oldName);  
System.out.println("新值:" + jedis.get("name"));  
System.out.println("----------------------");  
  
// 一次为多个字符串设置值  
jedis.mset("k1", "v1", "k2", "v2", "k3", "v3");  
List<String> list = jedis.mget("k1", "k2", "k3");  
System.out.println("取值：");  
for (String str : list) {  
System.out.print(str + " ");  
}  
System.out.println("----------------------");  
  
// 判断指定的字符串 类型的key存不存在  
Boolean flag = jedis.exists("name1234");  
System.out.println("判断key是否存在：" + flag);  
System.out.println("----------");  
  
// 进行数值类型的递增操作  
jedis.set("num", "9");  
System.out.println("递增后的值：" + jedis.incr("num"));  
System.out.println("----------");  
  
// 获取字符串长度  
System.out.println("字符串长度：" + jedis.strlen("name"));  
System.out.println("----------");  
  
// 从指定位置替换字符串  
jedis.setrange("name", 5, "san");  
System.out.println("替换后的值：" + jedis.get("name"));  
System.out.println("----------");  
  
// 追加字符串  
jedis.append("name", "lisi");  
System.out.println("追加后的值：" + jedis.get("name"));  
System.out.println("----------");  
  
// 批量删除  
jedis.del("k1", "k2", "k3");  
System.out.println("----------");  
  
}  
  
  
/*  
todo: Hash  
*/  
//hash的操作  
public void redisHash(Jedis jedis) {  
// 赋值  
jedis.hset("h1", "k1", "123");  
String k1 = jedis.hget("h1", "k1");  
System.out.println("取值：" + k1);  
System.out.println("------------------");  
  
// Hmset() 批量赋值  
Map<String, String> map = new HashMap<String, String>();  
map.put("k2", "123");  
map.put("k3", "124");  
jedis.hmset("h1", map);  
  
// Hmget() 批量取值  
List<String> list = jedis.hmget("h1", "k1", "k2", "k3");  
System.out.println("批量取值：");  
for (String str : list) {  
System.out.print(str + " ");  
}  
System.out.println("------------------");  
  
// Hgetall() 取出所有键值对  
Map<String, String> map1 = jedis.hgetAll("h1");  
System.out.println("取全部键值：");  
for (String key : map1.keySet()) {  
System.out.println(key + ":" + map1.get(key));  
}  
System.out.println("----------");  
  
// Hexists() 判断指定的key是否存在  
boolean flag = jedis.hexists("h1", "k4");  
System.out.println("判断key是否存在：" + flag);  
System.out.println("----------");  
  
// HincrBy() 数值类型的递增操作  
Long l = jedis.hincrBy("h1", "k4", -2);  
System.out.println("递增后的值：" + l);  
System.out.println("----------");  
  
// Hlen() 获取hash表的长度  
System.out.println("hash表长度：" + jedis.hlen("h1"));  
System.out.println("----------");  
  
// Hdel() 删除指定的key  
jedis.hdel("h1", "k4");  
  
}  
  
/*  
todo: List  
*/  
//List的操作  
public void redisList(Jedis jedis) {  
// 删除全部元素  
jedis.del("l1");  
System.out.println("----------");  
// 赋值  
Long len;  
len = jedis.lpush("l1", "王五");  
System.out.println("左添加：" + len);  
len = jedis.rpush("l1", "张三");  
System.out.println("右添加：" + len);  
System.out.println("----------");  
  
// 弹出列表元素  
String str = jedis.lpop("l1");  
System.out.println("左弹出：" + str);  
str = jedis.rpop("l1");  
System.out.println("右弹出：" + str);  
str = jedis.rpop("l2");  
System.out.println("空值弹出：" + str);  
System.out.println("----------");  
  
// 列表长度  
jedis.rpush("l1", "李四");  
jedis.lpush("l1", "张三");  
jedis.rpush("l1", "王二");  
System.out.println("列表长度：" + jedis.llen("l1"));  
System.out.println("----------");  
  
// 列表元素是否存在  
long flag = jedis.lrem("l1", 1, "张三");  
System.out.println(flag);  
System.out.println("----------");  
  
// Lindex() 获取指定索引的元素  
str = jedis.lindex("l1", 0);  
System.out.println("索引为0的元素：" + str);  
System.out.println("----------");  
  
// larnge() 获取指定范围的元素  
List<String> list = jedis.lrange("l1", 0, -1);  
System.out.println("列表元素0 ~ -1：");  
for (String s : list) {  
System.out.print(s + " ");  
}  
}  
  
/*  
todo: Set  
*/  
public void redisSet(Jedis jedis) {  
// 赋值  
jedis.sadd("s1", "a", "b", "c", "d", "e");  
System.out.println("添加元素：" + jedis.smembers("s1"));  
System.out.println("----------");  
  
// 移除元素  
long lon;  
lon = jedis.srem("s1", "g");  
System.out.println("移除个数元素：" + lon);  
System.out.println("----------");  
  
// SMove() 移动元素  
jedis.sadd("s2", "e", "f");  
jedis.smove("s1", "s2", "d");  
System.out.println("移动元素：" + jedis.smembers("s1") +  
" " + jedis.smembers("s2"));  
System.out.println("----------");  
  
// 元素个数  
System.out.println("元素个数：" + jedis.scard("s1"));  
// 全部元素  
System.out.println("全部元素：" + jedis.smembers("s1"));  
System.out.println("----------");  
  
// 判断元素是否存在  
boolean flag = jedis.sismember("s1", "a");  
System.out.println("元素是否存在：" + flag);  
System.out.println("----------");  
  
// 取交集  
jedis.sadd("s2", "a");  
Set<String> set1 = jedis.sinter("s1", "s2");  
System.out.println("取交集：" + set1);  
System.out.println("----------");  
  
// 取并集  
Set<String> set2 = jedis.sunion("s1", "s2");  
System.out.println("取并集：" + set2);  
System.out.println("----------");  
// 取差集  
Set<String> set3 = jedis.sdiff("s1", "s2");  
System.out.println("取差集：" + set3);  
System.out.println("----------");  
  
}  
  
  
/*  
todo: Sorted Set  
*/  
//Sorted Set的操作  
public void redisSortedSet(Jedis jedis) {  
// 赋值  
jedis.zadd("z1", 10, "a");  
// 批量赋值  
Map<String, Double> map = new HashMap<String, Double>();  
map.put("b", 20.0);  
map.put("c", 30.0);  
long lon;  
lon = jedis.zadd("z1", map);  
System.out.println("添加元素个数：" + lon);  
System.out.println("----------");  
  
// 移除元素  
lon = jedis.zrem("z1", "g");  
System.out.println("移除个数元素：" + lon);  
System.out.println("----------");  
  
// 元素个数  
System.out.println("元素个数：" + jedis.zcard("z1"));  
  
// 全部元素  
System.out.println("全部元素：" + jedis.zrange("z1", 0, -1));  
System.out.println("----------");  
  
// 取指定分数范围的元素数量  
lon = jedis.zcount("z1", 20, 40);  
System.out.println("分数范围元素个数：" + lon);  
System.out.println("----------");  
  
// 按分数排序(从小到大)  
List<String> z1 = jedis.zrangeByScore("z1", 0, 100);  
System.out.println("按分数排序(从小到大)：" + z1);  
System.out.println("----------");  
  
// 按分数排序(从大到小)  
List<String> z2 = jedis.zrevrangeByScore("z1", 100, 0);  
System.out.println("按分数排序(从大到小)：" + z2);  
System.out.println("----------");  
  
// 按元素排序并显示成绩  
List<Tuple> z3 = jedis.zrangeByScoreWithScores("z1", 0, 100);  
for (Tuple tuple : z3) {  
System.out.println(tuple.getElement() + ":" + tuple.getScore());  
}  
System.out.println("----------");  
}  
  
/*  
todo: pub/sub  
*/  
//发布订阅的操作  
public void redisPubSub(Jedis jedis) {  
// 创建订阅者  
JedisPubSub subscriber = new JedisPubSub() {  
@Override  
public void onMessage(String channel, String message) {  
// 处理订阅到的消息  
System.out.println("Received message: " + message + " from channel: " + channel);  
}  
};  
  
// 订阅频道  
jedis.subscribe(subscriber, "news");  
System.out.println("----------");  
// 在另一个线程中发布消息  
new Thread(() -> {  
try {  
Thread.sleep(1000);  
jedis.publish("news", "Hello, World!");  
} catch (InterruptedException e) {  
e.printStackTrace();  
}  
}).start();  
  
// 等待接收消息  
try {  
Thread.sleep(2000);  
} catch (InterruptedException e) {  
e.printStackTrace();  
}  
  
// 取消订阅  
subscriber.unsubscribe();  
}
```

## 题目
### 一、选择题
1. 关于Redis的RDB持久化策略，说法错误的是 C
   - A. RDB持久化是将当前进程数据以生成快照的方式保存到硬盘的过程
   - B. Redis默认的持久化机制是RDB持久化机制
   - C. RDB持久化模式可以做到实时的持久化间隔
   - D. 执行BGSAVE命令时要执行fork操作创建子进程

2. 下面关于Redis的应用场景，错误的说法是（）A
   - A. Redis作为数据库实现海量数据的存储
   - B. 作为中间件广泛应用于分布式缓存
   - C. Redis作为计算工具统计PV、UV等数据
   - D. Redis可以实现分布式锁

3. 下面关于Redis中zset数据类型与list数据类型的比较，错误的说法是 () D
   - A. zset与list中的数据都是有序的
   - B. zset相较于list更耗内存
   - C. zset相较于list访问中间元素更快
   - D. zset与list相比的底层数据结构都是链表

4. 下面关于Redis中RDB与AOF两种持久化机制的比较，错误的是（）D
   - A. 对业务数据敏感的应用场景选用AOF持久化机制
   - B. 追求大数据集恢复速度的应用场景选用RDB持久化机制
   - C. AOF持久化机制比RDB持久化机制的存储速度快
   - D. AOF持久化机制比RDB持久化机制的恢复速度快

5. 下面不是Redis的命令的有? A
   - A. clean
   - B. smem SMEMBERS KEY属于SET数据结构的命令用来获取key中所有元素
   - C. keys
   - D. lrange

6. 下面关于非关系型数据库Redis与MongoDB的比较，正确的说法是（）A
   - A. MongoDB将数据存放在内存，当内存不够时，只将热点数据放入内存，其他数据存在磁盘；Redis将数据全部存在内存，当内存不够时，可以选择指定的LRU算法删除数据
   - B. MongoDB和Redis都支持事务
   - C. Redis内置数据分析功能，MongoDB不支持
   - D. MongoDB支持的查询语言丰富，Redis支持的数据类型丰富

7. 下面关于Redis 6.0版本中的操作命令，正确的说法是（）C
   - A. 使用ttl key时返回-1，表示该key已经过期
   - B. dbsize查看数据库的空间大小
   - C. flushall 删除所有数据库的所有 key
   - D. selectdb用于切换数据库

8. 下面关于Redis持久化机制的说法，错误的是（）A 
   - A. Redis的默认采用AOF持久化机制
   - B. RDB持久化机制是以快照形式存储数据结果，存储格式简单
   - C. AOF持久化机制是以日志形式存储操作过程，存储格式复杂
   - D. 持久化的目的是防止数据的意外丢失，确保数据安全性

9. 下面关于Redis中SDS（简单动态字符串）的说法，错误的是（）C
   - A. SDS在C字符串的基础上加入了free和len字段
   - B. SDS可以实现修改字符串时内存的重分配
   - C. SDS可以存取二进制数据
   - D. 由于SDS记录了长度，可以杜绝缓冲区溢出

10下面关于Redis支持的HyperLogLog数据类型，错误的说法是（）D
    - A. HyperLogLog数据类型的应用场景包括统计网站的UV(独立访客)
    - B. 应用HyperLogLog数据类型时可能存在一定的错误率
    - C. HyperLogLog数据类型占用内存是固定的
    - D. HyperLogLog数据类型通过存储输入元素加速基数统计

11. 下面关于Redis中RDB与AOF两种持久化机制的比较，错误的是（）D
    - A. 对业务数据敏感的应用场景选用AOF持久化机制
    - B. 追求大数据集恢复速度的应用场景选用RDB持久化机制
    - C. AOF持久化机制比RDB持久化机制的存储速度快
    - D. AOF持久化机制比RDB持久化机制的恢复速度快

12. 下面关于Redis中string数据类型的数据结构，正确的说法是（）D
    - A. string的数据类型是简单静态字符串（simple static string）
    - B. string的内部结构实现上类似Java的HashMap
    - C. string进行扩容时是加倍现有空间
    - D. string采用预分配冗余空间的方式来减少内存的频繁分配

13. 下面关于Redis中配置文件的说法，正确的是（）B
    - A. Redis同时支持bytes和bit两种单位
    - B. 当Redis以“includes”的方式引入其他配置文件时，如果同一个配置项在不同配置文件中都有定义，那么以最后面的配置项为准
    - C. 在高并发情况下设置较低的tcp-backlog值以避免TCP的慢连接问题
    - D. Redis支持通过loglevel配置项设置日志等级，共分三级，即debug、notice、warning

14. 下面关于Redis中set数据类型的操作指令，正确的是（）C
    - A. 执行smembers \<key> 命令可以获取该集合的元素个数
    - B. 执行spop \<key> 将从集合中吐出第一个值
    - C. sismember \<key> \<value>用于判断成员元素是否是集合的成员
    - D. sunion \<key1> \<key2> 返回两个集合的交集元素

15. 关于Redis的持久化，下列描述错误的是（）C
    - A. RDB是以快照的形式，将内存中的数据整体拷贝到硬盘上。
    - B. 执行RDB存储时会产生阻塞，因此RDB不适合实时备份，而适合定时备份。
    - C. AOF是以日志形式，将内存中的数据整体拷贝到硬盘上
    - D. AOF操作的实时性好，但是产生的数据体积大，数据的恢复速度慢。

16. 下面不属于Redis中主从复制机制缺陷的是（）A
    - A. 无法实现数据冗余
    - B. 故障恢复无法自动化
    - C. 写操作无法负载均衡
    - D. 存储能力受到单机的限制

17. 下面关于Redis数据存储中redisObject对象的说法，错误的是（）C
    - A. type字段表示对象的类型，占4个比特
    - B. encoding表示对象的内部编码，占4个比特
    - C. lru记录对象最后一次被命令程序访问的时间，占24个比特
    - D. refcount记录的是该对象被引用的次数，占4个字节

18. 下面关于通过执行Redis命令搭建集群的说法，正确的是（）D
    - A. 各节点在启动节点阶段建立联系
    - B. 集群配置文件需要人工修改
    - C. 当数据库中半数的槽分配了节点，集群处于上线状态
    - D. 集群中指定主从关系使用cluster replicate命令

19. 下面关于Redis支持的list数据类型，错误的说法是（）D
    - A. list数据类型具有单键多值的特点
    - B. list数据类型底层是双向链表
    - C. list数据类型数据有序
    - D. list数据类型数据不支持索引下标操作

20. redis在有序集合中在数据量极少（小于128个）的情况下使用以下哪种存储方案 C
    - A. 压缩表
    - B. 跳跃表
    - C. 散列表
    - D. 双向链表

21. Redis支持的数据结构包括（）。 **F**
   - a) 字符串
   - b) 列表
   - c) 散列
   - d) 集合
   - e) 有序集合
   - f) 所有上述

22. Redis使用什么进行数据持久化。（） **B**
   - a) 内存
   - b) 硬盘
   - c) 缓存
   - d) 所有上述

23. Redis可以通过（）进行集群部署。 **D**
   - a) 主从复制
   - b) 分片
   - c) 哨兵
   - d) 所有上述

24. Redis的命令操作是（）的。 **A**
   - a) 同步的
   - b) 异步的
   - c) 半同步半异步的
   - d) 都有可能

25. Redis可以通过什么实现发布-订阅模式（）。 **B**
   - a) Keyspace Notifications
   - b) Pub-Sub
   - c) Event-driven
   - d) 所有上述

### 二、简答题
1. 请列举Redis的持久化方式及其特点？  
   Redis提供两种方式进行持久化：
   - **RDB**：将Redis中数据定时追加到硬盘。
     - **优点**：
       - 整个Redis数据库将只包含一个文件，灾难性故障时易于恢复。
       - 性能最大化，fork出子进程进行持久化，避免服务进程执行IO操作。
       - 启动效率高，尤其在数据集较大时。
     - **缺点**：
       - 数据易丢失，定时持久化之前宕机会导致未写入数据丢失。
       - 大数据集的fork可能导致服务暂停几百毫秒到1秒。

   - **AOF**：将Redis的操作日志以追加的方式写入文件。
     - **优点**：
       - 数据安全性高，提供多种同步策略（每秒同步、每修改同步等）。
       - 写入日志文件时采用append模式，宕机不会破坏已有内容。
       - 支持自动rewrite机制，保持日志文件的合理大小。
       - 日志格式清晰，易于理解和重建数据。
     - **缺点**：
       - AOF文件通常比RDB文件大，恢复大数据集速度较慢。
       - 运行效率往往低于RDB。

2. Redis的发布订阅模式是如何工作的？  
   **工作流程**：
   1. **订阅（Subscribe）**：客户端连接到Redis服务器，并订阅一个或多个频道（channel）。
   2. **发布（Publish）**：当发布者向频道发送消息时，Redis服务器将消息传递给所有订阅了该频道的客户端。
   3. **接收（Receive）**：订阅者接收到消息，并可以据此进行相应的处理。

3. Redis有哪些数据类型？ 

4. Redis的配置参数有哪些？ 
   - Maxclients
   - Maxmemory

5. 为什么Redis需要把所有数据放到内存中？

### 三、编程题
1. Redis默认十六个库，切换库命令。
2. 查看当前数据库中key的数目。
3. 查看key所存储值的数据类型返回值。
4. 删除指定存在的key，不存在的key忽略。
5. 用value覆盖（替换）key的存储的值从offset开始，不存在的key做空白字符串。
6. 同时设置一个或多个key-value对。
7. 获取哈希表key中所有的域和值。
8. 查询获取列表key中下标为指定index的元素（0表示列表的第一个元素，start, stop是列表的下标值，-1表示列表的最后一个元素）。
9. 获取集合key中的所有成员元素，不存在的key视为空集合。

## 参考
- [Redis命令参考](https://redis.io/commands)
- [Redis命令大全](https://www.runoob.com/redis/redis-tutorial.html)




