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
title: Untitled
date:  2024-06-03 14:06
modified:  2024-06-03 14:06
---

# 编程

SQL 3选2，标注使用哪个数据库

- 常见函数：
	1. 字符串replace
	2. 数值计算，考虑空值，平均值空值
	3. 日期，使用索引的三种写法
- 递归查询，withas
	- 起始体，union all,递归体，视图
- 外连接+数值计算

结构合理、关键字、函数都有分




# 论述：

1. 索引的叶节点
	- 结构：N个节点，N+1个link
		- 考试一般3扇出
	- 节点分裂/合并
		- 页节点和内部节点
		- 向上传递
2. 日志
	- Redo
	- Undo
	- 区别，实现，问题
3. 分区，分表，分库
	- 原因
	- 解决的问题
	- 带来新的问题
4. SQL解释器：**可能会考一个基于成本优化器的成本计算方式**
	1. 优化的基本逻辑
	2. 集成成本的优化器
	3. 基于规则的优化器
5. 建议和想法