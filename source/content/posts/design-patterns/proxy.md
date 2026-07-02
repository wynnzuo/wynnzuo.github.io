---
title: "代理模式 (Proxy Pattern)"
description: "为另一个对象提供一个替身或占位符以控制对它的访问"
date: 2026-07-02
draft: false
tags:
  - 设计模式
---

## 意图

为其他对象提供一种代理以控制对这个对象的访问。代理可以在不改变目标对象的情况下增加额外功能。

## 类图

```
┌──────────────┐
│   Client     │
└──────┬───────┘
       │ uses
┌──────▼───────┐      ┌──────────────────────┐
│   Subject    │      │     RealSubject      │
│  (interface) │      ├──────────────────────┤
├──────────────┤      │ +request()           │
│ +request()   │      └──────────────────────┘
└──────┬───────┘              ▲
       │ implements          │ delegates
┌──────▼───────┐      ┌──────┴───────────┐
│    Proxy     │      │ controls access  │
├──────────────┤──────▶                  │
│ -realSubject │      │ lazy init / log  │
│ +request()   │      │ / cache          │
└──────────────┘      └──────────────────┘
```

## Java 实现

```java
// Subject
interface Image {
    void display();
}

// RealSubject
class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading: " + fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying: " + fileName);
    }
}

// Proxy
class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName); // lazy loading
        }
        realImage.display();
    }
}

public class ProxyDemo {
    public static void main(String[] args) {
        Image image = new ProxyImage("photo.jpg");

        // Image not loaded yet
        System.out.println("--- First display ---");
        image.display(); // loads and displays

        System.out.println("--- Second display ---");
        image.display(); // only displays (no loading)
    }
}
```

## 关键点

- 代理与真实对象实现相同接口
- 可控制访问、延迟加载、日志、缓存等
- JDK 动态代理和 CGLIB 是 Java 常见实现

## 使用场景

- 延迟加载（虚拟代理）
- 访问控制（保护代理）
- 日志记录（日志代理）
- AOP（面向切面编程）
