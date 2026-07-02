---
title: "建造者模式 (Builder Pattern)"
description: "将复杂对象的构建过程与表示分离，使相同的构建过程可以创建不同的表示"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 创建型
---

## 意图

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

## 类图

```
┌──────────────┐     uses     ┌──────────────────────┐
│   Director   │─────────────▶│       Builder        │
├──────────────┤              │  (interface)         │
│ +construct() │              ├──────────────────────┤
└──────────────┘              │ +buildStepA()        │
                              │ +buildStepB()        │
                              │ +getResult()         │
                              └──────────┬───────────┘
                                         │ implements
                              ┌──────────┴───────────┐
                              │  ConcreteBuilder     │
                              ├──────────────────────┤
                              │ +buildStepA()        │
                              │ +buildStepB()        │
                              │ +getResult() ───────▶ Product
                              └──────────────────────┘
```

## Java 实现

```java
// Product
class Pizza {
    private String dough;
    private String sauce;
    private String topping;

    public void setDough(String dough) { this.dough = dough; }
    public void setSauce(String sauce) { this.sauce = sauce; }
    public void setTopping(String topping) { this.topping = topping; }

    public void show() {
        System.out.println("Pizza [" + dough + ", " + sauce + ", " + topping + "]");
    }
}

// Builder
interface PizzaBuilder {
    void buildDough();
    void buildSauce();
    void buildTopping();
    Pizza getPizza();
}

class HawaiianPizzaBuilder implements PizzaBuilder {
    private Pizza pizza = new Pizza();
    public void buildDough()    { pizza.setDough("thin"); }
    public void buildSauce()    { pizza.setSauce("tomato"); }
    public void buildTopping()  { pizza.setTopping("ham+pineapple"); }
    public Pizza getPizza()     { return pizza; }
}

class SpicyPizzaBuilder implements PizzaBuilder {
    private Pizza pizza = new Pizza();
    public void buildDough()    { pizza.setDough("thick"); }
    public void buildSauce()    { pizza.setSauce("chili"); }
    public void buildTopping()  { pizza.setTopping("pepperoni"); }
    public Pizza getPizza()     { return pizza; }
}

// Director
class Waiter {
    private PizzaBuilder builder;
    public void setBuilder(PizzaBuilder b) { builder = b; }
    public Pizza construct() {
        builder.buildDough();
        builder.buildSauce();
        builder.buildTopping();
        return builder.getPizza();
    }
}

public class BuilderDemo {
    public static void main(String[] args) {
        Waiter waiter = new Waiter();
        waiter.setBuilder(new HawaiianPizzaBuilder());
        Pizza pizza = waiter.construct();
        pizza.show();
    }
}
```

## 关键点

- Director 控制构建过程顺序，Builder 提供构建步骤
- 相同构建过程可产生不同产品表示
- 与 Abstract Factory 不同：Builder 侧重分步构建复杂对象

## 使用场景

- 对象构造过程固定但需要多种表示
- 如 SQL 查询构建器、菜单构建器
