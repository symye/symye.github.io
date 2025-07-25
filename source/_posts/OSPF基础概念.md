---
# HCIP-OSPF
layout: pags
title: OSPF基础概念
date: 2025-06-23 15:23:54
tags: Network
categories: 
- [HCIP,1.1OSPF基本概念]
---

### OSPF基本信息
   
- 开放最短路径优先
- 链路状态路由协议：（1）限：区域内 （2）基于拓扑信息计算路由
- 网络层协议：（1）运行于IP协议之上 （2）协议ID：89
- 优先级： 内部10 外部150
- 协议特点：
1. 快速收敛（触发更新，增量更新，周期更新，维护三张表）
2. 组播/单播路由更新(OSPF更新更为高效)
3. 支持CIDR，VLSM（支持路由聚合，路由计算携带掩码
4. 支持认证 明文/MD5（抵御恶意的路由Flood攻击）
5. 区域内无环路路由协议（通过STP算法保障）
6. 支持等价负载均衡（通告的路由必须源自相同区域才能负载）
<!-- more -->
7. OSPF路由建立过程（OSPF邻居建立，OSPF拓扑表建立，OSPF路由计算）
8. 路由信息交互于路由计算分离 （OSPF路由建立过程不依赖邻居路由交互）
- 分区域部署：
1. 分区域的价值：减低SPF计算资源消耗;减小LSDB大小，加速OSPF收敛;降低拓扑的复杂性;本地化网络拓扑变化结构的影响;减小路由表大小
2. 区域分类：骨干区域（区域0），非骨干区域（普通区域和特殊区域）；非骨干区域之间必须通过骨干区域传递
3. 路由器分类：骨干路由器（BR）:至少存在一个接口都位于骨干区域； 内部路由器（IR）:所有接口都位于非骨干区域；区域边界路由器（ABR）:至少包含一个接口位于骨干区域；至少连接两个或以上区域；至少存在一个Full邻居关系位于骨干区域；自治系统边界路由器（ASBR）:与其他AS交换路由信息的设备称为ASBR 
- 查看路由器类型：
```bash
dis ospf abr-asbr
```

![命令](../imgs/2025.6.25-1.png)

4. OSPF路由类型：内部路由，区域间路由，外部路由 
5. 配置OSPF单区域：[单区域实验](https://symye.github.io/2025/06/25/OSPF%E5%8D%95%E5%A4%9A%E5%8C%BA%E5%9F%9F%E5%AE%9E%E9%AA%8C/)
6. 配置OSPF多区域：[多区域实验](https://symye.github.io/2025/06/25/OSPF%E5%8D%95%E5%A4%9A%E5%8C%BA%E5%9F%9F%E5%AE%9E%E9%AA%8C/)
- 多种网络类型
1. 适用不同的网络结构（例如部分互联网络）
2. 调整DR以及相关的邻居关系的结构

### OSPF邻居建立

- OSPF信息类型：Hello,DD,LSR,LSU,LSACK [报文详解](https://symye.github.io/2025/06/23/OSPF%E9%82%BB%E5%B1%85%E6%8A%A5%E6%96%87%E8%AF%A6%E8%A7%A3/)
- 邻居和邻接建立过程：[建立过程详解](https://symye.github.io/2025/06/25/OSPF%E9%82%BB%E5%B1%85%E5%92%8C%E9%82%BB%E6%8E%A5%E5%BB%BA%E7%AB%8B%E8%BF%87%E7%A8%8B%E8%AF%A6%E8%A7%A3/)
- OSPF表项：邻居表，拓扑表，路由表
- OSPF网络类型:
1. 部署目的：适应网络拓扑

![图文](../imgs/2025.6.25-2.png)

  NBMA网络类型需要互指邻居
2. 配置接口网络类型

![图文](../imgs/2025.6.25-3.png)

3. Next-hop变化
MA网络不变；P2MP会改变且产生32位主机路由
4. 查看接口OSPF网络类型

![图文](../imgs/2025.6.25-4.png)

5. 邻居建立与介质访问方式相关  
（展开讲解比较复杂）放在实验文章中，见链接[DR/BDR实验](https://symye.github.io/2025/06/25/DR%E5%92%8CBDR%E5%AE%9E%E9%AA%8C-%E7%BD%91%E7%BB%9C%E7%B1%BB%E5%9E%8B/)

- OSPF路由计算：
1. 基于带宽：计算公式（Metric=10^8/带宽（byte）
2. SPF算法

### OSPF LSA类型

- LSA实验拓扑：

![图文](../imgs/2025.6.25-5.png)

- 区域内LSA
1. LAS1（路由器LSA）  
   由每台区域内路由器产生（数量：1）  
   传递范围：区域内  
   描述内容：链路ID,接口IP或掩码，链路类型，链路开销  
   
2. LSA2(网络LSA)  
   描述MA网络连接的路由器与链路的掩码  
   由DR通告MA网络结构  
   传递范围：区域内  
   Link State ID：DR的IP地址    
   MA网络前缀：LS ID+Net mask

![图文](../imgs/2025.6.25-6.png)
  
3. LSA3（区域间LSA）  
   描述OSPF区域间前缀与掩码  
   传递范围：OSPF整个自制系统  
   ABR通告  
   ABR不传递通过非骨干区域接收的LSA3  
   查看LSA3

   ```bash
   dis ospf lsbd summary
   ```

![图文](../imgs/2025.6.25-7.png)

4. LSA4 (边界LSA)  
   描述抵达ASBR路由 ，即ASBR路由器ID  
   传递范围：OSPF自制系统  
   由ABR产生  
   查看LSA4
   
   ```bash
   dis ospf lsdb asbr
   ```
 ![图文](../imgs/2025.6.25-8.png)

5. LSA5（外部LSA）  
   描述非OSPF网络路由
   由ASBR产生
   传递范围：OSPF自制系统  
   查看LSA5

   ```bash
   dis ospf lsdb ase
   ```

 ![图文](../imgs/2025.6.25-9.png)

 6. LSA7  
   基于NSSA区域描述外部网络
   传递范围：NSSA区域
   ABR（较大路由器ID）执行LSA7->LSA5  
   查看LSA7

   ```bash
   dis ospf lsdb nssa
   ```
 ![图文](../imgs/2025.6.25-10.png)

- LSA同步
  1. LSA更新触发条件  
    网络拓扑变化：(1) 链路的UP/DOWN (2)链路开销改变  
    网络拓扑稳定：（1）周期更新 (2)：时间间隔：30min
  2. 接受到不存在的LSA  
    接受并泛洪此LSA  
    判定依据：LS类型，Link ID，通告者
  3. 接受到已存在的LSA  
    LSA区分机制：LSA ID ,LSA 类型，LSA通告者  
    判定依据：1.基于序列号  2.LSA生存时间 3.校验和  

### OSPF虚连接

- OSPF部署需求：OSPF要求所有非骨干区域必须与骨干区域保持连通，并且骨干区域之间也要保持连通。
- 穿越区域限制：不支持穿越NSSA，不支持穿越Stub，不支持穿越Area 0
- 在穿越区域的ABR之间设置
- 互指对方的路由器ID
- Vlink保障骨干区域连续与Vlink环路场景：[虚连接实验](https://symye.github.io/2025/06/25/OSPF%E8%99%9A%E8%BF%9E%E6%8E%A5/)
  
### OSPF路由

- OSPF路由类型
  1. OSPF本区域路由：Transit ，Stub ，Intra Area
  2. OSPF区域间路由：Inter-area
  3. OSPF外部路由  
       Type1：OSPF外部路由，沿途累加Metric  
       Type2(default)：OSPF外部路由默认类型，沿途不累加Metric  
       表示方式：O_ASE  
  4. OSPF NSSA区域引入外部路由：表示方式：O_NSSA
  5. 路由类型顺序：区域内路由>区域间路由>外部类型1(ON1,OE1)>外部类型2(ON2,OE2)  
       同种类型LSA7与LSA5路由比较  
       LSA5与LSA7比较实验：[实验](https://symye.github.io/2025/06/25/LSA5%E5%92%8CLSA7%E7%9A%84%E6%AF%94%E8%BE%83%E5%AE%9E%E9%AA%8C/)
- 外部路由引入： 
   alway：无需本地存在缺省路由，即可注入默认路由  
   metric：设置注入路由的度量值  
   metric-type：设置注入路由的类型（O E1, O E2） 
   默认外部类型2，度量值为1  


  





    





   


   



