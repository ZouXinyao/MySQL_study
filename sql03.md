# 检索——SELECT

SELECT 的作用是从一个表或多个表中检索出想要的数据行。

要返回满足条件的数据行，就需要在 SELECT 后面加上我们想要查询的列名，可以是一列，也可以是多个列。

#### 一、基础语法

查询某一列：在 SELECT 后面加上这个列的字段名即可

```
SELECT 字段名 FROM 表名
SELECT name FROM heros
```

对多个列进行检索

```
SELECT 多个字段名(用,隔开) FROM 表名
SELECT name, hp_max, mp_max, attack_max, defense_max FROM heros
```

检索所有的列

```
SELECT * FROM heros
```

生产环境尽量避免使用SELECT *

#### 二、起别名 AS

一般来说起别名的作用是对原有名称进行简化，从而让 SQL 语句看起来更精简。

```
SELECT name AS n FROM heros
```

显示的时候，列的字段名也会显示成简化后的名字

#### 三、查询常数：在 SELECT 查询结果中增加一列固定的常数列。

只从一个表中查询数据，通常不需要增加一个固定的常数列，但如果想整合不同的数据源，用常数列作为这个表的标记，就需要查询常数。

增加一个名为platform，值为123的常数列

```
SELECT 123 as platform, name FROM heros
```

#### 四、从结果中去除重复行——DISTINCT

- DISTINCT 必须放到所有列名的前面。
- DISTINCT 其实是对后面所有列名的组合进行去重。

```
SELECT DISTINCT attack_range FROM heros
```

#### 五、排序检索数据——ORDER BY

- 排序的列名：ORDER BY 后面可以有一个或多个列名，如果是多个列名进行排序，会按照后面第一个列先进行排序，当第一列的值相同的时候，再按照第二列进行排序，以此类推。
- 排序的顺序：ORDER BY 后面可以注明排序规则，ASC 代表递增排序，DESC 代表递减排序。默认递增。
- 非选择列排序：ORDER BY 可以使用非选择列进行排序，所以即使在 SELECT 后面没有这个列名，你同样可以放到 ORDER BY 后面进行排序。
- ORDER BY 的位置：ORDER BY **通常**位于 SELECT 语句的最后一条子句，否则会报错。

按照hp_max排序，当hp_max一样时，再按照mp_max排序

```
SELECT name, hp_max FROM heros ORDER BY hp_max, mp_max DESC 
```

#### 六、返回前几条数据——LIMIT

LIMIT放在最后

```
SELECT name, hp_max FROM heros ORDER BY hp_max DESC LIMIT 5
```

约束返回结果的数量可以减少数据表的网络传输量，也可以提升查询效率。如果我们知道返回结果只有 1 条，就可以使用LIMIT 1，告诉 SELECT 语句只需要返回一条记录即可。这样的好处就是 SELECT 不需要扫描完整的表，只需要检索到一条符合条件的记录即可返回。

#### 七、SELECT 的执行顺序

1、关键字执行顺序

```
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ...
```

2、SELECT 语句的执行顺序

```
FROM > WHERE > GROUP BY > HAVING > SELECT的字段 > DISTINCT > ORDER BY > LIMIT
```

在 SELECT 语句执行这些步骤的时候，每个步骤都会产生一个虚拟表，然后将这个虚拟表传入下一个步骤中作为输入。