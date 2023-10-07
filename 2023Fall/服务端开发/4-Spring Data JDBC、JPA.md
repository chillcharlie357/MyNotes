---
aliases: 
tags:
  - 关系型数据库
  - Spring_Data
categories: 
sticky: 
thumbnail: 
cover: 
excerpt: false
mathjax: true
comment: true
title: 4-Spring Data JDBC、JPA
date:  Saturday,October 7th 2023
modified:  Saturday,October 7th 2023
---

持久化存储

**JDBC是JDK中的内容**，并非spring实现

# JDBCTemplate简化JDBC访问

依赖：`spring-boot-starter-jdbc`

## 使用原始的JDBC访问数据库

- `RawJdbcIngredientRepository`
- 提供样板代码，减少原始JDBC访问数据库时的重复工作
- **SQLException**,checked异常（即必须处理，否则编译器会报错）
	- runtimeExcetion可,unchecked,以不catch

## 异常体系

- SQLException
	- 发现异常很难恢复
	- 难以确定异常类型
- Hibernate异常
	- 定义了许多具体异常，方便定位
	- 对业务对象侵入
- <font color="#ff0000">Spring提供与平台无关的异常</font>
	- `DataAccessException`
	- 具体异常，方便定位
	- 隔离具体数据库

- Spring优点：
	- 不需要强制写try catch
	- 和底层细节解耦

## 使用JdbcTemplate

- 依赖
	- `spring-boot-starter-jdbc`
	- Jdbc驱动程序
		- 例子：h2,内存数据库


# Spring Data JDBC

# Spring Data JPA

JPA是另一个规范  
ORM：对象关系映射