---
# HCIP-OSPF
layout: pags
title: OSPF诊断
date: 2025-06-29 15:23:54
tags: Network
categories: 
- [HCIP,1.5OSPF诊断] 
---

### 邻居关系无法建立

1. OSPF计时器不匹配

```bash
  interface g0/0/0 
    ospf timer hello 10
    ospf timer dead 40
```
<!-- more -->
2. OSPF区域ID不匹配

```bash
 ospf 1
    area 0
    network 155.1.12.0 0.0.0.255
interface g0/0/0 
    ospf enable 1 area 0.0.0.0
```

3. OSPF区域类型不匹配
 
 ```bash
 ospf 1
    area 1 
        stub/nssa
```

-  末梢和完全末梢可以建立邻居关系(之前文档中有实验过)

4. OSPF认证

```bash
 interface f0/0 
    ospf authentication-mode md5/simple/null
 interface f0/0 
    ospf authentication-mode md5 1 plain HUAWEI
    ospf authentication-mode simple plain HUAWEI
```

- Vlink与Area0认证类型

5. 双方router-id相同
6. 前缀或掩码不一致
7. OSPF网络类型不匹配
- NBMA与P2P,MA,P2MP无法建立
- 以下网络类型可以建立邻居  
MA与P2P，MA与P2MP，P2P与P2MP  

### OSPF卡在中间状态

1. 路由器优先级均为0
- 适应于MA,NBMA环境

```bash
 interface g0/0/0 
    ospf dr-priority 0
```

2. MTU检测
- 华为默认禁用，思科默认启用

```bash
 interface g0/0/0 
    ospf mtu-enable
```
- 不匹配的MTU，DD交互存在问题-卡在EXstaer

```bash
interface G0/0/0 
    MTU 1400
```

### 排错工具

1. debug ospf error
2. display ospf error


 
