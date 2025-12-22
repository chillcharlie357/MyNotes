---
aliases: 
tags:
  - 2024_Spring_软件系统设计
  - 课程
categories: 2024_Spring_软件系统设计
sticky: 
thumbnail: 
cover: 
excerpt: false
mathjax: true
comment: true
title: 02-软件架构intro
date:  2024-04-16 14:04
modified:  2024-06-14 15:06
---

- 软件的四个本质难题
	1. 复杂度
	2. 一致性 
	3. 可变性 
	4. 不可见性：没有几何结构

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F04%2F16%2F14-49-29-00d7cb1dc073a5a241fb8794c7cb365d-20240416144928-c3fd7b.png)

# 架构来源

- Architecture:
	1. Component interfaces
	2. Component communications and dependencies
	3. Component responibilities
		1. 性能，可用......

- communication
	1. Data passing
	2. Control flow

## NFR

- 架构强调非功能需求NFR：how well a system works
- 包括
	1. 技术限制
	2. 商业限制
	3. 质量属性


# architecture views

有多个视图  
解决不可见

## 4+1 view model

Logical view ：架构的重要元素和他们之间的关系
Process view  ：
Physical view  
Development view

Architecture use cases: 四个视图是关于某个场景的

## generic design strategies

1. Decomposition
2. Abstraction
3. Stepwise: Divid and Conquer
4. Generate and Test，先生成一个设计，再对它测试
5. Iteration: Incremental Refinement
6. Reuseable elements，重用现有的设计

# what does a software architect do?

1. Liaison
	1. 调和用户、市场、开发等
2. software engineering
3. technology knowledge
4. risk management

# 架构设计过程

1. 识别和架构相关的重要需求
2. 架构设计
3. 文档化，以视图为中心
4. 架构评估