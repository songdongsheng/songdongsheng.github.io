---
title: Block Chain
description: Block Chain
date: 2019-06-29 08:23:09
tags:
  - Trends
categories: [Trends]
permalink: block-chain
---

# 区块链

## 概论

1. 公有链

    公有链是指对所有人开放，任何人都可以参与的区块链。

2. 联盟链

    联盟链是被多个组织构成的联盟控制，进入和退出需要授权的区块链。

3. 私有链

    私有链是完全被单独的个人或某个组织控制的区块链。

大部分企业应用场景都不适合公有链，未来企业应用的重点是联盟链。现阶段关注的重点是公有链，公有链是区块链技术的试验田，它会遇到各种复杂的情况和问题，是对新技术和新业务的测试，这能为企业应用提供很好的借鉴。

## 蒙代尔不可能三角关系

对于区块链的去中心化、安全、高效这三个特性，符合蒙代尔不可能三角关系，即不可能同时满足三个条件。公有链实现了完全的去中心化和安全，因此在性能上就很低；联盟链为了企业应用，提高了性能和安全，就不得不在去中心化上进行妥协，通过一个中心化的授权方式来管理节点，实现了半中心化。

## 区块链 2.0 的优势

区块链 1.0 被称之为“全球账簿”。相应的，区块链 2.0 可以被看作一台“全球计算机”：实现了区块链系统的图灵完备，可以在区块链上传和执行应用程序，并且程序的有效执行能得到保证，在此基础上实现了智能合约的功能。相对于区块链 1.0，区块链 2.0 有如下优势：

1. 支持智能合约

    区块链 2.0 定位于应用平台，在这个平台上，可以发布各种智能合约，并能与其它外部IT系统进行数据交互和处理，从而实现各种行业应用。

2. 适应大部分应用场景的交易速度

    通过采用 PBFT、POS、DPOS 等新的共识算法，区块链2.0的交易速度有了很大的提高，峰值速度已经超过 了 3000 TPS（每秒处理交易数量），远远高于比特币的 5 TPS，已经能够满足大部分的金融应用场景。

3. 支持信息加密

    区块链 2.0 因为支持完整的程序运行，可以通过智能合约对发送和接收的信息进行自定义加密和解密，从而达到保护企业和用户隐私的目的，同时零知识证明等先进密码学技术的应用进一步推动了其隐私性的发展。

4. 无资源消耗

    为了维护网络共识，比特币使用的算力超 122029 TH/s，相当于 5000  台天河 2 号 A 运算速度，每天耗电超过 2000 MWh，约合几十万人民币（估测数据）。区块链 2.0 采 用PBFT、DPOS、POS 等新的共识算法，不再需要通过消耗算力达成共识，从而实现对资源的零消耗，使其能绿色安全的部署于企业信息中心。

## 区块链 2.0 技术架构

区块链 2.0 采用五层架构，从下到上分别是数据层、网络层、共识层、激励层、智能合约层：

![image](1ddacfe6-f2cb-3b9f-bfb9-be281f32f7fa.png)

1.  数据层

    数据层最底层的技术，是一切的基础，主要实现了两个功能，一个是相关数据的存储，另一个是账户和交易的实现与安全。数据存储主要基于Merkle树，通过区块的方式和链式结构实现，大多以 KV 数据库的方式实现持久化，比如以太坊采用 leveldb。帐号和交易的实现基于数字签名、哈希函数和非对称加密技术等多种密码学算法和技术，保证了交易在去中心化的情况下能够安全的进行。

2. 网络层

    网络层主要实现网络节点的连接和通讯，又称点对点技术，是没有中心服务器、依靠用户群交换信息的互联网体系。与有中心服务器的中央网络系统不同，对等网络的每个用户端既是一个节点，也有服务器的功能，其具有去中心化与健壮性等特点。

3. 共识层

    共识层主要实现全网所有节点对交易和数据达成一致，防范拜占庭攻击、女巫攻击、51% 攻击等共识攻击，其算法称为共识机制，因为其应用场景不同，区块链 2.0 出现了多种富有特色的共识机制。

    1. PoS：Proof of Stake，权益证明

        原理：节点获得区块奖励的概率与该节点持有的代币数量和时间成正比，在获取区块奖励后，该节点的代币持有时间清零，重新计算。但由于代币在初期分配时人为因素过高，容易导致后期贫富差距过大。

    2. DPoS：Delegate Proof of Stake，股份授权证明

        原理：所有的节点投票选出 100 个（或其他数量）委托节点，区块完全由这 100 个委托节点按照一定算法生成，类似于美国的议会制。

    3. Casper：投注共识

        原理：以太坊下一代的共识机制，每个参与共识的节点都要支付一定的押金，节点获取奖励的概率和押金成正比，如果有节点作恶押金则要被扣掉。

    4. PBFT：Practical Byzantine Fault Tolerance，拜占庭容错算法

        原理：与一般公有链的共识机制主要基于经济博弈原理不同，PBFT 基于异步网络环境下的状态机副本复制协议，本质上是由数学算法实现了共识，因此区块的确认不需要像公有链一样在若干区块之后才安全，可以实现出块即确认。

    5. PoET：Proof of Elapsed Time，消逝时间量证明

        原理：该共识机制由 intel 提出，核心是用 Intel 支持 SGX 技术的 CPU 硬件，在受控安全环境（TEE）下随机产生一些延时，同时 CPU 从硬件级别证明延时的可信性，类似于彩票算法，谁的延时最低，谁将获取记账权。这样，增加记账权的唯一方法就是多增加 CPU 的数量，具备了当初中本聪设想的一个 CPU 一票的可能，同时增加的 CPU 会提升整个系统的资源，变相实现了记账权与提供资源之间的正比例关系。

    共识机制有各自的优缺点，适应不同的场景：

    ![image](ce8cb631-ef24-3f40-b80e-945e32c5fc6d.png)

4. 激励层

    激励层主要实现区块链代币的发行和分配机制，比如以太坊，定位以太币为平台运行的燃料，可以通过挖矿获得，每挖到一个区块固定奖励 5 个以太币，同时运行智能合约和发送交易都需要向矿工支付一定的以太币。

5. 智能合约层

    智能合约赋予账本可编程的特性，区块链2.0通过虚拟机的方式运行代码实现智能合约的功能，比如以太坊的以太坊虚拟机（EVM）。同时，这一层通过在智能合约上添加能够与用户交互的前台界面，形成去中心化的应用（DAPP）。当然，在某些技术文档中认为 DAPP 应该在智能合约层之上单独为应用层，也是有一定道理，只要不影响读者理解即可。

## 智能合约

### 智能合约简介

智能合约又称智能合同，是由事件驱动的、具有状态的、获得多方承认的、运行在区块链之上的、且能够根据预设条件自动处理资产的程序，智能合约最大的优势是利用程序算法替代人仲裁和执行合同。

本质上讲，智能合约也是一段程序，但是与传统的 IT 系统不同，智能合约继承了区块链的三个特性：数据透明、不可篡改、永久运行。
1. 数据透明

    区块链上所有的数据都是公开透明的，因此智能合约的数据处理也是公开透明的，运行时任何一方都可以查看其代码和数据。

2. 不可篡改

    区块链本身的所有数据不可篡改，因此部署在区块链上的智能合约代码以及运行产生的数据输出也是不可篡改的，运行智能合约的节点不必担心其他节点恶意修改代码与数据。

3. 永久运行

    支撑区块链网络的节点往往达到数百甚至上千，部分节点的失效并不会导致智能合约的停止，其可靠性理论上接近于永久运行，这样就保证了智能合约能像纸质合同一样每时每刻都有效。

### 智能合约运行原理

通过最典型的以太坊为例简述智能合约运行的原理。

1. 以太坊虚拟机（EVM）

    以太坊虚拟机（EVM）是以太坊中智能合约的运行环境。如果做比喻的话智能合约更像是 Java 程序，Java程序通过 Java 虚拟机（JVM）将代码解释字节进行执行，以太坊的智能合约通过以太坊虚拟机（EVM）解释成字节码进行执行。EVM 被沙箱封装起来，也就是说运行在 EVM 内部的代码不能接触到网络、文件系统或者其他进程，甚至智能合约之间也只有有限的调用。

2. RPC接口

    RPC接口是以太坊与其他IT系统交互的接口，以太坊节点在8545端口提供了 JSON RPC API 接口，数据传输采用 JSON 格式，可以执行 Web3 库的各种命令，可以向前端，比如 Mist 等图形化客户端提供区块链的信息。

    智能合约是部署在区块链的代码，区块链本身不能执行代码，代码的执行是每个节点在本地通过太坊虚拟机（EVM）实现, 智能合约的运行原理如下图所示。

    ![image](3b511bd2-30d9-36b3-b57a-0aaf2f870e64.png)

    部署在区块链上的智能合约是一段能够在本地产生原智能合约代码的数据串，可以理解区块链为一个数据库，首先客户端通过发起一笔交易，告诉以太坊节点需要调用的函数及相关参数，然后所有的以太坊节点都会接收到这笔交易，从区块链这个数据库中读取了存储的智能合约运行代码，在本地EVM运行出结果，最后为避免节点作恶，节点运行智能合约的结果将与其他以太坊节点进行对比，确认无误后才将结果写入到了区块链中，从而实现智能合约的正确执行。