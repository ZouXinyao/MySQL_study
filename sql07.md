# 连接

​		SQL建立在关系型数据库之上，可以在各个数据表之间进行连接查询。关系型数据库的典型数据结构就是数据表，这些数据表的组成都是结构化的。关系模型就是二维表格模型，这个二维表格是由行和列组成的。**每一个行就是一条数据，每一列就是数据在某一维度的属性。**

​		表的组成是基于关系模型的，所以一个表就是一个关系。一个数据库中可以包括多个表，也就是存在多种数据之间的关系。SQL 语言对各个数据表进行复杂查询，核心就在于连接，**SQL可以用一条 SELECT 语句在多张表之间进行查询。**

1、连接的基本操作：

- 内连接：将多个表之间满足连接条件的数据行查询出来。它包括了等值连接、非等值连接和自连接。
- 外连接：会返回一个表中的所有记录，以及另一个表中匹配的行。它包括了左外连接、右外连接和全连接。
- 交叉连接：也称为笛卡尔积，返回左表中每一行与右表中每一行的组合。在 SQL99 中使用的 CROSS JOIN。

一、笛卡尔积

作用就是可以把任意表进行连接，即使这两张表不相关。

假设我有两个集合 X 和 Y，那么 X 和 Y 的笛卡尔积就是 X 和 Y 的所有可能组合，也就是第一个对象来自于 X，第二个对象来自于 Y 的所有可能。

```
player表的数据是集合X：
SELECT * FROM player
team表的数据为集合Y：
SELECT * FROM team

笛卡尔积结果：
SELECT * FROM player, team
```

二、等值连接

两张表的等值连接就是用两张表中都存在的列进行连接。

```
举例：
player表和team表都存在team_id这一列：
SQL: SELECT player_id, player.team_id, player_name, height, team_name FROM player, team WHERE player.team_id = team.team_id

使用表的别名会让SQL语句更加简洁：
SELECT player_id, a.team_id, player_name, height, team_name FROM player AS a, team AS b WHERE a.team_id = b.team_id
```

如果我们使用了表的别名，在查询字段中就只能使用别名进行代替，不能使用原有的表名

三、非等值连接

当我们进行多表查询的时候，如果连接多个表的条件是等号时，就是等值连接，其他的运算符连接就是非等值查询。

四、外连接

作用：除了查询满足条件的记录以外，外连接还可以查询某一方不满足条件的记录

两张表的外连接，会有一张是主表，另一张是从表。如果是多张表的外连接，那么第一张表是主表，即显示全部的行，而第剩下的表则显示对应连接的信息。

左外连接，就是指左边的表是主表，需要显示左边表的全部行，而右侧的表是从表，（+）表示哪个是从表。

右外连接，指的就是右边的表是主表，需要显示右边表的全部行，而左侧的表是从表。

```
举例：
左：
SELECT * FROM player, team where player.team_id = team.team_id(+)
SELECT * FROM player LEFT JOIN team on player.team_id = team.team_id
右：
SELECT * FROM player, team where player.team_id(+) = team.team_id
SELECT * FROM player RIGHT JOIN team on player.team_id = team.team_id
```

LEFT JOIN 和 RIGHT JOIN 只存在于 SQL99 及以后的标准中，在 SQL92 中不存在，只能用（+）表示。

五、自连接

自连接可以对多个表进行操作，也可以对同一个表进行操作。也就是说查询条件使用了当前表的字段。

```
举例：
a是'布雷克-格里芬'，b是身高比他高的球员。
SELECT b.player_name, b.height FROM player as a , player as b WHERE a.player_name = '布雷克-格里芬' and a.height < b.height

如果分开就需要两次查询：先求出'布雷克-格里芬'的身高，然后再查询一次求比他身高高的。
SELECT height FROM player WHERE player_name = '布雷克-格里芬'
SELECT player_name, height FROM player WHERE height > 2.08
```

六、交叉连接(SQL92 中的笛卡尔乘积)

CROSS JOIN

```
两个表交叉连接：
SELECT * FROM t1 CROSS JOIN t2
3个表：
SELECT * FROM t1 CROSS JOIN t2 CROSS JOIN t3
```

七、自然连接(相当于SQL92 中的等值连接)

自动查询两张连接表中所有相同的字段，然后进行等值连接。

自然连接自动判断相同名称的列，而后形成匹配。缺点是，虽然可以指定查询结果包括哪些列，但不能人为地指定哪些列被匹配。另外，自然连接的一个特点是连接后的结果表中匹配的列只有一个。

NATURAL JOIN

```
这里匹配的列是随机的，而且只匹配一个列
SELECT player_id, team_id, player_name, height, team_name FROM player NATURAL JOIN team 
```

八、ON 连接

ON 连接用来指定想要的连接条件；一般来说在 SQL99 中，我们需要连接的表会采用 JOIN 进行连接，ON 指定了连接条件，后面可以是等值连接，也可以采用非等值连接。

```
上面的例子使用ON连接后：
SELECT player_id, player.team_id, player_name, height, team_name FROM player JOIN team ON player.team_id = team.team_id
```

九、USING 连接

可以用 USING 指定数据表里的同名字段进行等值连接。

```
上面的例子使用USING后：
SELECT player_id, team_id, player_name, height, team_name FROM player JOIN team USING(team_id)
```

USING 指定了具体的相同的字段名称，你需要在 USING 的括号 () 中填入要指定的同名字段。同时使用 JOIN USING 可以简化 JOIN ON 的等值连接，

十、SQL99的外连接

- 左外连接：LEFT JOIN 或 LEFT OUTER JOIN
- 右外连接：RIGHT JOIN 或 RIGHT OUTER JOIN
- 全外连接：FULL JOIN 或 FULL OUTER JOIN
- 全外连接的结果 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据。
- 一般省略 OUTER 不写
- MySQL 不支持全外连接

十一、SQL99的自连接

与SQL92一样

```
举例：
SQL92:
SELECT b.player_name, b.height FROM player as a , player as b WHERE a.player_name = '布雷克-格里芬' and a.height < b.height
SQL99:
SELECT b.player_name, b.height FROM player as a JOIN player as b ON a.player_name = '布雷克-格里芬' and a.height < b.height
```

十二、SQL92和SQL99的区别

1、SQL92的WHERE和SQL99的JOIN

- 在 SQL92 中进行查询时，会把所有需要连接的表都放到 FROM 之后，然后在 WHERE 中写明连接的条件。
-  SQL99 更灵活，它不需要一次性把所有需要连接的表都放到 FROM 之后，而是采用 JOIN 的方式，每次连接一张表，可以多次使用 JOIN 进行连接。

```
举例：
SELECT ...
FROM table1
    JOIN table2 ON table1和table2的连接条件
        JOIN table3 ON table2和table3的连接条件
```

2、SQL99 在 SQL92 的基础上提供了一些特殊语法，比如 NATURAL JOIN 和 JOIN USING。

综上所述，SQL99更加清爽，结构更加清晰

十三、连接的性能问题

连接表就像for循环一样，表越多性能越差。

使用WHERE筛选符合条件的数据行。

多用自连接，少用子查询。子查询实际上是通过未知表进行查询后的条件判断，而自连接是通过已知的自身数据表进行条件判断。而且自连接也有优化。