### MongoDb

##### 概述

MongoDB 是一个跨平台的，面向文档的数据库，它介于关系数据库和非关系数据库之间，是非关系数据库当中功能最丰富，最像关系数据库的产品，它支持的数据结构非常松散，是类似 JSON 的 BSON 格式，因此可以存储比较复杂的数据类型

##### 特点

MongoDB 最大的特点是他支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。它是一个面向集合的，模式自由的文档型数据库

##### 体系结构

MongoDB 由文档、集合、数据库三部分组成

- MongoDB 的文档（document），相当于关系数据库中的一行记录
- 多个文档组成一个集合（collection），相当于关系数据库的表
- 多个集合（collection），逻辑上组织在一起，就是数据库（database）
- 一个 MongoDB 实例支持多个数据库（database）

##### 数据类型

- null ----> `{"x":null}`
- 布尔型 ----> `{"x":true}`
- 数值
  - 浮点型 ----> `{"x":3.14}`
  - 整形 ----> `{"x":NumberInt("3")}` `{"x":NumberLong("3")}`    
- 字符串 ----> `{"x":"abcd"}`
- 日期 ----> `{"x":new Date()}`
- 正则表达式 ----> `{"x":/[abc]/}`
- 数组 ----> `{"x":["a","b","c"]}`
- 内嵌文档
- 对象 Id ----> `{"x":onjectId()}`
- 二进制数据
- 代码 ----> `{"x":function(){...}}`

##### 使用服务

```bash
# 创建数据存放目录
md d:\data
# 启动服务，指定数据仓库
mongod --dbpath=d:\data
# mongod --port=27017 默认端口为 27017
# 登录
mongo

```

##### 常用命令

选择或创建数据库，不存在自动创建

```sql
-- use database_name
use spitdb
```

插入数据

```sql
-- db.集合名称.insert(数据)
db.spit.insert({content:"good night", userid:"1000"})
```

查询数据

```sql
-- db.集合名称.find
-- 查询出所有数据， _id 字段相当于主键，插入时未指定自动创建，类型为 ObjectID
db.spit.find
-- 按条件查询
db.spit.find({userid:"1000"})
-- 返回结果第一条数据
db.spit.findOne({userid:"1000"})
-- 指定返回结果数
db.spit.find().limit(3)
```

修改数据

```sql
-- db.集合名称.update(条件，修改后的数据)
-- $set 修改器，防止其它字段丢失
db.spit.update({_id:"1"},{$set:{visits:NumberInt(1000)})
```

删除数据

```sql
-- db.集合名称.remove(条件)
-- 删除所有
db.spit.remove({})
-- 按条件删除
db.spit.remove({visits:1000})
```

统计条数

```sql
db.spit.count({userid:"1000"})
```

模糊查询

```sql
-- 以正则表达式方式实现
db.spit.find({content:/流量/})
db.spit.find({content:/^加班/})
```

大于、小于、不等于

```sql
-- visits > 1000
db.spit.find({visits:{$gt:1000}})
-- $lt $gte $lte $ne
```

包含与不包含

```sql
db.spit.find({userid:{$in:["1013","1014"]}})
db.spit.find({userid:{$nin:["1013","1014"]}})
```

条件连接

```sql
db.spit.find({$and:[ {visits:{$gte:1000}} ,{visits:{$lt:2000} }]})
db.spit.find({$or:[ {userid:"1013"} ,{visits:{$lt:2000} }]})
```

列值增长

```sql
db.spit.update({_id:"2"},{$inc:{visits:NumberInt(1)}} )
```

##### Java 操作 MongoDB

依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.mongodb</groupId>
        <artifactId>mongodb‐driver</artifactId>
        <version>3.6.3</version>
    </dependency>
</dependencies>
```

连接代码

```java
public static void main(String[] args) {
    // 创建连接
    MongoClient client = new MongoClient("localhost");
    // 打开数据库
    MongoDatabase spitdb = client.getDatabase("splitdb");
    // 获取集合
    MongoCollection<Document> spit = spitdb.getCollection("spit");
    // 构建查询条件
    BasicDBObject bson  = new BasicDBObject("userid", "1000");
    // 查询记录获取文档集合
    FindIterable<Document> documents = spit.find(bson);
    for(Document document ： documents) {
        System.out.println("内容： " + document.getString("content"));
        System.out.println("浏览量： " + document.getInteger("visits"));
    }
    // 关闭连接
    client.close();
}
```

##### 使用 SpringDataMongoDB

依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐data‐mongodb</artifactId>
</dependency>
```

application.yml

```yaml
spring:
	mongodb:
		host:localhost
		database: spiltdb
```



### ElasticSearch

##### 概述

基于 Lucene 的实时的分布式搜索和分析引擎，能够以极快的速度处理大规模数据，基于 RESTful web 接口提供了一个分布式多用户能力的全文搜索引擎

##### 特点

- 可以作为一个大型分布式集群（数百台服务器）技术，处理 PB 级数据，服务大公司；也可以运行在单机上
- 将全文检索、数据分析以及分布式技术，合并在了一起，才形成了独一无二的 ES
- 开箱即用，部署简单
- 全文检索，同义词处理，相关度排名，复杂数据分析，海量数据的近实时处理

##### 体系结构

索引(index): 类似于数据库

类型(type): 类似于表

文档(document): 类似于行 

##### 使用

解压安装包

在安装目录 bin 目录下执行 elasticsearch 即可启动

默认端口 9200

##### 通过 rest 请求的方式使用 ElasticSearch

##### 通过 Head 插件使用 ElasticSearch

##### IK 分词器

##### 使用 SpringDataElasticSearch

依赖

```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring‐data‐elasticsearch</artifactId>
</dependency>
```

application.yml

```yaml
spring:
    data: 
        elasticsearch: 
            cluster-nodes: localhost:9300
```

