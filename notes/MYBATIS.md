# MYBATIS

- [MyBatis 基础]()
- [源码分析]()

### 概述

MyBatis 原来是 apache 的一个开源项目 iBatis，2010 年由 apache software foundation 迁移到 Google code，并更名为 MyBatis，2013 年 11 月迁移到 GitHub。

MyBatis 是一个持久层框架，对 jdbc 的操作数据库的过程进行了封装，使开发者只需要关注 SQL 本身。它通过 xml 或注解的方式将要执行的各种 statement 配置起来，通过 Java 对象和 statement 中的 SQL 进行映射生成最终执行的 SQL 语句，最后由 MyBatis 框架执行 SQL 并将结果映射成 Java 对象并返回

官方文档：

MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录

### MyBatis 解决原生 JDBC 问题总结

1. 数据库频繁创建连接、释放资源造成系统资源的浪费，影响系统性能（使用数据库连接池可以解决）

   解决：SqlMapConfig.xml 中配置了数据库连接池，使用连接池管理数据库连接

2. SQL 语句存在硬编码问题，实际应用中 SQL 变化的可能性较大，SQL 变动需要改变 Java 代码，造成代码不易维护

   解决：将 SQL 语句配置在 mapper.xml 中实现与 Java 代码分离

3. 使用 preparedStatemet 对占位符传参数存在硬编码，SQL 语句的条件可能增加或减少

   解决：MyBatis 自动将 java 对象映射至 SQL 语句，通过 statement 中的 parameterType 定义输入参数的类型

4. 结果集的解析存在硬编码，SQL 变化可能导致解析代码变化（将数据库记录封装成 POJO 对象解析比较方便）

   解决：MyBatis 自动将 SQL 执行结果映射至 Java 对象，通过 statement中 的 resultType 定义输出结果的类型

### MyBatis 架构

![MyBatis 架构](../images/MYBATIS%20constructure.PNG)

Executor 具有 BaseExecutor 和 CacheExecutor 两个实现类

### 入门程序

##### 环境搭建

- 创建工程

- 导包

MyBatis 核心包：mybatis-xxx.jar

MyBatis 依赖包：lib 文件夹下

数据库驱动包

- 书写配置文件

log4j.properties, MyBatis 默认使用 log4j 作为输出日志信息

```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

SqlMapConfig.xml, MyBatis 的全局配置文件，配置了 MyBatis 的运行环境等信息

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 和spring整合后 environments配置将废除 -->
	<environments default="development">
        <!-- 运行环境，包含数据源和事务管理器 -->
		<environment id="development">
			<!-- 使用jdbc事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
</configuration>

```

- 创建 POJO 对象

POJO 类在 MyBatis 进行 SQL 映射时使用，其属性名通常与数据库表字段名一一对应

```java
public class User {
    private int id;
    private String username;
    private String password;
    private String gender;
    //getter and setter...
}
```

- 书写 SQL 映射文件 mapper.xml，名字任意，这里取名为 User.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，后面会讲 -->
<mapper namespace="user">
    
</mapper>
```

```xml
<!-- 在 SqlMapConfig.xml 中加载映射文件 -->
<mappers>
	<mapper resource="User.xml" />
</mappers>
```

- 编写测试程序

```java
public class Test {
    public void testQueryUserById() {
        //创建 SqlSessionFactoryBuilder 对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        //加载 SqlMapConfig.xml 配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //创建 SqlSessionFactory 对象
        SqlSessionFactory slqSessionFactory = sqlSessionFactoryBuilder.build(in);
        //创建 SqlSession 对象
        SqlSession sqlSession = sqlSessionFactory.openSesssion();
        //通过 sqlSession 对象执行查询，第一个参数指明方法，第二个参数为 SQL 执行参数
        User user  = sqlSession.selectOne("user.queryUserById",1);
        //释放资源
        sqlSession.close();
    }
}
```

User.xml 具体配置信息

```xml
<mapper namespace="user">
    <!-- 
 	parameterType：指定输入参数类型，mybatis通过ognl从输入对象中获取参数值拼接在sql中
	resultType：指定输出结果类型，mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象。如果有多条数据，则分别进行映射，并把对象放到容器List中
	-->
	<select id="queryUserById" paramerterType="Integer" resultType="com.project.mybatis.pojo.User">
        <!-- #{} 输入参数的占位符，相当于 jdbc 的 ？ -->
    	SELECT * FROM user WHERE id = #{v}
    </select>
    
    <!-- 根据用户名模糊查询配置 -->
    <!-- String、Integer等基本类型MyBatis已经进行封装，可不写完整类名 -->
    <select id="queryUserByName" parameterType="String" resultType="com.project.mybatis.pojo.User">
        <!--
			${}:括号类必须写value，传递的String参数不带引号，可替换为"%"#{}"%"，即使用字符串拼接。${}表示拼接sql串，通过${}可以将parameterType 传入的内容拼接在sql中且不进行jdbc类型转换， ${}可以接收简单类型值或pojo属性值，如果parameterType传输单个简单类型值，${}括号中只能是value。
			#{}:括号类可写任何值，传递的参数根据需要自动带引号，如必须需要String类型的参数，会自动添加引号。#{}表示一个占位符号，通过#{}可以实现preparedStatement向占位符中设置值，自动进行java类型和jdbc类型转换。#{}可以有效防止sql注入。 #{}可以接收简单类型值或pojo属性值。 如果parameterType传输单个简单类型值，#{}括号中可以是value或其它名称。
		-->
    	SELECT * FROM user WHERE username LIKE '%${value}%'
    </select>
    
    <!-- 添加用户 -->
    <!-- java代码中需手动提交事务才能成功操作数据库 -->
    <insert id="saveUser" parameterType="com.project.mybatis.pojo.User">
        <!-- 自增主键返回，例如添加的用户需要立即添加订单，则可直接返回用户id到user对象，不需要再去查找id进行user对象封装 -->
        <!-- selectKey 标签实现主键返回 -->
		<!-- keyColumn:主键对应的表中的哪一列 -->
		<!-- keyProperty：主键对应的pojo中的哪一个属性 -->
		<!-- order：设置在执行insert语句前执行查询id的sql，还是在执行insert语句之后执行查询id的sql，自增时再插入数据信息后再添加主键，所以值为after，当使用UUID作为主键时，先插入主键，再插入数据信息，此时应取值为before -->
        <selectKey keyColumn="id" keyProperty="id" resultType="Integer" order="after">
        	select last_insert_id()
        </selectKey>
    	insert into user(username,password,gender) values(#{username},#{password},#{gender})
    </insert>
</mapper>
```

### MyBatis 与 Hibernate 的区别

Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。mybatis可以通过XML或注解方式灵活配置要运行的sql语句，并将java对象和sql语句映射生成最终执行的sql，最后将sql执行的结果再映射生成java对象。

Mybatis学习门槛低，简单易学，程序员直接编写原生态sql，可严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，例如互联网软件、企业运营类软件等，因为这类软件需求变化频繁，一但需求变化要求成果输出迅速。但是灵活的前提是mybatis无法做到数据库无关性，如果需要实现支持多种数据库的软件则需要自定义多套sql映射文件，工作量大。

Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件（例如需求固定的定制化软件）如果用hibernate开发可以节省很多代码，提高效率。但是Hibernate的学习门槛高，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡，以及怎样用好Hibernate需要具有很强的经验和能力才行。

总之，按照用户的需求在有限的资源环境下只要能做出维护性、扩展性良好的软件架构都是好架构，所以框架只有适合才是最好。

### Mapper 动态代理开发

Mapper接口开发方法只需要程序员编写Mapper接口（相当于Dao接口），由Mybatis框架根据接口定义创建接口的动态代理对象

Mapper接口开发需要遵循以下规范：

1. Mapper.xml 文件中的 namespace 与 mapper 接口的类路径相同

2. Mapper 接口方法名和 Mapper.xml 中定义的每个 statement 的 id 相同 

3. Mapper 接口方法的输入参数类型和 mapper.xml 中定义的每个 sql 的 parameterType 的类型相同

4. Mapper 接口方法的输出参数类型和 mapper.xml 中定义的每个 sql 的 resultType 的类型相同

### MyBaits 核心配置文件

```xml
<!-- 主配置文件 -->
<configuration>
    <!-- 可以用来加载外部配置信息，比如引入 MySql 连接配置文件 -->
    <!-- 具体使用时用 el 表达式获取值，如 ${jdbc.username } -->
    <properties resource="db.properties" >
    	<!-- 也可在内部通过 property 定义属性，如果外部配置文件定义该属性，则内部定义被覆盖，因为 MyBatis 先读取 properties 元素体内定义的属性，再读取 properties 元素中 resource 或 url 加载的属性，后读取的覆盖先读取的同名属性 -->
        <property name="jdbc.username" value="root" />
    </properties>
    
    <settings></settings>
	
    <!-- 类型别名，MyBatis 默认已经支持一些别名，如基本数据类型及其包装类型，Stirng，Date，Map 等等，通过以下标签可以自定义别名 -->
    <typeAilases>
    	<!-- 单个别名定义 -->
        <typeAilas type="com.project.mybatis.pojo.User" alias="User" />
        <!-- 批量别名定义，扫描整个包下的类，别名为类名 -->
        <package name="com.project.mybatis.pojo" />
    </typeAilases>
    
    <mappers>
    	<!-- mapper 的多种配置方式 -->
        <mapper resource ="User.xml" />
        <!-- 使用 mapper 接口路径，要求 mapper.xml 文件名必须与 mapper 接口名相同，且放在同一个目录下，如以下配置映射文件名必须为 UserMapper.xml -->
        <mapper class="com.project.mybatis.mapper.UserMapper" />
        <!-- 基本不使用，因为需要绝对路径，window 下绝对路径带盘符 -->
        <mapper url="" />
        <!-- 要求 mapper.xml 文件名必须与 mapper 接口名相同，且放在同一个目录下 -->
        <package name="com.project.mybatis.mapper" />
    </mappers>

</configuration>
```



### 映射文件配置

```xml
<mapper>
    <!-- 开启二级缓存 -->
    <cache></cache>
    <!-- 引用其他命名空间作为二级缓存区域 -->
    <cache-ref namespace=""></cache-ref>
    <!-- 描述如何加载结果集 -->
    <resultMap></resultMap>
    <!-- SQL 片段，使用 include 进行引用 -->
    <sql id=""></sql>
    <!-- 显示均为默认值，当主键不是第一列时需要设置 keyColumn -->
    <insert flushCache="true"
            useGeneratedKeys="false"
            keyProperty="" 
            keyColumn=""></insert>
    <update></update>
    <delete></delete>
    <select flushCache="false"
            userCahe="true"></select>
</mapper>
```



### 输入映射和输出映射

###### parameterType

传递简单类型

``` xml
<!-- 使用 #{} 占位符或 ${} 进行 SQL 拼接 -->
<select id="findByUserId" parameterType="Integer" resultType="User"></select>
```

传递 POJO 对象

```xml
<!-- 使用 ognl 表达式解析对象字段的值，#{} 或 ${} 中的值为 POJO 属性名称 -->
<insert id="saveUser" parameter="User"></insert>
```

传递 POJO 包装对象

```xml
<!-- POJO 类中的一个属性是另一个 POJO 类，例如 QueryVo POJO 类中有一个属性为 User 类-->
<select id="findUserByName" parameterType="QueryVo" resultType="user">
	select * from user where username like '%${user.username}%'
</select>
```

###### resultType

输出简单类型

```xml
<select id="findUserCount" resultType="int">
	select count(1) from user
</select>
```

输出 POJO 对象

输出 POJO 列表

###### resultMap

resultType 可以指定将查询结果映射为 pojo，但需要 pojo 的属性名和 sql 查询的列名一致方可映射成功，如果sql查询字段名和 pojo 的属性名不一致，可以通过 resultMap 将字段名和属性名作一个对应关系，resultMap 实质上还需要将查询结果映射到 pojo 对象中， resultMap 可以实现将查询结果映射为复杂类型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询

```xml
<mapper>
    <!-- resultMap最终还是要将结果映射到pojo上，type就是指定映射到哪一个pojo,id为设置ResultMap的id -->
    <resultMap type="User" id="userResultMap">
    	<!-- 定义主键 -->
        <id property="id" column="u_id" />
        <!-- 定义普通属性 -->
        <result property="username" column="u_username" />
        <!-- 单表查询可以将属性名与表列名相同的属性省略，但多表查询时必须书写完整，否则省略不能映射成功 -->
        <result property="password" column="password" />
    </resultMap>
	<select id="findUserById" parameterType="Integer" resultMap="userResultMap">
        select * from user where id = #{value}
    </select>
</mapper>
```



### 关联关系

###### 一对一

方式一：使用 resultType，定义专门的pojo类作为输出类型，其中定义了sql查询结果集所有的字段

```java
//例如查询订单并关联查询出订单所属用户信息，定义专用 POJO 类继承订单内，在此类中定义需要的用户字段
public class OrderUser extends Order {
    //只需要定义用户信息字段
    private String username;
}
```

方式二：使用 resultMap，配置 resultMap 用于映射一对一查询结果

```java
//订单类中持有对象应用
public class Order {
    //...
    private User user;
    //getter setter...
}
```

```xml

<select id="findOrderById" parameter="Integer" resultMap="orderUser">
	select * from order o join user u on o.id=u.id where id = #{id}
</select>
<resultMap type="order" id="orderUser">
    <!-- 配置订单对象本身的属性，关联关系映射中，必须显示配置所有字段与列的对应关系，即使命名相同 -->
	<id property="id" column="id" />
	<result property="orderName" column="orderName" />
	<result property="number" column="number" />

	<!-- association ：配置一对一属性 -->
	<!-- property:order里面的User属性名 -->
	<!-- javaType:关联对象的属性类型，使用全限定类名指定 -->
	<association property="user" javaType="user">
		<!-- id:声明主键，表示user_id是关联查询对象的唯一标识-->
		<id property="id" column="user_id" />
		<result property="username" column="username" />
		<result property="address" column="address" />
	</association>
</resultMap
```

###### 一对多

在 POJO 类中一方持有多方的集合引用，多方持有一方对象引用

```xml
<resultMap type="user" id="userOrderResultMap">
	<id property="id" column="id" />
	<result property="username" column="username" />
	<result property="birthday" column="birthday" />
	<result property="sex" column="sex" />
	<result property="address" column="address" />

	<!-- 配置一对多的关系，通过 ofType 属性自动集合泛型 -->
	<collection property="orders" javaType="list" ofType="order">
		<!-- 配置主键，是关联Order的唯一标识 -->
		<id property="id" column="oid" />
		<result property="number" column="number" />
	</collection>
</resultMap>
```

###### 多对多

多对多的关联关系需要在两方实体对象中均持有关联方的集合，例如学生与课程的关系

```java
public class Student {
    private List<Course> courserList;
}
```

```java
publc class Course {
    private List<Student> stuList;
}
```

在表关系中，通过引入中间表来建立学生表与课程表之间的关系，中间表至少具有两个外键，分别指向学生表与课程表的主键

在 MyBatis 映射关系配置中，使用两个 collection 进行关系映射，即使用两个一对多



### 动态 SQL

动态 SQL 是 MyBatis 的强大特性之一，使用 JDBC 进行 SQL 拼接时，要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号等问题，而通过 MyBatis 提供的标签方法可简单快速实现动态拼接 SQL

###### if 

功能同流程控制语句中的 if 语句，但没有 else

```xml
<select id="selectUserByUser" parameterType="user" resultType="user">
	select id,username,password from user
    where 1=1
    <if test="username!=null and username!=''">
    	and username like '%${username}%'
    </if>
    <if test="password!=null and password!=''">
    	and password = #{password}
    </if>
</select>
```

###### choose, when ,otherwise

功能同 if...else if...else

```xml
<!-- 存在名字根据名字查，否则根据价格查，都为空根据描述查 -->
<select id="selectProduct" parameterType="Product" parameterType="Product">
	select * from product where
    <choose>
        <when test="pname!=null">
        	name=#{name}
        </when>
        <when test="price!=0">
        	price=#{price}
        </when>
        <otherwise>
        	desc=#{desc}
        </otherwise>
    </choose>
</select>
```

###### where

```xml
<select id="fingUserByUser" parameterType="user" resultType="user">
	select id,username,password from user
    <!-- where 元素只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句，而且，若语句的开头为“AND”或“OR”，where 元素也会将它们去除，这样可以不再添加1=1这样的条件去解决多and的问题 -->
    <where>
        <if test="username!=null and username!=''">
            and username like '%${username}%'
        </if>
        <if test="password!=null and password!=''">
            and password = #{password}
        </if>
    </where>
</select>
```

###### set

用于编写动态更新的 SQL 语句

```xml
<update id="">
	update Author
    <set>
	    <if test="username != null">username=#{username},</if>
	    <if test="password != null">password=#{password},</if>
	    <if test="email != null">email=#{email},</if>
	    <if test="bio != null">bio=#{bio}</if>
	</set>
	where id=#{id}
</update>
```

###### trim

```xml
<!-- 实现 where 的功能 -->
<select id="">
    select * from user
    <trim prefix="WHERE" prefixoverride="AND |OR ">
        <if test="name != null and name.length()>0"> AND name=#{name}</if>
        <if test="gender != null and gender.length()>0"> AND gender=#{gender}</if>
    </trim>
</select>
    
<!-- 实现 set 的功能 -->
<update id="">
    update user
    <trim prefix="set" suffixoverride="," suffix=" where id = #{id} ">
        <if test="name != null and name.length()>0"> name=#{name} , </if>
        <if test="gender != null and gender.length()>0"> gender=#{gender} ,  </if>
    </trim>
</update>
```

###### foreach

可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象传递给 *foreach* 作为集合参数。当使用可迭代对象或者数组时，index 是当前迭代的次数，item 的值是本次迭代获取的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值

```xml
<!-- 例如在 QueryVo 中包含ids的集合，根据ids查询用户 -->
<select id="queryUserByIds" parameterType="queryVo" resultType="user">
	SELECT * FROM user
	<where>
		<!-- foreach标签，进行遍历 -->
		<!-- collection：遍历的集合，这里是QueryVo的ids属性 -->
		<!-- item：遍历的项目，可以随便写，，但是和后面的#{}里面要一致 -->
		<!-- open：在前面添加的sql片段 -->
		<!-- close：在结尾处添加的sql片段 -->
		<!-- separator：指定遍历的元素之间使用的分隔符 -->
		<foreach collection="ids" item="item" open="id IN (" close=")"
			separator=",">
			#{item}
		</foreach>
	</where>
</select>
```

```xml
<!-- 使用数组作为参数 -->
<foreach collection="array" item="item" open="id IN (" close=")" separator=",">
    #{item}
</foreach>
```

```xml
<!-- 使用集合作为参数 -->
<foreach collection="list" item="item" open="id IN (" close=")" separator=",">
    #{item}
</foreach>
```

###### SQL 片段

```xml
<!-- sql片段可将重复的sql提取出来，使用时用include引用即可，最终达到sql重用的目的 -->
<sql id="selectSql">
	select * from user
</sql>
<select id="findUserById" parameterType="Integer" resultType="user">
    <!-- 如果要使用别的Mapper.xml配置的sql片段，可以在refid前面加上对应的Mapper.xml的namespace -->
	<include refid="selectSql" />
    where id = #{v}
</select>
```



### 延迟加载

延迟加载其实就是将数据加载时机推迟，将采用高级映射实现多表联查时向数据库发出的 SQL 语句拆分成若干条单表查询的 SQL 语句，当需要返回数据时才会向数据库发出只针对当前数据的 SQL 语句

先从单表查询、需要时再从关联表去关联查询，提升数据库性能，因为查询单表要比关联查询多张表速度要快，内存资源占用更少

###### 延迟加载 setting 参数配置

```xml
<configuration>
    <settings>
        <!-- true:开启懒加载，所有关联对象都会延迟加载，默认为false -->
    	<setting name="lazyLoadingEnabled" value="true" />
        <!-- 侵入式加载，任何方法的调用都会加载该对象的所有属性，否则每个对象按需加载 -->
        <setting name="aggressiveLazyLoading" value="false" />
        <!-- 指定哪个对象的方法触发一次延迟加载 -->
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
    </settings>
</configuration>
```

###### 延迟加载映射文件配置

```xml
<mapper>
    <resultMap id="" type="">
        <association property="" javaType="" select="" column="" fetchType=""/>
        <collection property="" ofType="" select="" column=""/>
    </resultMap>
</mapper>
```



### 缓存机制

合理使用缓存是常见的优化方式，通过将从数据库查询出来的数据放入缓存中，下次再需要使用此数据时就可以直接从缓存中读取，避免了频繁的对数据库进行操作，减轻了数据库的压力，同时提高系统使用性能

###### 一级缓存

一级缓存的作用范围为 SqlSession，不同 SqlSession 之间的缓存数据区域不能互相读取，在 SqlSession 实例对象中有一个数据结构用于存储缓存数据

一级缓存工作原理：

当用户发起查询请求后，SqlSession 先去缓存中查找数据，如果有数据则直接返回，没有则从数据库查询并返回，通知将查询到的数据存入一级缓存区域；如果 SqlSession 执行 commit，即执行增删改操作时会清空一级缓存区域，避免发生脏读

MyBatis 默认开启一级缓存，在与 spring 进行整合之后，如果没有事务，一级缓存是没有意义的

###### 二级缓存

二级缓存是 mapper 级别的缓存，多个 SqlSession 去操作同一个 Mapper 的 sql 语句，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的

UserMapper 有一个二级缓存区域（按 namespace 分），其它 mapper 也有自己的二级缓存区域（按 namespace 分）。每一个 namespace 的 mapper 都有一个二级缓存区域，两个 mapper 的 namespace 如果相同，这两个 mapper 执行 sql 查询到数据将存在相同的二级缓存区域中

开启二级缓存

```xml
<!-- 在主配置文件中打开二级缓存总开关 -->
<settings>
	<setting name="cacheEnabled" value="true" />
</settings>
```

```xml
<!-- 在需要开启二级缓存的 mapper.xml 中加入 cache 标签 -->
<mapper>
	<cache></cache>
</mapper>
```

```java
//使用二级缓存的 POJO 类实现 Serializable 接口
public class Student implements Serializable {}
```

何时使用二级缓存

对于查询多 commit 少且用户对查询结果实时性要求不高，此时采用 mybatis 二级缓存技术可以降低数据库访问量，提高访问速度；但是二级缓存也存在着弊端，二级缓存是建立在同一个 namespace 下的，如果对表的操作查询可能有多个 namespace，那么得到的数据就是错误的
<<<<<<< HEAD

注意：二级缓存是事务性的。这意味着，当 SqlSession 完成并提交时，或是完成并回滚，但没有执行 flushCache=true 的 insert/delete/update 语句时，缓存会获得更新。



### 注解使用

@Select

@Insert

@Update

@Delete

@Results

@Result

@One

@Many



### 逆向工程

`MyBatis`逆向工程需要用到的就是MyBatis官方提供的 `MyBatis Generator（MBG）`。`MBG` 是 `MyBatis` 和 `iBATIS` 的代码生成器，它将为所有版本的 `MyBatis` 以及版本 2.2.0 之后的 `iBATIS` 版本生成代码。`MBG` 对简单 `CRUD`（增删改查）的大部分数据库操作产生重大影响。但是您仍然需要为连接查询或存储过程手动编写SQL和对象代码

创建好数据库表之后，`MBG` 可以根据数据库表自动生成 `pojo类` 、 `example类(用于添加条件，相当where语句后面的部分 )`、`mapper文件`

###### 使用逆向工程

1. 导入 jar 包

   `mybatis-generator-core-1.3.7.jar`

2. 书写配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE generatorConfiguration
     PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
     "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   
   <generatorConfiguration>
       <context id="testTables" targetRuntime="MyBatis3">
           <commentGenerator>
               <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
               <property name="suppressAllComments" value="true" />
           </commentGenerator>
           <!--数据库连接的信息：驱动类、连接地址、用户名、密码 ,加上“useSSL=false”是因为我SSL连接数据库出现了错误 -->
           <jdbcConnection driverClass="com.mysql.jdbc.Driver"
               connectionURL="jdbc:mysql://localhost:3306/test"
               userId="root" password="xxx">
           </jdbcConnection>
           <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为true时把JDBC DECIMAL 
               和 NUMERIC 类型解析为java.math.BigDecimal -->
           <javaTypeResolver>
               <property name="forceBigDecimals" value="false" />
           </javaTypeResolver>
   
           <!-- targetProject:生成pojo类的位置 -->
           <javaModelGenerator targetPackage="pojo"
               targetProject=".\src\main\java">
               <!-- enableSubPackages:是否让schema作为包的后缀 -->
               <property name="enableSubPackages" value="false" />
               <!-- 从数据库返回的值被清理前后的空格 -->
               <property name="trimStrings" value="true" />
           </javaModelGenerator>
   
           <!-- targetProject:mapper映射文件生成的位置 -->
           <sqlMapGenerator targetPackage="mapper" targetProject=".\src\main\java">
               <!-- enableSubPackages:是否让schema作为包的后缀 -->
               <property name="enableSubPackages" value="false" />
           </sqlMapGenerator>
   
           <!-- targetPackage：mapper接口生成的位置 -->
           <javaClientGenerator type="XMLMAPPER"
               targetPackage="mapper" targetProject=".\src\main\java">
               <!-- enableSubPackages:是否让schema作为包的后缀 -->
               <property name="enableSubPackages" value="false" />
           </javaClientGenerator>
   
           <!-- 指定数据库表 -->
           <table schema="" tableName="empolyee"></table>
           <table schema="" tableName="department"></table>
       </context>
   </generatorConfiguration>
   ```

3. 书写逆向工程代码

   ```java
   public class Generator {
       public static void main(String[] args) {
          List<String> warnings = new ArrayList<String>();
          boolean overwrite = true;
          //读取xml配置文件，推荐使用这种方式
          File configFile = new File("generatorConfig.xml");
          ConfigurationParser cp = new ConfigurationParser(warnings);
          Configuration config = cp.parseConfiguration(configFile);
          DefaultShellCallback callback = new DefaultShellCallback(overwrite);
          MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
          myBatisGenerator.generate(null);
       }
   }
   ```

   

=======
>>>>>>> 8c0970afe291e1b9801a3938ce0f4c32bcc58683
