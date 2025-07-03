---
# HCIP-BGP
layout: pags
title: BGP调优
date: 2025-07-01 12:29:54
tags: Network
categories: 
- [HCIP,4.3BGP调优] 
---

### BGP路由控制

#### 1.BGP路由过滤

支持入方向或出方向      
过滤方法        
1. BGP正则表达式        
   1. 正则表达式为一种字符串匹配的模式
   2. 由普通字符（例如a到z，0-9）和特殊字符（例如？，#）组成
   3. 特殊字符表
 <!-- more -->
![命令](../imgs/BGP/正则表达式.png)

2. 使用前缀列表


