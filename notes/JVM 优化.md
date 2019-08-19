# JVM 优化

### 一、概述

生产环境中，遇到诸如此类的问题：

1. 运行的应用"卡住了"，日志不输出，程序没有过反应
2. 服务器的 CPU 负载突然升高
3. 多线程应用下，如何分配线程的数量

需要通过对 JVM 优化来进行处理



### 二、JVM 的运行参数

- 标准参数

  在 JVM 的版本中一般不会改变，如 `-help` `-version`

- -X 参数

  非标准参数，不稳定的，在 JVM 的版本中可能发生改变，如 `-Xint` `-Xcomp`

- -XX 参数

  非标准参数，常用于 JVM 调优和 DEBUG，使用率较高，如 `-XX:newSize` `-XX:+UserSerialGC`

- -showversion

  参看版本并执行后续命令

- -D<名称>=<值>

  设置系统参数，如 `java -Dstr=hello JVM`

- -server 与 -client
  - Server VM 的初始堆空间会大一些，默认使用的并行垃圾回收器，启动慢运行快
  - Client VM 相对来讲会保守一些，初始堆空间会小一些，使用串行的垃圾回收器，JVM 启动更快但运行速度比 Server VM 慢
  - JVM 启动时会根据硬件和操作系统自动选择使用 Server 还是 Client 类型的 JVM。32位操作系统中，windows 下不论硬件配置如何都默认使用 Client 类型的 JVM，其他操作系统机器配置 2GB 以上的内存同时有 2 个以上 CPU 时默认使用 Server 模式；64 位操作系统只有 server 模式

- 常用 -X 参数

  - `java -x` 查看非标准参数
  - `-Xint` : 解析模式，强制 JVM 执行所有的字节码，每次执行都会逐行执行字节码，会降低运行速度
  - `-Xcomp` : 编译模式，第一次使用时把所有的字节码编译成本地代码，带来最大程度的优化，第一次执行会比解释模式慢一些，此参数没有让 JVM 启用 JIT 编译器的全部功能，JIT 编译器可以对是否需要编译做判断，对于只执行一次的代码不需要进行编译
  - `-Xmixed` : 混合模式，解释模式与编译模式混合使用，由 JVM 自动决定，也是 JVM 的默认模式

- -XX 参数

  有两种方式，一种时 Boolean 类型，一种是非 Boolean 类型

  - boolean 类型

    格式：`-XX:[+-]<name>` 表示启用或禁用 `<name>` 属性，如：`-XX:+DisableExplicitGC` 表示禁止手动调用 gc 操作

  - 非 boolean 类型

    格式：`-XX:<name>=<value>` 表示 `<name>` 的值为 `<value>` ，如：`-XX:NewRatio=1` 表示新生代和老年代的比值

- -Xms 和 -Xmx 参数

  `-Xms` 与 `-Xmx` 分别是设置 JVM 的堆内存的初始大小和最大大小

  `-Xms2048m` 等价于 `-XX:MaxHeapSize`

  `-Xmx512m` 等价于 `-XX:InitialHeapSize`

- 查看 JVM 的运行参数

  - 运行 Java 命令时打印参数

    `java -XX:+PrintFlagsFinal -version` "=" 后为默认值，“:=” 为修改后的值

  - 查看正在运行的 JVM 参数

    `jinfo -flags <进程id>` 查看所有的参数，`jinfo -flags <参数名> <进程id>` 查看指定的参数值

    `jps` `jps -l` Java 提供的查看 Java 进程的命令，`ps -ef|grep tomcat` 查看进程



### 三、JVM 的内存模型

- JDK 1.7 的堆内存模型

  - Young 年轻代
  - Tenured 老年代
  - Perm 永久代

- JDK 1.8 的堆内存模型

  - Young 老年代

  - OldGen 老年代

  - Metaspace 元空间

    Metaspace 取代了以前的永久代，Metaspace 所占用的内存空间不是在虚拟机内部，而是在本地内存空间中，也叫非堆内存，包含 CCS 和 CodeCache 两部分

- 为什么要废弃 1.7 中的 PermGen

  官方解释：为融合 HotSpot JVM 和 JRockit VM 做出的努力，JRockit 没有永久代。

  实际使用中，永久代内经常不够用或发生内存泄漏，使用本地内存后不再有此问题

- jstat 查看堆内存使用情况

  - `jstat -class <pid>` 查看 class 加载统计
  - `jstat -compiler <pid>` 查看 编译统计
  - `jstat -gc <pid>` 查看垃圾回收统计

- jmap 的使用和内存溢出分析

  - `jmap -heap <pid>` 查看内存使用情况
  - `jmap -histo <pid> | more` 查看所有对象数量及大小，`jmap -histo:live <pid> | more` 查看活跃对象
  - `jmap -dump:format=b,file=dumpFileName <pid>` 将内存使用情况 dump 到文件中

- jhat 分析 dump 文件

  `jhat -port <port> <file>` ，可通过 OQL 查询对象

- MAT 分析 dump 文件

  基于 Eclipse 的内存分析工具



### 四、内存溢出的定位与分析

- 制造内存溢出情况

  代码编写：

  ```java
  public class TestJvmOutOfMemory {
      public static void main(String[] args) {
          List<String> list = new ArrayList<>();
          for(int i = 0; i < 1000000; i++) {
              String str = "";
              for(int j = 0; j < 1000; j++) {
                  str += UUID.randomUUID().toString();
              }
              list.add(str);
          }
          System.out.println("ok");
      }
  }
  ```

  JVM 参数设置：

  `-Xms8m -Xmx8m -XX:+HeapDumpOnOutofMemoryError` 



### 五、jstack 的使用

- `jstack <pid>` 查看 JVM 中的线程执行情况
- 线程状态：
  - 初始态
  - 就绪态
  - 运行态
  - 等待态
  - 超时等待态
  - 终止态



### 六、实战死锁

- 构建死锁

  代码编写：

  ```java
  pulic class TestDeadLock {
      private static Object obj1 = new Object();
      private static Object obj2 = new Object();
      
      public static void main() {
          new Thread(new Thread1()).start();
          new Thread(new Thread2()).start();
      }
      
      public class Thread1 implements Runnable {
          public void run() {
              synchronized(obj1) {
                  System.out.println("obj1 被锁定");
                  try {
                      Thread.sleep(2000);
                  } catch(InterruptedException e) {
                      
                  }
                  synchronized(obj2) {
                      System.out.println("obj2 被锁定");
                  }
              }
          }
      }
      public class Thread2 implements Runnable {
          public void run() {
              synchronized(obj2) {
                  System.out.println("obj2 被锁定");
                  try {
                      Thread.sleep(2000);
                  } catch(InterruptedException e) {
                      
                  }
                  synchronized(obj1) {
                      System.out.println("obj1 被锁定");
                  }
              }
          }
      }
  }
  ```

  

### 七、VisualVM 工具

- 监控本地
- 监控远程的 JVM 
  - JMX : 一个为应用程序、设备、系统等植入管理功能的框架。它可以跨越一系列异构操作系统平台、系统体系结构和网络传输协议，灵活的开发无缝集成的系统、网络和服务管理应用
  - 监控远程的 tomcat
    - 在 tomact 上进行配置
    - 使用 VisualVM 进行远程连接

- `rz` Linux 上传，`sz` Linux 下载