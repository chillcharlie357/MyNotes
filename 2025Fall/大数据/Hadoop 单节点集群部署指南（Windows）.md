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
modified:  2025-10-13 14:10
---

# Hadoop 单节点集群部署指南（Windows）

## 1 概述

本文档详细介绍如何在 Windows 系统上的WSL环境搭建 Hadoop 单节点集群，采用 Pseudo-Distributed Operation（伪分布式）模式。在此模式下，每个 Hadoop 守护进程运行在独立的 Java 进程中，模拟分布式环境，适用于开发和测试场景。

## 2 系统要求

### 2.1 平台要求

- Windows系统，使用WSL（Windows Subsystem for Linux）。

### 2.2 必需软件

#### 2.2.1 Java 环境

- **Java 版本**：推荐使用 OpenJDK 8 [1]
- **环境变量**：需要正确配置 `JAVA_HOME`

#### 2.2.2 SSH 服务

- **ssh**：用于远程连接和管理 Hadoop 守护进程
- **sshd**：SSH 守护进程必须运行
- **pdsh**：（可选）用于更好的 SSH 资源管理 [2]

#### 2.2.3 **Windows终端**

- 可从微软商店下载：[Windows Terminal - Windows官方下载 \| 微软应用商店 \| Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?hl=zh-CN&gl=CN)。

#### 2.2.4 **WSL**

- 安装方式：在Windows终端中输入`wsl --install`。
	- [安装 WSL \| Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)
- WSL默认安装在C盘，有需要可以迁移：[WSL从C盘迁移到其他盘区](https://zhuanlan.zhihu.com/p/621873601)。
- 使用默认的Ubuntu发行版即可。

## 3 环境准备

在WSL环境中执行以下操作：

### 3.1 更新系统包

```bash
# 更新包列表
sudo apt update

# 升级系统包
sudo apt upgrade -y
```

### 3.2 安装 Java

```bash
# 安装 OpenJDK 8
sudo apt install openjdk-8-jdk -y

# 验证 Java 安装
java -version
javac -version
```

### 3.3 配置 Java 环境变量

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

### 3.4 安装 SSH 服务

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

## 4 Hadoop安装

### 4.1 下载 Hadoop

```shell
# 创建 Hadoop 用户（推荐）
sudo adduser hadoop
sudo usermod -aG sudo hadoop

# 切换到 hadoop 用户
su - hadoop

# 确保在 hadoop 用户主目录进行下载
# 注意：Hadoop 程序文件建议安装在用户主目录，数据文件将存储在独立磁盘
cd /home/hadoop

# 下载 Hadoop 3.4.2（或最新稳定版本）
# 当前位置：/home/hadoop/ 目录
wget https://downloads.apache.org/hadoop/common/hadoop-3.4.2/hadoop-3.4.2.tar.gz

# 可以使用阿里云镜像
wget https://mirrors.aliyun.com/apache/hadoop/common/hadoop-3.4.2/hadoop-3.4.2.tar.gz

# 解压 Hadoop
tar -xzf hadoop-3.4.2.tar.gz

# 重命名目录为 hadoop（最终路径：/home/hadoop/hadoop）
mv hadoop-3.4.2 hadoop

# 设置 Hadoop 目录权限
sudo chown -R hadoop:hadoop ~/hadoop

# 清理下载的压缩包（可选）
rm hadoop-3.4.2.tar.gz
```

### 4.2 配置环境变量

```shell
# 编辑 .bashrc 文件
nano ~/.bashrc

# 添加以下内容到文件末尾
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# 重新加载 .bashrc
source ~/.bashrc

# 验证 Hadoop 安装
hadoop version
```

## 5 Hadoop 配置

## 6 SSH 无密码登录配置

## 7 验证安装

## 8 常用管理命令

## 9 故障排除

## 10 总结