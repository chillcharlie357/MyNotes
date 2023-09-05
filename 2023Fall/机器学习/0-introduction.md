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
title: 0-introduction
date:  Tuesday,September 5th 2023
modified:  Tuesday,September 5th 2023
---

学习：利用经验改善系统的性能

- 机器学习：现代特指统计机器学习
	- 任何通过数据训练的学习算法都属于机器学习

- 学习系统
	- 模型空间
	- 数据
	- 学习算法
	- 学得模型

# 1. 系统建模和模型选择

## 1.1. 常用术语和标记

输入：$x,x_i$  
权重： $W,w_{ij}$  
输出： $y,y(x,W)$  
目标：$t,t_j$  
误差：$E$

**维数灾难**：随着维数的增加，需要的数据也更多

## 1.2. 数据集

- 通常把数据集划分成三种
	- 训练集：训练模型的参数
	- 测试集
	- 验证集
- 划分方法
	- 留出法
	- 交叉验证法

## 1.3. 建模要素

模型/函数来源：按照先验做假设

- <font color="#ff0000">模型/映射</font>函数刻画：
- 确定目标/损失函数（如平方损失，交叉熵，凸与非凸）并获得优化模型
- 评测：泛化性能（举一反三能力）

# 2. 共性问题

过拟合（Over-fitting）：在训练样本上表现很好，测试样本误差反而上升  
欠拟合（Under-fitting）：在测试样本上学得不够  
好拟合(Good-fitting)

<font color="#ff0000">病态问题</font>：大量甚至无穷拟合函数/模型能满足给定的有限观察

## 2.1. 模型选择

mian'fei'w
模型选择：样本有限，先验甚少，因此所有建模没有对的，只有相对好的

- 方法
	1. 模型选择(直接选择)
	2. 正则化/规整化（给约束）
	3. 模型组合/集成
	4. 多视图（从数据角度）

为什么正则化可用？  
	意图：病态$\Rightarrow$良态  
	Tikhonov正则化

## 2.2. 评价指标

### 2.2.1. 混淆矩阵

- 混淆矩阵：
	- 真实类，预测类
	- 常用于多分类问题

在**二分类**问题中：

- 精度 Accuracy

$$
Accuracy = \frac{\#TP + \#TN}{\#TP + \#FP + \#TN + \#FN}
$$

- 查准 Precision

$$
$$

- 查全 Recall
- $F_1$度量 

$$
F_1 = 2 \frac{precision \times recall}{ precision + recall}
$$

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F09%2F05%2F7275e60fff263326a531c5d8539a68db_20230905201955.png)

### 2.2.2. ROC曲线

受试者工作特征曲线

True Positive R

$$
TPR=\frac{TP}{TP+FN}
$$

$$
FPR=\frac{FP}{FP+TN}
$$

### 2.2.3. 不平衡数据集

Matthew相关系数

$$
MCC=
$$

正例和反例相差巨大，数据特别不平衡

## 测量精度

- 系统可重复性：类似输入，产生相似的输出
- 物理意义类似概率分布中方差


## 先验的重要性

- 结合问题的先验去建模
	- 泛化=数据+知识
- 输入与输出映射应该光滑
	- 相似输入的输出应该相似

## 丑小鸭定理

没有天生好的特征，只有结合了问题域的知识才是好知识，与问题域有关
