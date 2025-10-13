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
title: Hadoop 单节点集群部署指南（WSL）
date:  2025-10-13 11:10
modified:  2025-10-13 11:10
---

# Hadoop 单节点集群部署指南（WSL）

## 系统要求

### 平台要求

- Windows系统，使用WSL（Windows Subsystem for Linux）。

#### 2.3 必需软件

##### 2.3.1 Java 环境

- **Java 版本**：推荐使用 OpenJDK 8 [1]
- **环境变量**：需要正确配置 `JAVA_HOME`

##### 2.3.2 SSH 服务

- **ssh**：用于远程连接和管理 Hadoop 守护进程
- **sshd**：SSH 守护进程必须运行
- **pdsh**：（可选）用于更好的 SSH 资源管理 [2]

### 软件安装

- **Windows终端**
	- 可从微软商店下载：[Windows Terminal - Windows官方下载 \| 微软应用商店 \| Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?hl=zh-CN&gl=CN)。
- **WSL**
	- 安装方式：在Windows终端中输入`wsl --install`。
		- [安装 WSL \| Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)
	- WSL默认安装在C盘，有需要可以迁移：[WSL从C盘迁移到其他盘区](https://zhuanlan.zhihu.com/p/621873601)。
	- 使用默认的Ubuntu发行版即可。
- **Java环境**
