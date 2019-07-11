# Redis

### 一、数据类型

Redis 以键值对的形式存储数据，键类型只能为字符创，值支持五种数据类型：字符串、列表、集合、散列表、有序集合

##### STRING

```
set hello world #添加
get hello #获取
del hello #删除
incr key-name
decr key-name
incrby key-name amount
decrby key-name amount
incrbyfloat key-name amount
append key-name value
getrange key-name start end
setrange key-name offset value
gitbit key-name offset
bitcount key-name [start end]
bitop operation dest-key key-name  [key-name...]

```

##### LIST

有序可重复

```
lpush list-key a #左添加
rpush list-key b #右添加
lrange list-key 0 -1 #范围获取
lpop list-key #左删除
rpop list-key #右删除
lindex list-key 1 #索引获取元素
ltrim key-name start end
blpop key-name [key-name...] timeout
brpop key-name [key-name...] timeout
rpoplpush source-key dest-key
brpoplpush source-key dest-key timeout
```

##### SET

无序不重复

```
sadd set-key a #添加
smembers set-key #获取所有元素
srem set-key a #删除
sismember set-key b #判断元素是否存在
scard key-name
srandmember key-name [count]
spop key-name
smove source-key dest-key item
sdiff key-name [key-name...]
sdiffstore dest-key key-name [key-name...]
sinter key-name [key-name...]
sinterstore key-name [key-name...]
sunion key-name [key-name...]
sunionstore dest-key key-name [key-name...]
```

##### HASH

hash 的键无序不重复

```
hset hash-key username zhangsan #添加
hset hash-key password 123456 #添加
hgetall hash-key #获取所有键值
hget hash-key username #获取指定键的值
hdel hash-key password #删除
hmget key-name key [key...]
hmset key-name key value [key value...]
hlen key-name
hexists key-name key
hkeys key-name
hvals key-name
hincrby key-name key increment
hincrbyfloat key-name key increment
```

##### ZSET

有序不重复，Redis 通过分数对集合元素进行排序

```
zadd zset-key 9000 dog #添加
zadd zset-key 70 cat #添加
zrange zset-key 0 -1 withscores #范围显示
zrangebyscore zset-key 0 800 withscores #通过分数范围显示
zrem zset-key dog #删除指定元素
zcrad key-name
zincrby key-name increment member
zcount key-name min max
zrank key-name member
zscore key-name member
zrange key-name start stop [WITHSCORES]
zrevrank key-name member
zrevrange key-name start stop [withscores]
zrangebyscore key min max [withscores]
zrevrangebyscore key max min [withscores]
zremrangebyrank key-name start stop
zremrangebyscore key-name min max
zinterstore dest-key key-count key [key ...]
zunion dest-key key-count key [key ...]
```

##### 键的过期时间

```
persist key-name
ttl key-name 
expire key-name seconds
expireat key-name timestamp
pttl key-name 
pexpire key-name milliseconds
pexpireat key-name timestamp-milliseconds
```

##### common command

```
keys * #列出所有键
type key #返回键的类型
del key #删除键
```

### 二、数据结构

##### 简单动态字符串

Redis 采用 SDS 抽象类型作为字符串的默认表示

```c
struct sdshdr {
    int len;
    int free;
    char buf[];
};
```

优点：

- 常数复杂度获取字符串

- 杜绝缓冲区溢出

- 减少修改字符串长度时所需的内存重分配次数

- 二进制安全

- 兼容部分 C 字符串函数

##### 链表

链表广泛用于实现 Redis 的各种功能实现，如列表建、发布与订阅、慢查询、监视器

链表节点

```c
typedef struct listNode {
    struct listNode *prev;
    struct listNode *next;
    void *value;
}listNode;
```

链表

```c
typedef struct list {
    listNode *head;
    listNode *tail;
    unsigned long len;
    void *(*dup)(void *ptr);
    void *(*free)(void *ptr);
    int (*match)(void *ptr,void *key);
}
```

Redis 链表特点：

- 双端
- 无环
- 带表头指针和表尾指针
- 带链表长度计时器
- 多态（Redis 可以保存各种不同类型的值）

##### 字典

Redis 的数据库和哈希键都是使用字典实现的

字典的实现

Redis 的字典使用哈希表作为底层实现，一个哈希表里面可以有多个哈希表节点，每个哈希表保存了字典中的一个键值对

哈希表

```c
typedef struct dictht {
    dictEntry **table;
    unsigned long size;
    unsigned long sizemask;
    unsigned long used;
} dictht;
```

哈希表节点

```c
typedef struct dictEntry {
    void *key;
    union {
        void *val;
        uint64_tu64;
        int64_ts64;
    } v;
    struct dictEntry *next;
} dictEntry;
```

字典

```c
typedef struct dict {
    dictType *type;
    void *privdata;
    dictht ht[2];
    in trehashidx;
}
```

哈希算法

哈希冲突

rehash(扩展和收缩)

渐进式 rehash

##### 跳跃表

Redis 在两个地方用到了跳跃表，有序集合的实现，集群节点的内部结构

跳跃表节点

```c
typeof struct zskiplistNode {
    struct zskiplistLevel {
        struct zskiplistNode *forward; //前进指针
        unsigned int span; //跨度
    } levlel[]; //层
    struct zskiplistNode *backward; //后退指针
    double score; //分值
    robj *obj; //成员
} zskiplistNode;
```

跳跃表

```c
typedef struct zskiplist  {
    struct skiplistNode *header, *tail; //表头节点和表尾节点
    unsigned long length; //表中节点的数量（除表头）
    int level; //层数最大的节点的层数（除表头）
} zskiplist;
```

##### 整数集合

整数集合是集合键的底层实现之一

```c
typedef struct intset {
    uint32_t encoding; //编码方式
    uint32_t length; //集合包含的元素数量
    int8_t contents[]; //保存元素的数组
} intset;
```

升级

- 提升灵活性
- 节约内存

##### 压缩列表

##### 对象



### 三、持久化

##### RDB 持久化(快照)

通过保存数据库中的键值对来记录数据库的状态，可能丢失最近一次创建快照之后写入的所有数据

手动执行

- SAVE

  在快照创建完毕之前不再响应其他任何命令

- BGSAVE

  使用子进程负责快照写入，父进程继续处理命令请求

自动间隔性保存

- 设置保存条件

  ```ba
  save 60 1000
  save 1 100000
  ```

RDB 文件结构

##### AOF 持久化(只追加写入)

通过保存服务器所执行的写命令来记录数据库状态

AOF(append only file) 持久化实现

- 命令追加
- 文件写入
- 文件同步

appendfsync

- always

  每个 Redis 命令都要同步写入硬盘，效率低

- everysec

  每秒执行一次同步，显示将多个写命令同步到硬盘

- no

  让操作系统决定何时进行同步

AOF 重写

解决 AOF 文件体积不断增大的问题

- REWRITE
- BGREWRITEAOF

自动间隔性重写

```bash
# 当AOF文件体积大于64MB并且文件的体积比上一次重写之后大至少一倍时执行BGREWRITEAOF
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```



### 四、复制

`slaveof host port` 命令可以让当前服务器成为某一台服务器的从服务器

`info replication` 查看当前服务器状态

哨兵模式：Sentinel 系统用于管理多个 Redis 服务器

- 监控
- 提醒
- 自动故障迁移

配置 sentinel.conf 

```bash
# setinel monitor 被监控数据库名 IP PORT NUMBER
sentinel monitor server6379 10.211.55.6 6379 1
```

