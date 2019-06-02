# SPRINGMVC

### 入门案例

###### 配置前端控制器

```xml
<web-app>
    <!-- springmvc的核心就是前端控制器，它本身给是一个servlet，所以需要在web.xml中进行配置 -->
	<servlet>
    	<servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 不配置默认找/WEB-INF/[servlet名称]-servlet.xml -->
        <init-param>
        	<param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
    	<servlet-name>springmvc</servlet-name>
        <!--
            /*	过滤所有文件
            *.do	过滤以 .do 结尾的所有请求
            /	过滤除 jsp 以外的所有文件（本人有疑问？）
        -->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
</web-app>
```

###### 配置 springmvc.xml

```xml
<!-- 就是spring的配置文件 -->
<!-- 需要导入约束，这里没有导入 -->
<beans>
    <!-- 开启使用组件扫描器，省去在spring容器中配置每个Controller类 -->
    <!-- 配置controller扫描包，多个包用逗号分隔 -->
	<context:component-scan base-package="com.project.controller"></context:component-scan>
</beans>
```

###### 书写 controller 类

```java
//表示这是一个 Controller 类
@Controller
public class BookController {
    @RequestMapping(value="/book/showbooks.action")
    public ModelAndView showBooks() {
        List<Books> books = new ArrayList<Books>();
        ModelAndView mad = new ModelAndView();
        //类似于request.setAttribute("bookList",books)
        mad.addObject("bookList",books);
        //类似于请求转发
        mad.setViewName("/WEB-INF/book.jsp");
        return mad;
    }
}
```

### springmvc 架构

![springmvc 架构图](../images/springmvc.png)

###### 架构流程

请求---->DispatcherServlet---->HandlerMapping---->根据 url 返回包名+类名+方法名---->Dispatcherservlet

---->HandlerAdapter---->调用 Handler(后端控制器)---->返回 ModelAndView---->HandlerAdapter---->

DispatcherServlet---->ViewResolver---->DispatcherServlet---->Jsp, freemaker

###### 架构组件

前端控制器：DispatcherServlet，springmvc 框架流程控制的中心

三大组件：

> HandlerMapping: 它负责根据用户请求 url 找到 Handler 即处理器，springmvc 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等
>
> HandlerAdapter: 通过它对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行
>
> ViewResolver: 负责将处理结果生成 View 视图，它首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户

用户开发组件：

> Handler: Handler 是继 DispatcherServlet 前端控制器的后端控制器，在 DispatcherServlet 的控制下 Handler 对具体的用户请求进行处理，由于 Handler 涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发 Handler
>
> View: springmvc 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView 等，我们最常用的视图就是jsp，一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面

###### 默认加载组件

如果我们没有进行配置，框架将会加载默认的配件，它们定义在 DispatcherServlet.properties 中

由于默认注解处理器映射器和处理器适配器已经废弃，我们可以自己进行配置使用最新的组件

```xml
<!-- 配置处理器映射器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
<!-- 配置处理器适配器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
```

直接配置处理器映射器和处理器适配器比较麻烦，可以采用注解驱动来加载

```xml
<mvc:annotation-driven />
```

可以配置视图解析器，简化返回地址的书写

```xml
<!-- 此视图解析器支持JSP视图解析，是框架的默认配置，我们为了属性所以自己再进行配置 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 配置逻辑视图的前缀 -->
    <property name="prefix" value="/WEB-INF/JSP/"></property>
    <!-- 配置逻辑视图的后缀 -->
    <property name="suffix" value=".jsp"></property>
</bean>
```

### springmvc-spring-mybatis 整合

### 参数绑定

###### 默认支持的参数类型

HttpServletRequest

HttpServletResponse

HttpSession

Model: 除了 ModelAndView 以外，还可以使用 Model 来向页面传递数据，Model 是一个接口，在参数里直接声明 model 即可，如果使用 Model 则可以不使用 ModelAndView 对象，Model 对象可以向页面传递数据，View 对象则可以使用 String 返回值替代，不管是 Model 还是 ModelAndView，其本质都是使用 Request对象向 jsp 传递数据。

```java
public String queryBookById(HttpServletRequest request, Model model) {
    String id = request.getParameter("id");
    Book book = this.bookService.queryBookById(Integer.parseInt(id));
    //使用 model 传递数据
    model.addAttribute("book", book);
    //返回jsp页面，前后缀已配置在视图解析器
    return "book";
}
```

ModelMap: ModelMap 是 Model 接口的实现类，也可以通过 ModelMap 向页面传递数据，使用 Model 和 ModelMap 的效果一样，如果直接使用 Model，springmvc 会实例化 ModelMap

###### 绑定简单类型

当请求的参数名称和处理器形参名称一致时会将请求参数与形参进行绑定，支持的数据类型有

> 整形：Integer、int
>
> 字符串：String
>
> 单精度：Float、float
>
> 双精度：Double、double
>
> 布尔型：Boolean、boolean	对于布尔类型的参数，请求的参数值为 true 或 false，或者1或0

也可使用 @RequestParam 用于处理简单类型的绑定

```java
public String queryById(@RequestParam(value="id",required=true,dafaultValue="1") Integer id, ModelMap model){...}
```

###### 绑定 POJO 类型

提交的参数很多，或者提交的表单中的内容很多的时候，可以使用简单类型接受数据,也可以使用 pojo 接收数据，若使用 POJO 则要求 pojo 对象中的属性名和表单中 input 的 name 属性一致

```java
public String queryById(Book book, ModelMap model){...}
```

POST 提交参数乱码问题解决

```xml
<!-- 可以解决 post 请求的乱码问题
    get 请求乱码解决：
        1.修改tomcat配置文件，<Connector URLEncoding="UTF-8"...
        2.对参数重新进行编码，new String(str.getBytes("ISO8859-1"),"UTF-8")
-->
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!-- 设置编码参数为 UTF8 -->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <!--不能配置为 / -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

###### 绑定包装 POJO

将 input 表单参数的 name 属性设置为 `object.attribute`

```java
public class QueryVo {
    private Book book;
    //setter/getter...
}
```

```html
<!--  使用包装类持有对象 . 属性名为 input 标签 name 属性值-->
<input name="book.name">
```

###### 自定义参数绑定

由于日期数据有很多种格式，springmvc 没办法把字符串转换成日期类型，所以需要自定义参数绑定，前端控制器接收到请求后，找到注解形式的处理器适配器，对 RequestMapping 标记的方法进行适配，并对方法中的形参进行参数绑定，可以在 springmvc 处理器适配器上自定义转换器 Converter 进行参数绑定，一般使用`<mvc:annotation-driven/>` 注解驱动加载处理器适配器，可以在此标签上进行配置

自定义 Converter

```java
public class DateConverter implements Converter<String, Date> {
    @Override
    public Date convert(String source) {
        try {
            DateFormat format = new SimpleDateFormat("yy:MM:dd HH:mm:ss");
            Date date = format.parse(source);
            return date;
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

配置 Converter

可以使用纯 xml 配置 Converter， 需要独立配置处理器映射器、适配器，要使用到`ConfigurableWebBindingInitializer`，它需要注入转换工厂，再将它注入适配器，以下使用注解驱动方式配置

```xml
<!-- 配置注解驱动 -->
<mvc:annotation-driven conversion-service="conversionService" />
<!-- 配置转换器 -->
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <!-- 可以使用array,list,set... -->
        <list>
            <!-- 可以配置多个转换器，如时间转换器、去前后空字符转换器... -->
            <bean class="com.project.converter.DateConverter" />
        </list>
    </property>
</bean>
```

### springmvc 与 strust2 区别

springmvc 的入口是一个 servlet 即前端控制器，而 struts2 入口是一个 filter 过滤器

springmvc 是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例(多线程，为每个请求创建方法副本)或多例(创建类的副本，建议单例)，struts2 是基于类开发，传递参数是通过类的属性，只能设计为多例

Struts 采用值栈存储请求和响应的数据，通过 OGNL 存取数据， springmvc 通过参数解析器是将 request 请求内容解析，并给方法形参赋值，将数据和视图封装成 ModelAndView 对象，最后又将 ModelAndView 中的模型数据通过 request 域传输到页面，Jsp 视图解析器默认使用 jstl