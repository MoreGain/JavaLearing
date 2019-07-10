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

