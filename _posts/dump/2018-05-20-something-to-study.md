---
layout:  post
title: 一些学习了解的
category: dump
description: 以后使用
---


1、规则引擎（Drools）

2、Jmeter 测试的使用

3、finalize()方法  
与析构函数类似，Java中没有析构函数。在对象结束，调用gc时候调用，可以重载方法

4、单例模式的Double-checked Locking (DCL)  
用来在lazy initialisation 的单例模式中避免同步开销
```
public class Singleton {
    private Singleton(){
    }
     private static class SingletonHolder {
     static Singleton instance = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
    public static void main(String [] args)
    {
        Singleton.getInstance();
    }
}
```
在该方法中，Singleton有一个静态内部类，内部类在外部类加载的时候并不会加载，只有在调用getInstance方法的时候加载SingletonHolder类

