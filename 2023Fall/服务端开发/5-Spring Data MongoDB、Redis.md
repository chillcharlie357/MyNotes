---
aliases: 
tags:
  - 2023_Fall_服务端开发
  - 课程
categories: 2023_Fall_服务端开发
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: 5-Spring Data MongoDB、Redis
date:  2023-10-12 18:10
modified:  2023-12-31 15:12
---

# 1. MongoDB

## 1.1. 介绍

文档数据库，存储类Json文件。  
可以分布式存储。

|  SQL   | MongoDB    |解释     |
| --- | --- | --- |
|  database   |database     |  数据库   |
|  table   |  collection   |  数据库表/集合   |
|  row   |  document   |   数据库记录行/文档  |
|   column  |  field   |  数据字段/域   |

数据库document可以直接包含子document（在关系型数据库中只能创建子表），不同的document包含字段可能不一样。

## 1.2. 使用

- Id为String，mongoDB会自动生成
- 可以使用mongdb-shell交互

## 1.3. Spring Data MongoDB👍

业务代码不需要修改

### 1.3.1. 定义存储库

继承`CrudRepository<>`接口

### 1.3.2. 领域类

1. 领域类加`@Document`
	- Collection默认名：类名，第一个字母改成小写
2. Id字段加`@Id`，需要定义为**String**类型
	- 因为String类型MongoDB可以自动创建并唯一赋值

- <font color="#ff0000">和关系型数据库主要区别</font>
	1. List，可以把原来关系型数据库的子表放到同一个Collection里。document可以直接包含子document。
	2. 可能会导致重复存储，但可以通过加索引等方法解决

## 1.4. 在Spring中配置

在Appication配置文件中指定

`spring.data.mongodb.url=<url>`

## 1.5. 性能对比

- 批量插入数据
	- 直接从client插入
	- Spring Data Mongodb插入

Spring插入时会自动创建一个`_class`字段

# 2. Redis

## 2.1. 介绍👍

- 分布式存储
- 内存数据库，存在内存里，常用于作**缓存**
- 可以持久化，但不太重要，不是主要用途
- 可以集群式部署
	- <font color="#c00000">主从复制</font>，主机写，从机并发读
- <font color="#c00000">key-value的Hash表结构，区分大小写</font>

## 2.2. Redis命令

可以设置key的存活时长  
默认创建16个数据库

flushdb：deletes the keys in a database  
flushall：deletes all keys in all databases

## 2.3. Redis数据类型👍

指的是**value类型**

1. String
2. List
3. Hash
4. Set
5. ......

在cilent内：  
`type`查看value类型  
`lrang mylist 0 -1`：查看list内容  
`lpush`,`rpush`插入数据  
`sadd mysqet 1 2 3 4`：add set  
`smember myset`：查看set内容

## 2.4. Redis客户端

- Jedis：早期Spring集成
- Lettuce：目前Spring采用的客户端

## 2.5. Spring Data Redis

- **RedisConnectionFactory接口**
	- Spring Boot会自动创建，直接注入即可。
	- 连接需要提供Redis Server的地址、端口、用户和密码等信息
		- 这些信息在Application文件配置

- **通过`RedisTemplate`模板对象访问Redis**
	- RedisTemplate<Key,Value>

```java
public RedisTemplate<String, Product> redisTemplate(RedisConnectionFactory cf){
	RedisTemplate<String,Product> redis = new RedisTemplate();
	redis.setFactory(cf);
	return redis;
}
```

- RedisTemplate的子API: 对应不同的value类型
	- 使用简单的值： opForValue()
	- lIst类型的值：opForList()
		- rigjtPush, leftPop(), range
	- Set上的操作: opForSet()
		- add, union,
	- 绑定到某个key上
		- BoudListOperations绑定key，不需要每个操作都写一遍

```java
 BoudListOperations cart = redisTemplate.boundListOp(''cart")
```

[12.3.2　使用 Redis Template - Spring 实战(第四版)](https://potoyang.gitbook.io/spring-in-action-v4/untitled-6/untitled-1/12.3.2-shi-yong-redis-template)

## 2.6. 序列化👍

<font color="#c00000">mongodb没有这个要求，因为它会把所有值都转成json串</font>。  
redis默认使用JDK序列化，但JSON序列化更常用。

### 2.6.1. JDK序列化

- 存储java对象需要序列化
	- 即持久化
- **必须需要实现`Serializable`接口**
- 把java对象通过key-value读出来，反序列化

做得很失败，性能也不好

### 2.6.2. JSON序列化

```java
redis.setKeySerializer(new StringRedisSerializer())
redis.setVAlueSerializer(new Jackson2JsonRedisSerializer<Product.class>)
```

1. Key比较简单，可以直接用String序列化
2. 指定Value使用`Jackson2JsonRedisSerializer`进行序列化

对数据存取操作的代码没有影响。  
<font color="#c00000">可读性更好。 </font>  
直接使用**RedisTemplate**获取的Value会经过反序列化，仍然为Java对象  
如果想要获取String，可以使用**StringRedisTemplate**读取Value。
