---
title: Distributed System Design
description: Distributed System Design
date: 2018-12-22 11:37:40
tags:
  - Architecture
categories: [Architecture]
permalink: distributed-system-design
---

# Distributed System Design

- https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing
- http://www.hpcs.cs.tsukuba.ac.jp/~tatebe/lecture/h23/dsys/dsd-tutorial.html
- https://dzone.com/articles/understanding-the-8-fallacies-of-distributed-syste

## The 8 Fallacies of Distributed Systems

1. The network is reliable.
    - Qutomatically retry
    - Queuing systems
2. Latency is zero.
    - bring back all the data you might need
    - move the data closer to the clients (CDN)
    - invert the flow of data (Pub/Sub)
3. Bandwidth is infinite.
    - DDD
    - CQRS
4. The network is secure.
    - TLS
    - DTLS
    - HTTPS
5. Topology doesn't change.
    - DNS
    - discovery service
    - Service Bus
6. There is one administrator.
    - Everyone should be responsible for the release process
    - Logging and monitoring
    - Decoupling
    - Isolate 3rd party dependencies
7. Transport cost is zero.
    - The cost of the networking infrastructure
    - The cost of serialization/deserialization
        - SOAP or XML is more expensive than JSON
        - JSON is more expensive than binary protocols like Google’s Protocol Buffers
8. The network is homogeneous.
    - focus on standard protocols.
    - choose standard formats in order to avoid vendor lock-in
