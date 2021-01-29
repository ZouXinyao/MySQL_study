提升查询效率的方法：

1. 约束返回结果数量；
2. 指定筛选条件，进行过滤，减少返回时不必要的数据行。

过滤用到的是WHERE子句。

一、WHERE中的比较运算符

```
=	<	>	!=(<>)	<=	>=	
为空值：IS NULL
指定的两个数值之间：BETWEEN
```

基本格式：

```
SELECT ……(列名) FROM ……(表名) WHERE ……(子句条件)
举例：
SELECT name, hp_max FROM heros WHERE hp_max > 6000
SELECT name, hp_max FROM heros WHERE hp_max BETWEEN 5399 AND 6811
对hp_max字段进行空值检查：
SELECT name, hp_max FROM heros WHERE hp_max IS NULL
```

二、逻辑运算符

```
AND		OR		IN		NOT
```

举例：

```
SELECT name, hp_max, mp_max FROM heros WHERE hp_max > 6000 AND mp_max > 1700 ORDER BY (hp_max+mp_max) DESC
```

() 优先级最高，其次优先级是 AND，然后是 OR。

```
SELECT name, hp_max, mp_max FROM heros WHERE ((hp_max+mp_max) > 8000 OR hp_max > 6000) AND mp_max > 1700 ORDER BY (hp_max+mp_max) DESC
```

三、通配符

检索文本中包含某个词的所有数据，这里就需要使用通配符。通配符就是我们用来匹配 值的一部分的特殊字符。需要使用到 LIKE 操作符。

1、匹配任意字符串出现的任意次数，需要使用（%）通配符。

```
查找英雄名中包含“太”字的英雄
SELECT name FROM heros WHERE name LIKE '%太%'
```

- 字符串的搜索可能是需要区分大小写的
- 匹配单个字符，就需要使用下划线 () 通配符
- （%）代表零个或多个字符，而（_）只代表一个字符。

```
查找英雄名除了第一个字以外，包含‘太’字的英雄有哪些。
SQL：SELECT name FROM heros WHERE name LIKE '_%太%'
```

四、通配符的注意事项

在实际操作过程中，尽量少用通配符，因为它需要消耗数据库更长的时间来进行匹配。

LIKE 后面尽量不能以（%）开头，以（%）开头会对全表进行扫描。