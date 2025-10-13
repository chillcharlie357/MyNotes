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
title: Hadoop 单节点集群部署指南（Windows）
date:  2025-10-13 11:10
modified:  2025-10-13 21:10
---

# Hadoop 单节点集群部署指南（Windows）

## 1 概述

本文档详细介绍如何在 Windows 系统上的WSL环境搭建 Hadoop 单节点集群，采用 Pseudo-Distributed Operation（伪分布式）模式。在此模式下，每个 Hadoop 守护进程运行在独立的 Java 进程中，模拟分布式环境，适用于开发和测试场景。

## 2 系统要求

### 2.1 平台要求

- Windows系统，使用WSL（Windows Subsystem for Linux）。

### 2.2 必需软件

#### 2.2.1 **Windows终端**

- 可从微软商店下载：[Windows Terminal - Windows官方下载 \| 微软应用商店 \| Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?hl=zh-CN&gl=CN)。

#### 2.2.2 **WSL**

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

### 5.1  配置 Hadoop 环境

编辑 `$HADOOP_HOME/etc/hadoop/hadoop-env.sh` 文件：

```shell
# 编辑 hadoop-env.sh
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh

# 添加或修改以下行
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

### 5.2 配置核心组件

#### 5.2.1 配置 core-site.xml

```shell
# 编辑 core-site.xml
nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

添加以下配置内容：

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
        <description>默认文件系统 URI</description>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/mnt/hadoop/tmp</value>
        <description>Hadoop 临时目录（使用独立磁盘避免填满根分区）</description>
    </property>
</configuration>
```

#### 5.2.2 配置 hdfs-site.xml

```shell
# 编辑 hdfs-site.xml
nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

添加以下配置内容：

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
        <description>数据块副本数量（单节点设置为1）</description>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/mnt/hadoop/data/namenode</value>
        <description>NameNode 数据存储目录（使用独立磁盘）</description>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/mnt/hadoop/data/datanode</value>
        <description>DataNode 数据存储目录（使用独立磁盘）</description>
    </property>
</configuration>
```

#### 5.2.3 配置 mapred-site.xml

```shell
# 编辑 mapred-site.xml
nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

添加以下配置内容：

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
        <description>MapReduce 框架名称</description>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
        <description>ApplicationMaster 环境变量</description>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
        <description>Map 任务环境变量</description>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/home/hadoop/hadoop</value>
        <description>Reduce 任务环境变量</description>
    </property>
</configuration>
```

**重要提示**：

- 配置文件中只能有一个 `<configuration>` 标签
- 请根据实际的 Hadoop 安装路径调整 `HADOOP_MAPRED_HOME` 的值
- 必须使用绝对路径

#### 5.2.4 配置 yarn-site.xml

```shell
# 编辑 yarn-site.xml
nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

添加以下配置内容：

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
        <description>NodeManager 辅助服务</description>
    </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>localhost</value>
        <description>ResourceManager 主机名</description>
    </property>
</configuration>
```

### 5.3 准备磁盘和创建必要目录

#### 5.3.1 挂载独立磁盘（开发测试环境）

- 安装XFS工具集

```Shell
%% 安装XFS 文件系统的工具集 %%
sudo apt install xfsprogs -y
```

- 在WSL 内创建loop device，并挂载到/mnt/hadoop

```PowerShell
# WSL 内创建 5GB 文件
fallocate -l 5G ~/hadoop_disk.img

# 挂载到 loop 设备
sudo losetup -fP ~/hadoop_disk.img

# 查看 loop 设备
lsblk

# 格式化 XFS
sudo mkfs.xfs /dev/loop0

# 确保磁盘已挂载到 /mnt/hadoop（使用 xfs 优化选项）
sudo mkdir -p /mnt/hadoop
sudo mount -t xfs -o noatime,nodiratime,logbufs=8,logbsize=32k /dev/loop0 /mnt/hadoop

# 验证挂载状态和文件系统类型
df -h /mnt/hadoop
mount | grep /mnt/hadoop

# 设置挂载点权限
sudo chown -R $USER:$USER /mnt/hadoop
sudo chmod 755 /mnt/hadoop
```

#### 5.3.2 创建 Hadoop 数据目录

```shell
# 创建 Hadoop 数据目录（使用独立磁盘）
mkdir -p /mnt/hadoop/data/namenode
mkdir -p /mnt/hadoop/data/datanode
mkdir -p /mnt/hadoop/tmp

# 设置目录权限
chmod 755 /mnt/hadoop/data/namenode
chmod 755 /mnt/hadoop/data/datanode
chmod 755 /mnt/hadoop/tmp
```

## 6 SSH 无密码登录配置

### 6.1 生成 SSH 密钥

```shell
# 生成 SSH 密钥对
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

# 将公钥添加到授权文件
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

# 设置正确的权限
chmod 0600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

### 6.2 测试 SSH 连接

```shell
# 测试无密码 SSH 连接
ssh localhost

# 如果成功，应该能够无密码登录
# 退出 SSH 会话
exit
```

## 7 启动 Hadoop 集群

### 7.1 格式化 HDFS

```shell
# 验证数据目录是否存在
ls -la /mnt/hadoop/data/

# 格式化 NameNode（仅在首次启动时执行）
hdfs namenode -format

# 确认格式化操作，输入 'Y' 并按回车
# 格式化成功后会在 /mnt/hadoop/data/namenode 目录下创建相关文件
```

### 7.2 启动 HDFS 服务

```shell
# 启动 HDFS 守护进程
start-dfs.sh

# 验证进程是否启动
jps
```

预期输出应包含：

- NameNode
- DataNode
- SecondaryNameNode

### 7.3 启动 YARN 服务

```shell
# 启动 YARN 守护进程
start-yarn.sh

# 再次验证进程
jps
```

预期输出应包含：

- NameNode
- DataNode
- SecondaryNameNode
- ResourceManager
- NodeManager

## 8 验证安装

### 8.1 Web 界面访问

#### 8.1.1 本地访问

如果您在本地机器上部署 Hadoop，可以直接访问以下地址：

- **HDFS Web UI**：[http://localhost:9870/](http://localhost:9870/)
- **YARN Web UI**：[http://localhost:8088/](http://localhost:8088/)

#### 8.1.3 Web UI 功能说明

- **HDFS Web UI**：查看 HDFS 状态、文件系统信息、DataNode 状态等
- **YARN Web UI**：查看 YARN 集群状态、应用程序信息、资源使用情况等

### 8.2 系统状态检查

```shell
# 检查磁盘使用情况
df -h /mnt/hadoop

# 检查 Hadoop 数据目录
ls -la /mnt/hadoop/data/

# 查看 HDFS 状态报告
hdfs dfsadmin -report
```

### 8.3 HDFS 基本操作测试

```shell
# 创建用户目录
hdfs dfs -mkdir -p /user/hadoop

# 创建测试目录
hdfs dfs -mkdir /user/hadoop/input

# 上传测试文件
hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml /user/hadoop/input

# 查看上传的文件
hdfs dfs -ls /user/hadoop/input

# 查看文件内容
hdfs dfs -cat /user/hadoop/input/core-site.xml
```

### 8.4 运行 MapReduce 示例

**重要提醒**：如果您在配置 `mapred-site.xml` 时添加了 `HADOOP_MAPRED_HOME` 环境变量，需要重启 Hadoop 服务：

```shell
# 停止所有服务
stop-all.sh

# 启动所有服务
start-all.sh

# 验证服务状态
jps
```

然后运行 MapReduce 示例：

```shell
# 运行 grep 示例程序
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.4.2.jar grep /user/hadoop/input /user/hadoop/output 'dfs[a-z.]+'

# 查看输出结果
hdfs dfs -cat /user/hadoop/output/*

# 将结果下载到本地
hdfs dfs -get /user/hadoop/output output
cat output/*
```

