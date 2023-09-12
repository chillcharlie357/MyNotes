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
title: 1-概率与学习-KNN
date:  Tuesday,September 12th 2023
modified:  Tuesday,September 12th 2023
---

# 回顾

## 特征和属性

**统计学角度**看机器学习：得到映射函数$x\rightarrow y$

将原始数据通过**特征工程**（手工/自动）得到**特征向量**x,再求**映射**函数$x\rightarrow y$，得到**标签**y  
**属性**：特征向量的每个维度

## 分类问题和回归问题

分类：输出离散值，找到一个决策边界  
回归：输出连续值

## 距离度量/相似性度量

[[2023Fall/机器学习/0-introduction#距离度量函数|距离度量]]

机器学习中大多是向量运算。
# k-近邻分类器

## 算法流程

1. 计算**测试样本**$\overline{x}$和$D_{train}$中的**训练样本**$x_i$之间的距离$d(\overline{x},x_i)$
2. 对所有距离值（相似度）进行升序（降序）排序
3. 选择k个最邻近（距离最小，相似度最高）的训练样本
4. **采取投票法**，将近邻中样本数最多的标签分配给$\overline{x}$

## k的影响

- k一般是**奇数**，避免平局
- k不同取值，结果不同
- k太小，对噪声敏感，模型整体变得复杂，容易过拟合
- k太大，对噪声不敏感，模型整体变得简单，容易欠拟合
# 最近邻分类器

## 算法流程

k近邻的简化，选取最近的一个

1. 计算测试样本$\overline{x}$和$D_{train}$中的训练样本$x_i$之间的距离$d(\overline{x},x_i)$
2. 对所有距离值（相似度）进行升序（降序）排序
3. 选择**最近的**那个训练样本
4. 标签分配给$\overline{x}$

## 泛化错误率

虽然简单，但错误率不超过贝叶斯分类器的两倍
# k-近邻回归

## 算法流程

1. 计算测试样本$\overline{x}$和$D_{train}$中的训练样本$x_i$之间的距离$d(\overline{x},x_i)$
2. 对所有距离值（相似度）进行升序（降序）排序
3. 选择k个最邻近（距离最小，相似度最高）的训练样本
4. 将**距离值倒数**作为权重，然后求**k个最近邻的标签加权平均**，作为$\overline{x}$的预测值

和分类器主要**区别**在于回归输出是连续值。

## 近邻平滑

加权平均plus，得到很好的回归模型。

- 核平滑法
	- 二次核
	- 次方核
	- 高斯核

## 讨论

- k-NN是典型的“懒惰学习”(lazy)
	- 训练阶段仅仅把数据收集起来，**训练开销时间为零**，带收集到测试样本后再开始处理
- KVM、CNN是典型的“急切学习”(eager learning)
	- 训练开销很大，但是测试时可以直接运行。尝试在训练阶段构造一个通用的、与输入无关的目标函数。

### 优缺点

- 优点：
	- 精度高
	- 对异常值不敏感（k取值不一样，设置更大的k可以减少异常值的影响）
	- 无数据输入假定（无训练阶段）
- 缺点
	- 计算复杂度高
	- 空间复杂度高

### 时间复杂度

假设$d(x_i,x_j)$是欧氏距离，时间复杂度$O(d)$
训练阶段：$0$
测试阶段：$O(nd+nlogk)$

[What would be the time complexity to find top K elements in an unsorted array of size N using MaxHeap (of size N) and MinHeap (of size K) and which one would be more efficient? - Quora](https://www.quora.com/What-would-be-the-time-complexity-to-find-top-K-elements-in-an-unsorted-array-of-size-N-using-MaxHeap-of-size-N-and-MinHeap-of-size-K-and-which-one-would-be-more-efficient)

Top k问题时间复杂度：

[algorithm - Find the top K elements in O(N log K) time using heaps - Stack Overflow](https://stackoverflow.com/questions/49217910/find-the-top-k-elements-in-on-log-k-time-using-heaps)
# 降低近邻计算


- 维度2-5：维诺图
- 维度6-30：KD-Tree
- 高维特征：
	- 降维算法：如PCA
	- 近似最邻近（ANN）
	- 哈希

## 维诺图

根据一组给定的目标，将一个平面分成靠近每一个目标的多个区块。


维诺单元定义$R_k$
假设X是一个点集


### 查询测试



## KD-Tree

对K维空间中的实例点进行存储一便对其快速检索的树状数据结构

KD树是二叉树，表示对k维空间的一个划分。

[Introduction to K-D Trees | Baeldung on Computer Science](https://www.baeldung.com/cs/k-d-trees)

### 构造


## 降维

## 近似最近邻ANN

flann

## 哈希

- 把任意长度的输入映射成固定长度的输入



概率化的k-NN