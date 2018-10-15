---
layout:  post
title: synchronized原理
category: blog
description: 
---


## syncronized

JMM关于synchronized的两条规定：  
　　1）线程解锁前，必须把共享变量的最新值刷新到主内存中  
　　2）线程加锁时，将清空工作内存中共享变量的值，从而使用共享变量时需要从主内存中重新获取最新的值  
　　　（注意：加锁与解锁需要是同一把锁）  


### 加锁

* 方法 public synchronized void demo1(){};

实例锁，，同一个实例等待锁释放  
测试代码  
![image](https://user-images.githubusercontent.com/26774647/46900460-11652f80-ced5-11e8-987c-0babb41b11d2.png)
测试结果，使用屏障等待其他线程一起到达屏障点，只能有一个线程到达  
![image](https://user-images.githubusercontent.com/26774647/46900478-5be6ac00-ced5-11e8-8019-6f86d9f6f95d.png)


* 实例 synchronized(this){};

实例锁，，同一个实例等待锁释放  
测试方式与方法的一样  

* 类 synchronized(Class.class){};

全局锁，不管是同一个实例，还是所有其他实例对象都不能操作

* 对象 synchronized(obj){};

对象锁，看这个对象的位置，唯一就是全局锁，类中的成员对象就是实例锁
