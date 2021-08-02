# 1. 什么是JUC

java.util.concurrent工具包

# 2. 线程和进程

进程: 一个程序, QQ.exe Music.exe 程序的集合;
一个进程往往可以包含多个线程, 至少包含一个!
Java默认有几个线程?  2个：mian、GC

## 并发和并行

并发编程: 并发、并行

**并发(多线程操作向-个资源)**

- CPU一核,模拟出来多条线程,天下武功,唯快不破,快速交替

**并行(多个人一起行走)**

- CPU多核,多个线程可以同时执行;

并发编程的本质: 充分利用CPU的资源

## 线程的状态

```java
public enum State {
    /**
     * 新生
     */
    NEW,

    /**
     * 运行态
     */
    RUNNABLE,

    /**
     * 阻塞
     */
    BLOCKED,

    /**
     * 等待，死等
     */
    WAITING,

    /**
     * 超时等待
     */
    TIMED_WAITING,

    /**
     * 终止
     */
    TERMINATED;
}
```

## wait/sleep区别

1. **来自不同的类**

wait -> Object

sleep -> Thread

2. **关于锁的释放**

wait会释放锁, sleep睡觉了,抱着锁睡觉,不会释放!

3. **使用的范围不同**

wait 必须在同步代码块中使用，sleep可以在任意地方使用

4. **是否需要捕获异常**

wait不需要捕获异常，sleep需要

# 3. Lock

## synchronized

本质：队列，锁

## Lock

![image-20210624111147145](JUC.assets/image-20210624111147145.png)



![image-20210624111242536](JUC.assets/image-20210624111242536.png)



![image-20210624111004185](JUC.assets/image-20210624111004185.png)

公平锁：先来后到。

非公平锁：可以插队（默认）

```java
// Lock三部曲
// 1、new ReentrantLock();
// 2、lock.Lock(); //加锁
// 3、finally=> lock. unlock(); //解锁
class Ticket2 {
    //属性、方法
    private int number = 30;
    Lock lock = new ReentrantLock();
    public void sale() {
        lock.lock(); //加锁
        try {
            //业务代码
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock(); //解锁 
        }
    }
}
```

## lock 和 synchronized区别

1、Synchronized 内置的Java关键字 ，Lock 是一个ava类

2、Synchronized 无法判断获取锁的状态, Lock可以判断是否获取到了锁

3、Synchronized 会自动释放锁, lock必须要手动释放锁!如果不释放锁,死锁

4、Synchronized 线程1 (获得锁,阻塞)、线程2 (等待,傻傻的等) ; Lock锁就不一定会等待下去;

5、Synchronized 可重入锁 ,不可以中断的,非公平; Lock , 可重入锁,可以判断锁,非公平(可以自己设置) ;

6、Synchronized 适合锁少量的代码同步问题, Lock适合锁大量的同步代码!

## 锁是什么，如何判断锁

**synchronized 锁的对象是方法的调用者!**

如果两个方法带有synchronized 关键字修饰的**成员方法**由同一个对象调用，则这两个方法用的是同一个锁，谁先拿到谁执行!

如果是**静态方法**，synchronized 锁的是该类加载后生产的Class对象

生产者消费者模式

![image-20210624140725636](JUC.assets/image-20210624140725636.png)

问题在于if只会判断一次。而判断过后就wait了, 它醒了之后也不会再判断了.

![image-20210624141324470](JUC.assets/image-20210624141324470.png)

```java
package com.heaven;

import javax.swing.plaf.synth.SynthOptionPaneUI;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

//JUC版本的生产者消费者模式
public class B {
    public static void main(String[] args) {
        final Data data = new Data();

        new Thread(()->{
            for (int i=0;i<10;i++) {
                try {
                    data.increment();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"A").start();
        new Thread(()->{
            for (int i=0;i<10;i++) {
                try {
                    data.decrement();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
        new Thread(()->{
            for (int i=0;i<10;i++) {
                try {
                    data.increment();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"C").start();
        new Thread(()->{
            for (int i=0;i<10;i++) {
                try {
                    data.decrement();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}
class Data{
    private int num = 0;
    private Lock lock = new ReentrantLock();
    private Condition condition=lock.newCondition();


    public void increment() throws InterruptedException {
        lock.lock();
        try {
            while(num!=0){
                //等待
                condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName()+"=>"+num);
            //唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

    }
    public void decrement(){
        lock.lock();
        try {
            while (num==0){
                //等待
                condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName()+"=>"+num);
            //唤醒
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

    }
}
```

## Condition

**精准的通知和唤醒线程**

```java
package com.heaven;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

//A执行完调用B，B执行完调用C，C执行完调用A
public class C {
    public static void main(String[] args) {
        DataC dataC = new DataC();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                dataC.printA();
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                dataC.printB();
            }
        },"B").start();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                dataC.printC();
            }
        },"C").start();
    }

}
class DataC{
    private Lock lock=new ReentrantLock();
    private Condition conditionA=lock.newCondition();
    private Condition conditionB=lock.newCondition();
    private Condition conditionC=lock.newCondition();

    private int num=1; //1A,2B,3C

    public void printA(){
        lock.lock();
        try {
            while(num!=1){
                //等待
                conditionA.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>AAAAAAAAAAAAAAAAAA");
            num=2;
            conditionB.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public void printB(){
        lock.lock();
        try {
            //业务
            while(num!=2){
                //等待
                conditionB.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>BBBBBBBBBBBBBBBBBB");
            num=3;
            conditionC.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public void printC(){
        lock.lock();
        try {
            while(num!=3){
                //等待
                conditionC.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>CCCCCCCCCCCCCCCCCC");
            num=1;
            conditionA.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

# 4. 集合不安全问题

解决方案：

```java
// List<String> list=new Vector<>();
// List<String> list= Collections.synchronizedList(new ArrayList<>());
List<String> list=new CopyOnWriteArrayList<>();
```

CopyOnWrite  写入时复制，COW，计算机程序设计领域的一种优化策略;

简单来说，就是平时查询的时候，都不需要加锁，随便访问，只有在更新的时候，才会从原来的数据复制一个副本出来，然后修改这个副本，最后把原数据替换成当前的副本。修改操作的同时，读操作不会被阻塞，而是继续读取旧的数据。这点要跟读写锁区分一下。

**1.优点**

对于一些读多写少的数据，写入时复制的做法就很不错，例如配置、黑名单、物流地址等变化非常少的数据，这是一种无锁的实现。可以帮我们实现程序更高的并发。

CopyOnWriteArrayList 并发安全且性能比 Vector 好。Vector 是增删改查方法都加了synchronized 来保证同步，但是每个方法执行的时候都要去获得锁，性能就会大大下降，而 CopyOnWriteArrayList 只是在增删改上加锁，但是读不加锁，在读方面的性能就好于 Vector。

**2.缺点**

数据一致性问题。这种实现只是保证数据的最终一致性，在添加到拷贝数据而还没进行替换的时候，读到的仍然是旧数据。

内存占用问题。如果对象比较大，频繁地进行替换会消耗内存，从而引发 Java 的 GC 问题，这个时候，我们应该考虑其他的容器，例如 ConcurrentHashMap。

![image-20210624152428751](JUC.assets/image-20210624152428751.png)

# 5. Map不安全的问题

![image-20210624153952863](JUC.assets/image-20210624153952863.png)

# 6.Callable

```java
package com.heaven;

import java.util.concurrent.*;

public class Test1 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // new Thread(new Runnable()).start();
        // new Thread(new FutureTask<>()).start();
        // new Thread(new FutureTask<>(Callable)).start();
        MyThread thread = new MyThread();
        FutureTask task = new FutureTask(thread);  //适配器类
        new Thread(task,"A");
        Integer o =(Integer) task.get();  //获取callable的返回结果  //会阻塞
        System.out.println(o);

    }
}
//方法返回值就是泛型的值
class MyThread implements Callable<Integer>{

    @Override
    public Integer call() throws Exception {
        System.out.println("11111");
        return 0;
    }
}
```

# 7. 常用的辅助类（必记住）

## 7.1 CountDownLatch（减法计数）

![image-20210624161705855](JUC.assets/image-20210624161705855.png)

```java
package com.heaven;

import java.util.concurrent.CountDownLatch;

public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(6);//6个线程
        for (int i = 0; i < 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" Go out");
                countDownLatch.countDown();  //数量-1
            },String.valueOf(i)).start();
        }
        countDownLatch.await();  //等待计数器归零，然后再向下执行

        System.out.println("Close Door");
    }
}
```

原理：

`countDownLatch.countDown(); `  数量 -1

`countDownLatch.await();`  //等待计数器归零，然后再向下执行

## 7.2 CyclicBarrier（加法计数）

![image-20210624164129125](JUC.assets/image-20210624164129125.png)

```java
package com.heaven;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierDemo {
    /*
    集齐龙珠召唤神龙
     */
    public static void main(String[] args){
        CyclicBarrier cyclicBarrier= new CyclicBarrier(7,()->{
            System.out.println("召唤神龙！");
        });
        for (int i = 1; i <= 7; i++) {
            final int temp=i;
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"收集了"+temp+"颗龙珠");
                try {
                    cyclicBarrier.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```

## 7.3 Semaphore(信号量)

![image-20210624165115354](JUC.assets/image-20210624165115354.png)

抢车位 6车---3个车位

```java
public class SemaphoreDemo {
    public static void main(String[] args) {
        //线程数量：停车位
        Semaphore semaphore = new Semaphore(3);

        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                //acquire() 得到
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"抢到车位");
                    TimeUnit.SECONDS.sleep(2); //停两秒
                    System.out.println(Thread.currentThread().getName()+"离开车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }finally {
                    semaphore.release();//release() 释放
                }

            },String.valueOf(i)).start();
        }
    }
}
```

# 8. 读写锁

![image-20210624185942750](JUC.assets/image-20210624185942750.png)

```java
package com.heaven;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockDemo {
    public static void main(String[] args) {
        Mycache mycache = new Mycache();

        for (int i = 1; i <= 5 ; i++) {
            final int tmep=i;
            new Thread(()->{
                mycache.put(tmep+"",tmep+"");
            },String.valueOf(i)).start();
        }
        for (int i = 1; i <= 5 ; i++) {
            final int tmep=i;
            new Thread(()->{
                mycache.get(tmep+"");
            },String.valueOf(i)).start();
        }
    }
}
class Mycache{
    private volatile Map<String,Object>  map = new HashMap<>();
    private ReadWriteLock lock=new ReentrantReadWriteLock();

    public void put(String key,Object value){
        lock.writeLock().lock();
        try{
            System.out.println(Thread.currentThread().getName()+"写入"+key);
            map.put(key,value);
            System.out.println(Thread.currentThread().getName()+"写入OK");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.writeLock().unlock();
        }

    }
    public void get(String key){
        lock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName()+"读取"+key);
            map.get(key);
            System.out.println(Thread.currentThread().getName()+"读取OK");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.readLock().unlock();
        }
    }
}
```

# 9. 阻塞队列

![image-20210624191445883](JUC.assets/image-20210624191445883.png)

阻塞队列：BlockingQueue

![image-20210624191553939](JUC.assets/image-20210624191553939.png)

多线程和线程池中会使用。

![image-20210624192601788](JUC.assets/image-20210624192601788.png)

**四组API ：**

| 方式     | 抛出异常  | 有返回值 | 阻塞等待           | 超时等待                    |
| -------- | --------- | -------- | ------------------ | --------------------------- |
| 添加     | add()     | offer()  | put() //一直阻塞   | offer(元素，时间，时间单位) |
| 删除     | remove()  | poll()   | take()  //一直阻塞 | poll(时间，时间单位)        |
| 返回队首 | element() | peek()   |                    |                             |

## 同步队列

SynchronousQueue 同步队列，没有容量，只有取出来了才能继续放东西

# 10. 线程池（重点）

线程池：三大方法，7大参数、4种拒绝策略

程序的运行, 本质: 占用系统的资源! 优化资源的使用! =>池化技术
线程池、连接池、内存池、对象池/.....创建和销毁十分浪费资源
**池化技术：**事先准备好一 些资源，有人要用,就来我这里拿,用完之后还给我。

**线程池的好处：**

1、降低资源的消耗

2、提高响应的速度

3、方便管理。

**线程复用、可以控制最大并发数、管理线程**

## 三大方法

![image-20210624194950835](JUC.assets/image-20210624194950835.png)

![image-20210624201611522](JUC.assets/image-20210624201611522.png)

![image-20210624202638947](JUC.assets/image-20210624202638947.png)

底层

![image-20210624202841196](JUC.assets/image-20210624202841196.png)

![image-20210624205550666](JUC.assets/image-20210624205550666.png)

创建线程池

```java
package com.heaven;

import java.util.concurrent.*;

/**
 * new ThreadPoolExecutor.AbortPolicy()   //满了之后，还有人进来，不处理，抛出异常
 * new ThreadPoolExecutor.CallerRunsPolicy()  //哪里来的去哪里执行
 * new ThreadPoolExecutor.DiscardPolicy()  //不会抛出异常，丢掉任务
 * new ThreadPoolExecutor.DiscardOldestPolicy() //尝试竞争第一个，丢弃任务，不会抛出任务
 */
public class Test1 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService= new ThreadPoolExecutor(
                2,
                Runtime.getRuntime().availableProcessors(),
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy());
        try {
            //最大承载队列+最大值
            for (int i = 0; i <=10; i++) {
                executorService.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + "OK");
                });
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            executorService.shutdown();
        }

    }
}
```

## 最大的线程数量应该怎么定义

### CPU 密集型

CPU是几核的就定义为几，可以保持CPU的最高效率。

`Runtime.getRuntime().availableProcessors()`获取CPU的核数

### IO密集型

判断程序中有多少十分消耗IO资源的线程，要求大于这个IO线程的数量]b

# 11. 四大函数式接口（必须）

新时代程序员：lambda表达式、链式编程、函数式接口、Stream流式计算

函数式接口：只有一个方法的接口

```java
@FunctionalInterface
public interface Runnable {

    public abstract void run();
}
//超级多FunctionalInterface
//简化编程模型，在新版本的框架底层大量应用!

```

![image-20210624214044238](JUC.assets/image-20210624214044238.png)

> Function：函数型接口

![image-20210624214319915](JUC.assets/image-20210624214319915.png)

![image-20210624214327316](JUC.assets/image-20210624214327316.png)

> Predicate: 断定型接口

![image-20210624214600727](JUC.assets/image-20210624214600727.png)

![image-20210624214540423](JUC.assets/image-20210624214540423.png)

> Consumer：消费型接口

![image-20210624214747381](JUC.assets/image-20210624214747381.png)

![image-20210624214850176](JUC.assets/image-20210624214850176.png)

> supplier: 供给型接口

![image-20210624214921559](JUC.assets/image-20210624214921559.png)

![image-20210624215019222](JUC.assets/image-20210624215019222.png)

# 12 Stream流式计算

> 什么是流式计算

大数据：存储+计算

集合、MySql本质就是存东西。

计算都应该交给流来操作

![image-20210624215834818](JUC.assets/image-20210624215834818.png)

# 13 ForkJoin

分支合并，Forkjoin在JDK 1.7. 并行执行任务!提高效率。大数据量!

![image-20210624220128838](JUC.assets/image-20210624220128838.png)

特点：工作窃取

工作窃取: 执行更快的子任务可以去拿执行得慢的子任务去执行，因为维护的都是双端队列

![image-20210624220227163](JUC.assets/image-20210624220227163.png)

# 异步回调

Future设计的初衷: 对将来的某个事件的结果进行建模

```java
package com.heaven;

import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.TimeUnit;

/**
 * 异步调用：Ajax,CompletableFuture
 * 异步执行
 * 成功回调
 * 失败回调
 */
public class AsynDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 没有返回的 runAsync 回调
        // CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(()->{
        //     try {
        //         TimeUnit.SECONDS.sleep(2);
        //     } catch (InterruptedException e) {
        //         e.printStackTrace();
        //     }
        //     System.out.println(Thread.currentThread().getName()+"runAsync=>Void");
        // });
        // System.out.println("1111111111111111");
        // System.out.println(completableFuture.get());

        //有返回值的
        //ajax 成功和失败的回调
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread().getName()+"supplyAsync=>Integer");
            int i=1/0;
            return 1024;
        });
        System.out.println(completableFuture.whenComplete((t, u) -> {
            System.out.println("t=>" + t);  //返回结果
            System.out.println("u=>" + u);  //错误信息 java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
        }).exceptionally((e) -> {
            System.out.println(e.getMessage());
            return 233;  //可以获取到错误的返回结果
        }).get());
    }
}

```

# JMM

**JMM**：用于屏蔽掉各种硬件和操作系统的内存访问差异，以实现让Java程序在各种平台下都能达到一致的并发效果，**JMM规范了Java虚拟机与计算机内存是如何协同工作的**：规定了一个线程如何和何时可以看到由其他线程修改过后的共享变量的值，以及在必须时如何同步的访问共享变量。

JMM详解：**https://zhuanlan.zhihu.com/p/29881777**

总线嗅探机制：线程和主存之间有一条总线。加入关键字后开启协议，监听线程a中的共享变量（MESI协议是一个基于失效的缓存一致性协议）

**关于JMM的一些同步的约定:**

1、线程解锁前,必须把共享变量**立刻刷回主存**。

2、线程加锁前,必须读取主存中的最新值到工作内存中!

3、加锁和解锁是同一把锁

**8种操作**：

![image-20210625092953849](JUC.assets/image-20210625092953849.png)

**read（读取）**：作用于主内存变量，把一个变量值从主内存传输到线程的工作内存中，以便随后的load动作使用

**load（载入）**：作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。

**use（使用）**：作用于工作内存的变量，把工作内存中的一个变量值传递给执行引擎，每当虚拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。

**assign（赋值）**：作用于工作内存的变量，它把一个从执行引擎接收到的值赋值给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。

**store（存储）**：作用于工作内存的变量，把工作内存中的一个变量的值传送到主内存中，以便随后的write的操作。

**write（写入）**：作用于主内存的变量，它把store操作从工作内存中一个变量的值传送到主内存的变量中。

**lock（锁定）**：作用于主内存的变量，把一个变量标识为一条线程独占状态。

**unlock（解锁）**：作用于主内存变量，把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。

# volatile

volatile是Java虚拟机提供轻量级的同步机制

1、保证可见性 (JMM)

**2、不保证原子性**（使用原子类 AtomicInteger）

3、禁止指令重排

​    指令重拍：计算机并不是按源代码的顺序执行，因为存在编译器优化，指令并行重排，内存系统重排。前提是符合数据之间的依赖性。

内存屏障。CPU指令。作用:
1、保证特定的操作的执行顺序!
2、可以保证某些变量的内存可见性( 利用这些特性volatile实现了可见性)

![image-20210625100327008](JUC.assets/image-20210625100327008.png)

# CAS(Compare and Swap)

![image-20210625101959949](JUC.assets/image-20210625101959949.png)

![image-20210625102137039](JUC.assets/image-20210625102137039.png)

CAS :比较当前工作内存中的值（期望值）和主内存中的值,如果这个值是期望的,那么则执行操作!如果不是就一 直循环!

ABA问题：

![image-20210625102740763](JUC.assets/image-20210625102740763.png)

![image-20210625103040474](JUC.assets/image-20210625103040474.png)

**原子引用`AtomicReference`（解决ABA）：**带版本号的原子操作

![image-20210625104156484](JUC.assets/image-20210625104156484.png)

# 各种锁

## 公平锁、非公平锁

公平锁: 非常公平，不能够插队,必须先来后到!

非公平锁: 非常不公平, 可以插队(默认都是非公平)

## 可重入锁

递归锁

## 自旋锁（spinlock）

![image-20210625104643250](JUC.assets/image-20210625104643250.png)

