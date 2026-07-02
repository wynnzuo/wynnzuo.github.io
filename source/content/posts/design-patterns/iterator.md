---
title: "迭代器模式 (Iterator Pattern)"
description: "提供一种顺序访问聚合对象元素的方法，而不暴露其内部表示"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 行为型
---

## 意图

提供一种方法顺序访问一个聚合对象中各个元素，而又不需要暴露该对象的内部表示。

## 类图

```
┌──────────────────────┐    ┌──────────────────────┐
│   IterableCollection │    │      Iterator<T>      │
│  (interface)         │    │  (interface)          │
├──────────────────────┤    ├──────────────────────┤
│ +createIterator()────┼───▶│ +hasNext(): boolean   │
└──────────┬───────────┘    │ +next(): T            │
           │                └──────────────────────┘
           │ implements               ▲ implements
┌──────────┴───────────┐   ┌──────────┴───────────┐
│   ConcreteCollection │   │   ConcreteIterator    │
├──────────────────────┤   ├──────────────────────┤
│ -items: List<T>      │   │ +hasNext()─► check idx│
│ +add()               │   │ +next()───► return item│
└──────────────────────┘   └──────────────────────┘
```

## Java 实现

```java
import java.util.*;

// Iterator interface
interface Iterator<T> {
    boolean hasNext();
    T next();
}

// Aggregate interface
interface IterableCollection<T> {
    Iterator<T> createIterator();
}

// Concrete Iterator
class ListIterator<T> implements Iterator<T> {
    private ConcreteCollection<T> collection;
    private int index;

    public ListIterator(ConcreteCollection<T> collection) {
        this.collection = collection;
        this.index = 0;
    }

    @Override
    public boolean hasNext() {
        return index < collection.size();
    }

    @Override
    public T next() {
        return collection.get(index++);
    }
}

// Concrete Aggregate
class ConcreteCollection<T> implements IterableCollection<T> {
    private List<T> items = new ArrayList<>();

    public void add(T item) { items.add(item); }
    public int size() { return items.size(); }
    public T get(int index) { return items.get(index); }

    @Override
    public Iterator<T> createIterator() {
        return new ListIterator<>(this);
    }
}

public class IteratorDemo {
    public static void main(String[] args) {
        ConcreteCollection<String> collection = new ConcreteCollection<>();
        collection.add("Java");
        collection.add("Python");
        collection.add("Go");

        Iterator<String> it = collection.createIterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
```

## 关键点

- 分离遍历行为与聚合对象
- 多种遍历方式可由不同 Iterator 实现
- 符合单一职责原则

## 使用场景

- Java 集合框架（List、Set 的 iterator()）
- 需要统一遍历接口的不同数据结构
