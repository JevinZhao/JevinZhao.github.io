---
title: Java高并发编程详解
date: 2023-04-04 20:25:57
tags: 读书笔记
categories: 并发编程
---
# Java高并发编程详解

[![pp5C0eO.md.png](https://s1.ax1x.com/2023/04/04/pp5C0eO.md.png)](https://imgse.com/i/pp5C0eO)

总结：对juc包的各个核心类讲解还不深入，很多内容点到为止。看过《Java多线程编程实战指南（核心篇）》，本书前14都可以快速翻过。第四部分设计模式有点意思，不少所谓设计模式其实就是对juc包中的一个类的使用。

## 线程

概念：对于计算机来说，每一个任务就是一个进程（process），每一个进程至少包含一个线程（Thread），线程是程序的执行路径，每一个线程都有自己的局部变量表，程序计数器（指向正在执行的指令指针），以及各自的生命周期

### 线程的生命周期

- NEW

  - new 完一个Thread对象状态

- RUNNABLE

  - 具备执行资格，等待CPU调度
  - 只能进入意外终止或者进入RUNNING状态

- RUNNING

  - 被CUP选中，正在执行的线程
  - 处于RUNNING状态的线程也是RUNNABLE状态

- BLOCKED

  - 线程阻塞

- TERMINATED

  - 线程终止

- 线程状态的转化关系 

### start源码分析

- 模板设计模式

  - 父类编写算法结构代码，子类实现逻辑细节，程序结构由父类控制，子子类只需要实现想要的逻辑任务

- start方法会调用start0方法
- start0方法是JNI方法（本地方法），会调用run方法
- Thread被构造后的NEW状态，内部ThreadStatus是0
- 不能两次启动Thread
- 线程启动后会加入到ThreadGroup
- 到了TERMINATED状态，再次调用start方法是不允许的

### Runnable接口分析

- 创建线程只有一种方式那就是构造Thread类
- 实现线程的执行单元

  - 重写Thread类的run方法
  - 实现Runnable接口的run方法，且将Runnable实例用作为Thread的参数
  - 两者不同点

    - Thread类中run方法不能共享，而同一个Runnable实例可以构造不同的Thread实例

- 策略设计模式

  - 无论是Runnable的run方法，还是Thread类的run方法（实际上也是实现Runnable接口）都是将线程的控制本身和业务逻辑的运行分离开来

## Thread类

### 构造函数

- 线程命名

  - Thread()
  - Thread(Runnable  target)
  - Thread(ThreadGroup group，Runnable target)

- 线程父子关系

  - 一个线程的创建肯定是由另一个线程完成
  - main函数所在的线程是由JVM创建，所有线程父线程都是main线程

- Thread与JVM虚拟机栈
- 守护线程

  - 后台线程，退出JVM进程时，一些线程可以自动关闭

### 常用API

- sleep

  - 不会放弃monitor锁的所有权
  - 会使线程短暂block

- yield

  - 放弃当前CPU的资源，会使线程状态由RUNNING切换到RUNNABLE
  - 只是一个提示，实际会不会执行要看CPU调度器

- setPriority
- getPriority
- getID
- currentThread
- getContextLoader

  - 获取线程的类加载器

- interrupt

  - 打断阻塞状态
  - 源码：在一个线程内部存在名为interrupt flag的标志，一个线程被interrupt，它的flag将会被设置

- join

  -  join会使当前线程永久等待下去，直到期间被另外线程中断，或者join的线程执行结束

- 线程关闭

  - 正常关闭
  - 异常退出
  - 假死

## 线程安全与线程同步

### 数据不一致问题产生的原因

- 多个线程对共享变量同时操作

### synchronized关键字

- 提供了一种互斥机制，在同一时刻只能有一个线程访问同步资源
- 用法

  - 修饰同步方法
  - 同步代码块

- 某个线程获取了与特定对象关联的monitor锁
- monitorenter
- monitorexit
- 注意事项

  - 与monitor关联的对象不能为空
  - synchronized作用域不能太大，越大效率越低
  - 不同的monitor不能锁相同方法
  - 避免多个锁的交叉，容易导致死锁

## 线程间的通信

### 同步阻塞

### 异步非阻塞

### 单线程间的通信

- wait/notify

  - wait方法会导致当前线程进入阻塞，直到其他线程调用notify/notifyAll方法唤醒
  - wait/notify方法必须拥有该对象的monitor，必须在同步方法中使用
  - 当前线程执行了该对象wait方法后，会放弃对该monitor的所有权并进入与该对象关联的wait set中
  - wait与sleep对比

    - 都可以使线程进入阻塞状态
    - 都是可中断方法
    - wait是Object方法，sleep是Thread类方法
    - wait必须在同步方法中进行，sleep不需要
    - 在同步方法中执行sleep不会释放锁，但wait方法会释放
    - sleep短暂休眠后会主动退出阻塞，而wait(未设置时间)需要被打断

### 多线程间的通信

- wait set

  - 线程调用某个对象的wait方法之后都会加入与该对象monitor关联的wait set中，并且释放monitor的所有权

- synchronized关键字的缺陷

  - 无法控制阻塞时长
  - 阻塞不可被中断

## 线程组/HOOK线程

### 略

## 线程池

### 原理

- 任务队列
- 线程数量管理
- 任务拒绝策略
- 线程工厂
- 队列大小
- 自动维护时间间隔

### 自定义

- 略

## 类加载

## 设计模式

*XMind: ZEN - Trial Version*