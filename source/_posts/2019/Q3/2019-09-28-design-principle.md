---
title: Design Principle
description: Design Principle
date: 2019-09-28 10:06:07
tags:
  - Architecture
categories: [Architecture]
permalink: design-principle
---

# Design Principle

1. 单一职责原则 （Single Responsiblity Principle, SRP）:每个类只有一个职责。
2. 开闭原则（Open Closed Principle，OCP）：对扩展开放，对修改关闭。
3. 里氏代换原则（Liskov Substitution Principle，LSP）：任何基类可以出现的地方，子类一定可以出现。
4. 依赖倒转原则（Dependency Inversion Principle，DIP）：把父类都替换成它的子类，程序的行为没有变化。要依赖于抽象，不要依赖于具体。简单的说就是要求对抽象进行编程，不要对实现进行编程，这样就降低了客户与实现模块间的耦合。
5. 接口隔离原则（Interface Segregation Principle，ISP）：每一个接口应该是一种角色，不多不少。使用多个隔离的接口，比使用单个接口要好。降低依赖，降低耦合。
6. 合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）：要尽量使用合成/聚合，尽量不要使用继承。
7. 最小知道原则（Principle of Least Knowledge，PLK，也叫迪米特法则）：一个对象应对其他对象有尽可能少的了解。实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。要求限制软件实体之间通信的宽度和深度。
