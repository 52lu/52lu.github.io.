---
title: Redis-面试题整理
date: 2017-09-18 11:12:32
tags:
 - redis
categories:
 - 面试题
---

#### 1.Redis与Memcached的区别与比较
- Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储。memcache支持简单的数据类型，String。
- Redis支持数据的备份，即master-slave模式的数据备份。
- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用,而Memecache把数据全部存在内存之中。
- redis的速度比memcached快很多
- Memcached是多线程，非阻塞IO复用的网络模型；Redis使用单线程的IO复用模型。

#### 2.Redis与Memcached的选择
使用Redis的String类型做的事，都可以用Memcached替换，以此换取更好的性能提升； 除此以外，优先考虑Redis；
 
#### 3.使用redis有哪些好处？
- **速度快:**，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)
- **支持丰富数据类型:**，支持string，list，set，sorted set，hash
- **支持事务:** redis对事务是部分支持的，如果是在入队时报错，那么都不会执行；在非入队时报错，那么成功的就会成功执行。
- **丰富的特性:** 可用于缓存，消息，按key设置过期时间，过期后将会自动删除


#### 4.Redis为什么是单线程的？

多线程处理会涉及到锁，而且多线程处理会涉及到线程切换而消耗CPU。因为CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存或者网络带宽。单线程无法发挥多核CPU性能，不过可以通过在单机开多个Redis实例来解决。

#### 5.Redis单点吞吐量

单点TPS达到8万/秒，QPS达到10万/秒，补充下TPS和QPS的概念

- **QPS(应用系统每秒钟最大能接受的用户访问量):** 每秒钟处理完请求的次数，注意这里是处理完，具体是指发出请求到服务器处理完成功返回结果。

- **TPS(每秒钟最大能处理的请求数):** 每秒钟处理完的事务次数，一个应用系统1s能完成多少事务处理，一个事务在分布式处理中，可能会对应多个请求，对于衡量单个接口服务的处理能力，用QPS比较合理。


#### 6.Redis有哪几种数据淘汰策略？

在Redis中，允许用户设置最大使用内存大小server.maxmemory，当Redis 内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略。淘汰策略如下几种:

1. volatile-lru: 从已设置过期的数据集中挑选最近最少使用的淘汰

2. volatile-ttl: 从已设置过期的数据集中挑选将要过期的数据淘汰

3. volatile-random: 从已设置过期的数据集中任意挑选数据淘汰

4. allkeys-lru: 从数据集中挑选最近最少使用的数据淘汰

5. allkeys-random: 从数据集中任意挑选数据淘汰

6. noenviction: 禁止淘汰数据

redis淘汰数据时还会同步到aof

#### 7.Redis的并发竞争问题如何解决?
Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，但是在Jedis客户端对Redis进行并发访问时会发生连接超时、数据转换错误、阻塞、客户端关闭连接等问题，这些问题均是由于客户端连接混乱造成。对此有2种解决方法：
- 1.客户端角度，为保证每个客户端间正常有序与Redis进行通信，对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。

- 2.服务器角度，利用setnx实现锁。

>注：对于第一种，需要应用程序自己处理资源的同步，可以使用的方法比较通俗，可以使用synchronized也可以使用lock；第二种需要用到Redis的setnx命令，但是需要注意一些问题。


#### 8.Redis常见性能问题和解决方案:

1. Master最好不要做任何持久化工作，如RDB内存快照和AOF日志文件
2. 如果数据比较重要，某个Slave开启AOF备份数据，策略设置为每秒同步一次
3. 为了主从复制的速度和连接的稳定性，Master和Slave最好在同一个局域网内
4. 尽量避免在压力很大的主库上增加从库


#### 9.Redis有哪些数据结构？

字符串String、字典Hash、列表List、集合Set、有序集合SortedSet。

#### 10.Redis大量数据插入

例如，如果我们需要生成一个10亿的`keyN -> ValueN’的大数据集，我们会创建一个如下的redis命令集的文件(data.txt)：
```
SET Key0 Value0
SET Key1 Value1
...
SET KeyN ValueN
```
一旦创建了这个文件，其余的就是让Redis尽可能快的执行。在以前我们会用如下的netcat命令执行：
```
(cat data.txt; sleep 10) | nc localhost 6379 > /dev/null
```
然而这并不是一个非常可靠的方式，因为用netcat进行大规模插入时不能检查错误。从Redis 2.6开始redis-cli支持一种新的被称之为pipe mode的新模式用于执行大量数据插入工作。
使用pipe mode模式的执行命令如下：
```
cat data.txt | redis-cli --pipe
```
输出如下信息：
```
All data transferred. Waiting for the last reply...
Last reply received from server.
errors: 0, replies: 1000000
```

**pipe mode的工作原理是什么？**
难点是保证redis-cli在pipe mode模式下执行和netcat一样快的同时，如何能理解服务器发送的最后一个回复。
这是通过以下方式获得：
- redis-cli –pipe试着尽可能快的发送数据到服务器。
- 读取数据的同时，解析它。
- 一旦没有更多的数据输入，它就会发送一个特殊的ECHO命令，后面跟着20个随机的字符。我们相信可以通过匹配回复相同的20个字符是同一个命令的行为。
- 一旦这个特殊命令发出，收到的答复就开始匹配这20个字符，当匹配时，就可以成功退出了。

同时，在分析回复的时候，我们会采用计数器的方法计数，以便在最后能够告诉我们大量插入数据的数据量。

#### 11.Redis中的管道有什么用？

一次请求/响应服务器能实现处理新的请求即使旧的请求还未被响应。这样就可以将多个命令发送到服务器，而不用等待回复，最后在一个步骤中读取该答复。这就是管道（pipelining），是一种几十年来广泛使用的技术。例如许多POP3协议已经实现支持这个功能，大大加快了从服务器下载新邮件的过程。


#### 12.怎么理解Redis事务？

- 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
- 事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。


#### 13.为什么要做Redis分区？
分区可以让Redis管理更大的内存，Redis将可以使用所有机器的内存。如果没有分区，你最多只能使用一台机器的内存。分区使Redis的计算能力通过简单地增加计算机得到成倍提升,Redis的网络带宽也会随着计算机和网卡的增加而成倍增长。

#### 14.你知道有哪些Redis分区实现方案？

- 客户端分区: 就是在客户端就已经决定数据会被存储到哪个redis节点或者从哪个redis节点读取。大多数客户端已经实现了客户端分区。

- 代理分区: 意味着客户端将请求发送给代理，然后代理决定去哪个节点写数据或者读数据。代理根据分区规则决定请求哪些Redis实例，然后根据Redis的响应结果返回给客户端。redis和memcached的一种代理实现就是Twemproxy

- 查询路由(Query routing) 的意思是客户端随机地请求任意一个redis实例，然后由Redis将请求转发给正确的Redis节点。Redis Cluster实现了一种混合形式的查询路由，但并不是直接将请求从一个redis节点转发到另一个redis节点，而是在客户端的帮助下直接redirected到正确的redis节点

#### 15.Redis的内存用完了会发生什么？

如果达到设置的上限，Redis的写命令会返回错误信息（但是读命令还可以正常返回。）或者你可以将Redis当缓存来使用配置淘汰机制，当Redis达到内存上限时会冲刷掉旧的内容。
