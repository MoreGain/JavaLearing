# SQL(Structure Query Language)

[TOC]



##### 基础

数据库(database)：保存有组织的数据的容器

表(table)：某种特定类型数据的结构化清单

模式(schema)：关于数据库和表的布局及特性的信息

列(column)：表中的一个字段，表由一个或多个列组成

数据类型(datatype)：容许的数据类型，每个表列都有相应的数据类型

行(row)：表中的一个记录

主键(primary key)：能够唯一区别表中每个行的一列（一组列），应该总是定义主键

> 任意两行都不具有相同的主键值
>
> 每个行都必须具有一个主键值（主键列不允许NULL值）

> 使用建议：
>
> 不更新主键列中的值
>
> 不重用主键列的值
>
> 不在主键列中使用可能会更改的值

DBMS(Database Manage System)：数据库管理系统

##### MySql 简介

优点：

客户机-服务器软件：

##### 库操作

连接DBMS

> ```sql
> mysql -u -p
> ```

创建数据库

```sql
-- 字符集：定义 MySQL 存储字符串的方式
-- 校对规则：定义比较字符串的方式，解决排序和字符分组的问题
CREATE DATABASE [IF NOT EXISTS] database_name[[DEFAULT] CHARACTER SET character_name] [[DEFAULT] COLLATE collate_name];
```

显示数据库

```sql
SHOW databases;
```

使用数据库

```sql
USE database_name;
```

删除数据库

```sql
DROP DATABASE [IF EXISTS] db_name
```

其他 show 语句

```sql
-- 显示服务器状态信息
show status;
-- 显示建库语句
show create database database_name;
-- 显示授权用户的安全权限
show grants;
-- 显示错误或警告信息
show errors;
show warnings;
```



##### 表操作

显示所有表

```sql
SHOW tables;
```

查看表结构

```sql
DESC table_name;
DESCRIBE table_name;
SHOW COLUMNS FROM table_name;
```

查看建表语句

```sql
SHOW CREATE TABLE table_name;
```

创建表

```sql
CREATE TABLE table_name(
	attribute_name, datatype, [not null][primary key][auto_increment],
    ...,
    PRIMARY KEY(column1,column2) #指定多个主键
) ENGINE=InnoDB
```

获得最后一个自动增长的值

```sql
SELECT last_insert_id();
```

引擎类型

> InnoDB : 可靠地事务处理引擎，不支持全文搜索
>
> MEMORY : 功能同 MyISAM，数据存储在内存中，速度很快，很适合于临时表
>
> MyISAM : 性能极高的引擎，支持全文搜索，不支持事务

> 引擎类型可以混用，即一个数据库中的表可以有多种引擎，但外键不能跨引擎



##### 修改表

添加列

```sql
ALTER TABLE table_name ADD column_name ... [first|after column]
```

更改列默认值

```sql
ALTER TABLE table_name ALTER column SET default new_default_value
```

更新列

```sql
ALTER TABLE table_name CHANGE old_column_name new_column_name ... [first|after column]
```

修改列参数

```sql
ALTER TABLE table_name MODIFY column_name ... [first|after column]
```

删除列

```sql
ALTER TABLE table_name DROP COLUMN column_name
```

删除主键

```sql
ALTER TABLE table_name DROP primary key
```

修改表名

```sql
ALTER TABLE table_name rename AS new_table_name
rename TABLE old_name TO new_name
```

删除表

```sql
DROP TABLE [if exists] table_name
```

添加非空约束

```sql
ALTER TABLE table_name MODIFY column ... NOT NULL
```

添加唯一性约束

```sql
ALTER TABLE table_name ADD CONSTRAINT uk_name UNIQUE(column)
ALTER TABLE table_name MODIFY column ... UNIQUE
```

添加主键

```sql
ALTER TABLE table_name ADD CONSTRAINT pk_name PRIMARY KEY(column)
```

添加外键

```sql
ALTER TABLE table_name ADD CONSTRAINT fk_name FOREIGN KEY(column) REFERENCES table_name(column)
```



##### 检索数据

检索单个列

```sql
SELECT column FROM table_name;
```

检索多个列

```sql
SELECT column1,column2,column3 FROM tale_name;
```

检索所有列

```sql
SELECT * FROM table_name;
```

检索去重

```sql
-- cloumn 中重复的行将被去掉
SELECT DISTINCT column FROM table_name;
```

> 不能部分使用 DISTINCT，它应用于所有列而不仅是前置它的列

检索限制结果（返回固定行的数据，如：分页）

```sql
-- a 表示起始位值(从0开始)，b 表示显示的行数
SELECT column FROM table_name LIMIT a,b;
```

检索排序

```sql
SELECT column FROM table_name ORDER BY column1 (ASC/DESC),column2
```

> 默认为升序排列，多列排序时每列都需指定排序规则，与 LIMIT 结合使用需放在之前



##### 数据过滤

使用 WHERE 关键字设置过滤条件

```sql
SELECT column FROM table_name WHERE id=1;
```

> SQL 过滤：数据在数据库服务器端过滤
>
> 应用过滤：数据在应用层过滤，影响性能，增加网络宽带

WHERE 子句操作符

<table>
    <tr>
    	<th>操作符</th>
        <th>说  明</th>
    </tr>
    <tr>
        <td>=</td>
        <td>等于</td>
    </tr>
    <tr>
        <td><></td>
        <td>不等于</td>
    </tr>
    <tr>
        <td>!=</td>
        <td>等于</td>
    </tr>
    <tr>
        <td><</td>
        <td>小于</td>
    </tr>
    <tr>
        <td>></td>
        <td>大于</td>
    </tr>
    <tr>
        <td><=</td>
        <td>小于等于</td>
    </tr>
    <tr>
        <td>>=</td>
        <td>大于等于</td>
    </tr>
    <tr>
        <td>BETWEEN NAD</td>
        <td>两值之间</td>
    </tr>
</table>


##### 过滤数据

1. 空值检查 (IS NULL)

```sql
-- NULL 与0、空字符串、空格不同
SELECT column FROM table_name WHERE column IS NULL
```

> NULL 与不匹配：过滤选择出不匹配的值时不会返回具有 NULL 值的行

2. 组合 WHERE 子句 (AND OR)

```sql
-- AND 检索满足所有给条件的行
SELECT name FROM user WHERE id>10 AND age<18;
-- OR 检索满足任一条件的行
SELECT name FROM user WHERE id>10 OR age<18;
```

> AND 在计算次序中优先级更高，尽量使用括号声明计算次序

3. (NOT)IN 操作符

> 指定条件范围，其中的每个条件都可以匹配，与 OR 关键值功能相当
>
> NOT IN 表示每个条件都不匹配

```sql
-- 返回 user 表中 id （不）为 1,2,3 的用户名字
SELECT name FROM user WHERE id (NOT) IN (1,2,3);
```

4. 模糊查询(LIKE)

> 通配符：用来匹配一部分的特殊字符
>
> 搜索模式：由字面值、通配符或两者结合构成的搜索条件

- % 通配符

  > 任意字符出现任意次数(0次、1次、多次)
  >
  > 注意尾空格：尾空格可能会干扰结果
  >
  > 不能匹配 NULL
  >
  > ```sql
  > SELECT coulmn FROM table_name WHERE name LIKE '%a%'； 
  > ```

- _ 通配符

  > 匹配一个字符
  >
  > ```sql
  > SELECT column FROM table_name WHERE name LIKE '_a_';
  > ```

> NOTE:
>
> ​	不要过度使用通配符，若其他操作符能达到目的时则不使用通配符
>
> ​	不要将通配符置于搜索模式起始处，通配符置于起始处搜索是最慢的

5. 正则表达式进行搜索

```sql
-- 基本匹配(检索包含 1000 的所有行)
SELECT column FROM table_name WHERE column REGEXP '1000';
-- 区分大小写
SELECT column FROM table_name WHERE column REGEXP BINARY '1000';
```



##### 创建计算字段

> 计算字段并不是实际存在于数据库，它是运行语句时创建的

1. 拼接字段

```sql
SELECT Concat(column1,"(",column2,")") FROM table_name;

-- 去除值左右两边空格， RTime() 去除右空格， LTrim() 去除左空格
SELECT Concat(Trim(column),"(",Trim(column2),")") FROM table_name;

-- 使用别名
SELECT column AS costom_name FROM table_name;
```

2. 执行算数计算

> 可通过算术运算符(+ - * /)对查询结果进行计算

```sql
SELECT column1*column2 AS show_column_name FROM table_name;
```

##### 函数

1. 文本处理函数

| 函  数      | 说  明                                                      |
| ----------- | ----------------------------------------------------------- |
| Length()    | 返回串的长度                                                |
| Lower()     | 将串转换为小写                                              |
| Upper()     | 将串转换为大写                                              |
| Trim()      | 去除串两边的空格                                            |
| RTrim()     | 去除串右边的空格                                            |
| LTrim()     | 去除串左边的空格                                            |
| Left()      | 返回串左边指定长度的字符                                    |
| Right()     | 返回串右边指定长度的字符                                    |
| SubString() | 返回子串的字符（指定起始位置与长度）                        |
| Locate()    | 找出串的一个子串                                            |
| Soundex     | 返回串的SOUNDEX值（将文本串转换为描述其语音表示的字母模式） |

2. 日期时间处理函数

| 函  数        | 说  明                       |
| ------------- | ---------------------------- |
| Date()        | 返回一个日期时间的日期部分   |
| Time()        | 返回一个日期时间的时间部分   |
| Year()        | 返回一个日期的年份部分       |
| Month()       | 返回一个日期的月份           |
| Day()         | 返回一个日期的天数           |
| Hour()        | 返回一个时间的小时部分       |
| Minute()      | 返回一个时间的分部分         |
| Second()      | 返回一个时间的秒部分         |
| DayOfWeek()   | 返回一个日期的星期数         |
| CurTime()     | 返回当前时间的时间部分       |
| CurDate()     | 返回当前时间的日期部分       |
| Now()         | 返回当前时间日期             |
| AddDate()     | 增加一个日期                 |
| AddTime()     | 增加一个时间                 |
| DateDiff()    | 计算两个日期之差             |
| Date_Add()    | 日期运算函数                 |
| Date_Format() | 返回一个格式化的日期或时间串 |

3. 数值处理函数

| 函  数 | 说  明             |
| ------ | ------------------ |
| Abs()  | 返回一个数的绝对值 |
| Cos()  | 返回一个角度的余弦 |
| Exp()  | 返回一个数的指数值 |
| Mod()  | 返回除操作的余数   |
| Pi()   | 返回圆周率         |
| Rand() | 返回一个随机数     |
| Sin()  | 返回一个角度的正弦 |
| Sqrt() | 返回一个数的平方根 |
| Tan()  | 返回一个数的正切   |

<table border="1px solid black">
<tr>	
	<th>函数</td>
	<th>说明</td>
</tr>
<tr>
	<td>Abs()</td>
    <td>返回一个数的绝对值</td>
</tr>
<tr>
	<td>Cos()</td>
    <td>返回一个角度的余弦</td>
</tr>
</table>

4. 聚集函数

<table border="1px solid black">
<tr>	
​	<th>函数</td>
​	<th>说明</td>
</tr>
<tr>
	<td>AVG()</td>
    <td>返回某列的平均值</td>
</tr>
<tr>
	<td>COUNT()</td>
    <td>返回某列的行数</td>
</tr>
<tr>
	<td>MAX()</td>
    <td>返回某列的最大值</td>
</tr>
<tr>
	<td>MIN()</td>
    <td>返回某列的最小值</td>
</tr>
<tr>
	<td>SUM()</td>
    <td>返回某列之和</td>
</tr>
</table>


> 聚合函数可以配合 DISTINCT 使用，如 COUNT(DISTINCT column)，聚合函数也可组合使用，聚合函数会忽略 NULL 值

##### 分组函数

使用 GROUP BY 关键字进行分组

```sql
SELECT column FROM table_name GROUP BY column
```

使用 HAVING 关键字进行分组过滤

```sql
SELECT column FROM table_name GROUP BY column HAVING condition
```

> WHERE 与 HAVING 的区别
>
> WHERE : 过滤行
>
> HAVING : 过滤分组

使用 WITH ROLLUP 关键字可对分组进行汇总

```sql
SELECT column FROM table_name GROUP BY column WITH ROLLUP
```



##### 子查询

嵌套在其他查询中的查询

```sql
# 在 WHERE 子句中使用子查询
SELECT column FROM table_name WHERE name =
	(SELECT name FROM table_name WHERE id = 1)
```

##### 联结表

内部联结(等值联结)

```sql
# 通过 WHERE 子句联结表
SELECT column1,column2 FROM t1,t2 WHERE t1.column = t2.column
# 通过 INNER JOIN ON 进行联结
SELECT column1,column2 FROM t1 INNER JOIN t2 ON t1.column = t2.column
```

> 完全限定列名：在引用的列可能出现二义性时，必须使用完全限定列名。即明确指定引用的列为那张表的列

> 笛卡尔积：没有联结条件的表关系返回的结果为笛卡尔积

自联结

> 从相同的表中查询数据多次可使用自联结，即联结同一张表。自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句(有时处理联结比处理子查询快得多)

自然联结

> 进行联结时至少有一个列出现在不止一个表中。自然联结排除多次出现，使每个列只返回一个值，这个过程由我们自己完成

外部联结

> 联结包含在相关表中没有关联行的行
>
> - 左外部联结

```sql
# 联结包含左表中没有关联行的行
SELECT column FROM t1 LEFT OUTER JOIN t2 ON t1.column = t2.column 
```

- 右外部联结

```sql
# 联结包含右表中没有关联行的行
SELECT column FROM t1 RIGHT OUTER JOIN t2 ON t1.column = t2.column
```

##### 组合查询

复合查询（并查询）：执行多个查询，并将结果作为单个查询结果集返回

> 使用组合查询的情况：
>
> ​	在单个查询中从不同的表返回类似结构的数据
>
> ​	对单个表执行多个查询，按单个查询返回数据

组合两个查询

```sql
# 只使用 UNION 会去除重复的行
SELECT column1,column2 FROM table_name WHERE condition
UNION
SELECT column1,column2 FROM table_name WHERE condition

# 使用 UNION ALL 包含重复的行
SELECT column1,column2 FROM table_name WHERE condition
UNION ALL
SELECT column1,column2 FROM table_name WHERE condition

# 对组合查询结果排序(ORDER BY 只能出现在最后一条查询语句之后)
SELECT column1,column2 FROM table_name WHERE condition
UNION
SELECT column1,column2 FROM table_name WHERE condition
ORDER BY column
```

> UNION 规则：
>
> ​	至少两条 SELECT 语句组成
>
> ​	每个查询必须包含相同的列、表达式或聚合函数（各个列次序可以不同）
>
> ​	列数据类型必须兼容



##### 索引

索引是一种特殊的文件，它们包含着对数据库表里所有记录的指针引用，能够加快数据库的查询速度，简单说就是为了提高表的搜索效率而对某些字段中的值建立的目录

索引分类：普通索引、唯一索引、主键索引、全文索引

创建单列索引

```sql
CREATE index index_name on table_name(column_name)
-- 通过修改表结构创建索引
ALTER TABLE table_name ADD index index_name(column_name)
-- 创建表时创建索引
CREATE TABLE table_name(
	id int PRIMARY KEY,
    name varchar(20),
    index index_name(column_name)
)
```

创建多列索引

```sql
-- 使用多列索引需要满足最左前缀列
CREATE index index_name on table_name(column_name,column_name...)
-- 适用场景：
-- 匹配全值，对索引中所有列都指定具体的值
-- 匹配最左前缀
-- 匹配部分左前缀
-- 匹配第一列范围查询（只能使 like a%）
```

查看索引

```sql
SHOW index FROM table_name
```

删除索引

```sql
-- 修改表结构删除
ALTER TABLE table_name DROP index index_name
```

创建唯一索引

```sql
-- 索引列的值必须唯一，可以有空值，组合索引列值的组合必须唯一
CREATE UNIQUE index index_name on table_name(column_name)
ALTER TABLE  table_name ADD UNIQUE index(column_name)
-- 创建表的时候添加索引（unique 字段会被默认创建唯一索引）
CREATE TABLE table_name (
	...
    UNIQUE index index_name (column_name)
)
```

创建主键索引

```sql
-- 主键索引是特殊的唯一索引，不允许有空值，为表定义主键将自动创建主键索引
ALTER TABLE table_name ADD PRIMARY KEY(column_name)
CREATE TABLE table_name (
	id int PRIMARY KEY,
    ...
)
```

创建全文索引

```sql
-- 全文索引仅适用于 MyISAM 引擎的表
CREATE fulltext index index_name on table_name(column_name)
ALTER TABLE table_name ADD fulltext index_name(column_name)
CREATE TABLE table_name (
	...
    fulltext index_name(column_name)
)
```

使用全文索引

```sql
-- 全文搜索中，mysql 指定了最小字符长度，默认为4，即必须陪匹配大于4才会有返回结果
-- 可通过 show variables like 'ft_min_word_len' 来查看指定的字符长度
-- 可在 mysql 配置文件 my.ini 更改最小字符长度，增加一行例如：ft_min_word_len=2
-- mysql 还会计算一值的权值决定是否出现在结果集中，mysql 不支持中文的全文索引
SELECT * FROM table_name WHERE match(column,column,...) against("serach_value")
```



##### 全文本搜索

> MySql 中 MyISAM 引擎支持全文本搜索，InnoDB 不支持

使用 FULLTEXT(column) 定义索引

> 可在创建表时指定，也可稍后指定。定义之后，MySQL 会自动维护索引，在增加、更新或删除行时，索引随之更新
>
> 不要在导入数据时使用 FULLTEXT。先导入数据，再修改表定义 FULLTEXT

使用 Match() 和 Against() 进行全文本搜索

```sql
-- Match 指定列进行搜索，它的值必须与FULLTEXT中定义的相同，Against 指定要搜索的文本
SELECT column FROM table_name WHERE Match(column) Against('heavy')
```

> LIKE 也能完成全文搜索的功能，但全文本搜索效率更高，并且它会对结果按照等级排序

查询扩展(MySQL 4.1.1 引入)

```sql
SELECT column FROM table_name WHERE Match(column) Against('heavy' WITH QUERY EXPANSION);
```

> 查询扩展会放宽返回的全文本搜索结果的范围，通过对数据和索引进行两遍扫描来完成搜索

布尔文本搜索

```sql
SELECT column FROM table_name WHERE Match(column) Against('heavy -rope*' IN BOOLEAN MODE);
```

全文本布尔操作符

<table border="1 solid black">
	<tr>
		<td width="5%" align="center">+</td>
		<td>包含，词必须存在</td>
	</tr>
    <tr>
		<td width="5%" align="center">-</td>
		<td>排除，词必须不出现</td>
	</tr>
    <tr>
		<td width="5%" align="center">></td>
		<td>包含，且增加等级值</td>
	</tr>
    <tr>
		<td width="5%" align="center"><</td>
		<td>包含，且减少等级值</td>
	</tr>
    <tr>
		<td width="5%" align="center">()</td>
		<td>把词组成子表达式（允许这些子表达式作为一个组被包含、排除、排列等）</td>
	</tr>
    <tr>
		<td width="5%" align="center">~</td>
		<td>取消一个词的排序值</td>
	</tr>
    <tr>
		<td width="5%" align="center">*</td>
		<td>词尾的通配符</td>
	</tr>
    <tr>
		<td width="5%" align="center">" "</td>
		<td>定义一个短语（它匹配整个短语以便包含或排除这个短语）</td>
	</tr>
</table>


##### 插入数据

插入一行数据所有列值

```sql
INSERT INTO table_name VALUES(v1,v2...);
```

插入一行数据特定列值

```sql
INSERT INTO table_name(c1,c2...) VALUES(c_v1,c_v2...)
```

插入多行数据

```sql
INSERT INTO table_name(c1,c2...) VALUES(c_v1,c_v2...),(c_v1,c_v2...),...
```

> 处理单条 INSERT 语句比使用多条 INSERT 语句快，所以插入多行数据尽量使用此方式

降低 INSERT 语句的优先级

```sql
INSERT LOW_PRIORITY INTO ...
```

插入查询的数据(可用于将新表的数据导入已有表)

```sql
INSERT INTO table_name(c1,c2...) SELECT c1,c2... FROM table_name 
```



##### 更新和删除数据

更新特定行一列数据

```sql
UPDATE table_name SET column=newValue WHERE condition;
```

更新特定行多列数据

```sql
UPDATE table_name SET column1=newValue,column2=newValue WHERE condition;
```

更新所有行

```sql
-- 注意不要省略 WHERE，不使用 WHERE 过滤条件进行更新将更新所有行
UPDATE table_name SET column=newValue;
```

INGORE 关键字，更新多行时，即使发生错误，也继续更新

```sql
UPDATE IGNORE table_name ...;
```

删除某个列的值

```sql
UPDATE table_name SET column=NULL WHERE condition;
```

删除特定行

```sql
DELETE FROM table_name WHERE condition;
```

删除所有行（删除行，不删除表本身）

```sql
-- 注意不要省略 WHERE，不使用 WHERE 过滤条件进行删除将更新所有行
DELETE FROM table_name;
-- 更快的删除（删除原来的表并重建表）
TRUNCATE table table_name;
```



##### 视图

> 视图是虚拟的表，它不包含任何实际的数据，它包含的是一个 SQL 查询

创建视图

```sql
CREATE VIEW view_name AS SELECT ...;
```

查看视图创建语句

```sql
SHOW CREATE VIEW view_name;
```

删除视图

```sql
DROP VIEW view_name;
```

更新视图

```sql
CREATE OR REPALCE VIEW view_name;
```

> 可通过 INSERT、UPDATE 和 DELETE 更新视图，更新视图实际上是更新的基表
>
> 视图通常用于检索，而不用于更新



##### 存储过程

简单的说，存储过程是为以后的使用而保存的一条或多条 MySQL 语句的集合

存储过程的特点：

- 优点：简单、安全、高性能
- 缺点：编写复杂、无创建权限

MySQL 将编写存储过程的安全和访问与执行存储过程的安全与访问区分开，所以不能编写存储过程也依然可以使用

使用存储过程

- 执行存储过程

```sql
-- 执行名为 pricepricing 的存储过程，返回产品的最低、最高、平均价格
-- pricelow 等为变量，它们是内存中一个特定的位置，用来临时存储数据
CALL productpricing(@pricelow,@pricehigh,@priceaverage);
-- 显示结果
SELECT @pricelow,@pricehigh,@priceaverage;
```

- 创建存储过程

```sql
-- 更改 MySQL 命令行实用程序使用 // 作为分隔符，以便解释存储过程
DELIMITER //

-- () 内为存储过程接收的参数，BEGIN AND 用于限制存储过程体
CREATE PROCEDURE productpricing()
BEGIN
	SELECT Avg(prod_price) AS priceaverage FROM products;
END //

-- 恢复初始分隔符 
DELIMITER ;
```

- 删除存储过程

```sql
-- 只需给出存储过程名，不需要 ()
DROP PROCEDURE productpricing;
```

- 使用参数

```sql
-- 存储过程不显示结果，它把结果返回给指定的变量
CREATE PROCEDURE productpricing(
	OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
	SELECT MIN(price) INTO pl FROM products;
	SELECT MAX(price) INTO ph FROM products;
	SELECT AVG(price) INTO pa FROM products;
END //

-- 使用 IN 向存储过程传递参数
REATE PROCEDURE productpricing(
	IN onumber INT,
    OUT total DECIMAL(8,2)
)
BEGIN
	SELECT SUM(price*quantity) FROM orderitems 
	WHERE order_number = onumber INTO total;
END //
```

> 可通过 IN、OUT、INOUT 给存储过程传递参数和从存储过程传出结果

- 变量定义与赋值

  [mysql 变量](https://www.cnblogs.com/Brambling/p/9259375.html)

```sql
CREATE PROCEDURE test()
BEGIN
	-- 局部变量：只能在声明的存储过程内部使用
	-- 全局变量：
	-- 用户变量：
	-- 会话变量：
	
	-- 变量定义
	DECLARE number1 int DEFAULT 0;
	DECLARE number2 int;
	-- 变量赋值
	set number1=10;
	set number2:=20; -- 推荐使用，因为赋值与判断均使用的是 =
	-- dual 表示伪表，是 mysql 提供的默认表
	SELECT 	number1,number2 [from dual];
END
```

- 智能存储过程

> 只有在存储过程内包含业务规则和智能处理时，存储过程才发挥用处

> 场景：获得订单合计，但要对订单合计增加营业税，不过只针对某些顾客

``` sql
-- Name: ordertotal
-- Parameters: onumber = order number
--				taxable = 0 if not taxable, 1 if taxable
--				ototal = order total variable

CREATE PROCEDURE ordertotal(
	IN onumber INT,
    IN taxable BOOLEAN,
    OUT ototal DECIMAL(8,2)
) COMMENT 'Obtain order total, optionally adding tax'
BEGIN
	-- Declare variable for total
	DECLARE total DECIMAL(8,2);
	-- Declare tax percentage
	DECLARE taxrate INT DEFAULT 6;
	
	-- Get the order total
	SELECT SUM(item_price*quantity) FROM orderitems 
	WHERE order_num = onumber INTO total;
	
	--Is this taxable?
	IF taxable THEN
		-- Yes, so add taxrate to the total
		SELECT total+(total/100*taxrate) INTO total;
	END IF;
	
	-- And finally, save to out variable
	SELECT total INTO ototal;
END //
```

- 检查存储过程

```sql
-- 显示存储过程创建语句
SHOW CREATE PROCEDURE ordertotal;
-- 查看存储过程状态
SHOW PROCEDURE STATUS LIKE 'ordertotal';
-- 查询数据库的存储过程
SHOW PROCEDURE STATUS WHERE DB='shop';
```



##### 游标(cursor)

> 游标是一个存储在 MySQL 服务器上的数据库查询，它不是一条 SELECT 语句，而是被该语句检索出来的结果集，MySQL 游标只能用于存储过程和函数

使用游标

- 创建游标

```sql
CREATE PROCEDUCE processorders()
BEGIN
	-- Declare a cursor named ordernumbers
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
END //
```

- 打开和关闭游标

```sql
CREATE PROCEDUCE processorders()
BEGIN
	-- Declare a cursor named ordernumbers
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
	
	-- Open the cursor
	OPEN ordernumbers;
	
	-- Close the cursor
	-- 不再需要游标时应该关闭，MySQL会在到达END语句时自动关闭游标
	CLOSE ordernumbers;
END //
```

- 使用游标数据

```sql
CREATE PROCEDUCE processorders()
BEGIN
	-- Declare local variables
	DECLARE o INT;
	
	-- Declare the cursor
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
	
	-- Open the cursor
	OPEN ordernumbers;
	
	-- Get order number
	FETCH ordernumbers INTO o;
	
	-- Close the cursor
	CLOSE ordernumbers;
END //
```

```sql
CREATE PROCEDUCE processorders()
BEGIN
	-- Declare local variables
	DECLARE done BOOLEAN DEFAULT 0;
	DECLARE o INT;
	DECLARE t DECIMAL(8,2);
	
	-- Declare the cursor
	DECLARE ordernumbers CURSOR
	FOR
	SELECT order_num FROM orders;
	
	-- Declare continue handler
	DECLARE CONTINUE HANDLER FOR SQLSTATE '02000' SET done=1;
	
	-- Create a table to store the results
	CREATE TABLE IF NOT EXISTS ordertotals(
        order_num INT, total DECIMAL(8,2));
	
	-- Open the cursor
	OPEN ordernumbers;
	
	-- Loop through all rows
	REPEAT
	
		-- Get order number
		FETCH ordernumbers INTO o;
		--Get the total for this order
		CALL ordertotal(o, 1, t);
		
		-- Insert order and total into ordertotals
		INSERT INTO ordertotals(order_num, total) VALUES(o, t)
		
	-- End of loop
	UNTIL done END REPEAT;
	
	-- Close the cursor
	CLOSE ordernumbers;
END //
```



##### 触发器

> 触发器是在事件发生时(表发生更改)自动执行的语句，事件可为 DELETE、UPDATE、INSERT语句 或位于 BEGIN AND 语句之间的一组语句

创建触发器

```sql
CREATE TRIGGER newproduct AFTER INSERT ON products FOR EACH ROW SELECT 'Product added';
```

> 只有表才支持触发器，视图、临时表不支持
>
> 每个表每个事件每次只允许一个触发器，单个触发器不能与多个事件或多个表关联
>
> BEFORE 触发器失败，将不执行请求的操作；BEFORE 触发器失败或语句本身失败，将不执行 AFTER 触发器

删除触发器

```sql
DROP TRIGGER newsproduct;
```

> 触发器不能更新或覆盖，只能删除重建

使用触发器

- INSERT 触发器

  > 在 INSERT 触发器代码内，可引用一个名为 NEW 的虚拟表访问被插入的行
  >
  > 在 BEFORE INSERT 触发器中，NEW 的值也可以被更新，因此通常将 BEFORE 用于数据验证和净化

  ```sql
  CREATE TRIGGER neworder AFTER INSERT ON orders FOR EACH ROW SELECT NEW.order_num;
  ```

- DELETE 触发器

  > 在 DELETE 触发器代码内，可引用一个名为 OLD 的虚拟表访问被删除的行

  ```sql
  -- 在任意订单删除前将被删除的订单存入存档表
  CREATE TRIGGER deteleorder BEFORE DELETE ON orders FOR EACH ROW
  -- 可通过BEGIN AND编写多语句触发器
  BEGIN
  	INSERT INTO archive_orders(order_num, order_date, cust_id) 
  	VALUES(OLD.order_num, OLD.order_date, OLD.cust_id);
  END //
  ```

- UPDATE 触发器

  > 在 UPDATE 触发器代码中，可引用 OLD 虚拟表访问以前的值，引用 NEW 虚拟表访问更新的值

  ```sql
  -- 更新数据时保证州名缩写总是大写
  CREATE TRIGGER updatevendor BEFORE UPDATE ON vendors FOR EACH ROW SET NEW.vend_state = Upper(NEW.vend_state);
  ```

  

##### 管理事务处理

> 事务(transaction)：一组 SQL 语句
>
> 回退(rollback)：撤销指定 SQL 语句的过程
>
> 提交(commit)：将未存储的 SQL 语句结果写入数据库表
>
> 保留点(savepoint)：事务中临时设置的占位符，可以对它发布回退

开启事务

```sql
START TRANSACTION;
```

回滚事务

> 事务处理用来管理 INSERT、UPDATE、DELETE 语句
>
> 不能回退 SELECT 语句，不能回退 CREATE、DROP 语句

```sql
ROLLBACK;
```

提交事务

> 当 COMMIT 或 ROLLBACK 语句执行后，事务会自动关闭，将来的更改会隐含提交(implicit commit)

```sql
COMMIT;
```

使用保留点

```sql
-- 创建占位符（保留点）
SAVEPOINT delete1;
-- 回退到保留点
ROLLBACK TO delete1;
-- 明确释放保留点（保留点在事务处理完成即执行ROLLBACK或COMMIT语句后自动释放）
RELEASE delete1;
```

更改默认提交行为

```sql
-- 指示 MySQL 不自动提交更改
SET autocommit=0;
```



##### 全球化和本地化

数据库表用于存储和检索数据，不同的语言和字符需要以不同的方式存储和检索

字符集：字母和符号的集合<br/>编码：为某个字符成员的内部表示<br/>校对：为规定字符如何比较的指令

查看 MySQL 支持的字符集

```sql
SHOW CHARACTER SET;
```

查看 MySQL 支持的校对顺序

```sql
SHOW COLLATION;
```

指定表的字符集和校对

```sql
CREATE TABLE table_name(
	...
) DEFAULT CHARACTER SET hebrew
  COLLATE hebrew_geneal_ci; -- _ci代表不区分大小写，_cs区分
```

指定列的字符集和校对

```sql
CREATE TABLE table_name(
	name varchar(10) CHARACTER SET latin1 COLLATE latin1_general_ci
)
```

校对在对 ORDER BY 子句检索出来的数据排序时起重要作用，如下可指定与表不同的校对规则

```sql
SELECT * FROM user ORDER BY name COLLATE latin1_general_cs;
```

使用 Cast() 或 Convert() 函数可将串在字符集间进行转换

##### 安全管理

用户应该对他们需要的数据具有适当的访问权，既不能多也不能少

MySQL 用户账号和信息存储在 名为mysql 数据库中，如下获得所有用户账号列表

```sql
USE mysql;
SELECT user FROM mysql;
```

创建用户账号

```sql
-- IDENTIFIED BY 指定纯文本口令，MySQL 会在存储信息前对其进行加密
-- GRANT 和 INSERT 语句也可用于创建用户
CREATE USER ben IDENTIFIED BY 'abc132';
```

重命名用户账号

```sql
RENAME USER ben TO ben2;
```

删除用户账号

```sql
-- MySQL 5 之前删除用户需先用 REVOKE 删除账号相关权限
DROP USER ben2;
```

显示用户权限

```sql
SHOW GRANTS FOR ben;
```

设置用户权限

```sql
-- 设置用户ben对school数据库所有表具有SELECT权限
GRANT SELECT ON school.* TO ben;
```

撤销用户权限

```sql
REVOKE SELECT ON school.* TO ben;
```

更改用户口令

```sql
-- Password()对口令进行加密，不指定用户名时更新当前用户口令
SET PASSOWRD FOR ben = Password('456zxc');
```

