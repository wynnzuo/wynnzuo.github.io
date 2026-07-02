---
title: "享元模式 (Flyweight Pattern)"
description: "通过共享对象来减少内存使用，支持大量细粒度对象的复用"
date: 2026-07-02
draft: false
tags:
  - 设计模式
---

## 意图

运用共享技术有效地支持大量细粒度的对象复用，减少内存开销。

## 类图

```
┌──────────────────────┐
│  FlyweightFactory    │
├──────────────────────┤
│ -pool: Map           │──── manages ────┐
│ +getFlyweight(key)   │                │
└──────────────────────┘                │
                          ┌─────────────▼──────────────┐
                          │     Flyweight              │
 Client ──► getFlyweight │  (interface)               │
   ──► reuse or create   ├────────────────────────────┤
                          │ +operation(extrinsicState) │
                          └──────────────┬─────────────┘
                                         │
                          ┌──────────────▼─────────────┐
                          │   ConcreteFlyweight        │
                          ├────────────────────────────┤
                          │ -intrinsicState: shared    │
                          │ +operation(extrinsicState) │
                          └────────────────────────────┘
```

## Java 实现

```java
import java.util.*;

// Flyweight
interface Tree {
    void display(int x, int y);
}

// ConcreteFlyweight (shared)
class TreeType implements Tree {
    private String name;      // intrinsic
    private String color;     // intrinsic
    private String texture;   // intrinsic

    public TreeType(String name, String color, String texture) {
        this.name = name;
        this.color = color;
        this.texture = texture;
    }

    @Override
    public void display(int x, int y) {  // extrinsic
        System.out.println(name + " tree at (" + x + "," + y + ")");
    }
}

// FlyweightFactory
class TreeFactory {
    private static Map<String, TreeType> types = new HashMap<>();

    public static TreeType getTreeType(String name, String color, String texture) {
        String key = name + "-" + color;
        return types.computeIfAbsent(key, k -> new TreeType(name, color, texture));
    }
}

// Client
class Forest {
    private List<Tree> trees = new ArrayList<>();

    public void plantTree(int x, int y, String name, String color, String texture) {
        TreeType type = TreeFactory.getTreeType(name, color, texture);
        trees.add(type); // type is shared
        System.out.println("Planted at (" + x + "," + y + ")");
    }
}

public class FlyweightDemo {
    public static void main(String[] args) {
        Forest forest = new Forest();
        for (int i = 0; i < 10000; i++) {
            forest.plantTree(i, i % 100, "Oak", "Green", "OakTexture");
        }
        // Only 1 TreeType object created for 10000 "Oak,Green" trees!
    }
}
```

## 关键点

- 内部状态（intrinsic）可共享，外部状态（extrinsic）由客户端维护
- 工厂负责管理和复用享元对象
- 大幅减少对象数量，降低内存

## 使用场景

- 大量相似对象的场景（字符渲染、粒子系统）
- String 常量池、Integer 缓存
