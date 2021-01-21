# Redis 内容介绍
> 通过一文快速了解 Redis 的知识点。思考在测试环节需要关注的地方和运用 Redis 的地方。

## 什么是 Redis
Redis(Remote Dictionary Server)远程数据字典服务器。Redis 可基于内存同样也能够持久化的日志型、Key-Value 数据库。

## Redis 的用途
Redis 在启动时会把数据加载到内存中，因此 redis 被广泛应用于缓存，每秒可以处理超过 10 万次读写操作，是已知性能最快的 Key-Value 数据库。另外，Redis 也经常用来做分布式锁。
### 会话缓存

使用 Redis 来统一存储多台应用服务器的会话信息。当应用服务器不再存储用户的会话信息，也就不再具有状态，一个用户可以请求任意一个应用服务器，从而更容易实现高可用性以及可伸缩性。

### 全页缓存

全页缓存（FPC） 除基本的会话 token 之外，Redis 还提供很简便的 FPC 平台。能帮助你以最快速度加载你曾浏览过的页面。

### 消息队列(发布/订阅功能)

List 是一个双向链表，可以通过 lpush 和 rpop 写入和读取消息。

### 分布式锁

实现在分布式场景下，无法使用单机环境下的锁来对多个节点上的进程进行同步。可以使用 Redis 自带的 SETNX 命令实现分布式锁，先拿 setnx 来争抢锁，抢到之后，再用expire给锁加一个过期时间防止锁忘记了释放。
## Redis持久化

持久化就是把内存的数据写到磁盘中去，防止服务宕机了内存数据丢失。Redis 在默认情况下是异步的把数据写入磁盘。

## Redis 数据类型

键的类型只能为字符串，值常见的数据类型有五种：字符串、哈希、列表、集合、有序集合。

### 字符串命令(String)

SET 设置指定 key 的值；GET 获取指定 key 的值

如：get register:phone:188xxxxxxxx:+86:code
### 哈希命令(Hash)

HMSET 同时将多个 field-value (域-值)对设置到哈希表 key 中

```bash
HMSET rediscomcn name "redis" url "http://www.redis.com.cn" rank 1 visitors 240000000
```

### 列表命令（List）

Redis 列表是按插入顺序排序的字符串列表。可以在列表的头部（左边）或尾部（右边）添加元素。

LPUSH 将一个或多个值插入到列表头部

如：lpush name value

### 集合 ( Sets )

Redis 的 Set 是 string 类型的无序集合。集合成员是唯一的，这就意味着集合中没有重复的数据。

SADD 向集合添加一个或多个成员
### 有序集合(sorted set)

每个元素都会关联一个 double 类型的分数，有序集合的成员是唯一的,但分数 ( score ) 可以重复。

ZADD 命令向有序集合添加一个或多个成员，或者更新已存在成员的分数

### HyperLogLog 是用来做基数统计的算法

基数(不重复元素)，并给出输入元素的基数估算值

PFADD 添加指定元素到 HyperLogLog 中。如元素中有重复的，只记录一次。

### 位图

map 结构存放 0 或 1( bit 位 ) 作为值

## Redis 客户端连接

Redis 命令用于在 redis 服务上执行操作。

Redis 客户端 redis-cli。

连接客户端，在远程服务上执行命令：redis-cli -h host -p port -a password

## Redis 发布/订阅

Redis 发布/订阅是一种消息传模式，其中发送者（在 Redis 中称为发布者）发送消息，而接收者（订阅者）接收消息。传递消息的通道称为 channel。

订阅某个频道：SUBSCRIBE rediscomcnChat

在频道发布消息：PUBLISH rediscomcnChat "Redis PUBLISH test"。当有新消息通过 PUBLISH 命令发送给频道 rediscomcnChat 时， 这个消息就会被发送给订阅它的客户端。

## Redis 事务

事务是指一个完整的动作，要么全部执行，要么什么也没有做。

Redis 事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

### 事务过程

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。
- 命令入队。
- 执行事务
1. 命令 MULTI 用来组装一个事务；
2. 后续输入的命令插入到了缓存队列；
3. 命令 EXEC 用来执行一个事务；

### Redis 事务错误

调用 EXEC 之前的错误：插入队的命令有误，导致无法执行

调用 EXEC 之后的错误：插入的命令一个事务中如果某一条命令执行失败，并不会影响接下来的其他命令的执行.

### WATCH 监控

类似于“乐观锁”的效果，即CAS（check and set）。

WATCH 本身的作用是监视 key 是否被改动过，而且支持同时监视多个 key，只要还没真正触发事务，WATCH 都会尽职尽责的监视，一旦发现某个 key 被修改了，在执行 EXEC 时就会返回 nil，表示事务无法触发。watch age //开始监视 age

## Redis 连接命令

Redis 连接命令用于控制和管理到 Redis Server 的客户端连接。

Redis Server 命令用于管理 Redis 服务器。INFO：获取 Redis 服务器的各种信息和统计数值

## Redis 备份和恢复

使用 SAVE 命令创建当前数据库的备份。

将 Redis 备份文件（dump.rdb）移动到 Redis 目录中并启动服务器以恢复 Redis 数据。

## Redis 流水线

因为 Redis 是一个支持请求/响应协议的 TCP 服务器。在 Redis 中，请求分两步完成：

- 客户端通常以阻塞方式向服务器发送命令。
- 服务器处理该命令并将响应发送回客户端。

所以流水线操作有助于客户端向服务器发送多个请求，而无需等待回复。由于多个命令同时执行，它极大地提高了协议性能。

## Redis 分区

分区用于将 Redis 数据拆分为多个 Redis 实例，以便每个实例仅包含一部分 key。

分区类型：范围分区、哈希分区

范围分区：通过将对象的范围映射到特定的 Redis 实例来完成。分区通常不支持具有多个键的操作。

## Redis 架构模式

### 单机版

用户直接访问一台 Redis 服务器。

### 主从复制

允许用户根据一个 Redis 服务器来创建任意多个该服务器的复制品，其中被复制的服务器为主服务器（master），通过复制创建出来的服务器复制品为从服务器。只要主从服务器之间的网络连接正常，主从服务器两者会具有相同的数据，主服务器就会一直将发生在自己身上的数据更新同步给从服务器，从而一直保证主从服务器的数据相同。可以进行读写分离。

### 哨兵

Redis sentinel 哨兵是一个分布式系统中监控 redis 主从服务器，并在主服务器下线时自动进行故障转移。

监控（Monitoring）： Sentinel 会不断地检查你的主服务器和从服务器是否运作正常。

提醒（Notification）： 当被监控的某个 Redis 服务器出现问题时， Sentinel 可以通过 API 向管理员或者其他应用程序发送通知。

自动故障迁移（Automatic failover）： 当一个主服务器不能正常工作时， Sentinel 会开始一次自动故障迁移操作。

### 集群（直连型）

Redis-Cluster采用无中心结构，每个节点保存数据和整个集群状态,每个节点都和其他所有节点连接。节点间数据共享，可动态调整数据分布。数据通过异步复制,不保证数据的强一致性。


## 缓存问题

### 缓存穿透

一般的缓存系统，都是按照 key 去缓存查询。如果不存在对应的value，就应该去后端系统查找（比如DB）。

故意查询不存在的key,需对查询结果为空的情况也进行缓存，缓存时间设置短一点，或者该key对应的数据insert了之后清理缓存。

对一定不存在的key进行过滤。可以把所有的可能存在的key放到一个大的Bitmap中，查询时通过该bitmap过滤。

在接口层增加校验，比如用户鉴权校验，参数做校验，不合法的参数直接代码Return，分页查询时，没对分页参数的大小做限制，容易一下请求最大值。

### 缓存雪崩

当缓存服务器重启或者大量缓存集中在某一个时间段失效。通过加锁或者队列来控制读数据库写缓存的线程数量。

做二级缓存，A1为原始缓存，A2为拷贝缓存，A1失效时，可以访问A2，A1缓存失效时间设置为短期，A2设置为长期。

### 缓存击穿

是指一个Key非常热点，大并发集中对这一个点进行访问，当这个Key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库