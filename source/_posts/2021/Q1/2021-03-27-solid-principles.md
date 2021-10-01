---
title: SOLID Principles
excerpt: SOLID Principles
date: 2021-03-27 16:04:07
tags:
    - Architecture
    - Principle
categories: [Architecture, Principle]
---

# SOLID Principles

## In a nutshell

SOLID Principles makes your code more extendable, logical, and easier to read. When the developer builds software following a bad design, the code can become inflexible and more brittle. Small changes in the software can result in bugs. For these reasons, we should follow SOLID Principles.

![SOLID Principles](solid-principles.png)

## What is S.O.L.I.D

**SOLID** is an acronym that represents five principles very important when we develop with the OOP paradigm, in addition, it is essential knowledge that every developer must know.

Understanding and applying these principles will **allow you to write better quality code** and therefore be a better developer.

These 5 principles were introduced by Robert C. Martin (Uncle Bob), in his [Design Principles and Design Patterns book](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf). The actual SOLID acronym was, however, identified later by Michael Feathers.

## Why S.O.L.I.D Principles

These principles, when combined together, make it easy for a programmer to develop software that is easy to **maintain and extend**. They also make it easy for developers to **avoid code smells, easily refactor code**, and are also a part of agile or adaptive software development.

Let’s break down the letters of SOLID and see the details of each of these.

## Advantages of SOLID Principles

- Principled way of managing dependencies especially in a large application.
- Loose coupling (degree of interdependence between software modules)
- More cohesion (the degree to which the elements of a module belong together)
- Code is more reusable, robust, flexible, testable, and maintainable

## Single Responsibility Principle (SRP)

> A class should have one and only one reason to change, meaning that a class should have only one job.

- **Simple Definition**: Do not burden one class with too many responsibilities.

- **Advantages**: Increase cohesion, low coupling, reduced complexity.

## Open-Closed Principle (OCP)

> Objects or entities should be open for extension but closed for modification.

- **Simple Definition**: You should be able to extend a class’s behavior, without modifying it.

- **Other definition**: Ideally, once a class is done, it’s done.

- **Advantages**: Increased cohesion, extensible code, less chance of introducing bugs in the existing application.

## Liskov Substitution Principle (LSP)

> Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.

- **Simple Definition**: Objects of the derived class must behave in a manner consistent with the promises made in the base class’ contract.

- **Other definition**: All derived classes should be exact sub-types of the base class not kind of sub-types.

- **Advantages&&: Code re-usability, reduced coupling, and less chance of introducing bugs in the existing application.

## Interface Segregation Principle (ISP)

> many client-specific interfaces are better than one general-purpose interface.

- **Simple Definition**: Clients should not be forced to implement interfaces they do not use.

- **Other definition**: Break down a large class into smaller manageable interfaces

- **Advantages**: Increased cohesion, more extensible, and robust.

## Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules. Both should depend on abstractions.
>
> Abstractions should not depend on details. Details should depend on abstractions.

- **Simple Definition**: Entities must depend on abstractions, not on concretions.”

- **Other definition**: Anytime I new up a class it’s time to look at DIP

- **Advantages**: Loose coupling.
