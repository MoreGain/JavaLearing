# JVM

### 内存区域与内存溢出异常

###### 运行时数据区域

五大区域各自作用与特点（线程拥有、内存溢出）



### 垃圾收集器与内存分配策略

##### 如何判断一个对象是否可被回收？

- 引用计数法
- 根搜索算法

##### 引用类型

- 强引用(Strong Reference)
- 软引用(Soft Reference)
- 弱引用(Weak Reference)
- 虚引用(Phantom Reference)

##### finalize() 拯救对象

##### 方法区（老年代）回收

- 废弃常量
- 无用的类
  - 该类所有实例都被回收
  - 加载该类的 ClassLoader 已被回收
  - 该类的 Class 对象不再被引用

##### 垃圾收集算法（思路、缺点）

- 标记-清除算法
- 复制算法
- 标记-整理算法
- 分代收集算法

##### 垃圾收集器

- Serial 收集器

  新生代收集器，单线程、Stop the World

- ParNew 收集器

  新生代收集器，同 Serial 收集器，多线程，在单 CPU 环境下效果不如 Serial

  -XX:ParallelGCThreads 限制参与垃圾收集的线程数

- Parallel Scavenge 收集器

  新生代收集器，吞吐量优先收集器，为了达到一个可控制的吞吐量（=运行用户代码时间/(运行用户代码时间+垃圾回收时间)）

  -XX:MaxGCPauseMillis 最大垃圾收集停顿时间

  -XX:GCTimeRatio 吞吐量大小，默认值99

  -XX:+UseAdaptiveSizePolicy 自适应调节策略的开关参数

- Serial Old 收集器

- Parallel Old 收集器

- CMS 收集器

  老年代收集器，Concurrent Mark Sweep, 以获取最短回收停顿时间为目标，使用“标记-清除”算法实现

  运行步骤：

  - 初始标记

    Stop the world, 标记 GC Roots 能直接关联的对象，速度很快

  - 并发标记

    GC Roots Tracing

  - 重新标记

    Stop the world, 修正并发标记期间标记变动的标记记录

  - 并发清除

  被称为并发低停顿收集器，缺点：

  - 对 CPU 资源非常铭感

    会占用一部分线程导致应用程序变慢，当 CPU 小于 4 个时对用户程序的影响可能变得很大

  - CMS 收集器无法处理浮动垃圾

    清理阶段用户线程产生的垃圾无法收集，需要预留足够的内存空间给用户线程使用，可能出现“Concurrent Mode Failure”；CMS 收集器默认在老年代使用了 68% 的空间后就会被激活，设置-XX:CMSInitiatingOccupancyFraction 可提供触发百分比；后备预案：使用 Serial Old 收集器进行老年代的收集，停顿时间很长

  - 标记-清除算法的空间问题

    使用 -XX:UseCMSCompactAtFullCollction 开关参数设置 CMS 收集完成后进行碎片整理；-XX:CMSFullGCBeforeCompaction 设置多少次进行一次压缩的 Full GC

- G1 收集器

  基于“标记-整理”算法实现；可以精确的控制停顿时间，即可以明确指定在 M 毫秒的时间片内消耗在垃圾收集的时间不超过 N 毫秒

  G1 收集器可以实现在基本不牺牲吞吐量的前提下完成低停顿的内存回收，它将 Java 堆划分为多个大小固定的独立区域，跟踪每个区域的垃圾堆积程度，并维护一个优先列表，优先回收垃圾最多的区域

##### 内存分配

- 对象优先在 Eden 分配

  > Minor GC : 新生代的垃圾收集动作，频繁且回收速度快
  >
  > Full GC / Major GC : 老年代的 GC， 速度比 Minor GC 慢 10 倍以上

  对象在 Eden 区中分配，空间不足时发起 Minor GC

  -XX:+PrintGCDetails 收集器日志参数

  案例：尝试分配 3 个 2M 和 1 个 4M 的对象，虚拟机参数设置：-Xms20M、-Xmx20M、-Xmn10M、-XX:SurvivorRatio=8，分析内存分配过程。

- 大对象直接进入老年代

  大对象：需要大量连续空间的 Java 对象

  -XX:PretenureSizeThresold 设置大于某个值的对象直接在老年代中分配内存，此参数只对 Serial 和 ParNew 收集器有效

  案例：分配一个 4M 的对象，虚拟机参数设置：-XX:PretenureSizeThreshold=3145278(3M)

- 长期存活的对象进入老年代

  当对象年龄计数器增加到一定程度（默认15岁）时会被晋升到老年代，-XX:MaxTenuringThreshold

- 动态对象年龄判定

  Survivor 空间中相同年龄所有对象大小的总和大于 Survivor 空间的一半，年龄大于或等于该年龄的对象可以直接进入老年区

- 空间分配担保

  发生 Minor GC 时，虚拟机会检测之前每次晋升到老年代的平均大小是否大于老年代的剩余空间，大于则直接进行一次 Full GC，小于则查看 HandlePromotionFailure 设置是否允许担保失败



### Java 内存模型与线程

###### 概述

服务器性能高低：Transaction Per Second

物理计算机的并发问题：存储设备与处理器的运算速度存在大差距---->高速缓存(cache)/代码乱序执行优化(Out-Of-Order Execution)---->缓存一致性(Cache Coherence)---->缓存协议

###### Java 内存模型

定义了各个变量的访问规则（共享变量）

主内存(Main Memory)：存储所有变量

工作内存(Working Memory)：线程私有，存储该线程使用到的变量的主内存副本

内存交互

> lock(锁定): 作用于主内存，标识变量为线程独占
>
> unlock(解锁): 作用于主内存，释放锁定变量
>
> read(读取): 作用于主内存，传输变量到工作内存 
>
> load(载入): 作用于工作内存，将 read 的值放入工作内存变量副本
>
> use(使用): 作用于工作内存，将变量传递给执行引擎
>
> assign(赋值):  作用于工作内存，将从执行引擎接收的值赋值给工作内存
>
> store(存储): 作用于工作内存，传输变量到主内存
>
> write(写入): 作用于主内存，将 store 的值放入主内存
>

内存交互规则：
>
> 不允许 read 和 load、store 和 write 单独出现
>
> 不允许线程丢弃最近的 assign 操作
>
> 未发生 assign 不能同步到主内存
>
> 变量只能在主内存诞生
>
> 一个变量只能同一时刻只能被一个线程 lock
>
> 一个变量被 lock ，将会清空工作内存此变量的值
>
> 未 lock 的变量不能 unlock
>
> 执行 unlock 前，必须执行 store 和 write

###### volatile

特性

> 保证变量对所有线程的可见性：volatile 变量在各个线程中是一致的，但基于 volatile 的运算在并发下不一定是安全的。只保证可见性， 没有保证原子性
>
> 禁止指令重排序优化

long 和 double 型变量的特殊规则

> 允许虚拟机将没有被 volatile 修饰的64位数据的读写操作划分为两次32位的操作来进行，非原子协定；最终结论是我们不需要将用到的 long 和 volatile 变量专门声明为 volatile

原子性(Atomicity)、可见性(Visibility)和有序性(Ordering)

> 原子性：基本数据类型的访问读写时具备原子性的，synchronized 关键字提供更大范围的原子性保证
>
> 可见性：一个线程修改了共享变量的值，其他线程能够立即得知这修改；Java 可见性通过内存交互同步与更新实现可见性；volatile、synchronized、final 均能实现可见性
>
> 有序性：线程内表现为串行，线程间有指令重排和内存同步延迟现象；volatile 和 synchronized 可以保证线程间的有序性

先行发生(happens-before)原则

> Java 内存模型已有的先行发生关系：程序次序规则、管程锁定规则、volatile 变量规则、线程启动规则、线程终止规则、线程中断规则、对象终结规则、传递性
>
> 时间上的先后顺序与先行发生原则之间没有太大关系

###### Java 与线程

线程的实现

> 使用内核线程实现：KLT、LWP，1：1
>
> 使用用户线程实现：UT，1：N
>
> 使用混合实现：M：N

Java 线程的实现

> JDK 1.2 之前基于“绿色线程”用户线程实现，之后基于操作系统原生线程模型实现

Java 线程调度

> 协同式线程调度：线程本身控制执行时间，主动通知系统切换
>
> 抢占式线程调度：系统分配执行时间，系统决定线程切换；Java 使用抢占式线程调度，Thread.yield() 可让出执行时间，Thread.MIN_PRIORITY-Thread.MAX_PRIORITY 可设置线程优先级

状态转换

> 一个线程只能有且只有一种状态：
>
> 新建(new)
>
> 运行(Runable)
>
> 无限期等待(Waiting)：Object.wait()、Thread.join()、LockSupport.park()
>
> 限期等待(Timed Waiting)：Thread.sleep()、Object.wait(timeout)、Thread.join(timeout)、LockSupport.parkNanos()、LockSupport.parkUntil()
>
> 阻塞(Blocked)
>
> 结束(Terminated)

