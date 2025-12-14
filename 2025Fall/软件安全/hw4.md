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
title: hw4
date:  2025-12-06 18:12
modified:  2025-12-14 13:12
---

# 第四次实验

## 题目

下载 Crackme 程序，综合运用 OllyDbg、IDA 和 UltraEdit 等工具进行注册登录功能的破解。完成实验报告。

## 实验过程

### 1. 下载crakme程序

![[assets/Pasted image 20251207130755.png]]

### 2. 使用xdg32打开

![[assets/Pasted image 20251207133425.png]]

### 3. 进行逆向

- 直接搜索字符串，发现“注册成功”注释  
![[assets/Pasted image 20251207133605.png]]

- 这行代码附件有一个跳转  
![[assets/Pasted image 20251207134008.png]]

- 把跳转修改为nop使其失效  
![[assets/Pasted image 20251207134924.png]]

- 注册成功  
![[assets/Pasted image 20251207135133.png]]