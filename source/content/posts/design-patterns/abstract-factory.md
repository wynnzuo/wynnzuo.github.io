---
title: "抽象工厂模式 (Abstract Factory Pattern)"
description: "提供一个创建一系列相关或相互依赖对象的接口，而无需指定具体类"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 创建型
---

## 意图

提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

## 类图

```
┌──────────────────────┐      ┌──────────────────────┐
│   AbstractFactory    │      │ AbstractProductA     │
│  (interface)         │      │ (interface)          │
├──────────────────────┤      └──────────────────────┘
│ +createProductA()────┼─────▶        ▲
│ +createProductB()────┼──┐          │ implements
└──────────┬───────────┘  │  ┌────────┴──────────┐
           │ implements   │  │   ProductA1       │
┌──────────▼───────────┐  │  ├───────────────────┤
│    WinFactory        │  │  │                   │
├──────────────────────┤  │  └───────────────────┘
│ +createProductA()    │──┘
│ +createProductB()    │──┐  ┌──────────────────────┐
└──────────────────────┘  │  │ AbstractProductB     │
                          └─▶│ (interface)          │
┌──────────────────────┐     └──────────────────────┘
│    MacFactory        │              ▲
├──────────────────────┤           implements
│ +createProductA()    │     ┌────────┴──────────┐
│ +createProductB()    │     │   ProductB1       │
└──────────────────────┘     └───────────────────┘
```

## Java 实现

```java
// Abstract products
interface Button {
    void render();
}

interface Checkbox {
    void check();
}

// Concrete products
class WinButton implements Button {
    public void render() { System.out.println("WinButton"); }
}

class WinCheckbox implements Checkbox {
    public void check() { System.out.println("WinCheckbox"); }
}

class MacButton implements Button {
    public void render() { System.out.println("MacButton"); }
}

class MacCheckbox implements Checkbox {
    public void check() { System.out.println("MacCheckbox"); }
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

// Client
public class AbstractFactoryDemo {
    public static void main(String[] args) {
        GUIFactory factory = new WinFactory();
        Button btn = factory.createButton();
        Checkbox cb = factory.createCheckbox();
        btn.render();
        cb.check();
    }
}
```

## 关键点

- 创建产品族而非单一产品
- 新增产品族容易，新增产品种类困难
- 常用于跨平台 UI 工具包
