---
layout: post
title:  "Spring Core 之 IoC Container 篇, Part1: 核心理念"
date:   2019-08-26 17:27:44 +0800
categories: Spring
tags: [Spring, IoC]
---

# 控制反转（Inversion of Control, IoC）的概念

*IoC* 是一种程序设计的理念，它主要解决的是程序耦合的问题。而实现这个理念有好多种方式，而最常见的就是*依赖注入*。

首先让我们看一个最常见的例子： 在订单生成时，抛一个事件出去：

```java
class Order {
    private final EventPublisher eventPublisher;

    public Order() {
        eventPublisher = new EventPublisher();
    }

    public long save () {
        //...

        eventPublisher.publishEvent("save");

        //...
    }
}
```

这里把无关的代码去掉，聚焦在`Order`和`EventPublisher`关系上，`Order`是依赖于`EventPublisher`的，于是`Order`在构造时，自己把`EventPublisher`创建出来了。

问题是`Order`知道`EventPublisher`如何创建的，看到`new xxObject`, 基本也就断定了，产生了紧耦合。换句话说`Order`需要知道它本不需要知道的事，它知道太多了。

这里违返了两个设计原则，第一是封装变化
