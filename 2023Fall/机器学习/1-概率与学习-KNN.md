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

1. 计算所有样本$\overline{x}$和$D_{train}$中的训练样本$x_i$之间的距离$d(\overline{x},x_i)$
2. 对所有距离值（相似度）进行升序（降序）排序
3. 选择k个最邻近（距离最小，相似度最高）的训练样本
4. 采取投票法，将近邻中样本数最多的标签分配给$\overline{x}$

## k的影响

- k一般是**奇数**，避免平局
- k不同取值，结果不同
- k太小，对噪声敏感，模型整体变得复杂，容易过拟合
- k太大，对噪声不敏感，模型整体变得简单，容易欠拟合
# 最近邻分类器

# k-近邻回归

# 降低近邻计算