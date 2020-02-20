[TOC]
---
Java锁简介
---

synchronized原理分析

https://www.jianshu.com/p/e62fa839aa41

JVM源码分析之synchronized实现

https://www.jianshu.com/p/c5058b6fe8e5

HotSpot Synchronization

https://wiki.openjdk.java.net/display/HotSpot/Synchronization

Java 中的偏向锁、轻量级锁和重量级锁

https://jacobchang.cn/lock-of-synchronized.html

java 中的锁 -- 偏向锁、轻量级锁、自旋锁、重量级锁

https://blog.csdn.net/zqz_zqz/article/details/70233767

### 1.是否锁住同步资源

#### 1.1.悲观锁

* `sychronized` 关键字
* 大部分实现了`java.util.concurrent.locks.Lock`接口的锁类

> 重量级锁是悲观锁

适用于写多读少的场景

#### 1.2.乐观锁

适用于度多写少的场景

`CAS`算法实现的锁都是乐观锁，包括（偏量锁及轻量级锁）

> 大部分CAS算法均使用了com.sun.misc.Unsafe.compareAndSwap*方法
>
> 如java.util.concurrent中的原子类

```java
// CASE实现
```



### 2.获取锁失败，线程不阻塞

####2.1.自旋锁

```java
//伪码
int maxLoopCount=10;//-XX:PreBlockSpin=maxLoopCount
boolean result=false;
while(maxLoopCount>0){
  result=compareAndUpdate(args);
  if(!result) {
    maxLoopCount--;
    continue
  }
}
//自旋失败
if(!result){
  lockObj.wait();
}
```



#### 2.2.适应性自旋锁

自适应意味着自旋的时间（次数）不再固定，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。如果在同一个锁对象上，自旋等待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也是很有可能再次成功，进而它将允许自旋等待持续相对更长的时间。如果对于某个锁，自旋很少成功获得过，那在以后尝试获取这个锁时将可能省略掉自旋过程，直接阻塞线程，避免浪费处理器资源。

> 其他自旋锁
>
> * TicketLock
> * CLHlock
> * MCSlock

### 3.多个线程竞争同步资源(synchronized)

指锁的状态

#### 3.1.无锁

#### 3.2.偏向锁

#### 3.3.轻量级锁

####3.4.重量级锁

### 4.多线程竞争锁是否排队

#### 4.1.公平锁

#### 4.2.非公平锁

### 5.同一线程是否可多次获取同一锁

####5.1.可重入锁

#### 5.2.非可重入锁

### 6.多线程共享一个锁

#### 6.1.共享锁

#### 6.2.排它锁

