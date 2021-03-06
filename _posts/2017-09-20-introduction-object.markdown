---
layout:     post
title:      "对象导论"
subtitle:   "thinking in Java first chapter"
date:       2017-09-20 16:00:00
author:     "石头人m"
header-img: "img/about-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Java
---

## 抽象过程——面向对象程序设计：

万物皆为对象；<br>
程序是对象的集合，它们通过发送消息来告知彼此所要做的；<br>
每个对象都有自己的由其他对象所构成的存储；<br>
每个对象都拥有其类型；<br>
某一特定类型的所有对象可以接收同样的消息。

## 每个对象都有一个接口：

问题空间的元素和解空间的对象之间创建一对一的映射。<br>
接口确定了对某一特定对象所发出的请求。<br>

## 每个对象都提供服务：

创建或寻找能够提供理想的服务来解决问题的一系列对象。

## 被隐藏的具体实现：

访问控制：让客户端程序员无法触及他们不应该触及的部分；允许库设计者可以改变类内部的工作方式而不用担心会影响到客户端程序员。

public：对任何人都是可用的；<br>
private：除类型创建者和类型的内部方法之外任何人都不能访问；<br>
protected：与private相当，继承的类可以访问protected成员，但不能访问private成员；<br>
默认访问权限：包访问权限，类可以访问在同一个包中的其他类的成员，但是在包之外，这些成员如同指定了private一样。

## 复用的具体实现：

最简单的方式：创建一个成员对象。<br>
组合（聚合），“has-a”拥有关系。在建立新类时，应该首先考虑组合，再考虑继承。

## 继承：

基类与导出类产生差异：添加新方法（is-like-a）；覆盖（is-a）。

## 多态：
```java
void doSomething(Shape shape){
shape.erase();
//...
shape.draw();
}
Circle circle = new Circle();
Triangle triangle = new Triangle();
Line line = new Line();
doSomething(circle);
doSomething(triangle);
doSomething(line);
```

向上转型（upcasting）。

## 单根继承结构：

终极基类：Object。<br>
保证所有对象都具备某些功能；使垃圾回收器的实现变得容易得多。

## 容器：

List、Map、Set...<br>
不同的容器提供了不同类型的接口和外部行为；不同的容器对于某些操作具有不同的效率。<br>
参数化类型。

## 对象的创建和生命周期：

动态内存分配：new<br>
垃圾回收机制。

## 异常处理：

抛出--捕获<br>
异常不能被忽略，保证一定会在某处得到处理。<br>
异常提供了一种从错误状况进行可靠恢复的途径。

## 并发编程：

同一时刻处理多个任务的思想。<br>
Java的并发是内置于语言中的。