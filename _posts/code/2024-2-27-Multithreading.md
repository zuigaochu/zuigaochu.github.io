---
layout:     post
title:      "Java多线程"
date:       2024-2-27
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目组件文档
---

# 多线程的几种实现方式

* 继承Thread实现

  * 创建一个类，继承自 `Thread` 类。
  * 重写 `run()` 方法，该方法包含线程的执行代码。
  * 创建线程对象，调用 `start()` 方法启动线程。

  ```java
  public class Multithreading extends Thread{
      public void run() {
          for (int i = 0; i < 5; i++) {
              System.out.println(i);
          }
      }
      public static void main(String args[]) {
          Multithreading t1 = new Multithreading();
          Multithreading t2 = new Multithreading();
          t1.start();
          t2.start();
      }
  }
  ```

  

* 实现Runnbale接口
  * 创建一个类，实现 `Runnable` 接口。
  * 实现 `run()` 方法，该方法包含线程的执行代码。
  * 创建线程对象，将实现了 `Runnable` 接口的对象作为参数传递给 `Thread` 类的构造方法，然后调用 `start()` 方法启动线程。

```java
public class Multithreading implements Runnable{
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(i);
        }
    }
    public static void main(String args[]) {
        Thread t1 = new Thread(new Multithreading());
        Thread t2 = new Thread(new Multithreading());
        t1.start();
        t2.start();
    }
}
```

* 匿名内部类
  * 使用 `ExecutorService` 和 `Future` 对象来启动线程并获取线程执行结果。

```java
    public static void main(String args[]) {
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getId() + " Value " + i);
                }
            }
        });

        Thread t2 = new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getId() + " Value " + i);
                }
            }
        });

        t1.start();
        t2.start();
    }
```

* 使用 Callable 和 Future：

  * 创建一个类实现 `Callable` 接口，重写 `call()` 方法。
  * 使用 `ExecutorService` 和 `Future` 对象来启动线程并获取线程执行结果。

  ```java
  package com.ma.book.test;
  
  import java.util.concurrent.Callable;
  import java.util.concurrent.ExecutorService;
  import java.util.concurrent.Executors;
  import java.util.concurrent.Future;
  
  class MyCallable implements Callable<String> {
      public String call() {
          for (int i = 0; i < 5; i++) {
              System.out.println(Thread.currentThread().getId() + " Value " + i);
          }
          return "Task Completed";
      }
  }
  
  public class CallableExample {
      public static void main(String args[]) throws Exception {
          ExecutorService executorService = Executors.newFixedThreadPool(2);
  
          Future<String> future1 = executorService.submit(new MyCallable());
          Future<String> future2 = executorService.submit(new MyCallable());
  
          System.out.println(future1.get());
          System.out.println(future2.get());
  
          executorService.shutdown();
      }
  }
  
  ```

  

# .线程的调度、优先级、分类和生命周期

* 线程的调度
  1. 调度策略：时间片轮转策略和抢占式策略
  2. Java的调度方法：
     同优先级线程组成先进先出队列(先到先服务)，使用时间片轮转策略
     对高优先级，使用优先调度的抢占式策略
* 线程的优先级
  1. 常量表示的优先级：MAX_PRIORITY:10、MIN _PRIORITY:1、NORM_PRIORITY:5
     setPriority(int newPriority) :改变线程的优先级
     getPriority() :返回线程优先值
  2. 线程创建时继承父线程的优先级
     低优先级只是获得调度的概率低，并非一定是在高优先级线程之后才被调用
* 线程的分类
  1. Java中的线程分为两类:一种是守护线程，一种是用户线程。
  2. 守护线程是用来服务用户线程的，通过在start()方法前调用 thread.setDaemon(true)可以把一个用户线程变成一个守护线程。
  3. Java垃圾回收就是一个典型的守护线程。
  4. 若JVM中都是守护线程，当前JVM将退出。



* 线程的生命周期

Java线程它的一个完整的生命周期中通常要经历如下的五种状态:

1. 新建: 当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状态
2. 就绪:处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源
3. 运行:当就绪的线程被调度并获得CPU资源时,便进入运行状态， run()方法定义了线程的操作和功能
4. 阻塞:在某种特殊情况下，被人为挂起或执行输入输出操作时，让出 CPU 并临时中 止自己的执行，进入阻塞状态
5. 死亡:线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束
   



# 多线程死锁

* 什么是死锁？

​    **死锁 是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。**也就是两个线程拥有锁的情况下，又在尝试获取对方的锁，从而造成程序一直阻塞的情况。



```java
        public static void main(String[] args) {
            Object lockA = new Object();
            Object lockB = new Object();

            Thread t1 = new Thread(() -> {
                // 1.占有一把锁
                synchronized (lockA) {
                    System.out.println("线程1获得锁A");
                    // 休眠1s（让线程2有时间先占有锁B）
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    // 获取线程2的锁B
                    synchronized (lockB) {
                        System.out.println("线程1获得锁B");
                    }
                }
            });
            t1.start();

            Thread t2 = new Thread(() -> {
                // 占B锁
                synchronized (lockB) {
                    System.out.println("线程2获得锁B");
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    // 获取线程1的锁A
                    synchronized (lockA) {
                        System.out.println("线程2获得了锁A");
                    }
                }
            });
            t2.start();
        }
```



* 死锁产生的原因

形成死锁主要由以下 4 个因素造成的：

1. 互斥条件：一个资源只能被⼀个线程占有，当这个资源被占用之后其他线程就只能等待。
2. 不可被剥夺条件：当⼀个线程不主动释放资源时，此资源一直被拥有线程占有。
3. 请求并持有条件：线程已经拥有了⼀个资源之后，还不满足，又尝试请求新的资源。
4. 环路等待条件：多个线程在请求资源的情况下，形成了环路链。



*  如何解决死锁问题

 改变产生死锁原因中的任意⼀个或多个条件就可以解决死锁的问题，其中可以被修改的条件只有两个：**请求并持有条件** 和 **环路等待条件**。 

*  改变环路等待条件

  通过修改获取锁的有序性来改变环路等待条件，修改代码如下： 

```java
        public static void main(String[] args) {
            Object lockA = new Object();
            Object lockB = new Object();



            Thread t1 = new Thread(() -> {
                synchronized (lockA) {
                    System.out.println("线程1得到锁A");
                    try {
                        TimeUnit.SECONDS.sleep(1);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (lockB) {
                        System.out.println("线程1得到锁B");
                        System.out.println("线程1释放锁B");
                    }
                    System.out.println("线程1释放锁A");
                }
            }, "线程1");
            t1.start();

            Thread t2 = new Thread(() -> {
                synchronized (lockA) {
                    System.out.println("线程2得到锁A");
                    try {
                        TimeUnit.SECONDS.sleep(1);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (lockB) {
                        System.out.println("线程2得到锁B");
                        System.out.println("线程2释放锁B");
                    }
                    System.out.println("线程2释放锁A");
                }
            }, "线程2");
            t2.start();


        }
```

* 破坏请求并持有条件

   可以通过破坏请求并持有条件解决死锁，修改代码如下

```java
 public static void main(String[] args) {
        Object lockA = new Object();
        Object lockB = new Object();
 
        Thread t1 = new Thread(() -> {
            synchronized (lockA) {
                System.out.println("线程1得到锁A");
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
//                synchronized (lockB) {
//                    System.out.println("线程1得到锁B");
//                    System.out.println("线程1释放锁B");
//                }
                System.out.println("线程1释放锁A");
            }
        }, "线程1");
        t1.start();
 
        Thread t2 = new Thread(() -> {
            synchronized (lockB) {
                System.out.println("线程2得到锁B");
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
//                synchronized (lockA) {
//                    System.out.println("线程2得到锁A");
//                    System.out.println("线程2释放锁A");
//                }
                System.out.println("线程2释放锁B");
            }
        }, "线程2");
        t2.start();
    }

```

