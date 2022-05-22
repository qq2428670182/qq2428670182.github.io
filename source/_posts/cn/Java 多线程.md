---
title: Java 多线程
catalog: true
date: 2020-08-05 02:34:17
subtitle: Java 多线程
lang: cn
header-img: /img/header_img/lml_bg.jpg
tags:
- 多线程
categories:
- 多线程
---

### 线程池

总体来说，线程池有如下的优势：

（1）降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。

（2）提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。

（3）提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

线程池的真正实现类是 **ThreadPoolExecutor**，一般来说创建线程池的方式是直接new出ThredPoolExecutor类对象，调用其构造方法创建池对象

需要如下几个参数：

```txt
corePoolSize（必需）：核心线程数。默认情况下，核心线程会一直存活，但是当将 allowCoreThreadTimeout 设置为 true 时，核心线程也会超时回收。

maximumPoolSize（必需）：线程池所能容纳的最大线程数。当活跃线程数达到该数值后，后续的新任务将会阻塞。

keepAliveTime（必需）：线程闲置超时时长。如果超过该时长，非核心线程就会被回收。如果将 allowCoreThreadTimeout 设置为 true 时，核心线程也会超时回收。

unit（必需）：指定 keepAliveTime 参数的时间单位。常用的有：TimeUnit.MILLISECONDS（毫秒）、TimeUnit.SECONDS（秒）、TimeUnit.MINUTES（分）。

workQueue（必需）：任务队列。通过线程池的 execute() 方法提交的 Runnable 对象将存储在该参数中。其采用阻塞队列实现。

threadFactory（可选）：线程工厂。用于指定为线程池创建新线程的方式。
handler（可选）：拒绝策略。当达到最大线程数时需要执行的饱和策略。

```





#### Executors功能性线程池

##### 提供的API有四种：

- 定长线程池（FixedThreadPool）
- 定时线程池（ScheduledThreadPool ）
- 可缓存线程池（CachedThreadPool）(可缓存的线程池最常用，但是实际应用中不推荐，推荐直接ThreadPoolExtcutor，不然即使缓存，不进行自定义内存限制，线程数最大创建仍没有限制，可能超出物理内存)
- 单线程化线程池（SingleThreadExecutor）

考虑到ThreadPoolExecutor的[构造函数](https://so.csdn.net/so/search?q=构造函数&spm=1001.2101.3001.7020)实在是有些复杂，所以Java并发包里提供了一个线程池的静态工厂类Executors，利用Executors你可以快速创建线程池。

这是一个类，但不建议使用该类，不建议使用Executors的最重要的原因是：Executors提供的很多方法默认使用的都是无界的LinkedBlockingQueue（如下图），高负载情境下，无界队列很容易导致OOM，而OOM会导致所有请求都无法处理，这是致命问题。所以强烈建议使用有界队列。

> 注：LinkedBlockingQueue是有界队列，但是不设置大小的话，就默认为Integer.MAX_VALUE，相当于无界队列了。



![工具类方法](https://img-blog.csdnimg.cn/20200319153306281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hlbnJ5X0xpbl9XaW5k,size_16,color_FFFFFF,t_70)

##### 正常线程池的创建和关闭流程

```java
// 创建线程池
ThreadPoolExecutor threadPool = new ThreadPoolExecutor(CORE_POOL_SIZE,
                                             MAXIMUM_POOL_SIZE,
                                             KEEP_ALIVE,
                                             TimeUnit.SECONDS,
                                             sPoolWorkQueue,
                                             sThreadFactory);
// 向线程池提交任务
threadPool.execute(new Runnable() {
    @Override
    public void run() {
        ... // 线程执行的任务
    }
});
// 关闭线程池
threadPool.shutdown(); // 设置线程池的状态为SHUTDOWN，然后中断所有没有正在执行任务的线程
threadPool.shutdownNow(); // 设置线程池的状态为 STOP，然后尝试停止所有的正在执行或暂停任务的线程，并返回等待执行任务的列表
```

注意execute执行方法的参数是线程对象，通过创建实现Runnable接口的匿名内部类作为对象参数（run方法重写）