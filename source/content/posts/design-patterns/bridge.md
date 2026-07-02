---
title: "桥接模式 (Bridge Pattern)"
description: "将抽象部分与实现部分分离，使它们都可以独立变化"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - bridge
---

## 意图

将抽象部分与它的实现部分分离，使它们都可以独立地变化。

## 类图

```
┌──────────────────────┐       ┌──────────────────────┐
│    Abstraction       │       │    Implementor       │
├──────────────────────┤       │  (interface)         │
│ -impl: Implementor───┼──────▶├──────────────────────┤
│ +operation()         │       │ +operationImpl()     │
└──────────┬───────────┘       └──────────┬───────────┘
           ▲                              ▲ implements
           │ extends                      │
┌──────────┴───────────┐       ┌──────────┴───────────┐
│ RefinedAbstraction   │       │   ConcreteImplA      │
├──────────────────────┤       ├──────────────────────┤
│                      │       │ +operationImpl()     │
└──────────────────────┘       └──────────────────────┘
```

## Java 实现

```java
// Implementor
interface DrawApi {
    void drawCircle(int x, int y, int radius);
}

class RedCircle implements DrawApi {
    @Override
    public void drawCircle(int x, int y, int radius) {
        System.out.println("Red circle at (" + x + "," + y + ") r=" + radius);
    }
}

class GreenCircle implements DrawApi {
    @Override
    public void drawCircle(int x, int y, int radius) {
        System.out.println("Green circle at (" + x + "," + y + ") r=" + radius);
    }
}

// Abstraction
abstract class Shape {
    protected DrawApi drawApi;

    protected Shape(DrawApi drawApi) {
        this.drawApi = drawApi;
    }

    public abstract void draw();
}

class Circle extends Shape {
    private int x, y, radius;

    public Circle(int x, int y, int radius, DrawApi drawApi) {
        super(drawApi);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    @Override
    public void draw() {
        drawApi.drawCircle(x, y, radius);
    }
}

public class BridgeDemo {
    public static void main(String[] args) {
        Shape redCircle = new Circle(10, 20, 5, new RedCircle());
        Shape greenCircle = new Circle(30, 40, 8, new GreenCircle());
        redCircle.draw();
        greenCircle.draw();
    }
}
```

## 关键点

- 用组合替代继承，解耦抽象与实现
- 两大维度可独立扩展
- 避免类爆炸

## 使用场景

- 多个维度变化的场景（形状 × 颜色、消息类型 × 发送渠道）
- 不希望抽象与实现永久绑定时
