---
title: Moving average
description: Moving average
date: 2019-02-09 10:54:09
tags:
  - Java
categories: [Programming, Java]
permalink: moving-average
---

# Moving average

In statistics, a moving average (rolling average or running average) is a calculation to analyze data points by creating a series of averages of different subsets of the full data set. It is also called a moving mean (MM) or rolling mean and is a type of finite impulse response filter. Variations include: simple, and cumulative, or weighted forms.

Given a series of numbers and a fixed subset size, the first element of the moving average is obtained by taking the average of the initial fixed subset of the number series. Then the subset is modified by "shifting forward"; that is, excluding the first number of the series and including the next value in the subset.

A moving average is commonly used with time series data to smooth out short-term fluctuations and highlight longer-term trends or cycles. The threshold between short-term and long-term depends on the application, and the parameters of the moving average will be set accordingly.

## Variations

+ MA:     moving average
+ SMA:    simple moving average
+ LWMA:   linearly weighted moving average
+ EMA:    exponential moving average
+ DEMA:   double exponential moving average
+ TEMA:   triple exponential moving average
+ DMA:    displaced moving average

## Sample coding

### EMA

```java
class ExponentialMovingAverage {
    private double alpha;
    private Double oldValue;
    public ExponentialMovingAverage(double alpha) {
        this.alpha = alpha;
    }

    public double average(double value) {
        if (oldValue == null) {
            oldValue = value;
            return value;
        }
        double newValue = oldValue + alpha * (value - oldValue);
        oldValue = newValue;
        return newValue;
    }
}
```

## Time windowed EMA

```java
/**
 * An exponentially weighted moving average implementation that decays based on the elapsed time since the last update,
 * approximating a time windowed moving average.
 */
public class MovingAverage {
  private final long windowNanos;

  // Mutable state
  private volatile long lastNanos;
  private volatile double average;

  /**
   * Creates a moving average of samples over a {@code window} for the {@code timeUnit}.
   */
  public MovingAverage(long window, TimeUnit timeUnit) {
    this.windowNanos = timeUnit.toNanos(window);
  }

  /**
   * Updates the average with the {@code sample}.
   */
  public synchronized void update(double sample) {
    long now = System.nanoTime();

    if (lastNanos == 0) {
      average = sample;
      lastNanos = now;
      return;
    }

    long elapsedNanos = now - lastNanos;
    double coeff = Math.exp(-1.0 * ((double) elapsedNanos / windowNanos));
    average = (1.0 - coeff) * sample + coeff * average;
    lastNanos = now;
  }

  /**
   * Returns the moving average.
   */
  public double get() {
    return average;
  }

  /**
   * Tick the internal moving average clock back by the {@code duration}. Useful for testing.
   */
  void tick(long duration, TimeUnit timeUnit) {
    if (lastNanos != 0)
      lastNanos -= timeUnit.toNanos(duration);
  }
}
```
