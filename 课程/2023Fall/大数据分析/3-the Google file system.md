---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: 3-the Google file system
date:  Monday,September 25th 2023
modified:  Monday,September 25th 2023
---

# Goals of DFS

1. Performance
2. Scalability
3. Reliability
4. Availability

# Assumptions

1. 系统由大量廉价组件组成，经常失效
2. 存储大文件
3. 工作负载：大规模的流式读取&小规模的随机读取
4. 大量、顺序写入
5. 为多客户端实现良好定义的语义
6. 高带宽比低延迟更重要

## Interface

1. 目录的形式组织
	- no-POSIX
2. Snapshots：快速创建文件
3. Record appends：允许多个客户端同时追加数据，保证原子性

## GFS clusters

- master
- 很多chunkserver

周期性通过**心跳**与数据节点通信


## files

1. 文件被划分成64MB的chunk
	- 类似文件系统的聚簇
2. 每个chunk由64bit的标签
	- master创建文件时候分配
3. GFS维护logical mappings
4. chunks重复
	- 至少三个

## Single master

1. 减少主节点参与，避免成为瓶颈
	- chunkserver会缓存信息 
3. 客户端直接与chunkserver通信完成读写
4. 存储metadata
5. heartbeat message与chunkserver通信

