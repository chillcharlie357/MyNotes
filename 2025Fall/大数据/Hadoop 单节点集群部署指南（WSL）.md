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

## 1 系统要求

### 1.1 平台要求

- Windows系统，使用WSL（Windows Subsystem for Linux）。

### 1.2 必需软件

#### 1.2.1 Java 环境

- **Java 版本**：推荐使用 OpenJDK 8 [1]
- **环境变量**：需要正确配置 `JAVA_HOME`

#### 1.2.2 SSH 服务

- **ssh**：用于远程连接和管理 Hadoop 守护进程
- **sshd**：SSH 守护进程必须运行
- **pdsh**：（可选）用于更好的 SSH 资源管理 [2]

#### 1.2.3 **Windows终端**

- 可从微软商店下载：[Windows Terminal - Windows官方下载 \| 微软应用商店 \| Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?hl=zh-CN&gl=CN)。

#### 1.2.4 **WSL**

- 安装方式：在Windows终端中输入`wsl --install`。
	- [安装 WSL \| Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)
- WSL默认安装在C盘，有需要可以迁移：[WSL从C盘迁移到其他盘区](https://zhuanlan.zhihu.com/p/621873601)。
- 使用默认的Ubuntu发行版即可。

## 2 环境准备

### 2.1 ### 3.1 更新系统包

```bash
# 更新包列表
sudo apt update

# 升级系统包
sudo apt upgrade -y
```

### 2.2 安装 Java

```bash
# 安装 OpenJDK 8
sudo apt install openjdk-8-jdk -y

# 验证 Java 安装
java -version
javac -version
```

### 2.3 配置 Java 环境变量

```bash
# 查找 Java 安装路径
sudo find /usr -name "java" -type f 2>/dev/null | grep bin

# 编辑环境变量文件
sudo nano /etc/environment

# 添加以下内容（根据实际路径调整）
JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

# 重新加载环境变量
source /etc/environment

# 验证 JAVA_HOME
echo $JAVA_HOME
```

### 2.4 安装 SSH 服务

```bash
# 安装 SSH 和 pdsh
sudo apt-get install ssh -y
sudo apt-get install pdsh -y

# 启动 SSH 服务
sudo systemctl start ssh
sudo systemctl enable ssh

# 验证 SSH 服务状态
sudo systemctl status ssh

# 配置 PDSH 使用 SSH（重要：避免启动 Hadoop 服务时出错）
echo 'export PDSH_RCMD_TYPE=ssh' >> ~/.bashrc
source ~/.bashrc

```

## Hadoop安装

## 5. Hadoop 配置

## 6. SSH 无密码登录配置

## 8. 验证安装

## 9. 常用管理命令

## 10. 故障排除

## 13. 总结