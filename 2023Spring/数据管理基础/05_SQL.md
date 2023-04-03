---
title: 05_SQL
tags: 2023_Spring数据管理基础 课程
categories: 2023_Spring数据管理基础
date:  2023-03-16 10:26
modified:  星期一 3日 四月 2023 15:31:12
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
---



- **SQL（Structured Query Language）** 结构化查询语言，是关系数据库的标准语言
 是一个ISO标准，变种和版本极多，概念和实际实现也有差异
- 本课程使用MySQL，环境无所谓



# 特点
- 综合统一
- 高度非过程化
	- 看不见内模式，不知道具体操作的步骤
	- 只需要告诉SQL**做什么**
- 面向集合的操作方式
	- 所有的分量、标量都是集合
	- nullptr=>空集
	- 4=>一个元素的集合
- 以同一种语法结构提供两种使用方法
	- 独立语言
	- 嵌入式语言，嵌入高级语言（C++,java...）
- 语言简洁，易学易用（？）
	- 核心功能9个动词

## SQL与数据库三级模式
不自由，但确保不会出错



# 数据定义
ch12
## 层次化的数据库对象命名机制
- 一个关系数据库管理系统的实例（Instance）中可以建立多个数据库
- 一个数据库中可以建立多个模式
	- 类比：一个用户多个文件夹
	- 一个数据库也可能只有一个模式
- 一个模式下通常包括多个表、视图和索引等数据库对象
	- 表：文件 
	- 视图：文件的分身
	- 索引：加速查找
![POWERPNT_319_208.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/09/80de805b7fb0bde22482a9b302292210_POWERPNT_319_208.png)
## 数据定义
### 定义
- `CREATE SCHEMA <模式名> AUTHORIZATION <用户名>  [<表定义子句>| <视图定义子句>|<授权定义子句>]`

- 模式定义
	- 模式==>namespace
- 表定义
- 视图和索引定义

### 删除
- `DROP SCHEMA <模式名> <CASCADE|RESTRICT>`
- 级联CASCADE
	- eg: windows删除文件夹递归删除
	- 删除模式的同时把该模式中所有的数据库对象全部删除
- 限制RESTRICT
	- eg: linux非空文件夹无法直接rm
	- 如果该模式中定义了下属的数据库对象（如表、视图等），则拒绝该删除语句的执行
	- 仅当该模式中没有任何下属的对象时才能执行

限制考虑安全性，级联考虑便利性

### 定义基本表

CREATE TABLE <表名>
      (<列名> <数据类型>[ <列级完整性约束条件> ]
      [,<列名> <数据类型>[ <列级完整性约束条件>] ] 
   …
      [,<表级完整性约束条件> ] );
<表名>：所要定义的基本表的名字
<列名>：组成该表的各个属性（列）
<列级完整性约束条件>：涉及相应属性列的完整性约束条件
<表级完整性约束条件>：涉及一个或多个属性列的完整性约束条件 
如果完整性约束条件涉及到该表的多个属性列，则必须定义在表级上，否则既可以定义在列级也可以定义在表级。 


### 数据类型

- SQL中域的概念用数据类型来实现
- 定义表的属性时需要指明其数据类型及长度 
- 选用哪种数据类型 
	- 取值范围 
	- 要做哪些运算 
![POWERPNT_645_418.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/09/1951d6c8c0607e4d87893b9774c13fdb_POWERPNT_645_418.png)
### 修改基本表
- 在关系型数据库中，先有型再有数据，加入新的数据要按照形
- nosql可以不要型

ALTER TABLE <表名>
[ ADD[COLUMN] <新列名> <数据类型> [ 完整性约束 ] ]
[ ADD <表级完整性约束>]
[ DROP [ COLUMN ] <列名> [CASCADE| RESTRICT] ]
[ DROP CONSTRAINT<完整性约束名>[ RESTRICT | CASCADE ] ]
[ALTER COLUMN <列名><数据类型> ] ;

<表名>是要修改的基本表
- `ADD`子句用于增加新列、新的列级完整性约束条件和新的表级完整性约束条件
- `DROP COLUMN`子句用于删除表中的列
	- 如果指定了`CASCADE`短语，则自动删除引用了该列的其他对象
	- 如果指定了`RESTRICT`短语，则如果该列被其他对象引用，关系数据库管理系统将拒绝删除该列
- `DROP CONSTRAINT`子句用于删除指定的完整性约束条件
- `ALTER COLUMN`子句用于修改原有的列定义，包括修改列名和数据类型

### 删除基本表
- DROP TABLE <表名>［RESTRICT| CASCADE］;
- RESTRICT：删除表是有限制的。
	- 欲删除的基本表不能被其他表的约束所引用
	- 如果存在依赖该表的对象，则此表不能被删除
- CASCADE：删除该表没有限制。
	- 在删除基本表的同时，相关的依赖对象一起删除 

### 索引
- 建立索引的目的：加快查询速度。提升查询效率，降低增删改效率的数据结构
	- 由数据库**管理员**或表的**拥有者**建立，大多数情况下使用者就是建立者
	- 由关系数据库管理系统**自动完成维护**
	- 关系数据库管理系统自动使用合适的索引作为存取路径，用户不必也**不能显式地选择索引**
- 关系数据库管理系统中常见索引：
	- 顺序文件上的索引
	- B+树索引
	- 散列（hash）索引
	- 位图索引



#### 索引语句
 -  `CREATE [UNIQUE] [CLUSTER] INDEX <索引名> ON <表名>(<列名>[<次序>][,<列名>[<次序>] ]…); `
	- <表名>：要建索引的基本表的名字
	- 索引：可以建立在该表的一列或多列上，各列名之间用逗号分隔
	- <次序>：指定索引值的排列次序，升序：ASC，降序：DESC。缺省值：ASC
	- UNIQUE：此索引的每一个索引值只对应唯一的数据记录
	- CLUSTER：表示要建立的索引是聚簇索引
		- 数据可能是分布式的，在多个节点上，可以提高速度

#### 修改/删除索引
- ALTER INDEX <旧索引名> RENAME TO <新索引名>
- DROP INDEX <索引名>;
	- 删除索引时，系统会从数据字典中删去有关该索引的描述


## 数据字典
- 关系数据库系统内部的一组系统表，记录了数据中的所有**定义信息**：
	- 关系模式定义
	- 视图定义
	- 索引定义
	- 完整性约束定义
	- 各类用户对数据库的操作权限
	- 统计信息等
		- DBA可以根据这个做优化


# 数据查询
## 单表查询
格式：
SELECT [ALL|DISTINCT] <目标列表达式>[,<目标列表达式>] …
       FROM <表名或视图名>[,<表名或视图名> ]…|(SELECT 语句)      
                    [AS]<别名>
					[ WHERE <条件表达式> ]
					[GROUP BY <列名1> [ HAVING <条件表达式> ] ]
					[ORDER BY <列名2> [ ASC|DESC ] ];

### 满足条件的查询
#### 比较大小

#### 确定范围

#### 确定集合

#### 字符匹配
- 谓词： [NOT] LIKE  ‘<匹配串>’  [ESCAPE ‘ <换码字符>’]
- <匹配串>可以是一个完整的字符串，也可以含有通配符%（任意长度（长度可以为0）的字符串）和 _（任意单个字符）

1. 匹配串为固定字符串

2. 匹配串为含**通配符**的字符串
	- 涉及中文需要考虑单字节、双字节问题

3. 使用换码字符将通配符**转义**为普通字符
	- ESCAPE '＼' 表示“ ＼” 为换码字符


#### 涉及空值查询
- 谓词：`IS NULL` 或`IS NOT NULL` 
	- `IS`不是= ，NULL不等于任何东西
	- 例子：两个性别为null的同学性别一样吗

#### 多重条件查询
- 逻辑运算符：AND和OR来连接多个查询条件


#### 排序
- 对查询结果排序：`ORDER BY <attribute>` 
	- 可以按一个或多个属性列排序（多个是主次关键字）
	- 升序：ASC;降序：DESC;缺省值为升序
	- 对于空值，排序时显示的次序由具体系统实现来决定

**作用**：
- select每次结果顺序不一定一样，因为内模式的实现可能不同
- order by一个主键：查询的**结果顺序唯一**，但顺序有意时才会排序
- 排序不影响语义，但是影响**开发效率**



### 聚集函数

- 统计**元组**个数
	 - COUNT(\*)
- 统计一**列中值**的个数	(<font color="#ff0000">不包含NULL</font>)
	 - COUNT([DISTINCT|ALL] <列名>)
- 计算一列值的总和（此列必须为<font color="#ff0000">数值型</font>）
	- SUM([DISTINCT|ALL] <列名>)	
- 计算一列值的平均值（此列必须为<font color="#ff0000">数值型</font>）
	- AVG([DISTINCT|ALL] <列名>)
- 求一列中的最大值和最小值
	- MAX([DISTINCT|ALL] <列名>)
	- MIN([DISTINCT|ALL] <列名>)

 ![POWERPNT_663_414.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/24d97c12edcb86061953edafd4d83d4c_POWERPNT_663_414.png)

### 分组查询

-  `GROUP BY`
- 细化聚集函数的作用对象
	- 如果未对查询结果分组，聚集函数将作用于整个查询结果
	- 对查询结果分组后，**聚集函数将分别作用于每个组** 
	- 按指定的**一列或多列**值分组，值相等的为一组

![POWERPNT_283_410.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/c72a17e1a1f1cd5fab5a65130e0aa264_POWERPNT_283_410.png)



- **HAVING**短语与WHERE子句的区别：
	- 作用对象不同
	- WHERE子句作用于基表或视图，从中选择满足条件的元组
		- WHERE子句中是不能用聚集函数作为条件表达式
	- HAVING短语**作用于组**，从中选择满足条件的组。 

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/0d0cab2ec14eda531d4a96d198446c90_20230314144343.png)

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/3dba9b7d0e54c1623eafd10780f5e7d1_20230314144601.png)


## 连接查询
- 定义：涉及两个及以上的表的查询
- 格式：
[<表名1>.]<列名1>  <比较运算符>  [<表名2>.]<列名2>
[<表名1>.]<列名1> BETWEEN [<表名2>.]<列名2> AND [<表名2>.]<列名3>

### 等值连接/自然连接查询 

- 等值连接：运算符` = `
- 连接参照关系和被参照关系

一般连接查询：WHERE在笛卡尔积中进行条件筛选
连接几乎是最耗时的操作，运算量很大
![1678777584.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/40978025b598475c08aef754d957021b_1678777584.png)

### 连接执行过程
无法自己选择，数据库决定

1. 嵌套循环法（NESTED-LOOP）
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/5d1e3488c03a84677d7b23b211428cf9_20230314151338.png)


2. 排序合并法（SORT-MERGE）
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/30a7592a2c0983c59629b2928dfe3dd6_20230314151438.png)

3. 索引连接（INDEX-JOIN）

同时进行选择和连接：一条SQL语句可以同时完成选择和连接查询，这时WHERE子句是由连接谓词和选择谓词组成的复合条件。


### 自身连接
**自身连接**：一个表与自己连接，需要给表取**别名**。由于所有属性都是同名属性，因此必须使用别名前缀


![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/7c01aaa3b25b941802740ad679a52f57_20230314152204.png)

### 外连接
- 外连接与普通连接的区别
	- 普通连接操作只输出满足连接条件的元组
	- 外连接操作以指定表为连接主体，将主体表中不满足连接条件的元组一并输出
 - 左外连接
	- 列出左边关系中所有的元组 
	- 右边可以含NULL
 - 右外连接
	- 列出右边关系中所有的元组 
	- 左边可以含NULL
![POWERPNT_684_426.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/e5f1e3e97c60395d4c5dd949abc1aad3_POWERPNT_684_426.png)
### 多表连接
两个表以上连接，语法和双表连接差不多
WHERE后面几个表的顺序无所谓
具体实现是数据库管理系统实现的，不需要关心

## 嵌套查询
- 一个SELECT-FROM-WHERE语句称为一个查询块
- 将一个查询块嵌套在另一个查询块的WHERE子句或HAVING短语的条件中的查询称为嵌套查询

**副作用**：
- SQL效率低下
- 整体语义难以理解


**限制**：
- 子查询不能用ORDER BY

### 求解方式

### 带IN谓词的子查询
![POWERPNT_660_393.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/d2b785211c207fa462112536dbd0e5fd_POWERPNT_660_393.png)
![POWERPNT_645_406.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/14/79b4f763dd7618a8b4f2ede6c198f9bc_POWERPNT_645_406.png)
- 推荐右边，使用语义描述而非过程

### 带比较运算符的子查询

当能确切知道内层查询返回单值时，可用比较运算符（>，<，=，>=，<=，!=或< >）。



### 带有ANY（SOME）或ALL谓词的子查询


- 执行效率
- 看不看得懂
- 项目编码规范允许吗？

![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/16/f965f24bdebfc9ba19fd6d88e514e23f_202303161618675.png)


### 带有EXISTS谓词的子查询

- 带有EXISTS谓词的子查询**不返回任何数据**，只产生逻辑真值“true”或逻辑假值“false”。
	- 若内层查询结果非空，则外层的WHERE子句返回真值
	- 若内层查询结果为空，则外层的WHERE子句返回假值

由EXISTS引出的子查询，其目标列表达式通常都用 * ，因为带EXISTS的子查询只返回真值或假值，给出列名无实际意义


NOT EXISTS相反

```MYSQL
SELECT Sname
     FROM Student
     WHERE EXISTS
                   (SELECT *
                    FROM SC
                    WHERE Sno=Student.Sno AND Cno= ' 1 ');

```





#### 不同形式查询的替换

一些带EXISTS或NOT EXISTS谓词的子查询不能被其他形式的子查询等价替换
所有带IN谓词、比较运算符、ANY和ALL谓词的子查询都能用带EXISTS谓词的子查询等价替换

#### 用EXISTS/NOT EXISTS实现全称量词


![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/17/df48423102be88d76ac8f83502203235_202303171518892.png)

SQL语言中没有全称量词（for all），但用存在量词可以实现等价的效果

关系代数、谓词演算=>用**数学简化**SQL语句，化简后一般是和取析取


## 集合查询

<font color="#ff0000">尽量避免集合操作，效率很差</font>

- 集合操作的种类
	- 并操作UNION
	- 交操作INTERSECT
	- 差操作EXCEPT
- 参加集合操作的各查询结果的列数必须相同；对应项的数据类型也必须相同

### 并操作

UNION：将多个查询结果合并起来时，系统自动去掉重复元组
UNION ALL：将多个查询结果合并起来时，保留重复元组 

从设计原则来说，表中不同记录应该记录相同类型的数据。但技术上可以实现不同类型记录。

[例 3.64]  查询计算机科学系的学生及年龄不大于19岁的学生。
```MySQL
SELECT *
FROM Student
WHERE Sdept= 'CS'
UNION
SELECT *
FROM Student
WHERE Sage<=19;
```
[例 3.65]  查询选修了课程1或者选修了课程2的学生。
```mysql
SELECT Sno
FROM SC
WHERE Cno=' 1 '
UNION
SELECT Sno
FROM SC
WHERE Cno= ' 2 ';
```


### 交操作

INTERSECT


### 差操作

EXCEPT


## 派生表

**子查询**不仅可以出现在WHERE子句中，还可以出现在**FROM子句**中，这时子查询生成的临时**派生表（Derived Table）**  成为主查询的查询对象

[例3.57]找出每个学生超过他自己选修课程平均成绩的课程号
```MySQL
SELECT Sno, Cno
    FROM SC, (SELECT Sno, Avg(Grade) 
                        FROM SC
    		   GROUP BY Sno)
                        AS   Avg_sc(avg_sno,avg_grade)
    WHERE SC.Sno = Avg_sc.avg_sno
      and SC.Grade >=Avg_sc.avg_grade

```


如果子查询中没有聚集函数，派生表可以不指定属性列，子查询SELECT子句后面的列名为其缺省属性。

[例3.60]查询所有选修了1号课程的学生姓名，可以用如下查询完成：
 
```MySQL
SELECT Sname
    FROM     Student,  
          (SELECT Sno FROM SC WHERE Cno=' 1 ') AS SC1
    WHERE  Student.Sno=SC1.Sno;
```


# 数据更新

## 插入
### 插入元组
- 将新元组插入指定表中，语句格式
	INSERT
	INTO <表名> [(<属性列1>[,<属性列2 >…)]
	VALUES (<常量1> [,<常量2>]… );

- 没有指定属性列和属性名：默认插入一条完整元组
- 有指定：其他未指定的取空值

**插入的值要符合预先定义的型**

### 插入子查询结果
- 语句格式:
	 INSERT 
	 INTO <表名>  [(<属性列1> [,<属性列2>…  )]
	 子查询;
 
- INTO子句
- 子查询
	- SELECT子句目标列必须与INTO子句匹配
	- 值的个数
	- 值的类型

## 修改

### 修改数据
- 语句格式
   UPDATE  <表名>
    SET  <列名>=<表达式>[,<列名>=<表达式>]…
    [WHERE <条件>];
- 功能
	- 修改指定表中满足WHERE子句条件的元组
	- SET子句给出<表达式>的值用于取代相应的属性列
	- 如果省略WHERE子句，表示要修改表中的所有元组



### 修改数据与完整性
- 关系数据库管理系统在执行修改语句时会检查修改操作是否破坏表上已定义的完整性规则
	- 实体完整性
	- 参照完整性
	- 用户定义的完整性
		 - NOT NULL约束
		 - UNIQUE约束
		 - 值域约束



## 删除
ch20
删除表格和删除元组是截然不同的操作

就算表格里没有数据，只是值不在了，型还在

# SQL中的空值

<font color="#ff0000">空值和0不一样</font>

## 意义
表达了不知道、不存在、无意义的值

- 一般有以下几种情况：
	- 该属性应该有一个值，但目前不知道它的具体值
	- 该属性不应该有值（不起作用，当前未知）
	- 由于某种原因不便于填写

## 判断
判断一个属性的值是否为空值，用IS NULL或IS NOT NULL来表示。


## 约束条件

- 属性定义（或者域定义）中
	- 有NOT NULL约束条件的不能取空值
	- 加了UNIQUE限制的属性不能取空值
	- 码属性不能取空值


## 运算
空值运算很特殊

**结果**
- 空值与另一个值（包括另一个空值）的算术运算的结果为空值
- 空值与另一个值（包括另一个空值）的比较运算的结果为UNKNOWN。
- 有UNKNOWN后，传统二值（TRUE，FALSE）逻辑就扩展成了三值逻辑
![](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/imgae/2023/03/16/1c3df813a6747b932d357388857a241c_202303161746822.png)
主要考虑方式还是二值逻辑

# 视图

## 特点
1. 虚表，是从一个或几个基本表（或视图）导出的表
2. 只存放视图的定义，不存放视图对应的数据
3. 基表中的数据发生变化，从视图中查询出的数据也随之改变



## 建立视图
CREATE  VIEW 
             <视图名>  [(<列名>  [,<列名>]…)]
       AS  <子查询>
       [WITH  CHECK  OPTION];

- WITH CHECK OPTION
	- 对视图进行UPDATE，INSERT和DELETE操作时要保证更新、插入或删除的行满足视图定义中的谓词条件（即子查询中的条件表达式）
- 子查询可以是任意的SELECT语句，是否可以含有ORDER BY子句和DISTINCT短语，则决定具体系统的实现。
- 关系数据库管理系统执行CREATE VIEW语句时只是把视图定义存入数据字典，并不执行其中的SELECT语句

### 视图属性列
- 全部省略:可以通过子查询过滤一些元组
- 明确指定列名:
	- 某个目标列是聚集函数或列表达式
	- **多表连接**时选出了几个同名列作为视图的字段
		- 有时一个关系会分成多个表,单表过大影响效率
		- 隔离了连接和选择的代码,在视图内连接,视图外选择
	- 需要在视图中为某个列启用新的更合适的名字
		- 更加适应场景
		- e.g:运动会, 学号->运动员编号

### 行列子集视图

保留了主码

增删改


### 基于视图的视图
如果不级联删除会出错

### 分组视图

子查询分组group by

无法修改


### SELECT *

修改**基表结构**后,视图和基表映射关系可能被破坏


## 查询视图

对用户来说和查询表差不多


关系数据库实现:视图消解(有些情况下可能无法正确生成查询)






## 更新视图


转换成对基本表的更新

- 允许对行列子集更新视图
- 对其他类型视图的更新不同系统有不同限制
	- 更新视图的限制：一些视图是不可更新的，因为对这些视图的更新不能唯一地有意义地转换成对相应基本表的更新 e.g.聚合函数得到的列
	- 基于不允许更新视图的视图也不能更新


