---
title: "责任链模式 (Chain of Responsibility Pattern)"
description: "将请求的发送者和接收者解耦，使多个对象都有机会处理请求，形成一条处理链"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - chain-of-responsibility
---

## 意图

使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止。

## 类图

```
                         ┌──────────────────────────┐
 Client ───► request ───▶│        Handler           │
                         │  (abstract)              │
                         ├──────────────────────────┤
                         │ #next: Handler           │
                         │ +setNext()                │
                         │ +handle()                 │
                         └──────────┬───────────────┘
                                    │ next
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
    ┌─────────▼──────────┐  ┌──────▼──────────┐  ┌───────▼──────────┐
    │ ConcreteHandler1   │  │ ConcreteHandler2│  │ ConcreteHandler3 │
    ├────────────────────┤  ├─────────────────┤  ├──────────────────┤
    │ if can handle: ok  │  │ if can handle   │  │ if can handle    │
    │ else: next.handle()│  │ else: next      │  │ else: next       │
    └────────────────────┘  └─────────────────┘  └──────────────────┘
```

## Java 实现

```java
// Handler
abstract class Logger {
    public static final int INFO = 1;
    public static final int DEBUG = 2;
    public static final int ERROR = 3;

    protected int level;
    protected Logger next;

    public Logger setNext(Logger next) {
        this.next = next;
        return next;
    }

    public void log(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (next != null) {
            next.log(level, message);
        }
    }

    protected abstract void write(String message);
}

class ConsoleLogger extends Logger {
    public ConsoleLogger() { this.level = INFO; }
    @Override
    protected void write(String msg) { System.out.println("Console: " + msg); }
}

class FileLogger extends Logger {
    public FileLogger() { this.level = DEBUG; }
    @Override
    protected void write(String msg) { System.out.println("File: " + msg); }
}

class ErrorLogger extends Logger {
    public ErrorLogger() { this.level = ERROR; }
    @Override
    protected void write(String msg) { System.out.println("Error: " + msg); }
}

public class ChainOfResponsibilityDemo {
    public static void main(String[] args) {
        Logger chain = new ConsoleLogger();
        chain.setNext(new FileLogger())
             .setNext(new ErrorLogger());

        chain.log(Logger.INFO, "Info message");    // Console
        chain.log(Logger.DEBUG, "Debug message");  // Console + File
        chain.log(Logger.ERROR, "Error message");  // Console + File + Error
    }
}
```

## 关键点

- 每个处理器决定处理或传递
- 可动态组合处理器链
- 避免请求发送者与接收者的硬编码耦合

## 使用场景

- Java Servlet Filter、Spring Interceptor
- 审批流程、日志级别过滤
