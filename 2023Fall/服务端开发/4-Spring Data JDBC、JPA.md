---
aliases: 
tags:
  - 关系型数据库
  - Spring_Data
  - 2023_Fall_服务端开发
  - 课程
categories: 2023_Fall_服务端开发
sticky: 
thumbnail: 
cover: 
excerpt: false
mathjax: true
comment: true
title: 4-Spring Data JDBC、JPA
date:  2023-10-10 18:10
modified:  2023-12-31 15:12
---

# 1. 数据库访问层的开发👍

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F31%2F14-28-47-548c2ef0125be168f0ca3fc94bf0437e-20231231142846-73f243.png)

基于接口，可以自己实现也可以让spring实现。

- <font color="#c00000">优点</font>：
	1. 方便mock测试
	2. 可以灵活替换实现，不需要改变业务逻辑层

# 2. JDBCTemplate简化JDBC访问

**JDBC是JDK中的内容**，并非spring实现

1. 依赖：`spring-boot-starter-jdbc`
2. 提供SQL语句
3. 把查询结果转成java对象

## 2.1. 使用原始的JDBC访问数据库

- `RawJdbcIngredientRepository`
- 提供样板代码（ResultSet、PreparedStatement、Connection）
- **SQLException**,checked异常（即必须处理，否则编译器会报错）
	- runtimeExcetion可,unchecked,以不catch

## 2.2. 异常体系

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

## 2.3. 使用JdbcTemplate

- 依赖
	- `spring-boot-starter-jdbc`
	- Jdbc驱动程序
		- 例子：h2,内存数据库
- <font color="#ff0000">需要scheme.sql脚本定义表结构，data.sql-数据初始化</font>
- Spring优点：
	- 不需要强制写try catch
	- 和底层细节解耦
	- 减少原始JDBC访问数据库时的重复工作

`spring-jdbc`提供了一个 [`GeneratedKeyHolder`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/support/GeneratedKeyHolder.html) 类，通过它可以获取到自增ID的值。

```java
public long getUsers() {
    return jdbcTemplate.queryForObject("SELECT COUNT(*) FROM users", (ResultSet rs, int rowNum) -> {
        // SELECT COUNT(*)查询只有一列，取第一列数据:
        return rs.getLong(1);
    });
}
```

# 3. Spring Data JDBC

属于Spring Data项目，和上面的JDBC不一样。进一步简化，只需要提供接口。

## 3.1. 异同

- 异
	- 只定义了一个接口
	- `CrudRepository`
- 同
	- 需要自己创建表（<font color="#ff0000">scheme.sql</font>脚本定义表结构）,data.sql初始化数据

## 3.2. 步骤

1. 添加依赖
2. 定义存储库接口
3. 为领域类添加持久化注解

## 3.3. 存储库接口

1. Spring Data会在运行时**自动生成存储库接口的实现**。但是，只有当**接口扩展自Spring Data提供的存储库接口**时，它才会帮我们实现这一点。
2. Repository接口是<font color="#ff0000">参数化</font>的，其中第一个参数是该存储库要**持久化的对象类型**；第二个参数是要持久化对象的**ID字段的类型**。

```java
public interface IngredientRepository extends Repository<Ingredient, String> {
  Iterable<Ingredient> findAll();

  Optional<Ingredient> findById(String id);

  Ingredient save(Ingredient ingredient);

}
```

```java
public interface IngredientRepository extends CrudRepository<Ingredient, String> {
}
```

**CrudRepository**接口包含了增删改查等基础操作  
当应用启动的时候，Spring Data会在运行时自动生成一个实现。这意味着存储库已经准备就绪，我们将其注入控制器就可以了。

## 3.4. 为领域类添加持久化注解

1. 涉及为标识属性**添加@Id**，以让Spring Data知道哪个字段代表了对象的唯一标识
2. **可选**：在类上添加@Table注解
	- 默认情况下，对象会基于领域类的名称映射到数据库的表上。在本例中，TacoOrder会映射至名为“Taco_Order”的表。

```java
@Data
@Table
public class TacoOrder implements Serializable {

  private static final long serialVersionUID = 1L;

  @Id
  private Long id;

 // ...

}
```

- 指定表名：

```java
@Table("Taco_Cloud_Order")
public class TacoOrder {
  ...
}
```

- 列名
	- 默认驼峰映射到下划线
- `@Column(<name>)`指定列名

## 3.5. CommandLineRunner预加载数据

Spring Boot提供了两个非常有用的接口，用于在应用启动的时候执行一定的逻辑，即CommandLineRunner和ApplicationRunner。

# 4. Spring Data JPA

ORM：对象关系映射

JPA：Java Persistence API ，另一个规范不同厂家有不同实现  
JPA宗旨是为POJO提供持久化标准范围  
JPQL是一种面向对象的查询语言

## 4.1. 步骤

1. 添加依赖
2. 为领域类添加`@Entity`注解
3. 声明JPA存储库

<font color="#c00000">不需要scheme脚本，可以根据java对象自动创建数据库表</font>

## 4.2. 领域类标注为实体

1. 为了将Ingredient声明为JPA实体，它必须添加@Entity注解
2. id属性需要使用@Id注解，以便于将其指定为数据库中唯一标识该实体的属性

```java
@Data
@Entity
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PRIVATE, force = true)
public class Ingredient {

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private String id;
  private String name;
  private Type type;
  public enum Type {
    WRAP, PROTEIN, VEGGIES, CHEESE, SAUCE
  }
}
```

- `@GeneratedValue(strategy = GenerationType.AUTO)`
	- 依赖数据库自动生成ID值

```java
@Data
@Entity
public class TacoOrder implements Serializable {

  private static final long serialVersionUID = 1L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;

  private Date placedAt = new Date();

  ...

  @OneToMany(cascade = CascadeType.ALL)
  private List<Taco> tacos = new ArrayList<>();

  public void addTaco(Taco taco) {
    this.tacos.add(taco);
  }

}
```

- `@OneToMany`
	- 一对多映射
	- 表明这些taco都属于这一个订单。除此之外，cascade属性设置成了CascadeType.ALL，因此在删除订单的时候，所有关联的taco也都会被删除

## 4.3. 声明JPA存储库

借助Spring Data JDBC，我们可以省略掉显式的实现类，**只需扩展CrudRepository接口**。实际上，CrudRepository同样适用于Spring Data JPA。

```java
public interface IngredientRepository extends CrudRepository<Ingredient, String> {

}
```

## 4.4. 自定义JPA存储库

### 4.4.1. DSL

- Spring Data定义了一组小型的**领域特定语言(Domain-Specific Language,DSL)**，在这里，持久化的细节都是通过存储库方法的签名来描述的。
- **存储库的方法**由一个**动词**、一个**可选的主题(subject)**、**关键词By**，以及一个**断言**组成。
	- 常用动词：get、read、find、count

- 例子:

```java
List<TacoOrder> findByDeliveryZip(String deliveryZip);
```

在findByDeliveryZip()这个样例中，动词是find，断言是DeliveryZip，主题并没有指定，暗含的主题是TacoOrder。

### 4.4.2. JPQL

- `@Query`
	- 在查询语句中写SQL语句

```java
@Query("Order o where o.deliveryCity = 'Seattle'")
List<TacoOrder> readOrdersDeliveredInSeattle();
```

- 同样适用于Spring DataJDBC,但存在以下差异
	1. 在@Query中声明的必须全部是**SQL查询**，不允许使用JPA查询
	2. 所有的自定义方法都需要使用@Query。这是因为，与JPA不同，我们没有映射元数据帮助Spring Data JDBC根据方法名自动推断查询。

## 4.5. 数据访问对象模拟

常用工具**Mockito**

- <font color="#ff0000">业务层依赖接口</font>（依赖倒置）
	- 接口实现可以替换不需要修改业务层
	- 方便测试

# 5. 三种方法区别、相同点👍

1. 数据表生成：1、2需要scheme脚本，3不需要（根据领域类自动生成）
2. 数据库访问层：1需要自己实现接口，2、3不需要
3. 领域类注解：1不需要为领域类加注解，2、3要为领域类加注解（提供领域类和表结构的映射关系）
	- 2： @Id
	- 3: @Entity, @Id
4. 自定义查询：2、3都可以使用@Querry定义查询逻辑，但3还可以使用基于方法名的DSL自定义查询
5. ID字段的处理：1需要手动获取数据库生成的Id，2、3不需要
6. 存储库接口：2、3都继承自CrudRepository接口
7. 包路径：2、3为领域类添加持久化的注解包路径不一样
	- JPA中的规范注解都来自javax.persisitence.* ，因为不是Spring自己实现
	- @Table，对象会基于领域类的名称映射到数据库的表上
	- @Id
		- 有两个来自不同包的@Id，主义区别
	- @Column

# 6. 数据表创建和初始化

三种方式，java代码初始化有两种。

### 6.1. 脚本

schema.sql表创建  
data.sql数据初始化

### 6.2. java代码初始化

CommandLineRunner接口  
ApplicationRunner接口
