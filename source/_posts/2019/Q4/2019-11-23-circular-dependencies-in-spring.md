---
title: Circular Dependencies in Spring
description: Circular Dependencies in Spring
date: 2019-11-23 18:35:31
tags:
  - Java
categories: [Programming, Java]
permalink: circular-dependencies-in-spring
---

# Circular Dependencies in Spring

## What Happens in Spring

When Spring context is loading all the beans, it tries to create beans in the order needed for them to work completely.

When having a circular dependency, Spring cannot decide which of the beans should be created first, since they depend on one another. In these cases, Spring will raise a BeanCurrentlyInCreationException while loading context.

It can happen in Spring when using constructor injection; if you use other types of injections you should not find this problem since the dependencies will be injected when they are needed and not on the context loading.

## The Workarounds

1. Redesign

When you have a circular dependency, it’s likely you have a design problem and the responsibilities are not well separated. You should try to redesign the components properly so their hierarchy is well designed and there is no need for circular dependencies.

2. Use @Lazy

A simple way to break the cycle is saying Spring to initialize one of the beans lazily. That is: instead of fully initializing the bean, it will create a proxy to inject it into the other bean. The injected bean will only be fully created when it’s first needed.

3. Use Setter/Field Injection

One of the most popular workarounds, and also what Spring documentation proposes, is using setter injection.

Simply put if you change the ways your beans are wired to use setter injection (or field injection) instead of constructor injection – that does address the problem. This way Spring creates the beans, but the dependencies are not injected until they are needed.

4. Use @PostConstruct

Another way to break the cycle is injecting a dependency using @Autowired on one of the beans, and then use a method annotated with @PostConstruct to set the other dependency.

```java
@Component
public class CircularDependencyA {

    @Autowired
    private CircularDependencyB circB;

    @PostConstruct
    public void init() {
        circB.setCircA(this);
    }

    public CircularDependencyB getCircB() {
        return circB;
    }
}

@Component
public class CircularDependencyB {

    private CircularDependencyA circA;

    private String message = "Hi!";

    public void setCircA(CircularDependencyA circA) {
        this.circA = circA;
    }

    public String getMessage() {
        return message;
    }
}
```

5. Implement ApplicationContextAware and InitializingBean

If one of the beans implements ApplicationContextAware, the bean has access to Spring context and can extract the other bean from there. Implementing InitializingBean we indicate that this bean has to do some actions after all its properties have been set; in this case, we want to manually set our dependency.

```java
@Component
public class CircularDependencyA implements ApplicationContextAware, InitializingBean {

    private CircularDependencyB circB;

    private ApplicationContext context;

    public CircularDependencyB getCircB() {
        return circB;
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        circB = context.getBean(CircularDependencyB.class);
    }

    @Override
    public void setApplicationContext(final ApplicationContext ctx) throws BeansException {
        context = ctx;
    }
}

@Component
public class CircularDependencyB {

    private CircularDependencyA circA;

    private String message = "Hi!";

    @Autowired
    public void setCircA(CircularDependencyA circA) {
        this.circA = circA;
    }

    public String getMessage() {
        return message;
    }
}
```

## In Conclusion

There are many ways to deal with circular dependencies in Spring. The first thing to consider is to redesign your beans so there is no need for circular dependencies: they are usually a symptom of a design that can be improved.

But if you absolutely need to have circular dependencies in your project, you can follow some of the workarounds suggested here.

The preferred method is using setter injections. But there are other alternatives, generally based on stopping Spring from managing the initialization and injection of the beans, and doing that yourself using one strategy or another.
