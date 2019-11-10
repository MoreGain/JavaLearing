# MYCAT

目的：掌握数据库负载增大时的处理方法、理解基础概念、掌握基础配置和监控方法

Amoeba\Cobar 阿里的中间件

###### Mycat 分布式数据库系统中间层

- 实现数据库的读写分离

  读负载均衡

  支持后端 MySQL 高可用

- 垂直拆分

- 水平拆分（分库分表）

###### Mycat 应用场景

- 需要进行读写分离的场景

  读操作远大于写操作

- 需要进行分库分表的场景

- 多租户场景

- 数据统计系统

- HABSE 的一种替代方案

- 需要使用同样的方式查询多种数据库的场景

###### Mycat 的优势

- 基于阿里的 Cobar 系统开发
- 开发社区活跃
- 完全开源可以自定义开发
- 支持多种关系型及 NOSQL 数据库
- 使用 JAVA 开发
- 具有多种行业和项目中使用的成功案例



### 基础

###### 基本概念

- Mycat 中的数据库 - 逻辑库

  只保存数据定义，不保存具体数据，结合视图进行理解

- 逻辑表

###### 特性

- 支持 SQL92 标准
- 支持自动故障切换，高可用性
- 支持读写分离
- 支持 JDBC 连接数据库
- 支持全局表
- 支持独有的基于 ER 关系的分片策略
- 支持一致性 HASH 分片
- 多平台支持，部署简单方便
- 支持全局序列号

###### LINUX 下安装Mycat

- 安装 JDK

  ```base
  yum insatll java-1.7.0-openjdk
  ```

- 安装 mycat

  ```base
  wget wget ~wget http://dl.mycat.io/1.6.5/Mycat-server-1.6.5-r
  ```

- 配置环境变量

  JAVA_HOME

  MYCAT_HOME

- LINUX 命令

  ```base
  adduser mycat
  tar zxf ...
  ls -lh
  mv mycat /usr/local/
  chown mycat:mycat -R mycat
  vi /etc/profile export JAVA_HOME=/usr
  su - mycat
  ps -ef
  ```

###### 配置文件

- schema.xml

  ```xml
  <!-- 定义逻辑库表 -->
  <schema><table></table></schema>
  <!-- 定义数据节点 -->
  <dataNode></dataNode>
  <!-- 定义数据节点的物理数据源 -->
  <dataHost></dataHost>
  ```

- rule.xml

  ```xml
  <!-- 定义表使用的分片规则 -->
  <tableRule name=""></tableRule>
  <!-- 定义分片算法 -->
  <function name=""></function>
  ```

- server.xml

  ```xml
  <!-- 定义系统配置 -->
  <system><property name=""></property></system>
  <!-- 定义连接Mycat的用户 -->
  <user></user>
  ```

- 读写分离配置

  - 修改 scheam.xml 文件
  - 修改 server.xml 文件

###### Mycat 监控

- 使用 MySQL 客户端管理 Mycat
- 如何配置 Mycat 日志

