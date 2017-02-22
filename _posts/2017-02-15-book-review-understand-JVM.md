---
layout: post
title: 《Understanding the JVM》Chapter 1-3
categories:  读书笔记
tags: 读书笔记
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## Part 1: 走进Java

## Chapter 1: 走进Java

#### Java技术体系

![img](../images/java-eco-system.png)

以上是根据各个组成部分的功能来进行划分的，如果按照技术所服务的领域来划分，或者说按照Java技术关注的重点业务领域来划分，Java技术体系可以分为4个平台，分别为：

- **Java Card**：支持一些Java小程序（Applets）运行在小内存设备（如智能卡）上的平台。
- **Java ME（Micro Edition）**：支持Java程序运行在移动终端（手机、PDA）上的平台，对Java API有所精简，并加入了针对移动终端的支持，这个版本以前称为J2ME。
- **Java SE（Standard Edition）**：支持面向桌面级应用（如Windows下的应用程序）的Java平台，提供了完整的Java核心API，这个版本以前称为J2SE。
- **Java EE（Enterprise Edition）**：支持使用多层架构的企业应用（如ERP、CRM应用）的Java平台，除了提供Java SE API外，还对其做了大量的扩充[3]并提供了相关的部署支持，这个版本以前称为J2EE。

## Part 2: 自动内存管理机制

### Chapter 2: Java内存区域于内存溢出异常

Java虚拟机在执行Java程序的过程中会把它所管理的内存划分为若干个不同的数据区域。这些区域都有各自的用途，以及创建和销毁的时间，有的区域随着虚拟机进程的启动而存在，有些区域则依赖用户线程的启动和结束而建立和销毁。根据《Java虚拟机规范（JavaSE 7版）》的规定，Java虚拟机所管理的内存将会包括以下几个运行时数据区域，如下图所示。

![img](../images/jvm-data-region.png)

#### 程序计数器 (Program Counter Register)

- 较小的内存空间，可看作当前线程代码的行号指示器；字节码解释器通过改变这个计数器的值来选取下一条需要执行的指令
- 由于JVM的多线程是通过线程轮流切换并分配CPU执行时间来实现的，一个CPU(对于多核处理器是一个kernel)只会执行一条线程中的指令；因此每条线程需要一个独立的程序计数器来指示当前代码的执行位置，这类内存为“线程私有内存”
- 如果线程正在执行的是一个Java方法，这个计数器记录的是正在执行的虚拟机字节码指令的地址；如果正在执行的是Native方法，这个计数器值则为空（Undefined）。此内存区域是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError的区域

#### Java虚拟机栈 (Java Virtual Machine Stacks)

- Java虚拟机栈（Java Virtual Machine Stacks）也是线程私有的，它的生命周期与线程相同。虚拟机栈描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。