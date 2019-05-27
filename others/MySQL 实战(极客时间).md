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