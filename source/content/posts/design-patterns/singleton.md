---
title: "单例模式 (Singleton Pattern)"
description: "确保一个类只有一个实例，并提供一个全局访问点"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - singleton
---

## 意图

确保一个类只有一个实例，并提供一个全局访问点。

## 类图

```
┌──────────────────────┐
│      Singleton       │
├──────────────────────┤
│ -instance: Singleton │
├──────────────────────┤
│ +getInstance()       │
│ ◀── returns itself   │
└──────────────────────┘
```

## Java 实现

```java
public class Singleton {
    private static volatile Singleton instance;

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

    public void showMessage() {
        System.out.println("Hello from Singleton!");
    }
}
```

## 关键点

- 私有构造器，防止外部实例化
- 静态方法提供全局访问点
- 双重检查锁（DCL）保证线程安全

## 使用场景

- 日志记录器、数据库连接池、配置管理器
- 需要严格控制全局唯一实例的场景
