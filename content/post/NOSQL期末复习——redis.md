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
## 参考
- [Redis命令参考](https://redis.io/commands)
- [Redis命令大全](https://www.runoob.com/redis/redis-tutorial.html)




