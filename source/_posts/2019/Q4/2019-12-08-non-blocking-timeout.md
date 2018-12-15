---
title: Specify a timeout using a non-blocking method
description: Specify a timeout using a non-blocking method
date: 2019-12-08 14:24:21
tags:
  - Java
categories: [Programming, Java]
permalink: non-blocking-timeout
---

# Specify a timeout using a non-blocking method

## Java 8

A simple implementation of timeoutAfter is as follows where delayer is an instance of a ScheduledThreadPoolExecutor:

```java
public <T> CompletableFuture<T> timeoutAfter(long timeout, TimeUnit unit) {
    CompletableFuture<T> result = new CompletableFuture<T>();
    delayer.schedule(() -> result.completeExceptionally(new TimeoutException()), timeout, unit);
    return result;
}
```

Timeout mechanism

```java
CompletableFuture.supplyAsync(() -> findBestPrice("LDN - NYC"), executorService)
    .thenCombine(CompletableFuture.supplyAsync(() -> queryExchangeRateFor("GBP")), this::convert)
    .acceptEither(timeoutAfter(1, TimeUnit.SECONDS), amount -> System.out.println("The price is: " + amount + "GBP"));
```

## Java 9 or later

Java 9’s CompletableFuture introduces several new methods amongst which are orTimeout and completeOnTimeOut that provide built-in support for dealing with timeouts.

The method orTimeout has the following signature:

```java
public CompletableFuture<T> orTimeout(long timeout, TimeUnit unit);
public CompletableFuture<T> completeOnTimeout(T value, long timeout, TimeUnit unit);
```

```java
CompletableFuture.supplyAsync(() -> findBestPrice("LDN - NYC"), executorService)
    .thenCombine(CompletableFuture.supplyAsync(() -> queryExchangeRateFor("GBP")), this::convert)
    .orTimeout(1, TimeUnit.SECONDS)
    .whenComplete((amount, error) -> {
        if (error == null) {
            System.out.println("The price is: " + amount + "GBP");
        } else {
            System.out.println("Sorry, we could not return you a result");
        }
    });

CompletableFuture.supplyAsync(() -> findBestPrice("LDN - NYC"), executorService)
    .thenCombine(CompletableFuture.supplyAsync(() -> queryExchangeRateFor("GBP")), this::convert)
    .completeOnTimeout(DEFAULT_PRICE, 1, TimeUnit.SECONDS)
    .thenAccept(amount -> {
        System.out.println("The price is: " + amount + "GBP");
    });
```
