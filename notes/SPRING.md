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

   Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts 、Hibernate 、MyBatis 、Quartz 等）的直接支持

6. 降低 JavaEE API 的使用难度

   Spring 对 JavaEE 开发中非常难用的一些 API (JDBC 、 JavaMail 、远程调用等)，都提供了封装，使这些 API 应用难度大大降低

### 入门程序

1. 导包

   4+2：

2. 准备对象

3. 书写配置

   建议放在 src 目录下，建议命名为 applicationContext.xml

4. 书写测试代码

### spring 思想

###### IOC 

Inverse of Control，控制反转；创建对象的方式反转了，从我们自己创建对象到将对象的创建交给容器完成

###### DI 

Dependency Injection，依赖注入；将必须的属性注入到对象当中，是实现 IoC 思想必须条件

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
<beans >
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
    
    <!-- bean元素的作用范围scope属性：
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

### WEB 环境中使用 spring 容器

1. 导包

   4(核心包)+2(日志包)+1(spring-web)

2. 在 web.xml 中配置 listener

   一个 web 应用中只创建一个 spring 容器，如何保证只创建一个容器？

   在 web 项目中，ServletContext 在容器中只存在一份，随应用加载创建，应用卸载销毁，通过将 spring 容器的创建绑定给 ServletContextListener 即可实现 spring 容器只创建一个

   ```xml
   <listen>
       <!-- 让spring容器随项目的启动而创建，随项目的关闭而销毁 -->
   	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listen>
   
   <!-- 指定加载 spring 配置文件的位置 -->
   <context-param>
   	<param-name>contextConfigLocation</param-name>
       <param-value>classpath:applicationContext.xml</param-value>
   </context-param>
   ```

3. 从 application 域获得 spring 容器

   ServletContextListener 将 spring 容器创建后放在 application 域对象中

   ```xml
   <!-- 获得 ServletContext 对象，以下为 strust 获得 ServletContext 的方法 -->
   ServletContext sc = ServletActionContext.getServletContext();
   <!-- 将 sc 传递给 spring 提供的工具类方法从域中获得 spring 容器 -->
   WebApplicationContext cs = WebApplicationContextUtils.getWebApplicationContext(sc);
   ```

   

### spring 中的注解

要使用注解，需要先为主配置文件引入新的命名空间，即 context 的 schema 约束，并开启使用注解代理配置文件(还需导入 spring-aop.jar)

```xml
<!-- 指定扫描com.project.bean下的所有类的注解；扫描时，会扫描指定包下所有的子孙包 -->
<context:component-scan base-package="com.project.bean"></context:component-scan>
```

使用注解

```java
@Component("user")	//<bean name="user" class="com.project.bean.User
//以下三个注解功能同 Component 注解，但能体现对象层次结构
@Service("user")	// service 层
@Controller("user")	// web 层
@Repository("user")	// dao 层

//指定对象作用范围
@Scope(scopeName="singleton|prototype|request|session")
public class User {
    //值类型注入属性，还可将注解加在setXXX方法上
    //加在属性上通过反射的Field赋值，破坏了对象的封装性；加在setXXX方法上通过set方法赋值，推荐使用
    @Value("tom")
    private String name;
    private Integer age;
    //引用类型属性注入
    //自动装配，存在问题：如果匹配多个类型一致的对象，将无法选择具体注入哪一个对象
    @Autowired 
    //使用@Qualifier注解告诉spring容器自动装配那个名称的对象，和Autowired配合使用
    @Qualifier("car1")
    //手动注入，指定注入哪个名称的对象，推荐使用
    @Resource(name="car2")
    private Car car;
    
    //指定初始化方法，在对象被创建后调用 init-method
    @PreConstruct()
    public void init() {
        
    }
    //销毁方法，在对象销毁之前调用 destory-method；作用域指定为prototype才有效，多例对象容器创建后不再控制
    @PreDestory()
    public void destory() {
        
    }
}
```

### STS 插件

spring 插件

### spring 与 junit 整合测试

1. 导包

   4+2+aop+test

2. 添加注解

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)	//帮助我们创建容器
   //告诉spring配置文件的位置，因为spring允许配置文件位置任意，所以每次需要指定创建容器使用的配置文件
   @ContextConfiguration("classpath:applicationContext.xml")
   public class Demo {
       @Resource(name="tom")
       private User u;
       //测试方法不再需要自己获得容器
       public void fun1() {
           System.out.println(u);
       }
   }
   ```

   

### spring AOP

AOP 思想：横向重复，纵向抽取；Filter、动态代理、struts 中的拦截器其实都使用了 AOP 思想

spring 能够为容器中管理的对象生成动态代理对象，以前要使用动态代理，我们需要使用以下方法

```java
Proxy.newProxyInstance(ClassLoader loader,Interface[] arr,InvocationHandler handler)
```

###### spring 实现 aop 的原理

动态代理（优先使用）：被代理对象必须实现接口，才能产生代理对象，如果没有接口将不能使用动态代理技术

CGLIB 代理（没有接口时使用）：第三发代理技术，可以对任何类产生代理，代理的原理是对目标对象进行继承代理，如果目标对象被 final 修饰，那么该类无法被 CGLIB 代理

###### AOP 名词

Joinpoint(连接点): 目标对象中，所有可以增强的方法

Pointcut(切入点): 目标对象中，已经增强的方法

Advice(通知/增强): 增强的代码（例如事务的开启与关闭，将通知切入连接点）

Target(目标对象): 被代理对象

Weaving(织入): 将通知应用到切入点的过程

Proxy(代理): 将通知织入到目标对象后，形成代理对象

Aspect(切面): 切入点+通知

###### spring 中使用 AOP 

1. 导包

   4(核心包)+2(日志包)+2(spring 的 AOP 包: aspects+aop)+2(第三方 AOP 包: aopalliance+weaver)

2. 准备目标对象

   ```java
   //使用动态代理目标对象必须实现接口
   public class UserServcieImpl implements UserService {
       public void save(){
           
       }
       public void delete(){
           
       }
   }
   ```

   

3. 准备通知

   ```java
   //通知类
   public class MyAdvice{
       //前置通知	目标方法运用之前调用
       public void before(){...}
       //后置通知	目标方法运行之后调用（如果出现异常则不会调用）
       public void afterReturning(){...}
       //环绕通知	在目标方法之前和之后都调用
       public Object around(ProceedingJoinPoint pjp){
           ...;
           Object proceed = pjp.proceed();	//调用目标方法
           ...;
           return proceed;
       }
       //异常拦截通知	如果出现异常则调用
       public void afterException(){...}
       //后置通知	目标方法运行之后调用（无论是否出现异常都会调用）
       public void after(){...}
   }
   ```

   

4. 配置进行织入，将通知织入目标对象中

   ```xml
   <!-- 导入 aop 约束命名空间 -->
   <!-- 配置目标对象 -->
   <bean name="userServiceTarget" class="com.project.service.UserServiceImpl"></bean>
   <!-- 配置通知对象 -->
   <bean name="myAdvice" class="com.project.aop.MyAdvice"></bean>
   <!-- 配置将通知织入目标对象 -->
   <aop:config>
   	<!-- 配置切入点
    		public void com.project.service.UserServiceImpl.save()
   	-->
       <aop:pointcut expression="execution(* com.project.service.*ServiceImpl.*(..))" id="pc"></aop:pointcut>
       <aop:aspect ref="myAdvice">
           <!-- 指定名为 before 方法作为前置通知 -->
       	<aop:before method="before" pointcut-ref="pc"></aop:before>
           <!-- 后置通知 -->
           <aop:after-returning></aop:before>
           <!-- 环绕通知 -->
           <aop:around></aop:before>
           <!-- 异常拦截通知 -->
           <aop:after-throwing></aop:before>
           <!-- 后置通知：被代理方法执行后立即执行，先于after-returning和after-throwing -->
           <aop:after></aop:before>
       </aop:aspect>
   </aop:config>
   ```

###### 使用注解配置 AOP

开启使用注解完成织入，开启后可直接在通知类中使用注解配置

```xml
<beans>
	<aop:aspect-autoproxy></aop:aspect-autoproxy>
</beans>
```

```java
@Aspect
public class MyAdvice{
    @Before("execution(* com.project.service.*ServiceImpl.*(..))")
    public void before(){...}
	@AfterReturning("execution(* com.project.service.*ServiceImpl.*(..))")
    public void afterReturning(){...}
	@Around("execution(* com.project.service.*ServiceImpl.*(..))")
    public Object around(ProceedingJoinPoint pjp){
        ...;
        Object proceed = pjp.proceed();	//调用目标方法
        ...;
        return proceed;
    }
    @AfterThrowing("execution(* com.project.service.*ServiceImpl.*(..))")
    public void afterException(){...}
    @After("execution(* com.project.service.*ServiceImpl.*(..))")
    public void after(){...}
}
```

切点抽取

```java
@Aspect
public class MyAdvice{
    @Pointcut("execution(* com.project.service.*ServiceImpl.*(..))")
    public void pc(){}
    @Before("MyAdvice.pc()")
    public void before(){...}
}
```

