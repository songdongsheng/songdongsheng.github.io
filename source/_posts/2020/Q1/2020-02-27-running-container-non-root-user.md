---
title: Running a Container With a Non Root User
excerpt: Running a Container With a Non Root User
date: 2020-02-27 23:27:51
tags:
  - Cloud
  - Container
  - Linux
categories: [Cloud, Container]
---

# Running a Container With a Non Root User

- https://docs.docker.com/engine/reference/builder/

## Introduction

设计良好的系统遵循最小特权原则。即应用程序仅应有权访问其执行其所需功能所需的资源。这在设计安全系统时至关重要。无论是恶意的还是由于错误的程序，一个进程在运行时都可能产生意想不到的后果。保护自己免受任何意外访问的最佳方法之一是仅授予运行某个进程所需的最小特权。

多数容器化流程是应用程序服务，因此不需要以 root 身份运行，也不应假定它们是 root。而是应该在 Dockerfile 中创建一个具有已知 UID 和 GID 的用户，然后以该用户身份运行您的进程。因为限制了对资源的访问，从而镜像更易于安全运行。

## Recommendation

- 使用 root 执行特权操作
    1. (可选) Multi-Stage Builds
    2. FROM 指令，指定基础镜像
    3. COPY 指令，复制应用程序的文件
    4. RUN 指令，创建应用程序需要的目录或文件
    5. RUN 指令，(可选)安装所需的软件包
    6. RUN 指令，(可选)使用系统工具处理配置文件等
    7. RUN 指令，创建系统用户组和系统用户 (**-r**)，切记要显式创建用户组，SUSE 的默认组和其它系统不一样
    8. RUN 指令，为新建的系统用户赋予目录和文件权限，修改属主时记得使用递归选项 **-R**。
- 使用新建用户运行应用程序服务
    1. USER 指令
    2. ENTRYPOINT 和 CMD 指令

### 以 root 身份运行的镜像

```dockerfile
FROM openjdk:8u242
COPY cassandra-table-cleanup.jar /var/lib/cassandra-table-cleanup/lib/
RUN mkdir -p /var/lib/cassandra-table-cleanup/log
WORKDIR /var/lib/cassandra-table-cleanup/log/
CMD ["/usr/bin/java", "-jar", "/var/lib/cassandra-table-cleanup/lib/cassandra-table-cleanup.jar"]
```

### 以 monte 身份运行的镜像

```dockerfile
FROM openjdk:8u242

COPY cassandra-table-cleanup.jar /var/lib/cassandra-table-cleanup/lib/
RUN mkdir -p /var/lib/cassandra-table-cleanup/log/ && \
    groupadd -r monte && useradd -r -g monte monte && \
    chown -R monte:monte /var/lib/cassandra-table-cleanup
WORKDIR /var/lib/cassandra-table-cleanup/log/
USER monte
CMD ["/usr/bin/java", "-jar", "/var/lib/cassandra-table-cleanup/lib/cassandra-table-cleanup.jar"]
```

## Tricks

### 用户组和用户

1. useradd 不指定 **-r**，创建用户时可能会进入交互模式，提示输入密码，所以 groupadd 和 useradd 一定要指定 **-r**，创建系统组和系统用户。
2. 创建用户时，一定要明确创建用户组 (**-g**)，因为不同系统的默认组不一样！
3. 如果需要使用持久化卷，必须显式指明 gid 和 uid，因为不同系统为新用户分配的默认 gid 和 uid 都不一样！

#### Debian/Ubuntu

```bash
root@debian-10:/# useradd -r user-01
root@debian-10:/# id user-01
uid=999(user-01) gid=999(user-01) groups=999(user-01)
```

#### RHEL/CentOS

```bash
[root@rhel-8 /]# useradd -r user-02
[root@rhel-8 /]# id user-02
uid=998(user-02) gid=996(user-02) groups=996(user-02)
```

#### SUSE

```bash
suse-15:/ # useradd -r user-03
suse-15:/ # id user-03
uid=499(user-03) gid=100(users) groups=100(users)
```

#### Alpine

```bash
/ # adduser -S user-04
/ # id user-04
uid=100(user-04) gid=65533(nogroup) groups=65533(nogroup),65533(nogroup)
```

### 目录与文件的权限

#### COPY and WORKDIR

使用 COPY 复制的文件和目录属主，以及 WORKDIR 创建的目录属主，与其与 USER 的**相对位置无关**，始终是 **root** !
重申一次，WORKDIR 一定要事先创建，并且修改属主，不然其属主是 root !
因此下面的例子看起来没错，实际不能正常写文件到当前目录：

```dockerfile
FROM openjdk:8u242
RUN useradd -r monte
USER monte
COPY cassandra-table-cleanup.jar /var/lib/cassandra-table-cleanup/lib/
WORKDIR /var/lib/cassandra-table-cleanup/log/
CMD ["/usr/bin/java", "-jar", "/var/lib/cassandra-table-cleanup/lib/cassandra-table-cleanup.jar"]
```

#### chown

1. chown 不指定 group 时，不会修改 group 为 user 对应的 group !
2. chown 不使用递归选项 **-R** 时，子目录及其中的文件属主不会被改变。

```dockerfile
FROM openjdk:8u242

COPY cassandra-table-cleanup.jar /var/lib/cassandra-table-cleanup/lib/
RUN mkdir -p /var/lib/cassandra-table-cleanup/log/ && \
    groupadd -r monte && useradd -r -g monte monte && \
    chown -R monte:monte /var/lib/cassandra-table-cleanup
WORKDIR /var/lib/cassandra-table-cleanup/log/
USER monte
CMD ["/usr/bin/java", "-jar", "/var/lib/cassandra-table-cleanup/lib/cassandra-table-cleanup.jar"]
```
