---
title: 'Spring Native: What, Why and How'
description: 'Spring Native: What, Why and How'
date: 2021-08-15 21:52:57
tags:
  - Programming
  - Java
categories:
  - Programming
  - Java
permalink: spring-native-what-why-and-how
---

# Spring Native: What, Why and How

## Introduction

[Spring Native](https://github.com/spring-projects-experimental/spring-native) makes sure we can compile Spring applications to a native executable. To get these native executables Spring Native uses the [GraalVM Native Image](https://www.graalvm.org/reference-manual/native-image/) compiler.

Compared to the Java Virtual Machine, native images can enable cheaper and more sustainable hosting for many types of workloads. These include microservices, function workloads, well suited to containers, and [Kubernetes](https://kubernetes.io/).

Using native image provides key advantages, such as instant startup, instant peak performance, and reduced memory consumption.

There are also some drawbacks and trade-offs that the GraalVM native project expect to improve on over time. Building a native image is a heavy process that is slower than a regular application. A native image has fewer runtime optimizations after warmup. Finally, it is less mature than the JVM with some different behaviors.

The key differences between a regular JVM and this native image platform are:

+ A **static analysis** of your application from the main entry point is performed at build time.
+ The unused parts are removed at build time.
+ **Configuration is required for reflection, resources, and dynamic proxies**.
+ **Classpath is fixed at build time**.
+ **No class lazy loading**: everything shipped in the executables will be loaded in memory on startup.
+ Some code will run at build time.
+ There are some [limitations](https://www.graalvm.org/reference-manual/native-image/Limitations/) around some aspects of Java applications that are not fully supported.

## What are GraalVM Native Images

Native Image is a technology to **ahead-of-time compile (AOT)** Java code to a standalone executable, called a native image. This executable includes the application classes, classes from its dependencies, runtime library classes, and statically linked native code from JDK.

## What is Spring Native

Like stated in the intro of this article, Spring Native is a set of tools and frameworks to make the Spring framework compatible with GraalVM Native Images.

## Advantages for native images

Here are some few advantages of native images:

### Faster startup

A native image will startup faster, but why can it start faster?

+ **No class loading**: All classes will already we loaded and even partially initiated during build time. This is made possible with the AOT compiler.
+ **No interpreted code**: We don’t have to initialize an interpreter and interpret byte-code
+ **No JIT**: We don’t have to spend any CPU resources to start a JIT compiler or use a JIT compiler
+ **Generating Image Heap during build**: Because we can already partially **initiate classes** at build time, we can also run some **initialization processes** at build time. So when we startup we don’t have to execute that part anymore

### Lower memory usage

The native image will have less memory usage which makes is more suitable for Docker Images, just to give an example. How does a native image archive this?

+ No metadata for loaded classes
+ No profiling data for JIT
+ No Interpreter code

## Disadvantages for native images

That all sounds good and way better than normal JVM applications. But there are of course also prices to pay when using native images.

### No Java agents, JMX, JVMTI, Java Flight Recorder support

Some of these features are really handy to manage, test and control your JVM applications. Because the native images does not live in a JVM container these features are not available.

### Reflection requires extra config

Reflection is widely used in a lot of frameworks so those frameworks need to do extra configuration and work to support native images. That is why Spring created the Spring Native project.

### No dumps

You will not be able to able to use thread and heap dumps. There are ways to fetch some information about threads by using Linux Kernel features.

### Limited Logging

+ Logback is supported, but not configuration with `logback.xml` so please configure it with `application.properties` or `application.yml` for now, see [Add support for logback.xml configuration file](https://github.com/spring-projects-experimental/spring-native/issues/625) for more details.
+ Log4j2 is not supported yet, see [Add support for Log4j2](https://github.com/spring-projects-experimental/spring-native/issues/115).

## When do we best to use Spring Native

Spring Native images is the best suited when you are building CLI tools or create some serverless functions. This all has to do with the short live span of the application and the faster startup time. **Spring Cloud Functions** fit really good with native images.

## Building an example Native Image

The easiest way to start with Spring Native is probably to go to [Spring Initializer Site](https://start.spring.io/), add the dependencies **Spring Reactive Web** and **Spring Native [Experimental]**, and read the [reference documentation](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle). Make sure to configure properly the [Spring AOT Maven and Gradle plugins](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle/#spring-aot) that are mandatory to get proper native support for your Spring application.

Make sure the following are installed: Java 11, Maven/Gradle, Docker, and [GraalVM native-image](https://www.graalvm.org/reference-manual/native-image/) compiler.

### Update pom.xml

Add **Spring Native [Experimental]** dependency to the **pom.xml**:

```xml
<dependency>
    <groupId>org.springframework.experimental</groupId>
    <artifactId>spring-native</artifactId>
    <version>${spring-native.version}</version>
</dependency>
```

Add **spring-aot-maven-plugin** to the **pom.xml**:

```xml
<plugin>
    <groupId>org.springframework.experimental</groupId>
    <artifactId>spring-aot-maven-plugin</artifactId>
    <version>${spring-native.version}</version>
    <executions>
        <execution>
            <id>test-generate</id>
            <goals>
                <goal>test-generate</goal>
            </goals>
        </execution>
        <execution>
            <id>generate</id>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

This plugin is used to compile your Spring application code to make it ready for native execution. This plugin will also add all the configuration that is needed to handle the Reflection that Spring uses.

### Spring AOT

Spring AOT (Ahead-of-Time) aims at improving compatibility and footprint of native images for Spring Native applications. To achieve that, this projects ships Maven and Gradle build plugins that generate and compile a separate set of Java sources to be packaged with your Spring Boot application. By looking at your application classpath and configuration, Spring AOT brings some of the configuration processing at build time and streamlines the native image compilation process.

Maven goals **spring-aot:generate** (**process-test-classes** phase) and **spring-aot:test-generate** (**prepare-package** phase) are automatically invoked in the Maven lifecycle when using the `mvn verify` or `mvn package` commands.

```bash
$ mvn -D skipTests=true package spring-aot:generate
Total time:  01:25 min
```

### Add endpoint

```java
@Bean
public RouterFunction<ServerResponse> routes() {
   return RouterFunctions.route()
      .GET("/", request -> ServerResponse.ok().body(Mono.just("Hello World!\n"), String.class))
      .build();
}
```

### Verification Endpoint

```bash
# mvn spring-boot:run
Started SpringBootNativeApplication in 6.65 seconds (JVM running for 7.325)

# curl -iSS http://localhost:8080
# curl -iSS --http2 http://localhost:8080
# curl -iSS --http2-prior-knowledge http://localhost:8080
```

### Building the image

Now we need to use maven to build the docker image. Spring boot already has support to build docker images with it’s plugin.

```bash
$ java -version
java version "11.0.12" 2021-07-20 LTS
Java(TM) SE Runtime Environment GraalVM EE 21.2.0 (build 11.0.12+8-LTS-jvmci-21.2-b06)
Java HotSpot(TM) 64-Bit Server VM GraalVM EE 21.2.0 (build 11.0.12+8-LTS-jvmci-21.2-b06, mixed mode, sharing)

$ gradle tasks

$ gradle bootBuildImage -x test
Successfully built image 'docker.io/library/spring-boot-native:0.0.1-SNAPSHOT'
BUILD SUCCESSFUL in 8m 5s

# docker images

# docker run --rm --name spring-native-example -p 8080:8080 docker.io/library/spring-boot-native:0.0.1-SNAPSHOT
```

```bash
$ mvn -U -D skipTests=true package spring-boot:build-image
Successfully built image 'docker.io/library/spring-boot-native:0.0.1-SNAPSHOT'
Total time:  06:43 min

# docker images

# docker run --rm --name spring-native-example -p 8080:8080 docker.io/library/spring-boot-native:0.0.1-SNAPSHOT
```

### Verification Native Image

```bash
$ curl -iSS http://localhost:8080
HTTP/1.1 200 OK
Content-Type: text/plain;charset=UTF-8
Content-Length: 13

Hello World!
```

```bash
$ curl -iSS --http2 http://localhost:8080
HTTP/1.1 101 Switching Protocols
connection: upgrade
upgrade: h2c

HTTP/2 200
content-type: text/plain;charset=UTF-8
content-length: 13

Hello World!
```

```bash
$ curl -iSS --http2-prior-knowledge http://localhost:8080
HTTP/2 200
content-type: text/plain;charset=UTF-8
content-length: 13

Hello World!
```

## Conclusion

We went a bit over the theory what native images are and how Spring Native fits into the equation. The example application shows that the native image boots up way faster. Currently Spring Native is still experimental and in beta, but it looks real promising for serverless architectures.

## Reference

+ [Spring Initializer Site](https://start.spring.io/)
+ [GraalVM Native Image](https://www.graalvm.org/reference-manual/native-image/)
+ [Spring Native](https://github.com/spring-projects-experimental/spring-native/)
+ [Spring Native reference documentation](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle/)
* [Official Gradle documentation](https://docs.gradle.org/)
* [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/)
* [Spring Web](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/bookmarks/)
