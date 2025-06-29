---
# HCIP-ISIS
layout: pags
title: ISIS收敛调优
date: 2025-06-30 10:29:54
tags: Network
categories: 
- [HCIP,2.3ISIS收敛调优] 
---

### ISIS收敛步骤

1. 检测到故障时间---可以基于BFD,ISIS,Hello改善  
2. 生成新LSP时间
3. 泛洪LSP时间
4. SPT计算时间
5. 加载到RIB
6. 更新路由到主控板

### ISIS快速收敛机制

1. ISPF
    1. 增量路由计算
    2. 当网络拓扑改变的时候，只对受影响的节点进行路由计算，从而加快路由的计算
2. PRC
    1. 部分路由计算
    2. 当网络上路由发生变化的时候，只对发生变化的路由重新计算
3. 智能定时器
    1. SPF智能定时器--- SPF智能定时器可以对少量的外界突发事件进行快速响应，又可以避免过度的占用CPU
    2. LSP生成智能定时器
        1. 对于突发事件（如接口UP/DOWN）快速响应，加快网络收敛速度
        2. 单网络变化频繁时，智能定时器的间隔时间会自动延长，避免过度占用CPU资源
    3. 网络变化频繁，延长定时器     超时时间，反之缩短定时器时间   
4. LSP快速扩散---收到一个或多个较新的LSP时，在路由计算之前，先将小于指数目的LSP扩散出去，加快LSDB的同步过程
5. 按优先级收敛---在大量路由情况下，能够让某些特定的路由（列如匹配指定IP前缀的路由）优先收敛的一种技术

### 配置LSP报文属性 
