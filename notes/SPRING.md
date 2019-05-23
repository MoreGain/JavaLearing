# SPRING

### 概述

spring 是一个分层的一站式开源框架，它的核心是 IoC 和 AOP

spring 是对象的容器，它是一个一站式框架，这是因为它是容器性质的框架，而容器中装什么东西就有什么功能

### 特点

1. 方便解耦，简化开发

   Spring 就是一个大工厂，可以将所有对象创建和依赖关系维护，交给 Spring 管理

2. AOP 编程的支持

   Spring 提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能

3. 声明式事务的支持

   只需要通过配置就可以完成对事务的管理，而无需手动编程

4. 方便程序的测试

   Spring 对 Junit4 支持，可以通过注解方便的测试 Spring 程序

5. 方便集成各种优秀框架

   Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts 、Hibe rnate 、MyBatis 、Quartz 等）的直接支持

6. 降低 JavaEE API 的使用难度

   Spring 对 JavaEE 开发中非常难用的一些 API JDBC 、 JavaMail 、远程调用等），都提供了封装，使这些 API 应用难度大大降低

### spring 思想

###### IOC 

Inverse of Control，控制反转，将对象的创建交给容器完成

###### DI 

Dependency Injection，依赖注入，

### ApplicationContext & BeanFactory

###### BeanFactory

spring 原始接口，针对原始接口的类功能较为单一

BeanFactory 接口实现的容器，每次在获得对象时才会创建对象

###### ApplicationContext

每次容器启动时就会创建容器中配置的所有对象，并提供更多功能

从类路径下加载配置文件：ClassPathXmlApplicationContext

从硬盘绝对路径下加载配置文件：FileSystemXmlApplicationContext

所以在 web 开发中，使用 ApplicationContext，开资源匮乏的环境可以使用 BeanFactory

### 配置文件

###### bean 元素

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<!-- Bean元素：描述需要spring容器管理的对象 -->
	<!-- 
 		name:给被管理对象取的名字，获得对象时根据该名称获得对象，可重复，可以使用特殊字符
		class:被管理对象的完整类名
		id:与name属性一样，但要遵循名称不可重复，不能使用特殊字符的原则
	-->
    <bean id="user" name="user" class="com.project.pojo.User"></bean>
    
    <!-- spring 创建对象的三种方式 -->
    <!-- 调用类的空参构造 -->
    <bean name="user" class="com.project.pojo.User"></bean>
    <!-- 静态工厂创建 -->
    <bean name="user2" class="com.project.factory.UserFactory" 
          factory-method="createUser"></bean>
    <!-- 实例工厂创建 -->
    <bean name="user3" factory-bean="userFactory" 
          factory-method="createUser2"></bean>
    <bean name="userFactory" class="com.project.factory.UserFactory"></bean>
    
    <!-- bean元素的scope属性：
 			singleton:默认值，单例对象，被标识为单例的对象在spring容器中只会存在一个实例。绝大多数情况下均使用默认值
			prototype:被标识为多例的对象，每次再获得才会创建，每次创建都是新的对象。整合struts2时，ActionBean必须配置为多例的
			request:web环境下，对象与request生命周期一致
			session:web环境下，对象与session生命周期一致
	-->
    <bean name="user" class="com.project.pojo.User" scope="single"></bean>
    
    <!-- bean元素的生命周期属性：
		init-method:可配置一个方法作为生命周期初始化方法，spring会在对象创建后立即调用；
		destory-method:可配置一个方法作为生命周期的销毁方法，spring会在关闭并销毁所有容器中的对象之前调用
	-->
    <!-- init/destory为User类的方法 -->
    <bean name="user" class="com.project.pojo.User" 
          init-method="init" destory-method="destory"></bean>
</beans>
```

###### spring 的分模块化配置

```xml
<!-- 导入其他的spring配置文件 -->
<import resource="com/project/applicationContext.xml" />
```

###### spring 的属性注入方式

set 方法注入

```xml
<bean name="user" class="com.project.pojo.User">
	<!-- 值类型注入：为user对象中的name属性注入zhangsan作为值 -->
    <property name="name" value="zhangsan"></property>
    <!-- 引用类型注入：为car属性注入cat对象，需要将car对象配置到容器中 -->
    <property name="car" ref="car"></property>
</bean>
```

构造函数注入

```xml
<bean name="user" class="com.project.pojo.User">
    <!-- 使用index和type属性解决构造函数重载的问题
 			name:构造函数参数名
			value:构造函数参数值
			index:构造函数参数位置
			type:构造函数参数类型
	-->
	<constructor-arg name="name" value="jerry" 
                     index="0" type="java.lang.String"></constructor-arg>
    <constructor-arg name="car" ref="car" index="1"></constructor-arg>
</bean>
```

p 名称空间注入

```xml
<!-- 需要先引入p命名空间 -->
<beans ...xmlns:p="http://www.springframework.org/schema/p"...>
    <!-- 使用p:属性完成注入，实际上也是使用的set方式注入
 			值类型：p:属性名="值"
			对象类型：p:属性名-ref="bean名称"
	-->
    <bean name="user" class="com.project.pojo.User"
          p:name="tom" p:age="18" p:car-ref="car"></bean>
</beans>

```

spel 注入

spring Expression Language，spring 表达式语言

```xml
<bean name="user" class="com.project.pojo.User">
    <!-- 使用user1的name值作为user的name值，使用user2的age值作为user的age值 -->
    <property name="name" value="#{user1.name}"></property>
    <property name="age" value="#{user2.age}"></property>
    <!-- 引用类型不能使用spel表达式注入 -->
    <property name="car" ref="car"></property>
</bean>
```

###### 复杂类型注入

数组类型注入

```xml
<bean name="user" class="com.project.pojo.User">
    <!-- 如果数组中只准备注入一个值（对象），直接使用 value/ref 即可 -->
    <property name="arr" value="tom"></property>
    <!-- 数组中注入多个值 -->
    <property name="arr">
        <array>
            <value>tom</value>
            <value>jerry</value>
            <ref bean="car"></ref>
        </array>
    </property>
</bean>
```

list/set 类型注入

```xml
<bean name="user" class="com.project.pojo.User">
    <!-- 如果list中只准备注入一个值（对象），直接使用 value/ref 即可 -->
    <property name="list" value="jack"></property>
    <!-- list中注入多个值 -->
    <property name="list">
        <list>
            <value>tom</value>
            <value>jerry</value>
            <ref bean="car"></ref>
        </list>
    </property>
</bean>
```

map 类型注入

```xml
<bean name="user" class="com.project.pojo.User">
    <property name="map">
        <map>
           <!-- 键为字符串，值为字符串 -->
           <entry key="url" value="jdbc:mysql:///shop"></entry>
           <!-- 键为字符串，值为对象 -->
           <entry key="user" value-ref="user1">
           <!-- 键为对象，值为对象 -->
           <entry key-ref="user2" value-ref="user3">
        </map>
    </property>
</bean>
```

properties 类型注入

```xml
<bean name="user" class="com.project.pojo.User">
    <property name="prop">
        <props>
            <prop key="driver">com.mysql.jdbc.Driver</prop>
            <prop key="username">root</prop>
            <prop key="password">123456</prop>
        </props>
    </property>
</bean>
```

