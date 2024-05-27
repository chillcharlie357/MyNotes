
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
