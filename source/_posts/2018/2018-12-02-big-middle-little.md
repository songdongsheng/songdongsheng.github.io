---
title: DynamIQ - From big.LITTLE to big.Middle.LITTLE
excerpt: big.LITTLE 技术作为 DynamIQ 的一部分，从而可以形成更复杂的异构处理技术
date: 2018-12-02 18:46:49
tags:
  - Arm
categories: [Processor, Arm]
---

# DynamIQ big.LITTLE 简介

DynamIQ 是 Arm Cortex-A CPU 的新技术，重新定义了多核计算，将作为下一代计算技术的基础。

+ 一种新的单一集群设计，具有多达八个异构 CPU，允许不同的配置，最多支持四个 big CPU 或八个 LITTLE CPU 或其组合。如 1 + 7（即 1xbig 和 7xLITTLE CPU），2 + 4, 1 + 3 等组合，以提供可扩展的解决方案
+ 专为在 CPU 上提供更多 AI 和 AR 计算机性能而设计的高级计算功能
+ 重新设计的内存子系统，在群集中的所有核心之间共享，以提高响应速度和效率
+ 无论有没有 LITTLE 配置，都可以更灵活地扩展到智能手机以外的不同市场
+ 通过内置硬件功能提高功耗，实现更高的效率
+ 将 Cortex-A CPU 与外部加速器和 I/O 相关 IP 紧密耦合，在 CPU 和 SoC 上的专用加速器硬件之间提供高达 10 倍的响应时间，从而实现更高的性能和更低的延迟。
+ 基于 DynamIQ 的系统还集成了高级功能，可提高工业和基础设施等应用的可靠性，可用性和可维护性（RAS）。

DynamIQ 带来了一种新的技术架构，可以改变异构处理的格局。它通过将 big 和 LITTLE 集群组合在一起形成一个完全集成的 CPU 集群来实现这一点，该集群由 big 和 LITTLE CPU组成。使用DynamIQ 构建的 big.LITTLE 设计称为 DynamIQ big.LITTLE。

## DynamIQ big.LITTLE 技术的优势

单线程性能提升带来更高的用户体验(UX)，例如人工智能（AI）和增强现实（AR），将继续需要更高级别的用户体验。但是热预算会限制设备可实现的性能。DynamIQ big.LITTLE系统可以实现更长时间的持续性能，以及使用高性能的突发请求，它还具有更快的 CPU 电源状态转换能力。

DynamIQ big.LITTLE 引入了以下好处：

+ 在完全集成的解决方案中更广泛的产品差异化
+ 通过更精细的 CPU 速度控制获得更高的用户体验（UX），AI 和 AR 等高级用例的数据处理速度更快，更复杂
+ 通过先进的电源管理功能提高能效

## DynamIQ big.LITTLE 技术带来的影响

DynamIQ 将改变计算发生的方式和位置，以下列方式影响我们的生活。例如：

+ 几乎无限的配置，为不同层级的解决方案和性能提供新级别的灵活性和可扩展性; 这为消费者和设备选择提供了更多选择，以满足他们的需求
+ 内置省电功能，可实现从设备到云的创新。更薄的设备，更长久的电池，服务器场的节能 - 电源效率优势涵盖所有市场
+ 为设备带来智能计算可提高数据安全性和隐私性，以及更快的响应时间，提高人工智能的性能 - 无论是在家中，还是在汽车中，还是在云中

Arm DynamIQ 技术是为下一代智能设备提供更智能，更快速，更强大的用户体验的新基础。它将在一个要求苛刻的新设计市场中推动创新，从更薄的设备到基于云的解决方案 - 无论在何处进行计算。

## 支持 DynamIQ big.LITTLE 的 CPU

+ 麒麟 980 采用了 2+2+4 的核心方案，2 个大核(Cortex-A76@2.6GHz，重载)、2 个中核(Cortex-A76@1.92GHz，中载)、4 个小核(Cortex-A55@1.8GHz，轻载)。

+ 骁龙 855 则是基于 Cortex-A76 的 kryo 485 架构的 1+3+4 的核心方案，1 个大核(2.84GHz，重载)、3 个中核(2.42 GHz，中载)、4 个小核(1.80 GHz，轻载)。

从技术规格上看，855 和麒麟 980 的相同点很多。二者都用了台积电 7nm 制程、CPU 部分都是在 A76 和 A55 核心之上仅做了小修改，都是 8 核心「大、中、小」三丛集设计，两颗 ISP，也都有 NPU。

区别有三处：第一处是 855 的 GPU 还是配备了高通的 Adreno  640，而 980 用了 ARM 公版的 Mali-G76；第二处是三丛集的搭配方案，855 选择单超大核的方案，而 980 是「224」的配置；第三是 855 上搭载的高通一些特色功能，比如 True Wireless、aptX、AIE SDK 等等。
