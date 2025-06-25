---
# IB网络笔记
layout: pags
title: IB网卡性能测试
date: 2025-06-23 15:23:54
tags: IB网络
categories: 
- InfiniBand
---

## IB网卡性能测试

- [参考链接1](https://blog.csdn.net/m0_37929348/article/details/106227581)
- [参考链接2](https://www.cnblogs.com/edenlong/p/10273433.html)

### 测试网卡
 CX6,最大速率200GB

### 服务端

```bash
ib_write_bw -a -d mlx5_0 --report_gbits
```
![命令](../imgs/2025.6.24-10.png)

### 客户端

其中 192.168.192.2 为服务端IB网卡的ip地址

```bash
ib_write_bw -a -F  192.168.192.2 -d mlx5_0 --report_gbits
```

![命令](../imgs/2025.6.24-11.png)

## IB诊断常用命令

### 查看IB网卡

```bash
lspci | grep Mell
```

![命令](../imgs/2025.6.24-12.png)

```bash
ibv_devices
```

![命令](../imgs/2025.6.24-13.png)

### 查看网络中IB设备，包括网卡和交换机

```bash
ibnodes
```

![命令](../imgs/2025.6.24-14.png)

### 查看IB网卡状态

```bash
ibstat
```
![命令](../imgs/2025.6.24-15.png)

```bash
ibstatus
```

![命令](../imgs/2025.6.24-18.png)

```bash
ibv_devinfo
```

![命令](../imgs/2025.6.24-16.png)

### 查看IB网卡的网口

```bash
ibdev2netdev
```

![命令](../imgs/2025.6.24-17.png)




