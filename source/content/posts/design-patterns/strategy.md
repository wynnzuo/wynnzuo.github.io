---
title: "策略模式 (Strategy Pattern)"
description: "定义一系列算法，将每个算法封装起来并使它们可以互相替换"
date: 2026-07-02
draft: false
tags:
  - 设计模式
---

## 意图

定义一系列算法，把它们一个个封装起来，并且使它们可以相互替换。本模式使得算法可独立于使用它的客户而变化。

## 类图

```
┌──────────────┐      delegates   ┌──────────────────────┐
│   Context    │─────────────────▶│      Strategy         │
├──────────────┤                  │  (interface)          │
│ -strategy    │                  ├──────────────────────┤
│ setStrategy()│                  │ +execute(data)        │
│ execute()────┼──► strategy.     └──────────────────────┘
│              │      execute()             ▲
└──────────────┘                           │ implements
               ┌───────────────────────────┼──────────────────┐
               │                           │                  │
    ┌──────────┴──────────┐  ┌─────────────┴──────┐  ┌──────┴──────────┐
    │   BubbleSort        │  │     QuickSort      │  │   MergeSort     │
    ├─────────────────────┤  ├────────────────────┤  ├─────────────────┤
    │ +execute(array)     │  │ +execute(array)    │  │ +execute(array) │
    └─────────────────────┘  └────────────────────┘  └─────────────────┘
```

## Java 实现

```java
import java.util.*;

// Strategy
interface SortingStrategy {
    void sort(int[] array);
}

// Concrete Strategies
class BubbleSort implements SortingStrategy {
    @Override
    public void sort(int[] arr) {
        System.out.println("Using Bubble Sort");
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
    }
}

class QuickSort implements SortingStrategy {
    @Override
    public void sort(int[] arr) {
        System.out.println("Using Quick Sort");
        quickSort(arr, 0, arr.length - 1);
    }

    private void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high);
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }

    private int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                int tmp = arr[i];
                arr[i] = arr[j];
                arr[j] = tmp;
            }
        }
        int tmp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = tmp;
        return i + 1;
    }
}

// Context
class SortContext {
    private SortingStrategy strategy;

    public void setStrategy(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    public void executeSort(int[] array) {
        strategy.sort(array);
    }
}

public class StrategyDemo {
    public static void main(String[] args) {
        int[] data = {5, 2, 8, 1, 9};

        SortContext context = new SortContext();

        context.setStrategy(new BubbleSort());
        context.executeSort(data);

        context.setStrategy(new QuickSort());
        context.executeSort(data);
    }
}
```

## 关键点

- 算法可以独立替换，运行时切换
- 消除大量条件判断
- 策略对象通常无状态（可共享）

## 使用场景

- 支付方式选择（支付宝、微信、银行卡）
- 排序算法切换、压缩算法、校验算法
- Java Comparator 接口
