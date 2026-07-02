---
title: "模板方法模式 (Template Method Pattern)"
description: "在父类中定义算法的骨架，将具体步骤的实现延迟到子类"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 行为型
---

## 意图

定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

## 类图

```
┌─────────────────────────────────────┐
│          AbstractClass              │
├─────────────────────────────────────┤
│ +templateMethod()  (final)          │
│   ├── primitiveOperation1()         │
│   ├── primitiveOperation2()         │
│   └── hook()  (optional)            │
│                                     │
│ #primitiveOperation1()  (abstract)  │
│ #primitiveOperation2()  (abstract)  │
│ #hook()  (default empty)            │
└──────────────────┬──────────────────┘
                   │ extends
          ┌────────┴────────┐
          │                 │
┌─────────▼──────┐  ┌──────▼─────────┐
│ ConcreteClassA │  │ ConcreteClassB │
├────────────────┤  ├────────────────┤
│ op1()          │  │ op1()          │
│ op2()          │  │ op2()          │
│                │  │ hook()──► yes  │
└────────────────┘  └────────────────┘
```

## Java 实现

```java
// Abstract Class
abstract class DataProcessor {
    // Template Method — defines algorithm skeleton
    public final void process() {
        loadData();
        processData();
        saveData();
        if (shouldValidate()) {
            validate();
        }
    }

    protected abstract void loadData();
    protected abstract void processData();

    protected void saveData() {
        System.out.println("Saving to database...");
    }

    // Hook method — subclasses can override
    protected boolean shouldValidate() {
        return false;
    }

    protected void validate() {
        System.out.println("Validating data...");
    }
}

// Concrete Class A
class CSVProcessor extends DataProcessor {
    @Override
    protected void loadData() {
        System.out.println("Loading CSV file...");
    }

    @Override
    protected void processData() {
        System.out.println("Processing CSV data...");
    }
}

// Concrete Class B
class JSONProcessor extends DataProcessor {
    @Override
    protected void loadData() {
        System.out.println("Loading JSON file...");
    }

    @Override
    protected void processData() {
        System.out.println("Processing JSON data...");
    }

    @Override
    protected boolean shouldValidate() {
        return true;
    }

    @Override
    protected void validate() {
        System.out.println("Validating JSON schema...");
    }
}

public class TemplateMethodDemo {
    public static void main(String[] args) {
        System.out.println("=== CSV Processing ===");
        DataProcessor csv = new CSVProcessor();
        csv.process();

        System.out.println("\n=== JSON Processing ===");
        DataProcessor json = new JSONProcessor();
        json.process();
    }
}
```

## 关键点

- 模板方法定义不变的部分，子类实现可变的部分
- 钩子方法（hook）提供可选扩展点
- 符合好莱坞原则："Don't call us, we'll call you"

## 使用场景

- JUnit 测试执行流程
- Spring JdbcTemplate、RestTemplate
- 工作流引擎中的固定流程模板
