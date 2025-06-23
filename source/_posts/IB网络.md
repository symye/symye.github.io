---
# IB网络笔记
layout: pags
title: IB网络学习笔记
date: 2025-06-23 15:23:54
tags: IB网络
---

### why(为什么出现了 IB 网络？背景？它解决了什么问题)

随着计算性能的快速提升，对于数据传输的性能要求也随之增高，而采用Intel架构的处理器输入/输出性能会受到PCI或者PCI-X总线的限制，IB 网络应运而生。

### what(IB 网络是什么)

- 定义：IB 是 InfiniBand 的缩写，直译为“无限带宽” 技术，是一个用于高性能计算的计算机通信标准
- 特点：具有极高的吞吐量和极低的延迟
- 应用场景：用于计算机与计算机之间的互联，或者服务器与存储系统之间、存储系统之间的互联
  
### IB 网络实现原理
利用RDMA(Remote Direct Memory Access，远程直接内存访问)提供基于通道的点对点消息队列转发模型，每个应用都可通过创建的虚拟通道直接获取本应用的数据消息，无需其他操作系统及协议栈的介入

- RDMA 技术是一种直接内存访问技术，将数据直接从一台计算机的内存传输到另一台计算机，无需双方操作系统的介入。
  
使用 RDMA 网卡和TCP/IP 协议栈数据传输流程上的区别

使用RDMA 方式和TCP/IP 协议栈对比

TCP/IP 协议栈

![one](../imgs/对比图.jpg)

### HOW 如何使用/测试IB网卡(以 ConnectX-6 为例)

#### 关于 NVIDIA MLNX OFED 驱动(基于 23.10-2.1.3.1 - LTS)

NVIDIA OFED(OpenFabrics Enterprise Distribution): 是一个单一虚拟协议互连 (VPI) 软件堆栈，可在所有 NVIDIA 网络适配器解决方案上运行. 所有 NVIDIA 网络适配器卡均兼容基于 OpenFabrics 的 RDMA 协议和软件，并受主要操作系统发行版的支持。





