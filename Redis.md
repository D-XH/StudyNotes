# Redis（remote dictionary server）

## 概念

1. 非关系型数据库
2. **高性能**的数据结构存储系统
3. 可以作为**数据库**、**缓存**和**消息中间件**
4. 基于**内存**运行并支持**持久化**

## 特性

1. 持久化：把数据从内存保存在磁盘中，重启时加载到内存
2. 数据类型：hash，string，set，zset，list
3. 数据备份：**master-slave**模式，**主从复制**
4. 默认端口：6379（mysql：3306）

## 优点

1. 读写速率高，读的速率11万次/秒，写8.1万次/秒
2. 支持丰富的数据类型
3. 原子性：所有操作都是原子的
4. 丰富的特性：发布订阅、key过期

## 配置文件

位置：`/etc/redis/6379.conf`

启动方法：`sudo redis-server /etc/redis/6379.conf`

```shell

daemonize yes # 以守护进程方式运行
loglevel notice # 日志级别
database 16 # 数据库数量

save 900 1	# 15分钟内至少1次变化，保存快照
dbfilename dump.rdb # 快照名
dir /var/lib/redis/6379	# 快照路径

appendonly no # AOF一种持久化方式
appendfsync always # 同步方式
```

## 持久化

#### 概念

把数据从内存保存到磁盘

#### 分类

RDB持久化：把当前数据保存到磁盘。（默认）
AOF持久化：讲写命令保存到磁盘。

##### RDB

在指定**时间间隔**内，执行指定次数的写操作

文件路径：`/var/lib/redis/6379.rdb`

触发快照的方式：

1. 执行shutdown，抱枕服务器正常关闭不丢失数据。
2. 执行flushall，清空数据库数据
3. 执行save（阻塞，只管保存快照，其他的等待）或bgsave（异步）。
4. 指定时间间隔内，指定指定次数写操作（自动触发）

优点：

1. 适合大规模数据的恢复，恢复速率快。
2. 如果业务对数据完整性和一致性要求不高，rdb是很好的选择
3. 使用二进制进行存储

缺点：

1. 数据完整性和一致性不高，因为可能在最后一次备份时宕机。
2. 备份时占用内存。因为redis在备份时会创建子进程，子进程把数据写入一个临时文件（此时内存中的数据是原来的两倍），最后再用临时文件替换之前的备份文件。

##### AOF

保存**写操作**，恢复时重新执行一遍

`appendonlydir`文件夹和`.aof`文件。

三种同步方式：

1. `appendfsync always`：每次有数据修改发生时都会写入AOF文件。
2. `appendfsync everysec`：每秒钟同步一次，该策略为缺省策略。
3. `appendfsync no`：从不同步。不持久化

AOF文件重建策略：

* 执行`bgrewriteaof`命令如`set k1 100`和`set k1 200`合并成一个`set k1 200`
* `auto-aof-rewrite-percentage 100`：增幅是100%以上
* `auto-aof-rewrite-min-size 64mb`：且文件大小大于64mb，自动重建

优点

1. 恢复精度高，数据的完整性和一致性高

缺点

1. 不适合大规模数据的恢复，恢复速度慢。
2. 因为使用的是文件追加的模式，所以文件体积比较大。

`redis-check-aof --fix`可以修复损坏的aof文件

## 五大数据类型

![image-20250701140807935](C:\Users\DENG\AppData\Roaming\Typora\typora-user-images\image-20250701140807935.png)

## 命令

```shell
# 切换数据库，默认有16个数据库
select [num]

# 保存快照
save

# 查看所有key
keys *

# 查看当前数据库的key的数量
dbsize

# 清空当前数据库
flushdb

# 清空所有数据库
flushall

# 设置键值对
set key value

# 按key获取值
get key

# 删除键值对
del key

# 把key从当前库移到目标库
move key [num]

# 判断key是否存在
exists key

# 查看key类型
type key

# 查看key剩余有效时间，-1表示永不过期，-2表示已经过期
ttl key

# 为key设置过期时间
expire key seconds
```

### string

* 二进制安全。可以存放任意类型数据

```shell
# 一次设置多个键值对
mset key1 1 key2 hello

# 一次获取多个key的值
mget k1 k2

# 把key值设为新值，返回旧值
getset key value

# 设置子串，（最后一个下标为-1）
setrange key start replace	# k1 1 hhhh

# 获取子串 (左闭右闭)
getrange key start end 	# k1 1 2

# 数值类型++
incr key 

# 数值类型加值
incrby key value
```

### list

* 双向链表

```shell
# 左插（头插）
lpush list 1 2 3

# 右插(尾插)
rpush list 3 2 1

# 头删
lpop list

# 尾删
rpop list

# 显示从start到end的节点值
lrange list start end

# 通过下表访问
lindex list index

# 通过下表设置列表元素值
lset list index value

# 删除指定个数的值
lrem list count value

# 删除区间外的元素
ltrim list start end

# 在列表的某个元素前或后插入元素（pivot是值）
linsert list before|after pivot value
```

### set

* **无序**集合（唯一）
* 用的**哈希表**实现

```shell
# 在set中插入值
sadd set 1 2 3 4

# 删除set中的值
srem set 2 4

# 遍历显示set中所有值
smembers set

# 查看值在不在set中
sismember set 2

# 把值移到另一个set
smove s_set d_set 2

# 随机选出n个元素
srandmember set 3

# 删除集合中n个元素，并返回
spop set 2

# 差集
sdiff set1 set2

# 交集
sinter set1 set2

# 并集
sunion set1 set2
```

### zset

* **有序**set，（同能重复）
* 每个元素关联一个**double**类型权重值，进行排序

```shell
# 插入元素
zadd zset 0.1 v1 0.2 v2 0.3 v3

# 删除元素
zrem zset v2 v3

# 成员数
zsard zset

# 返回元素的权重
zscore zset v2

# 指定权重之间的成员数
zcount zset 0.2 0.5

# 遍历zset中范围内元素，withscores带上权重值。
zrange zset start end [withscores]

# 返回权重在区间内的元素
zrangebyscore zset 0.1 0.6


```

### hash

```shell
# 插入键值对
hset hash k1 v1 k2 v2

# 获取值
hget hash k1

# 获取多个值
hmget hash k1 k2

# 获取hash表中所有键值对
hgetall hash

# 获取所有键
hkeys hash

# 获取所有值
hvals hash

# 获取键值对数量
hlen hash
```

## 事务

### 概念

一组命令的集合。（没有原子性，但是不会被其他命令打断）（没有隔离级别概念：没提交前不会实际执行）

### 三个阶段

1. 开始事务
2. 命令入队
3. 执行事务

### 命令

```shell
# 开始事务
multi

# 提交事务
exec

# 取消事务
discard

# 监视key变化
watch key

# 取消监视
unwatch
```

## 锁

### 悲观锁

每次访问都认为其他线程也会修改，所以每次都加锁

### 乐观锁

每次访问认为其他线程不会修改，所以不加锁，但是会用无锁机制，版本号机制和CAS（check and set）机制。

## 主从复制

### 概念

把一台redis服务器的数据，复制到其他redis服务器上。数据的复制是**单向**的。

### 配置

```shell
# 拷贝配置文件两份（本地主从复制），更改文件名

# 更改端口号
# 更改 pidfile
# 更改 logfile
# 更改 dbfilename
# 更改 appendfilename

# 进入对应服务器的客户端
# 查看主从复制信息 info replication，每个机器都是主机
# 在从机上设置主机 slaveof 127.0.0.1 6379 （slaveof no one 设置为主机）

# 从机挂了之后，主机直接抛弃从机。从机重启后以主机状态启动，
# 主机挂了之后，从机依然等待主机上线

# 主机断开后，一直不上线，需要写时会有问题。
# 哨兵模式
## 执行流言协议与投票协议（少数服从多数）
## 配置：在/etc/redis/下创建sentinel.con，然后添加
### #哨兵		  监视	主机名		主机ip  端口  票数
### # sentinel monitor master6379 127.0.0.1 6379 1
## 启动：redis-sentinel /etc/redis/sentinel.com
```

## 缓存雪崩

大量缓存数据在同一时间过期（1.分散过期时间 2.用不失效）

## 缓存击穿

某个热点数据失效了，导致本来可以在缓存访问到的，要在数据库中访问（1. 延长过期时间或永不失效 2. 数据库加锁）

## 缓存穿透

数据在缓存和数据库都不存在。也会增加底层数据库压力（1. 在缓存中创建值为空的键值对）