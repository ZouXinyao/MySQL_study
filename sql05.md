一、SQL函数

SQL的函数一般在数据上执行，方便转换和处理数据。从数据表中检索出数据之后，就可以进一步对这些数据进行操作

二、常用的 SQL 函数有哪些

内置函数：

1、算术函数

```
ABS(a)		取a绝对值
MOD(a, b)	a % b
ROUND(a, b)	对a四舍五入，小数点为b位数
举例：
SELECT ABS(-2)			运行结果为 2。
SELECT MOD(101,3)		运行结果 2。
SELECT ROUND(37.25,1)	运行结果 37.3。
```

2、字符串函数

```
SELECT CONCAT('abc', 123)						拼接；运行结果为 abc123。
SELECT LENGTH('abcdefg')						计算长度；运行结果为 6。
SELECT CHAR_LENGTH('你好')					   汉字算1个字符；运行结果为 2。
SELECT LOWER('ABC')								小写；运行结果为 abc。
SELECT UPPER('abc')								大写；运行结果 ABC。
SELECT REPLACE('fabcd', 'abc', 123)				替换；abc替换为123，运行结果为 f123d。
SELECT SUBSTRING('fabcd', 1,3)					截取；截取第1-3个字符；运行结果为 fab。
```

3、日期函数

```
CURRENT_DATE()						系统当前日期
CURRENT_TIME()						系统当前时间
CURRENT_TIMESTAMP()					系统当前日期+时间
EXTRACT()			抽取年、月、日：SELECT EXTRACT(YEAR FROM '2020-12-21'),结果是2020
DATE()				返回时间的日期部分
YEAR()				返回时间的年部分
MONTH()				返回时间的月部分
DAY()				返回时间的日部分
HOUR()				返回时间的小时部分
MINUTE()			返回时间的分钟部分
SECOND()			返回时间的秒部分
```

4、转换函数

```
CAST(原数a AS 类型type)			将原数转换为type类型
COALESCE()					   返回第一个非空值
举例：
SELECT CAST(123.123 AS INT)				运行结果会报错。
SELECT CAST(123.123 AS DECIMAL(8,2))	运行结果为 123.12。DECIMAL(8,2)指定了2位小数，总8位
SELECT COALESCE(null,1,2)				运行结果为 1。
```

5、注意

- 日期不要直接用字符串进行比较，因为很多时候你无法确认 birthdate 的数据类型是字符串，还是 datetime 类型，如果你想对日期部分进行比较，那么使用DATE(birthdate)来进行比较是更安全的。
- SQL函数的代码可移植性差，因为不同的DBMS差异较大。

三、命名规范的建议

- 关键字和函数名称全部大写；
- 数据库名、表名、字段名称全部小写；
- SQL 语句必须以分号结尾。

四、聚集函数

对一组数据进行汇总的函数，输入的是一组数据的集合，输出的是单个值。

可以利用聚集函数汇总表的数据，如果稍微复杂一些，我们还需要先对数据做筛选，然后再进行聚集

```
COUNT()		总行数，如果对单列统计，那么会忽略掉NULL
MAX()		最大值
MIN()		最小值
SUM()		和
AVG()		平均值
AVG、MAX、MIN 等聚集函数会自动忽略值为 NULL 的数据行
```

五、分组

我们可能会先对数据按照不同的数值进行分组，然后对这些分好的组进行聚集统计。

- 对数据进行分组，需要使用 GROUP BY 子句。
- 把GROUP BY后面的字段按值进行分类，相同的值一类，也可以后面跟多个值，就会产生更多的情况。
- NULL也是单独的一类

六、HAVING过滤，用于分组

1. HAVING 的作用和 WHERE 一样，都是起到过滤的作用
2. 只不过 WHERE 是用于数据行，而 HAVING 则作用于分组；
3. HAVING 支持所有 WHERE 的操作；

```
举例：
SQL: SELECT COUNT(*) as num, role_main, role_assist FROM heros GROUP BY role_main, role_assist HAVING num > 5 ORDER BY num DESC

首先我们需要获取的是英雄的数量、主要定位和次要定位，即SELECT COUNT(*) as num, role_main, role_assist。然后按照英雄的主要定位和次要定位进行分组，即GROUP BY role_main, role_assist，同时我们要对分组中的英雄数量进行筛选，选择大于 5 的分组，即HAVING num > 5，然后按照英雄数量从高到低进行排序，即ORDER BY num DESC。
```

SELECT查询中，关键字的顺序：

```
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ...
```

