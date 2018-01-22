---
title: Java 设计模式-单例模式(Singleton)
date: 2018-01-22 11:23:50
tags: [java, design mode]
---

单例设计模式是Java中应用很广泛的设计模式之一, 保证了一个类始终有且只有一个对象, 具体特点有:
* 构造函数私有化(private)  -  其它的类不能实例化创建该对象
* 引用时私有  -  外部不能修改该完成实例化的该类对象, 没有`setInstance()`方法
* `public static` 方法是获取类对象的唯一方式

<!-- more -->

单例模式的几种不同实现: 

### 饿汉模式
```java
public class Singleton {

  private static final Singleton instance = new Singleton();

  private Singleton() {}

  public Singleton getInstance() {
    return instance;
  }
}
```
优点: 
  * 多线程安全

缺点: 
  * 加载类时就初始化完成,无法延时加载

### 懒汉模式
```java
public class Singleton {

  private static Singleton instance;

  private Singleton() {}

  public static Singleton getInstance() {
    if (instance == null) {
      instance = new Singleton();
    }
    return instance;
  }
}
```
优点: 
  * 可以延迟加载

缺点: 
  * 多线程不安全

### 双重检查
```java
public class Singleton {

  private static Singleton instance;

  private Singleton() {}

  public static Singleton getInstance() {
    if (instance == null) {
      synchronized (Singleton.class) {
        if (instance == null) {
          instance = new Singleton();
        }
      }
    }
    return instance;
  }
}
```
优点: 
  * 线程安全
  * 延迟加载

缺点: 
  * 同步耗时

### 静态内部类
```java
public class Singleton {
  
  private Singleton() {}

  public static Singleton getInstance() {
    return SingletonHolder.instance;
  }

  private static class SingletonHolder {
    private static final Singleton instance = new Singleton();
  }
}
```
优点: 
  * 线程安全
  * 延迟加载
  * 耗时短(与双重检查相比)

### 用缓存实现
```java
public class Singleton {

  private static final String KEY = "InstanceKey";

  private static Map<String, Singleton> map = new HashMap<>();

  private Singleton() {}

  public static Singleton getInstance() {
    Singleton instance;
    if (map.get(KEY) == null) {
      instance = new Singleton();
      map.put(KEY, instance);
    }
    else {
      instance = map.get(KEY);
    }
    return instance;
  }
}
```
优点:
  * 线程安全

缺点: 
  * 占用内存较大

### 枚举模式
```java
public enum Singleton {
  
  INSTANCE;

  public void operate() {}
}
```
优点: 
  * 简洁

缺点: 
  * 占用内存大

参考[Java设计模式---单例模式 - DevWiki](https://segmentfault.com/a/1190000004502323)