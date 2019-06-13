# SPRING BOOT

### 概述

###### spring 精要

自动配置：针对很多 Spring 应用程序常见的应用功能，Spring Boot 能自动提供相关配置

起步依赖：告诉 Spring Boot 需要什么功能，它就能引入需要的库

命令行界面(CLI)：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建

Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟

###### 误会

spring boot 不是应用服务器

spring boot 没有实现诸如 JPA 或 JMS 之类的企业级规范

spring boot 没有引入任何形式的代码生成

###### 初始化 spring boot 项目

使用 Spring Initializr 初始化 spring boot 项目（web 界面、STS、intelij、CLI）



### 开发一个小程序（图书阅读列表应用程序）

###### 使用技术

Spring MVC: 处理 web 请求

Thymeleaf: 定义 web 视图

Spring Data JPA: 持久化数据

H2: 嵌入式数据库

###### 初始化的 spring boot 项目

DemoApplication.java: 启动引导类，也是主要的配置类

> 自动配置免去了很多 spring 配置，但依然需要少量配置来启动配置；@SpringBootApplication开启了 Spring 的组件扫描和 Spring Boot 的自动配置功能，它将三个注解组合在了一起：@Configuration @ComponentScan @EnableAutoConfiguration

application.properties: 用于配置应用程序和 Spring Boot 的属性

DemoApplicationTests:  基本的集成测试类

> @SpringApplicationConfiguration 标识如何加载Spring的应用程序上下文

###### 使用起步依赖

不使用 spring boot 起步依赖的开发步骤？

Spring Boot 通过提供众多起步依赖降低项目依赖的复杂度。起步依赖本质上是一个 Maven 项目对象模型（Project Object Model，POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能

覆盖起步依赖引入的传递依赖？

###### 使用自动配置

Spring Boot 的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定 Spring 配置应该用哪个，不该用哪个。如 Spring 的 JdbcTemplate 是不是在 Classpath 里？Thymeleaf 是不是在 Classpath 里？每当应用程序启动的时候，Spring Boot 的自动配置都要做将近200个这样的决定，涵盖安全、集成、持久化、Web 开发等诸多方面。所有这些自动配置就是为了尽量不让你自己写配置

###### 代码实战

书写实体类：使用 @Entity @Id @GeneratedValue

书写持久层接口：继承自 JpaRepoistory

书写 controller 层：使用 restful 风格，@RestController @PathVariable

书写页面：使用 Thymeleaf 模板引擎

项目启动：

###### 浅析 spring boot 项目执行过程

在向应用程序加入Spring Boot时，有个名为spring-boot-autoconfigure的JAR文件，其中包含了很多配置类。每个配置类都在应用程序的Classpath里，都有机会为应用程序的配置添砖加瓦。这些配置类里有用于Thymeleaf的配置，有用于Spring Data JPA的配置，有用于Spiring MVC的配置，还有很多其他东西的配置，你可以自己选择是否在Spring应用程序里使用它们。所有这些配置如此与众不同，原因在于它们利用了Spring的条件化配置，这是Spring 4.0引入的新特性。条件化配置允许配置存在于应用程序中，但在满足某些特定条件之前都忽略这个配置。

在Spring里可以很方便地编写你自己的条件，你所要做的就是实现Condition接口，覆盖它的matches()方法。

Spring Boot 定义了很多更有趣的条件，并把它们运用到了配置类上，这些配置类构成了 Spring Boot 的自动配置

###### 此小程序的自动配置

因为Classpath 里有H2 ， 所以会创建一个嵌入式的H2 数据库Bean ， 它的类型是javax.sql.DataSource，JPA实现（Hibernate）需要它来访问数据库。

因为Classpath里有Hibernate（Spring Data JPA传递引入的）的实体管理器，所以自动配置会配置与Hibernate 相关的Bean ， 包括Spring 的LocalContainerEntityManager-FactoryBean和JpaVendorAdapter

因为Classpath里有Spring Data JPA，所以它会自动配置为根据仓库的接口创建仓库实现。

因为Classpath里有Thymeleaf，所以Thymeleaf会配置为Spring MVC的视图，包括一个Thymeleaf的模板解析器、模板引擎及视图解析器。视图解析器会解析相对于Classpath根目录的/templates目录里的模板

因为Classpath 里有Spring MVC （ 归功于Web 起步依赖）， 所以会配置Spring 的DispatcherServlet并启用Spring MVC

因为这是一个Spring MVC Web应用程序，所以会注册一个资源处理器，把相对于Classpath根目录的/static目录里的静态内容提供出来。（这个资源处理器还能处理/public、/resources和/META-INF/resources的静态内容。）

因为Classpath里有Tomcat（通过Web起步依赖传递引用），所以会启动一个嵌入式的Tomcat容器，监听8080端口





### 自定义配置

###### 覆盖 spring boot 自动配置

想要覆盖Spring Boot的自动配置，你所要做的仅仅是编写一个显式的配置。Spring Boot会发现你的配置，随后降低自动配置的优先级，以你的配置为准

例如 SecurityConfig 安全配置类，我们可以自定义安全配置类，我们让 Spring Boot 跳过了安全自动配置，转而使用我们的安全配置，书写扩展了 WebSecurityConfigurerAdapter 的配置类即可

Spring Boot的设计是加载应用级配置，随后再考虑自动配置类

###### 通过属性文件外置配置

*自动配置微调*

Spring Boot自动配置的Bean提供了300多个用于微调的属性。当你调整设置时，只要在环境变量、Java系统属性、JNDI（Java Naming and Directory Interface）、命令行参数或者属性文件里进行指定就好了，它们具有优先级顺序

微调案例：

> spring.thymeleaf.cache=false	禁用 thymeleaf 缓存
>
> server.port=8000	配置服务器端口号
>
> spring.datasource.url=jdbc:mysql://localhost/readingList	配置数据库的URL和身份信息
> spring.datasource.username=root
> spring.datasource.password=123456
> spring.datasource.driver-class-name=com.mysql.jdbc.Driver	通常你都无需指定JDBC驱动，自动识别

*应用程序 Bean 的配置外置*

*使用 Profile 进行配置*

###### 定制错误页面

Spring Boot 自动配置的默认错误处理器会查找名为 error 的视图，如果找不到就用默认的白标错误视图，因此，最简单的方法就是创建一个自定义视图，让解析出的视图名为 error