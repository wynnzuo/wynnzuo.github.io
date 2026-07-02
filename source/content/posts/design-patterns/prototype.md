---
title: "原型模式 (Prototype Pattern)"
description: "通过复制现有对象来创建新对象，避免重复的初始化操作"
date: 2026-07-02
draft: false
tags:
  - 设计模式
---

## 意图

用原型实例指定创建对象的种类，并通过复制这些原型创建新的对象，避免昂贵的创建操作。

## 类图

```
┌──────────────────────┐
│      Prototype       │
│  (interface)         │
├──────────────────────┤
│ +clone(): Prototype  │◀──── returns copy
└──────────┬───────────┘
           │ implements
┌──────────▼───────────┐
│   ConcretePrototype  │
├──────────────────────┤
│ -field: String       │
├──────────────────────┤
│ +clone()             │
│   └─ returns copy    │
└──────────────────────┘

Client ───▶ uses ───▶ Prototype
```

## Java 实现

```java
// Prototype interface
abstract class Shape implements Cloneable {
    protected String type;
    protected String color;

    public abstract void draw();

    @Override
    public Shape clone() {
        try {
            return (Shape) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}

class Circle extends Shape {
    public Circle(String color) {
        this.type = "Circle";
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("Drawing " + color + " " + type);
    }
}

class Rectangle extends Shape {
    public Rectangle(String color) {
        this.type = "Rectangle";
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.println("Drawing " + color + " " + type);
    }
}

// Registry
class ShapeRegistry {
    private Map<String, Shape> cache = new HashMap<>();

    public ShapeRegistry() {
        cache.put("redCircle", new Circle("Red"));
        cache.put("blueRect", new Rectangle("Blue"));
    }

    public Shape getShape(String key) {
        return cache.get(key).clone();
    }
}

public class PrototypeDemo {
    public static void main(String[] args) {
        ShapeRegistry registry = new ShapeRegistry();
        Shape s1 = registry.getShape("redCircle");
        Shape s2 = registry.getShape("redCircle");
        s1.draw();
        System.out.println(s1 != s2); // true, different objects
    }
}
```

## 关键点

- 避免重复初始化开销，直接克隆已有对象
- Java 中需实现 Cloneable 接口
- 深拷贝与浅拷贝的选择需注意引用类型字段

## 使用场景

- 对象创建成本高（数据库查询、网络请求）
- 需要大量相似对象
- 需要保存对象状态的副本（快照）
