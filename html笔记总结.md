# web前端

### html笔记总结

##### html标签

```html
<h1></h1>	<p></p>	<br/>	<b></b>	<strong></strong>	<i></i>	<em></em>	<hr/>

<font color="" size="" face=""></font>

<img src="" width="400px(30%)" height="" alt="" />	<!-- 相对路径与绝对路径 ../../表示上上层目录 -->

<ul type="suqare"><li><a href="" target="_blank"></a></li></ul>
<ol type="a" start=""></ol>

<table border="" width="" height="" bgcolor="" align="">
    <tr>
        <td colspan="" rowspan=""></td>
    </tr>
</table>

<form action="" method=""></form>
<input type="" name="" id="" />
<!--  type:
text password radio checkbox file submit button reset hidden
h5新增: date number tel email...
-->
<textarea cosl="" rows=""></textarea>
<seclect>
    <option></option>
</seclect>

<frameset cols="" rows="">
    <frame src="" name=""/>
</frameset>

<div></div>	<span></span>
```

[带语义标签](https://www.jianshu.com/p/8f1ca8b776d3)

### CSS笔记总结

##### 引入方式

```html
<link rel="stylesheet" href="style.css" /> <!-- 外联样式 -->

<style></style> <!-- 内联样式 -->

<p style="color:red;font-size:7"></p> <!-- 行内样式 -->
```

##### 选择器

- 元素选择器

  ```html
  <style>
      p {
          color: red;
      }
  </style>
  <p>haha</p>
  ```

- ID选择器

  ```html
  <style>
      #p1 {
          color: red;
      }
  </style>
  <p id="p1">haha</p>
  ```

- 类选择器

  ```html
  <style>
      .p2 {
          color: red;
      }
  </style>
  <p class="p2">haha</p>
  ```

- 选择器优先级

  > 1.行内样式>ID选择器>类选择器>元素选择器
  >
  > 2.就近原则

- 其他选择器

  - 分组选择器	

    > h1,h2

  - 属性选择器	

    > a[title]	
    >
    > a[title='aaa']
    >
    > a[href]\[title='aaa']

  - 后代选择器

    > h1 em

  - 子元素选择器

    > h1 > em

  - 伪类

    > ```html
    > a:link {color: #FF0000}		/* 未访问的链接 */
    > a:visited {color: #00FF00}	/* 已访问的链接 */
    > a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
    > a:active {color: #0000FF}	/* 选定的链接 */
    > ```

  

##### 浮动与清除

> float: right; left; none; inherit;
>
> clear:  left; right; both; none; inherit;

##### 常用属性

>text-decoration
>
>text-align
>
>line-height
>
>list-style
>
>display

##### 盒子模型

> 外边距 	margin
>
> 内边距 	padding
>
> 绝对定位 	position: absolute	top	left

### JS笔记总结

##### JS引入方式

```html
<script src="xxx.js"></script>

<script>javascript code</script> <!-- <head>与<body>均可放入 -->
```

##### JS输出

```html
<script>
	//document.getElementById("").innerHTML = "";
    //document.write("");
    //通过反斜杠可以将代码换行
</script>
```



##### JS数据类型

- 基本类型

  > String, Number, Boolean, Null, Undefine

- 引用类型

  > 对象
  >
  > 内置对象

- 变量定义

  > var i;	//弱语言类型
  >
  > 使用typeof可以查看类型

##### JS函数

- 函数语法

  > function function_name(argument1, arguement2){	}
  >
  > var function_name = function(argument1, arguement2){	}

- 变量

  > 局部变量
  >
  > 全局变量
  >
  > 变量生命周期
  >
  > 未定义的变量直接赋值将被视为全局变量

##### JS组成

> ECMAScript	DOM	BOM

##### JS常用对象

- Browser对象

  - Window

    > setInterval()	setTimeout()	clearInterval()	clearTimeout()

- HTML对象

  - DOM Event

    > onload	onclick	onfocus	onblur	onkeyup	onchange	ondbclick

  - Style

    > display(布局)

##### DOM树

- 结构

  >  element， attribute， text

- 常用方法

  > getElementById()	getElementByTagName()	getElementByClassName()
  >
  > createElement()	createTextNode()	createAttribute()	appendChild()

### JQuery笔记总结

##### JQuery文档就绪函数

> $(document).ready(function(){	})；
>
> $(function(){	});

##### JQuery效果

> show(),hide(),toggle()

##### JQuery选择器

> 基本	层级	过滤

### Mysql入门

##### MySQL数据类型

> int, double, char/varchar, date/time/datetime/time..., blog/

##### SQL分类

> DDL: create	drop	alter
>
> DML: insert	update	delete
>
> DCL: grant	
>
> DQL: select

##### 数据库操作

> 创建：普通创建，带字符集创建，带比较规则创建
>
> 更改：更改字符集 
>
> 删除：
>
> 查看：查看所有数据库，查看某个数据库定义，查看选中的数据库
>
> 使用：

##### 表操作

> 创建：关键字（primary key, unique, not null, auto_increment）
>
> 更改：更换表名，更改表字符集，更改列名，更改列信息，添加列，删除列
>
> 删除：
>
> 查看：查看所有表，查看表定义，查看表信息

##### 数据操作

- 插入数据

  > insert into table_name(col1,col2...) values(col1,col2...);

- 修改数据

  > update table_name set col1=value,col2=value where condition;

- 删除数据

  > delete from table_name where condition;
  >
  > truncate table table_table

- 查询数据

  > select [distinct] {* | col1,col2} from table_name where codition group by having order by;

- 查询常用条件

  > 关系运算符: <> !=
  >
  > 逻辑运算符: and or not
  >
  > 范围: in   not in   any   all	between..and...	is null	is not null
  >
  > 模糊查询: like	"_" "%"
  >
  > 别名查询: as	表别名，列别名
  >
  > 聚合函数: sum(), avg(), count(), max(), min()
  >
  > ​	1.聚合函数不能嵌套	2.聚合函数不能用于条件查询
  >
  > 排序: order by 	desc asc
  >
  > 分组: group by
  >
  > 分组过滤: having	后可跟聚合函数

- 多表查询

  > 交叉连接查询 笛卡尔积
  >
  > ​	select * from table1_name,table2_name;
  >
  > 内连接查询(显隐区别)
  >
  > ​	隐式内连接: select * from product p,category c where p.pid=c.cid;
  >
  > ​	显示内连接: select * from product p inner join category c on p.pid=c.cid;
  >
  > 左外连接查询（将左表中的数据全查询）
  >
  > ​	select * from prduct p left outer join category c on p.pid=c.cid;
  >
  > 右外连接查询（将右表中的所有数据均查询）
  >
  > ​	select * from product p right outer join category c on p.pid=c.cid;
  >
  > 分页查询
  >
  > ​	select * from table_name limit limit_index,limit_number;
  >
  > 子查询	将一个查询的结果做为另一个查询的条件

##### 表与表的关系

- 外键约束: foreign key

  > alter table table_name1 add foreign key(col_name) references table_name2(col_name);

- 一对一（一般用于拆表操作，如将个人常用信息与不常用信息分离开）

  > 1.将一对一的情况,当作是一对多情况处理,在任意一张表添加一个外键,并且这个外键要唯一,指向另外一张表
  >
  > 2.直接将两张表合并成一张表
  >
  > 3.将两张表的主键建立起连接,让两张表里面主键相等

- 一对多

  > 在多的一方添加外键约束指向一的一方的主键

- 多对多

  > 建立中间表，给中间表添加两个外键，将多对多拆分为两个一对多。

### JDBC入门(Java Database Connectivity)

##### 使用基本步骤

> 注册驱动	DriverManager.registerDriver(new com.mysql.jdbc.Driver);
>
> ​			Class.forName("com.mysql.jdbc.Driver");
>
> 建立连接	Connection conn = DriverManager.getConnecttion("jdbc:mysql://localhost/database","user","passroot");
>
> 创建Statement	Statement st = conn.createStatement();
>
> 执行Sql	ResultSet rs = st.excuteQuery(sql);
>
> ​		      int result = st.excuteUpdate(sql);
>
> 遍历结果集
>
> 释放资源	rs.close()   st.close()   conn.close()

##### JDBCUtil

> 封装获得连接以及释放资源细节，提供接口----获得连接，释放资源
>
> 工具类通过读取配置文件建立连接

##### JUnit单元测试

> 添加JUnit支持，对方法添加注解@Test

##### DAO模式(Data Access Object)

> 将数据库有关操作均放入DAO包中
>
> 创建DAO接口将实现分离

##### Statement安全问题

> Statement先生成对象，再执行SQL语句，会将传入的参数中的关键字定义为关键字
>
> 使用PrepareStatement可以解决此问题，它先根据SQL生成对象，在将传入的参数代替？，不会将参数的关键字作为SQL关键字
>
> ​	PrepareStatement ps = conn.preopareStatement(sql);
>
> ​	ps.setString(1,arg)...;
>
> ​	ps.excuteQurey();

### 连接池技术

##### 自定义连接池

> javax.sql.DataResource
>
> 方法增强方式：1.继承	2.装饰器设计模式	3.动态代理

##### C3P0连接池

> c3p0-config.xml

##### DBCP连接池

> *.properties
>
> properties读取方式

##### DBUtils

> JavaBean
>
> QueryRunner(query(),update())	BeanListHandler(), BeanHandler(), ScalarHandler

### XML

##### XML语法

> <?xml version="1.0 ?">
>
> CDATA区

##### DTD约束

> xml文档约束

##### Schema约束

> xml文档约束，本省也为xml文档，扩展名为。xsd
>
> 命名空间

##### XML解析

> 解析方式：DOM, SAX, PULL
>
> 解析器
>
> 解析开发包：JAXP, JDom, jsoup, dom4j

- dom4j

  > 将整个文档加载到内存，生成DOM树，获得Document对象，通过此对象对DOM进行操作

  ```java
  SAXReader saxReader = new SAXReader();
  Document document = saxReader.read(new File("com/xiaowen/xml/a.xml"));
  Element rootElement = document.getRootElement();
  String version = rootElement.attributeValue("version");
  List<Element> list = rootElement.elements();
  //getName()	getText()
  ```

##### 反射

- Class对象

  ```java
  Class c = Class.forName("java.lang.String");
  //Class c = String.class;
  //Class c = "string".getClass();
  String s = c.newInstance();
  ```

- Constructor对象

  ```java
  Constructor constructor = getConstrucrot(c, String.class); 
  ```

- Method对象

  ```java
  Method m = getMethod(c, );
  ```


### HTTP & Tomcat

##### HTTP

- http请求

  > 请求由请求行、请求头、请求体构成
  >
  > get 方式提交没有请求体  

  ![](C:\Users\admin\Desktop\笔记\image\http请求.png)

- http响应

  > 响应由响应行、响应头、响应体构成

![](C:\Users\admin\Desktop\笔记\image\http响应.png)

##### tomcat目录结构



![](C:\Users\admin\Desktop\笔记\image\tomcat.jpg)



##### tomcat 启动异常

> 1. JAVA_HOME 未正确配置
>
> 2. 端口被占用
>
>    处理方式：a. 通过 netstat -aov 命令查看活动链接获得占用端口的 PID ，杀死此 PID 进程即可
>
>    ​		   b. 修改 tomcat 配置端口 

##### Web 应用目录结构

![](C:\Users\admin\Desktop\笔记\webapp structure.jpg)

##### Eclipse 下配置 tomcat

- 项目发布常见问题
  1. 在 Eclipse 中更改了项目名称后无法访问到项目
  2. 在 tomcat webapps 目录结构中是删除项目后无法再部署此项目

##### Intellij 下配置 tomcat

> 1. tomcat 构建web项目
> 2. 配置 tomcat 服务器的方式
> 3. 加入 jar 包的三种方式

### Servlet 

##### servlet 配置

```xml
<servlet>
    <servlet-name></servlet-name>
    <servlet-class></servlet-class>
    <init-param>
        <param-name></param-name>
        <para-value></para-value>
    </init-param>
    <load-on-startup>3</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name></servlet-name>
    <url-pattern></url-pattern>
</servlet-mapping>

<welcome-file-list>
	<welcome-file>index.html</welcome-file>
</welcome-file-list>
```

- url-pattern 配置方式

  > 完全匹配	目录匹配	扩展名匹配

- 缺省 Servlet

  ```xml
  <url-pattern>/</url-pattern>
  ```

  > 将在 tomcat 的全局配置 web.xml 中进行查找

##### servlet API

- API

  > init(ServletConfig config)
  >
  > ​	ServletConfig 对象：getServletName()  getServletContext()
  >
  > service(ServletRequest req, ServletResponse rep)
  >
  > destory()

- servlet 生命周期

  > 创建：第一次被访问
  >
  > 销毁：服务器关闭

##### servlet 访问过程

> tomacat 服务器获得请求url后，根据请求路径找到对应 Servlet 并创建，在创建 request, responce 对象执行 service 方法

#####  HttpServlet

> doGet()
>
> doPost()

##### ServletContext 对象

> ServletContext 代表一个 web 应用环境，一个 web 应用只有一个此对象

- ServletContext 生命周期

  > 创建：web 应用被加载
  >
  > 销毁：web 应用被卸载

- 获得 ServletContext 对象

  > config.getServletContext()
  >
  > this.getServletConfig()	(底层也是通过 config 获得)

- ServletContext 作用

  - 获得全局初始化参数

    ```java
    context.getInitParamter("");	//spring中有用到
    ```

    

    ```xml
    <context-name>
    	<param-name>Driver</param-name>
        <param-value>com.mysql.Driver</param-value>
    </context-name>
    ```

  - 获得 web 应用中任何资源的绝对路径

    ```java
    context.getRealPath("相对路径");
    //eclipse中最终是将WebContent中的所有类容拷贝到web应用中，其他资源无法进行访问
    //src下的资源存放在WEB-INF下的classes文件夹中
    //读取src(classes)下的文件时还可以用同类加载器
    ContextServlet.class.getClassLoader().getResource("相对路径(相对于src)").getPath();
    ```

  - ServletContext 是一个域对象

    > 域对象： 存储数据的区域
    >
    > ServletContext 域对象作用范围： 整个 web 应用（一个 servlet 中存储的数据，其他 servlet 均可取得）	

    ```java
    context.setAttribute(key, value);	//可通过此方式记录一个网站的总访问次数
    context.getAttribute(key);
    ```

    > 域对象的通用方法
    >
    > ​	setAttribute(String key, Object value);
    >
    > ​	getAttributr(String key);
    >
    > ​	removeAttribute(String key);

### Response

##### response 运行过程

> tomcat 引擎获得浏览器发出的请求信息后，根据请求封装一个 request 对象与一个空 response 对象，传给 servlet 的 doGet() 进行执行，服务器将根据请求获得的信息传入 response 缓冲区形成响应体，tomcat 引擎再从缓存区中取出信息加上引擎信息（响应行、响应头）封装成一个完整的 response 对象回传给浏览器进行显示

##### response API

- 设置响应行

  ```java
  setStatus(int sc);	//设置状态码
  ```

- 设置响应头

  > addHeader(String key, String value)	//常用	add 同名头信息多次则值会被多次添加
  >
  > addIntHeader(String key, int value)
  >
  > addDateHeader(String key, long date)	//date 为毫秒值
  >
  > setHeader(String key, String value)	//常用
  >
  > setIntHeader(String key, int value)
  >
  > setDateHeader(String key, long date)

- 重定向

  > 浏览器向 a 请求资源，a 无资源，告诉浏览器找 b
  >
  > 状态码：302	
  >
  > 响应头：Location

  ```java
  response.setStatus(302);
  response.setHeader("Location","b.html")；
  //封装的重定向方法
  response.sendRedirect("b.html");
  //通过定时刷新重定向(页面定时器 setInterval())
  response.setHeader("refresh","5;b.html");	//5 代表 5s
  ```

  

  > 会访问服务器两次
  >
  > 地址栏会发生变化

- 设置响应体

  ```java
  PrintWriter writer = response.getWriter();
  writer.write("-----");	//response 码表为 ISO-8859-1，写入中文会发生乱码
  
  //设置 response 查询码表为 utf-8,此语句也可不设置，当设置头信息后 response 缓冲区会自动以头信息设置的方式进行编码
  response.setCharacterEncoding("utf-8");
  //告知浏览器以 utf-8 解码
  response.setHeader("Content-Type","text/html;charset=utf-8");
  response.setContentType("text/html;charset=utf-8");
  
  //响应头设置字节
  ServletOutputStream out = response.getOutputStream();
  out.write(byte[] b);
  ```

- 实例：文件下载

  1. 什么情况下会文件下载？

     > 浏览器不能解析的文件会直接下载

  2. 什么情况下需要编写文件下载代码？

     > 实际开发中，要下载的文件均编写下载代码

     ```html
     <!-- 前端下载代码 -->
     <a href="download/downloadServlet?filename=a.mp3"></a>
     ```

     ```java
     //1.获得要下载的文件名
     String file = request.getParameter("filename");
     //2.告知要下载的文件的类型，浏览器通过文件的MIME类型区分文件类型
     response.setContentType(this.getServletContext.getMimeType(file));
     //3.告知客户端文件不直接解析，而是以附件形式进行下载
     response.setHeader("Content-Disposition","attachment;filename="+file);
     //4.获取文件绝对路径，用流进行读取写出
     ```

  3. 下载文件的文件名包含中文字符的异常？

     ```java
     //对获得的中文参数改变编解码方式
     file = new String(file.getBytes("ISP-8859-1","UTF-8");
     //获得请求头User-Agent信息，根据不同的浏览器设置文件不同的编码方式
     String agent = request.getHeader("User-Agent");
     if(agent.contains("Firefox")){...}                                            
     ```

##### response 细节点

> 1. response 获得的流不用手动关闭，tomcat 容器会帮助我们关闭
>
> 2. getWriter() 与 getOutputStream() 不能同时调用
>
>    会出现 500 状态码，错误：getWriter() has already been called for this response
>
> 3. response 默认缓冲区大小为 8k，可进行扩容，可通过 getBufferSize() 获得缓冲去大小
>
> 4. 重定向与转发代码后不再写代码

### Request

##### 获得请求行

- 获得请求方法

  ```java 
  String getMethod()
  ```

- 获得请求资源

  > String getRequestURI()	//所有地址都可叫做URI，获得的值与抓包得到的地址一致
  >
  > StringBuffer getRequestURL()	//一般把网络资源叫做URL
  >
  > String getContextPath()	//重要：获得web应用的名称
  >
  > String getQueryString()	//获得 get 提交 URL 地址后的参数字符串，要通过拆分字符串去获得具体值

- 获得客户端信息

  > getRomoteAddr()	//获得访问的客户端的 IP 地址

##### 获得请求头

> String getHeader(String key)	//常用
>
> int getIntHeader(String key)
>
> long getDateHeader(String key)
>
> Enumeration getHeaderNames()
>
> Enumeration getHeaders(String name)

- referer 头信息

  > 返回该次访问的来源
  >
  > 可用于做防盗链

##### 获得请求体

> 获得 post 请求的参数，get 提交方式也可使用

> String getParameter(String name)	//常用
>
> Map<String, String[]> getParameterMap()	//常用
>
> String[] getParameterValues(String name)
>
> Enumeration getParameterNames()

##### request 域对象

- request 作用范围

  >  request 是可用来储存数据的区域对象
  >
  > 作用范围：一次请求中

- request 请求转发

  > 浏览器向 a 请求资源，a 会自动去寻找 b 获得资源

  ```java
  //获得请求转发器
  RequestDispatcher dispatcher = request.getRequestDispatcher(String path);
  //执行转发
  dispatcher.forward(request, response);
  ```

- ServletContext 与 Request 域对象比较
  > 生命周期：
  >
  > ​	ServletContext: 服务器启动创建，服务器关闭销毁，存在于整个 web 应用中
  >
  > ​	Request: 每次访问时创建，响应结束时销毁，存在于一次请求中

- 转发与重定向区别

  > 1. 重定向为两次请求，转发为一次请求
  > 2. 重定向地址变化，转发地址不变
  > 3. 重定向可以访问外部网站，转发只能访问内部资源
  > 4. 转发的性能由于重定向

- 客户端地址与服务器地址的写法

  > 客户端地址：为访问的服务器的外部地址，需写上 web 应用的名称	直接访问，重定向
  >
  > 服务器地址：服务器内部资源的地址，不需写 web 应用的名称	转发

##### 注册功能实现

- 中文乱码问题

  ```java
  //设置 request 的编码 ----> 只适合 post 提交方式
  request.setCharacterEncoding("UTF-8");
  
  //乱码过程：中文--->utf-8编码--->iso8859-1解码--->乱码
  //get 方式乱码解决（适用于某一个参数）,也适用于 post 提交方式
  username = new String(username.getByte("iso8859-1"),"utf-8");
  ```

  

- BeanUtils

  > 将 Map 中的数据根据 key  与实体属性的对应关系封装，只要 key 的名字与实体的属性的名字一样就自动封装到实体中

  > 需要导入 beanutils 与 logging jar 包

  ```java
  Map<String, String[]> poperties = request.getParameterMap();
  User user = new User();
  BeanUtils.populate(user, poperties);
  ```

- UUID

  > 生成随机不重复的32位字符串，Java 代码生成后为36位

  ```java
  String uuid = UUID.randomUUID().toString();		
  ```

  

