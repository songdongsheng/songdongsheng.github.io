---
title: RBAC principle
description: RBAC principle
date: 2019-04-05 09:21:11
tags:
  - Architecture
  - RBAC
categories: [Architecture, RBAC]
permalink: rbac-principle
---

# RBAC principle

## RBAC 三角形

RBAC 三角形有 2 种流行的描述，who/what/how 和 who/what/where，没有实质上的不同。都是定义了什么人可以对什么资源做什么操作。

![RBAC 三角形](rbac-triangle.jpg)

1. Where 定义了操作范围 (scope)。
2. What 定义了角色可以做什么操作 (Role)。
3. Who 定义了谁可以执行操作 (Role Group)。

## RBAC 实体

1. User：有权访问系统的个体
2. Role：组织内的命名作业功能，通常是为了完成某项职责而必须具有的权限集合。
3. Permission：批准访问一个或多个对象的特定模式。
    + Object：所有权限控制对象
    + Operation：权限控制对象支持的操作。基本对象操作有：
      -  A = Append
      -  C = Create
      -  R = Read
      -  U = Update
      -  D = Delete
      -  E = Execute
    + ID：权限标识，由对象标识和操作组合而成。

4. Session：用户与激活的分配给用户的角色的子集的映射。

![image](rbac.jpg)

## RBAC 授权约束

授权约束分为静态约束(SSD)和动态约束(DSD)，用来实现职责分离，基于时间和地点的权限等。

1. 静态约束(SSD)，在系统配置时已经确定的约束。例如用户不应该既当裁判员，又当运动员。
2. 动态约束(DSD)，在系统运行时才确定的约束。例如只有在工作时间才能登陆，只有在特定地区才能具有特定权限。

## UML responsibility of RBAC

![image](rbac-uml.jpg)
