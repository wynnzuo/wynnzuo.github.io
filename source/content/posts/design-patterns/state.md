---
title: "状态模式 (State Pattern)"
description: "允许对象在内部状态改变时改变其行为，使对象看起来就像修改了它的类"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 行为型
---

## 意图

允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。

## 类图

```
┌──────────────┐     delegates    ┌──────────────────────┐
│   Context    │─────────────────▶│        State          │
├──────────────┤                  │  (interface)          │
│ -state: State│                  ├──────────────────────┤
│ request()────┼──► state.handle()│ +handle(context)     │
│ setState(s)  │                  └──────────────────────┘
└──────────────┘                            ▲
                                            │ implements
               ┌────────────────────────────┼────────────────────────┐
               │                            │                        │
    ┌──────────┴──────────┐      ┌──────────┴──────────┐   ┌────────┴──────────┐
    │     ReadyState      │      │    HasCoinState     │   │  DispensingState  │
    ├─────────────────────┤      ├─────────────────────┤   ├───────────────────┤
    │ +handle(context)    │      │ +handle(context)    │   │ +handle(context)  │
    │   insertCoin()      │      │   selectProduct()   │   │   dispense()      │
    └─────────────────────┘      └─────────────────────┘   └───────────────────┘
```

## Java 实现

```java
// State
interface VendingMachineState {
    void insertCoin();
    void selectProduct();
    void dispense();
}

// Concrete States
class ReadyState implements VendingMachineState {
    private VendingMachine machine;

    public ReadyState(VendingMachine machine) {
        this.machine = machine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Coin inserted");
        machine.setState(new HasCoinState(machine));
    }

    @Override
    public void selectProduct() {
        System.out.println("Insert coin first");
    }

    @Override
    public void dispense() {
        System.out.println("Insert coin first");
    }
}

class HasCoinState implements VendingMachineState {
    private VendingMachine machine;

    public HasCoinState(VendingMachine machine) {
        this.machine = machine;
    }

    @Override
    public void insertCoin() {
        System.out.println("Coin already inserted");
    }

    @Override
    public void selectProduct() {
        System.out.println("Product selected");
        machine.setState(new DispensingState(machine));
    }

    @Override
    public void dispense() {
        System.out.println("Select product first");
    }
}

class DispensingState implements VendingMachineState {
    private VendingMachine machine;

    public DispensingState(VendingMachine machine) {
        this.machine = machine;
    }

    @Override
    public void insertCoin() { System.out.println("Please wait"); }

    @Override
    public void selectProduct() { System.out.println("Please wait"); }

    @Override
    public void dispense() {
        System.out.println("Dispensing product...");
        machine.setState(new ReadyState(machine));
    }
}

// Context
class VendingMachine {
    private VendingMachineState state;

    public VendingMachine() {
        state = new ReadyState(this);
    }

    public void setState(VendingMachineState state) {
        this.state = state;
    }

    public void insertCoin()    { state.insertCoin(); }
    public void selectProduct() { state.selectProduct(); }
    public void dispense()      { state.dispense(); }
}

public class StateDemo {
    public static void main(String[] args) {
        VendingMachine machine = new VendingMachine();

        machine.selectProduct(); // Insert coin first
        machine.insertCoin();    // Coin inserted
        machine.insertCoin();    // Coin already inserted
        machine.selectProduct(); // Product selected
        machine.dispense();      // Dispensing product...
    }
}
```

## 关键点

- 每个状态封装了对应的行为，状态切换清晰
- 消除了大量的 if-else/switch 条件判断
- 状态对象可在多个上下文间共享（无状态时）

## 使用场景

- 工作流引擎、订单状态流转
- TCP 连接状态、游戏角色状态
