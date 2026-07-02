---
title: "访问者模式 (Visitor Pattern)"
description: "将操作与对象结构分离，使得在不修改类的前提下为类层次结构添加新的操作"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - visitor
---

## 意图

表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

## 类图

```
┌──────────────────────┐    ┌──────────────────────┐
│      Element         │    │       Visitor         │
│  (interface)         │    │  (interface)          │
├──────────────────────┤    ├──────────────────────┤
│ +accept(Visitor)     │    │ +visit(ElementA)      │
└──────────┬───────────┘    │ +visit(ElementB)      │
           │                │ +visit(ElementC)      │
           │ implements     └──────────────────────┘
┌──────────┴───────────┐              ▲
│  ConcreteElementA    │              │ implements
├──────────────────────┤   ┌──────────┴───────────┐
│ +accept(v)           │   │   ConcreteVisitor    │
│   └─ v.visit(this)   │   ├──────────────────────┤
└──────────────────────┘   │ +visit(ElementA): opA│
                           │ +visit(ElementB): opB│
┌──────────────────────┐   │ +visit(ElementC): opC│
│  ConcreteElementB    │   └──────────────────────┘
├──────────────────────┤
│ +accept(v)           │         ObjectStructure
│   └─ v.visit(this)   │    ┌──────────────────────┐
└──────────────────────┘    │ -elements: List      │
                           │ +accept(Visitor)──────│──► iterates & accepts
                           └──────────────────────┘
```

## Java 实现

```java
import java.util.*;

// Visitor
interface FileVisitor {
    void visit(TextFile file);
    void visit(ImageFile file);
    void visit(VideoFile file);
}

// Concrete Visitor
class FileSizeCalculator implements FileVisitor {
    private long totalSize = 0;

    @Override
    public void visit(TextFile file) {
        totalSize += file.getSize();
    }

    @Override
    public void visit(ImageFile file) {
        totalSize += file.getSize();
    }

    @Override
    public void visit(VideoFile file) {
        totalSize += file.getSize();
    }

    public long getTotalSize() { return totalSize; }
}

class FileExporter implements FileVisitor {
    @Override
    public void visit(TextFile file) {
        System.out.println("Exporting text: " + file.getName() + ".txt");
    }

    @Override
    public void visit(ImageFile file) {
        System.out.println("Exporting image: " + file.getName() + ".png");
    }

    @Override
    public void visit(VideoFile file) {
        System.out.println("Exporting video: " + file.getName() + ".mp4");
    }
}

// Element
interface FileElement {
    void accept(FileVisitor visitor);
}

// Concrete Elements
class TextFile implements FileElement {
    private String name;
    private int size;

    public TextFile(String name, int size) { this.name = name; this.size = size; }
    public String getName() { return name; }
    public int getSize() { return size; }

    @Override
    public void accept(FileVisitor visitor) { visitor.visit(this); }
}

class ImageFile implements FileElement {
    private String name;
    private int size;

    public ImageFile(String name, int size) { this.name = name; this.size = size; }
    public String getName() { return name; }
    public int getSize() { return size; }

    @Override
    public void accept(FileVisitor visitor) { visitor.visit(this); }
}

class VideoFile implements FileElement {
    private String name;
    private int size;

    public VideoFile(String name, int size) { this.name = name; this.size = size; }
    public String getName() { return name; }
    public int getSize() { return size; }

    @Override
    public void accept(FileVisitor visitor) { visitor.visit(this); }
}

// Object Structure
class FileSystem {
    private List<FileElement> files = new ArrayList<>();

    public void addFile(FileElement file) { files.add(file); }

    public void accept(FileVisitor visitor) {
        for (FileElement file : files) {
            file.accept(visitor);
        }
    }
}

public class VisitorDemo {
    public static void main(String[] args) {
        FileSystem fs = new FileSystem();
        fs.addFile(new TextFile("readme", 100));
        fs.addFile(new ImageFile("logo", 500));
        fs.addFile(new VideoFile("demo", 10000));

        FileSizeCalculator calculator = new FileSizeCalculator();
        fs.accept(calculator);
        System.out.println("Total size: " + calculator.getTotalSize() + " bytes");

        FileExporter exporter = new FileExporter();
        fs.accept(exporter);
    }
}
```

## 关键点

- 双重分派（Double Dispatch）：accept 调用 visit，visit 调用具体元素方法
- 新增操作容易（新增 Visitor），新增 Element 困难（需修改所有 Visitor）
- 将相关操作集中到 Visitor 中，避免散落在各 Element 类中

## 使用场景

- AST 遍历、文件系统操作
- 编译器中的语义分析
- Java 注解处理（APT）
