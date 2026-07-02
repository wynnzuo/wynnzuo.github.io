---
title: "观察者模式 (Observer Pattern)"
description: "定义对象之间的一对多依赖关系，当一个对象状态改变时，所有依赖者都会收到通知并自动更新"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - observer
---

## 意图

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

## 类图

```
┌──────────────────────┐          ┌──────────────────────┐
│       Subject        │          │       Observer       │
│  (interface/class)   │          │  (interface)         │
├──────────────────────┤          ├──────────────────────┤
│ -observers: List     │          │ +update()            │
│ +attach(observer)    │          └──────────────────────┘
│ +detach(observer)    │                    ▲
│ +notifyAll()─────────┼──► notifies        │
└──────────────────────┘      ┌─────────────┴─────────────┐
                              │                           │
                    ┌─────────┴──────┐          ┌─────────┴──────┐
                    │ ConcreteObsA   │          │ ConcreteObsB   │
                    ├────────────────┤          ├────────────────┤
                    │ +update()      │          │ +update()      │
                    └────────────────┘          └────────────────┘
```

## Java 实现

```java
import java.util.*;

// Observer
interface Observer {
    void update(String stockName, double price);
}

// Subject
class StockMarket {
    private Map<String, Double> stocks = new HashMap<>();
    private List<Observer> observers = new ArrayList<>();

    public void attach(Observer observer) { observers.add(observer); }
    public void detach(Observer observer) { observers.remove(observer); }

    public void setPrice(String stockName, double price) {
        stocks.put(stockName, price);
        notifyAllObservers(stockName, price);
    }

    private void notifyAllObservers(String stockName, double price) {
        for (Observer observer : observers) {
            observer.update(stockName, price);
        }
    }
}

// Concrete Observers
class MobileApp implements Observer {
    private String name;

    public MobileApp(String name) { this.name = name; }

    @Override
    public void update(String stockName, double price) {
        System.out.println(name + " received: " + stockName + " = $" + price);
    }
}

class WebDashboard implements Observer {
    @Override
    public void update(String stockName, double price) {
        System.out.println("Web Dashboard updated: " + stockName + " → " + price);
    }
}

public class ObserverDemo {
    public static void main(String[] args) {
        StockMarket market = new StockMarket();

        market.attach(new MobileApp("App-1"));
        market.attach(new WebDashboard());

        market.setPrice("AAPL", 175.50);
        market.setPrice("GOOGL", 2850.00);
    }
}
```

## 关键点

- Subject 维护观察者列表，状态变更时广播通知
- 观察者与 Subject 松耦合
- Java 标准库已有 Observable（已废弃）和 PropertyChangeListener

## 使用场景

- 事件驱动系统、消息推送
- MVC 架构中 Model 通知 View 更新
- 响应式编程（RxJava、Spring Reactor）
