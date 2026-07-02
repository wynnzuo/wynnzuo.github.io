---
title: "组合模式 (Composite Pattern)"
description: "将对象组合成树形结构以表示「部分-整体」的层次结构，使客户端对单个对象和组合对象的使用具有一致性"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 结构型
---

## 意图

将对象组合成树形结构以表示"部分-整体"的层次结构，使客户端对单个对象和组合对象的使用具有一致性。

## 类图

```
┌──────────────────────┐
│      Component       │
│  (interface)         │◀──── root of tree
├──────────────────────┤
│ +operation()         │
│ +add()               │
│ +remove()            │
└──────────┬───────────┘
           │ implements        ┌──────────────────────────┐
┌──────────┤───────────┐      │    Composite             │
│   Leaf   │           │      ├──────────────────────────┤
├──────────┘           │      │ -children: List<Compo...> │
│ +operation()         │      ├──────────────────────────┤
└──────────────────────┘      │ +operation()─► loops      │
                              │ +add() +remove()          │
                              └──────────────────────────┘
```

## Java 实现

```java
import java.util.*;

// Component
interface FileSystemNode {
    void display(String indent);
    void add(FileSystemNode node);
    void remove(FileSystemNode node);
}

// Leaf
class File implements FileSystemNode {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void display(String indent) {
        System.out.println(indent + "📄 " + name);
    }

    @Override
    public void add(FileSystemNode node) {
        throw new UnsupportedOperationException("File cannot add children");
    }

    @Override
    public void remove(FileSystemNode node) {
        throw new UnsupportedOperationException("File cannot remove children");
    }
}

// Composite
class Directory implements FileSystemNode {
    private String name;
    private List<FileSystemNode> children = new ArrayList<>();

    public Directory(String name) {
        this.name = name;
    }

    @Override
    public void display(String indent) {
        System.out.println(indent + "📁 " + name);
        for (FileSystemNode child : children) {
            child.display(indent + "  ");
        }
    }

    @Override
    public void add(FileSystemNode node) {
        children.add(node);
    }

    @Override
    public void remove(FileSystemNode node) {
        children.remove(node);
    }
}

public class CompositeDemo {
    public static void main(String[] args) {
        Directory root = new Directory("root");
        Directory docs = new Directory("docs");
        docs.add(new File("readme.md"));
        docs.add(new File("notes.txt"));

        Directory src = new Directory("src");
        src.add(new File("Main.java"));
        src.add(new File("Utils.java"));

        root.add(docs);
        root.add(src);
        root.display("");
    }
}
```

## 关键点

- Leaf 和 Composite 实现相同接口，对客户端透明
- Composite 维护子组件集合
- 安全方式和透明方式需权衡

## 使用场景

- 文件系统、UI 组件树、组织架构树
- 任何具有"部分-整体"层次结构的场景
