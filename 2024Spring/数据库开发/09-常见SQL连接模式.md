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
title: 09-常见SQL连接模式
date:  2024-06-03 14:06
modified:  2024-06-03 14:06
---
# 1. 叠加行集（Union & Union all）

不是关系代数

- 约束：上下的select字段要一样，字段数量要一样
- 区别：
	- Union：等同于针对Union all的输出再进行一次distinct操作
	- Union all：不去重


# 2. 查找只存在于一张表的数据（差）

MySQL: not in
Oracle: minus

```mysql
select deptno
from dept
where deptno not in (
	select deptno in emp
)
```

如果`deptno`不是主键：
```mysql
select distinct deptno
from dept
where deptno not in (
	select deptno in emp
)
```



如果`not in`嵌套查询里有空值:
```mysql
select deptno
from dept
where deptno not in (10, 50, null)
```

no=20 not in (10, 50, null)->(F or F or null)=null，查询结果为空


# 3. 从一个表检索另一个不相关的行（外连接）


左外连接
右外连接


# 4. 确定两个表是否有相同的数据


很复杂


# 5. 从多个表种返回缺失的值

全外连接


# 连接和聚合函数的使用

