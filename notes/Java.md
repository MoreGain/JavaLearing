# Java

## Java基础

##### 计算机语言

- 常用DOS命令

##### Java语言概述

- Java的三种技术架构

- Java语言的特点

  ​	**跨平台**

- JDK与JRE
- **环境变量**

##### Java语言基础组成

- 关键字

- 标识符

- 注释

- 变量与常量

  进制转换、基本数据类型(内存大小)、数据类型自动提升及强制转换、ASCII码表

  ```java
  //right?
  byte a = 4;
  a = a + 5;
  
  //right?
  short a = 4;
  a = (short)a + 4;
  ```

  

- 运算符

  算数运算符、转义字符、赋值运算符、比较运算符(intanceof)、逻辑运算符、三目运算符、位运算符

  ```java
  //what's value?
  int a = 4027;
  a = a / 1000 * 1000;
  
  //has any difference?
  short s = 4;
  s = s + 5;
  s += 5;
  
  //what's value?
  int x = 6 & 3;
  int y = 6 | 5;
  int z = 7 ^ 4 ^ 4;
  int m = ~ 4;
  
  //which way is more efficient? why?
  int p = 2 * 8;
  int p  = 2 << 3;
  ```

  [原码反码补码](https://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)

  > 交换两个数的值

  ```java
  //method one
  int temp = a;
  a = b;
  b = temp;
  
  //method two(if the value of a,b is big, it's easy to out of range) 
  n = n + m;
  m = n - m;
  n = n - m;
  
  //method three
  n = n ^ m;
  m = n ^ m;
  n = n ^ m;
  ```

  > 如和获得一个数的16进制？

  ```java
  int num = 100;	//0000-0000 0000-0000 0000-0000 0110-0100
  int part1 = num & 15;	//取出0100
  int part2 = num >>> 4 & 15;	//取出0110
  ```

  

- 程序流程语句

  判断语句

  > if-else if-if语句

  选择语句

  > switch语句

  循环语句

  > for语句、while语句、do-while语句

  > 累加思想	计数器思想

- 循环控制语句

  > break; 	continue; 	return;

- 函数

  > 函数定义
  >
  > 函数重载

- 数组

  > 数组定义
  >
  > 数组初始化
  >
  > 内存模型(栈堆结构)
  >
  > 数组操作的下标越界异常、空指针异常
  >
  > 数组遍历(自定义数组打印方法)

### 枚举

### 注解

注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助性的作用。

[什么是注解？注解的用处？注解的原理？元注解？常见注解？自定义注解及反射获取](https://www.cnblogs.com/acm-bingzi/p/javaAnnotation.html)

通过注解实现自动创建数据库表