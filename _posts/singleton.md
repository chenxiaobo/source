---
post_url: design-pattern
title: 单例模式
date: 2019-11-01 20:26:42
categories:
- 设计模式
tags:
- 单例
---

单例模式是常见的设计模式之一，是比较简单的一种设计模式。所谓单例模式，就是在整个应用系统中只有一个实例，在外部不能实例化，可以直接访问对象。

## 饿汉式
```java
public class Singleton {

    private static final Singleton singleton = new Singleton();

    private Singleton(){}

    public static Singleton getInstance(){
        return singleton;
    }

}
```
饿汉式是一种空间换时间的方式，类加载的时候就会创建实例，每次调用的时候直接获取实例，节省运行时间。
但是饿汉式资源利用率不高，如果getInstance一直没有运行，但是如果加载了这个类，那这个实例依然会进行初始化。

## 懒汉式
既然饿汉式可能造成资源浪费，我们就在需要调用获取这个类的实例的时候进行初始化，所以，我们就可以用下面的方式来创建实例。
```java
public class Singleton {

    private static Singleton singleton = null;

    private Singleton(){}

    public static Singleton getInstance(){
        if (singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }

}
```
上面的方式是在获取实例的时候判断对象是否为空，如果为空的话才进行实例化。这段代码在单线程下完全没有问题，但是没有加锁，多线程下可能会造成单例失败。

## 懒汉式-线程安全
```java
public class Singleton {

    private static Singleton singleton = null;

    private Singleton(){}

    public static synchronized Singleton getInstance(){
        if (singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }

}
```
上面这种方式用synchronized进行加锁，这种方式确保了单例。如果有一个线程需要获取实例，必须要等到前面的线程执行完才能获取，保证了单例。
这种方式的缺点就是效率太差了，如果我们有很多线程去同时获取这个实例，那么性能问题会越发严重。

## 双重校验锁
```java
public class Singleton {

    private static Singleton singleton = null;

    private Singleton(){}

    public static Singleton getInstance(){
        if (singleton == null){
            synchronized (Singleton.class){
                if (singleton == null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }

}
```
双重锁在保证了单例的同时，又能在多线程模式下保证高性能。

