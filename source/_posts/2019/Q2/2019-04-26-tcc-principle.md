---
title: TCC principle
excerpt: TCC principle
date: 2019-04-26 23:22:38
tags:
  - Architecture
categories: [Architecture, TCC]
---

# TCC principle

## In a nutshell

1. Try: 尝试执行业务
    + 完成所有业务检查(一致性)
    + 预留必须业务资源(准隔离性)
    + Autonomous Timeout and Cancel

2. Confirm: 确认执行业务
    + 真正执行业务
    + 不作任何业务检查
    + 只使用 Try 阶段预留的业务资源
    + 操作满足幂等性

3. Cancel: 取消执行业务
    + 释放 Try 阶段预留的业务资源
    + 操作满足幂等性


```java
public interface TccService<P extends TccRequest<R>, R extends Serializable> {
    R doTry(P param);
    void doConfirm(P param);
    void doCancel(P param);
}
```

## Participant

1. Try: ParticipantLink

    ```json
        {
            "participantLink":
            {
                "uri":"http://www.example.com/v1/tcc/tx/782973177474060290",
                "expires":"2017-10-24T10:20:12Z"
            }
        }
    ```

2. Confirm

    ```
    PUT /v1/tcc/tx/782973177474060290 HTTP/1.1
    Host: www.example.com
    Accept: application/tcc

    HTTP/1.1 204 No Content
    HTTP/1.1 404 Not Found
    ```

3. Cancel

    ```
    DELETE /v1/tcc/tx/782973177474060290 HTTP/1.1
    Host: www.example.com
    Accept: application/tcc

    HTTP/1.1 204 No Content
    ```

4. URL

    ```
    ^/v1/tcc/bo/{type}       POST
    ^/v1/tcc/bo/{type}/{id}  PATCH|PUT|DELETE
    ^/v1/tcc/tx/{txid}       PUT to confirm, DELETE to cancel
    ```

## Coordinator

1. Confirm all participants when asked to do so.
2. Recover after failures during the confirmation phase.
3. Allow cancellation of all participants when asked to do so.
4. URL

    ```
    ^/v1/tcc/dtc/confirm     PUT to confirm
    ^/v1/tcc/dtc/cancel      DELETE to cancel
    ```
