# MySQL 实战(极客时间)

### 一条语句是怎样执行的

```SQL
select * from T where id = 10;
```

##### MySQL 架构

###### 客户层

###### server 层

1. 连接器

   ```sql
   mysql -h$ip -p$port -u$user -p
   ```

   ```sql
   show processlist
   ```

   连接自动断开： `wait_timeout(default=8)` 非交互式连接 `interactive_timeout` 交互式连接

   长连接和短连接优缺点

   ```sql
   mysql_reset_connection
   ```

2. 查询缓存

   缓存以 `key-value` 的形式存储在内存中

   大多数情况下，查询缓存弊大于利--缓存失效

   缓存按需使用 `query_cache_type=DEMAND` `SQL_CACHE`

   ```SQL
   SELECT SQL_CACHE * FROM T WHERE ID = 1;
   ```

   MySQL 8.0 开始删掉了查询缓存功能

3. 分析器

   词法分析

   语法分析

   语义分析

4. 优化器

   索引选择

   表连接顺序选择

5. 执行器

   权限验证（为什么权限验证不在优化器之前进行？）

   根据表引擎接口打开表

   执行查询：

   > 无索引：取“第一行”接口，判断条件；取“下一行”接口，判断条件······
   >
   > 有索引：取“满足条件的第一行”接口，将结果存入结果集；取“满足条件的下一行接口”······

   引擎扫描行数跟 `rows_examined` 并不是完全相同的

###### 存储引擎层

###### 问题：“Unknown column 'id' in 'where clause'” 错误是从哪个阶段报出的？



### 一条 SQL 更新语句是怎样执行的

```sql
create table book(id int primary key, number int);
```

```sql
update book set number=number+10 where id = 5;
```

清空表缓存

###### 重做日志 redo log

```sql
innodb_flush_log_at_trx_commit=1
```

InnoDB 特有的日志

WAL(write-ahead logging)

InnoDB: update-->redo log-->update memory-->wirte to disk(free time)

write pos, checkpoint

crash-safe

###### 归档日志 binlog 

```sql
sync_binlog=1
```

Server 层的日志

两种日志的比较：redo log 引擎层，binlog Server 层；redo log 物理日志，binlog 逻辑日志；redo log 循环写入，binlog 追加写入

update 具体执行流程

两阶段提交

为什么要有两阶段提交？反证法（crash)

怎样让数据库恢复到半个月内任意一秒的状态？

redo log 和 binlog 都可以用于表示事务的提交状态，两阶段提交让这两个阶段保持逻辑上的一致

###### 什么场景下，一天一备比一周一备更有优势？它影响这个数据库系统的哪个指标？





### 事务

事务特性

隔离级别--修改隔离级别--适用场景

```sql
-- 查看当前隔离级别
show variables like 'transaction_isolation'
```

并发问题

事务实现：视图

事务隔离实现：回滚日志，多版本并发控制(MVCC)，回滚日志何时删除

长事务：大量回滚日志，占用锁资源

长事务避免方式：`(1)set autocommit = 1` `(2)commit work and chain`

```sql
-- 查询持续时间超过 60s 的长事务
select * from information_schema.innodb_trx where TIME_TO_SEC(timediff(now(),trx_started)) > 60
```





### 索引

哈希表：缺--哈希冲突、区间查询慢；优--维护简单、适用于等值查询(Memcached/NoSQL)

有序数组：缺--维护困难；优--区间查询、等值查询、适用于静态存储

二叉树：查询复杂度O(logN)，维护复杂度O(logN)

索引