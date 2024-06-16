---
aliases: 
tags:
  - 2024_Spring_数据库开发
  - 课程
categories: 2024_Spring_数据库开发
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: 08-SQL常识
date:  2024-05-28 14:05
modified:  2024-06-16 21:06
---

# 1. Group by

group by是一个过程化操作

1. 筛选分组用having，
2. count(\*)只要rowid存在都会算，count(name)会忽略NULL

# 2. 字符串处理

清洗数据在数据库里处理的代价比在应用程序里处理的代价低

## 2.1. 遍历字符串

数据透视表(T1,T10,T100,...)

返回部分数据的笛卡尔积

```sql
select ename, iter.pos

from (select ename

from emp

where ename = 'KING') e,

(select id as pos from t10) iter

where iter.pos <= length(e.ename)
```

## 2.2. 内嵌引号

想在字符串常量中嵌入引号

```sql
select 'g''day mate' qmarks from t1 union all

select 'beavers'' teeth' from t1 union all

select '''' from t1
```

## 2.3. 统计字符出现的个数

问题：出现多少个逗号

**repalce**+**length**

(len - len(replace)) / len(',')

## 2.4. 删除不想要的字符

oracle：`translate`  
mysql：嵌套`replace`

## 2.5. 分离数字和字符

oracle: translate对原来的字段处理两次  
mysql：正则表达式`REGEXP_REPLACE`

```mysql
SELECT

data,

REGEXP_REPLACE(data, ‘[0-9]’, ‘’) AS characters,

REGEXP_REPLACE(data, '[^0-9]', '') AS digits

FROM emp
```

## 2.6. 判断含有数字和字母的数值

mysql: 正则表达式，regexp

从表里筛选出部分行数据，筛选条件是只包含字母和数字字符:

```mysql
select data
from V
where data regexp '[^0-9a-zA-Z]' = 0
%% `[^0-9a-zA-Z]`表示匹配任何不是数字（0-9）或字母（a-zA-Z）的字符 %%
%% `0`表示这个正则表达式匹配的次数为0 %%
```



## 2.7. 提取姓名首字母

需要找到空格的位置或大写的位置，然后把那个位置的字母提出来

# 3. 数值操作

## 3.1. 计算平均值

难点在于空值

```sql
%% 统计空值 %%
select avg(coalesce(sal,0))
from t2



%% 不统计空值 %%
select avg(sal)
from t2
```

## 3.2. 累计求和

sum over

```sql
select ename, sal
sum(sal) over (order by sal,empno)
	as running_total
from emp
order by 2 //select中的第二个字段
```

## 3.3. 计算众数

group by  
count

## 3.4. 计算中位数

oracle: median()  
mysql：分组，记录个数，找到中间位置的值

## 3.5. 去掉最大值最小值，然后计算平均值

where sal not in (min_sal, max_sal)

# 4. 日期处理

## 4.1. 年月日加减法

oracle: add_mounths()  
mysql: date_add()

## 4.2. 两个日期之间的天数

mysql: datediff  
oracle：两个日期直接相减

## 4.3. 两个日期之间的工作日天数

需要获取日期是星期几，然后把星期六星期天去掉  
date_format,date_add

## 4.4. 判断闰年

加一年，然后判断时间差是多少天；  
判断二月份第一天到最后一天是几天

# 5. 常见SQL连接模式

## 5.1. 叠加行集（Union & Union all）

不是关系代数

- 约束：上下的select字段要一样，字段数量要一样
- 区别：
	- Union：等同于针对Union all的输出再进行一次distinct操作
	- Union all：不去重

## 5.2. 查找只存在于一张表的数据（差）

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

## 5.3. 从一个表检索另一个不相关的行（外连接）

左外连接  
右外连接

## 5.4. 确定两个表是否有相同的数据

很复杂

## 5.5. 从多个表种返回缺失的值

全外连接

## 5.6. 连接和聚合函数的使用