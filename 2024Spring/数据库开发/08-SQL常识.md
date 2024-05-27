
# Group by

group by是一个过程化操作

1. 筛选分组用having，
2. count(\*)只要rowid存在都会算，count(name)会忽略NULL



# 字符串处理

清洗数据在数据库里处理的代价比在应用程序里处理的代价低

## 遍历字符串

数据透视表(T1,T10,T100,...)


```sql
select substr(e.ename,iter.pos, 1) as C
from (select ename)
```


## 内嵌引号

```sql
' " '
```


## 统计字符出现的个数

问题：出现多少个逗号

**repalce**+**length**

len(replace) / len(',')

## 删除不想要的字符

oracle：`translate`
mysql：嵌套`replace`


## 分离数字和字符

oracle: translate对原来的字段处理两次
mysql：正则表达式`REGEXP_REPLACE`


## 判断含有数字和字母的数值


mysql: 正则表达式，regexp


## 提取姓名首字母

需要找到空格的位置或大写的位置，然后把那个位置的字母提出来



# 数值操作

## 计算平均值

难点在于空值

```sql
%% 统计空值 %%
select avg(coalesce(sal,0))
from t2



%% 不统计空值 %%
select avg(sal)
from t2
```



## 累计求和

sum over

```sql
select ename, sal
sum(sal) over (order by sal,empno)
	as running_total
from emp
order by 2 //select中的第二个字段
```


## 计算众数

group by
count


## 计算中位数


oracle: median()
mysql：分组，记录个数，找到中间位置的值