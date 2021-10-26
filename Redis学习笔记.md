---
title: Redis学习笔记
date: 2020-11-11 15:27:39
tags:
	- redis
	- 数据库
categories: 
	- redis
---

## Redis学习笔记

NoSQL（Not only SQL）

泛指非关系型数据库

<!--more-->

### Redis数据类型

#### String

一个 key 对应一个 value，二进制安全，可以存放jpg图片或者序列化的对象

```
set key value
get key
keys * // 获取所有key
```

```
incr key 自增1
decr key 自减1
INCRBY key 10 增10
DECRBY key 10 减10
```

```
GETRANGE key 0 3 获取字符串的0-3
GETRANGE key 0 -1 获取整个字符串
SETRANGE key 1 xxxxx 替换指定开始位置字符串
```

```
setex(set with expire)设置过期时间
setnx(set if not exist)不存在在设置

setex key 30 "xxx"设置字符串过期时间30s
setnx key "xxx"当可以不存在时设置，否则创建失败
```

```
mset k1 v1 k2 v2 k3 v3 一次设置多个值
msetnx k1 v1 k2 v2 设置k1 k2时同时成功同时失败

mget k1 k2 k3 一次获取多个值
```

```
set user:1 {name:zhangsan,age:18} 设置一个user:1 对象，值为json字符串
mset user:1:name zhangsan user:1:age 2
```

``` 
getset 先get再set
```

#### List

所有List命令都是l开头

List中的值可以重复

```
LPUSH list xxx 向列表头部插或多个
RPUSH list xxx 向尾部插
LANGE
LPOP list移除头
RPOP list移除尾

Lindex list 1 获取第一个（通过下标获取值）
Llen list 返回列表长度

Lrem list 1 one 移除列表中的一个one
Lrem list 2 three 移除列表中的两个three

trim list 1 2修剪，只要1到2的值

rpoplpush list newlist 从list pop,push到newlist

lset list 0 xxx 往0处插xxx

LINSERT list before "xxx" "yyy" 
LINSERT list after "xxx" "yyy"
```

#### Set

set中的值不可重复

```bash
sadd set "xxx"
smembers set 查看所有value
smember set xxx 判断xxx是否在set中
scard set 获取set中元素个数
srem set xxx 移除xxx

srandmember set 随机抽出元素
srandmember set 2 随机抽出2个元素

smove set newset xxx 将xxx从set移动到newset

spop随机删除一个元素

sdiff set1 set2 看两个set的不同元素
sinter set1 set2 看交集 
sunion set1 set2 看并集
```



#### Hash

key-map 

key-<key,map>

```
hset
hmset
hget
hmget
hdel
hgetall
hlen
```

Zset

在set的基础上增加了一个值

```
zset
zadd zset 1000 xxx
zrangescore zset -inf +inf 排序
```

### 事务

Redis单条命令保存原子性，事务不保证原子性

Redis没有隔离级别的概念

Redis事务本质一组命令的集合 ；一个事务中所有命令都会被序列化，在事务执行过程中，会按顺序执行；一次性顺序性排他性

```bash
multi 开启事务，下面输入的命令都会暂时放在队列里

exec执行队列中的命令
discard取消事务
```

 

- 有错误的命令（例如getset）其他所有的命令都不会执行

- 命令不存在语法性错误，命令执行时其他命令是我可以执行的

### Redis实现乐观锁

- 乐观锁

  认为什么时候都不会出现问题，不会上锁，更新数据时判断，在此期间是否有人修改过这个数据，数据被更改事务执行失败

- 悲观锁

  认为什么时候都会出现问题，无论做什么都加锁

  

```bash
watch xxx 监视xxx
multi 
命令
exec 若xxx在事务期间改变会执行失败
```

### Jedis

pom

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.3.0</version>
</dependency>
```



```
Jedis jedis = new Jedis("host",6379);
```