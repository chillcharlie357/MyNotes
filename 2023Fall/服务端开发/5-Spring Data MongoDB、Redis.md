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
title: 5-Spring Data MongoDB、Redis
date:  Thursday,October 12th 2023
modified:  Thursday,October 12th 2023
---

# MongoDB

## 介绍

文档数据库，存储类Json文件。  
可以分布式存储。

|  SQL   | MongoDB    |解释     |
| --- | --- | --- |
|  database   |database     |  数据库   |
|  table   |  collection   |  数据库表/集合   |
|  row   |  document   |   数据库记录行/文档  |
|   column  |  field   |  数据字段/域   |

数据库document可以直接包含子document（在关系型数据库中只能创建子表），不同的document包含字段可能不一样。

## 使用

Id为String，mongoDB会自动生成

可以使用mongdb-shell交互

## Spring Data MongoDB👍

业务代码不需要修改

### 定义存储库

继承`CrudRepository<>`接口

### 领域类

1. 领域类加`@Document`
	- Collection默认名：类名，第一个字母改成小写
2. Id字段加`@Id`，需要定义为**String**类型
	- 因为String类型MongoDB可以自动创建并唯一赋值

- <font color="#ff0000">和关系型数据库主要区别</font>
	- List，可以把原来的子表放到同一个Collection里。
	- 可能会导致重复存储，但可以通过加索引等方法解决

## 在Spring中配置

在Appication配置文件中指定

`spring.data.mongodb.url=<url>`
## 性能对比

- 批量插入数据
	- 直接从client插入
	- Spring Data Mongodb插入

Spring插入时会自动创建一个`_class`字段
# Redis

## 介绍

- 分布式存储
- 存在内存里，常用于作缓存
- 可以持久化，但不太重要，不是主要用途
- 主从复制
- key-value的Hash表结构，区分大小写

可以设置key的存活时长


## Redis数据类型👍

指的是value类型
1. String
2. List
3. Hash
4. Set

在cilent内用`type`查看value类型
`lrang mylist 0 -1`：查看list内容
`lpush`,`rpush`插入数据