---
title: JVM 1.走近Java
comments: true
categories: [Java]
tags: [Java,JVM]
https: //api.r10086.com/樱道随机图片api接口.php?图片系列=动漫综合2
cover: https://api.r10086.com/樱道随机图片api接口.php?图片系列=动漫综合2
---
> 深入理解Java虚拟机：JVM高级特性与最佳实践（第3版）周志明 著 ——学习笔记

Java不仅仅是一门编程语言，它还是一个由一系列计算机软件和规范组成的技术体系，这个技术 体系提供了完整的用于软件开发和跨平台部署的支持环境，并广泛应用于嵌入式系统、移动终端、企 业服务器、大型机等多种场合



### Java技术体系

Java的技术体系主要由支撑Java程序运行的 **虚拟机**、提供各开发领域接口支持的**Java类库**、**Java编程语言**及许许多多的第三方**Java框架**（如 Spring、MyBatis等）构成。

![image-20231022173418096](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/image-20231022173418096.png)

> :::SUMMARY:::
>
> Java技术体系
>
> - 虚拟机
> - 类库
> - 编程语言
> - 开发框架
>
> 扩展一般以javax.*作为包名，而以java.*为包名的包都是Java SE API的核心包



### Java演变

<div style="height: 50px; white-space: nowrap; overflow-x: auto;">
  绿色计划（Green Project）:开发一种能够在各种消费性电子产品上运行的程序架构 -> Oak 诞生 -> 1995年互联网潮流的兴起 -> Oak 变 Java1.0 : “Write Once，Run Anywhere” -> 1996年JDK 1.0发布（代表技术包括：Java虚拟机、Applet、 AWT等）-> JDK 1.1 （代表技术包括：JAR文件格式、JDBC、JavaBeans、RMI等。Java语 言的语法也有了一定的增强，如内部类（Inner Class）和反射（Reflection）） -> 1998年 JDK 1.2 拆分出三个方向：面向桌面应用开发的J2SE、面向企业级开发的J2EE和面向手机等移动终端开发的J2ME -> Java虚拟机第一次内置了JIT（Just InTime）即时编译器 -> 1999年4月27日，HotSpot虚拟机诞生 -> “JDK 1.x”的命名方式"从 1.5 开始变成以 “JDK x” 命名，在Java语法易用性上做出了非常大的改进。如：自动装箱、泛型、动态注解、枚举、可变长参数、遍历循环（foreach循环）等语法特性都是在JDK 5中加入的。在虚拟机和API层面上，这个版本改进了Java的内存模型（Java Memory Model，JMM）、提供了java.util.concurrent并发包等。 -> 2006年 JDK 6 对Java虚拟机内部做了大量改进，包括锁与同步、垃圾收集、类加载等方面的实现都有相当多的改动 -> JDK 7 -> 2014 Java 8
</div>



### Java虚拟机家族

- Sun Classic/Exact VM

  - ​	准确是内存管理 -> 句柄对象查找

- HotSpot VM
  - ​	HotSpot既继承了Sun之前两款商用虚拟机的优点
  - ​	热点代码探测能力
  - ​	有助于引入更复杂的代码优化技术，输出质量更高的本地代码。

- BEA JRockit/IBM J9 VM
- BEA Liquid VM/Azul VM


- Apache Harmony/Google Android Dalvik VM

- Microsoft JVM及其他


> PS: BEA 是 BEA System公司的简称



### Java虚拟机特点

1. 在千差万别 的物理机上面建立了统一的运行平台 ->  主要精力放在具体业务逻辑 -> 开发更高效快捷
2. 为了达到“所有硬件提供一致的虚拟平台”的目的，牺牲了一些硬件相关的 性能特性 -> 无法达到最优 -> 开发人员不了解虚拟机诸多技术特性的运行原理就无法写出 最适合虚拟机运行和自优化的代码



### Java OpenJDK 编译

使用 GCC编译，需要引入OpenJDK所需依赖包，以及上一版本的 OpenJDK (Bootstrap JDK)





