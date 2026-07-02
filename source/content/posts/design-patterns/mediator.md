---
title: "中介者模式 (Mediator Pattern)"
description: "用一个中介对象来封装一系列对象之间的交互，减少对象之间的耦合"
date: 2026-07-02
draft: false
tags:
  - 设计模式
  - 行为型
---

## 意图

用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

## 类图

```
┌─────────────────────────────────────────────────┐
│                Mediator (interface)              │
├─────────────────────────────────────────────────┤
│ +notify(sender, event)                           │
└─────────────────────────────────────────────────┘
                    ▲
                    │ implements
┌───────────────────┴────────────────────────────────┐
│              ConcreteMediator                       │
├────────────────────────────────────────────────────┤
│ -componentA: ComponentA                             │
│ -componentB: ComponentB                             │
│ +notify(sender, event)──► coordinates components   │
└──┬────────────────────────────────────────────────┬─┘
   │                                                │
   │ manages                                        │ manages
   │                                                │
┌──┴──────────────┐                      ┌─────────┴─────────────┐
│   ComponentA    │                      │     ComponentB        │
├─────────────────┤                      ├───────────────────────┤
│                 │──► notify(mediator)──▶│                       │
└─────────────────┘                      └───────────────────────┘
```

## Java 实现

```java
// Mediator
interface ChatMediator {
    void sendMessage(User user, String message);
    void addUser(User user);
}

// Concrete Mediator
class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    @Override
    public void addUser(User user) {
        users.add(user);
        user.setMediator(this);
    }

    @Override
    public void sendMessage(User sender, String message) {
        for (User user : users) {
            if (user != sender) {
                user.receive(sender.getName(), message);
            }
        }
    }
}

// Colleague
abstract class User {
    protected ChatMediator mediator;
    protected String name;

    public User(String name) { this.name = name; }
    public void setMediator(ChatMediator mediator) { this.mediator = mediator; }
    public String getName() { return name; }

    public abstract void send(String message);
    public abstract void receive(String from, String message);
}

class ChatUser extends User {
    public ChatUser(String name) { super(name); }

    @Override
    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.sendMessage(this, message);
    }

    @Override
    public void receive(String from, String message) {
        System.out.println(name + " received from " + from + ": " + message);
    }
}

public class MediatorDemo {
    public static void main(String[] args) {
        ChatMediator chatRoom = new ChatRoom();

        User alice = new ChatUser("Alice");
        User bob   = new ChatUser("Bob");
        User carol = new ChatUser("Carol");

        chatRoom.addUser(alice);
        chatRoom.addUser(bob);
        chatRoom.addUser(carol);

        alice.send("Hello everyone!");
        // Bob and Carol receive the message
    }
}
```

## 关键点

- 将多对多交互简化为一对多（中介者与各组件）
- 组件间不直接引用，只与中介者通信
- 降低耦合但中介者可能变得复杂

## 使用场景

- 聊天室、消息中间件
- GUI 组件交互（对话框中的按钮、文本框联动）
