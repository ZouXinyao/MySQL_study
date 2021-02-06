子查询：嵌套在查询中的查询。

应用场景：因为很多时候，我们无法直接从数据表中得到查询结果，需要从查询结果集中再次进行查询，才能得到想要的结果。

一、关联与非关联子查询

子查询从数据表中查询了数据结果，如果这个数据结果只执行一次，然后这个数据结果作为主查询的条件进行执行，那么这样的子查询叫做非关联子查询。

```
举例：
求哪个球员最高，最高身高是多少
SELECT max(height) FROM player先求出最高身高，这个过程没有依赖外层的查询。
SQL: SELECT player_name, height FROM player WHERE height = (SELECT max(height) FROM player)
```

如果子查询的执行依赖于外部查询，通常情况下都是因为子查询中的表用到了外部的表，并进行了条件关联，因此每执行一次外部查询，子查询都要重新计算一次，这样的子查询就称之为关联子查询。

```
举例：
求出平均身高，然后筛选出高于这个身高的姓名、ID、身高
首先将player建别名，分别为a、b。然后比较这两个表的team_id，这就将内层查询和外层查询关联了起来，这种就是关联子查询。
SELECT player_name, height, team_id FROM player AS a WHERE height > (SELECT avg(height) FROM player AS b WHERE a.team_id = b.team_id)
```

二、EXISTS 子查询

EXISTS 子查询用来判断条件是否满足，满足的话为 True，不满足为 False。

```
举例：
EXISTS (SELECT player_id FROM player_score WHERE player.player_id = player_score.player_id)判断两个表的player_id是否相等，相等的返回true，就进行外层的查询，返回false不查
SQL：SELECT player_id, team_id, player_name FROM player WHERE EXISTS (SELECT player_id FROM player_score WHERE player.player_id = player_score.player_id)
```

NOT EXISTS 就是不存在的意思，使用EXISTS方法一样，结果相反

三、集合比较子查询

集合比较子查询的作用是与另一个查询结果集进行比较

```
IN					判断是否在集合中
ANY/SOME			与比较操作符一起用，与子查询返回的任何值做比较
ALL					与比较操作符一起用，与子查询返回的所有值做比较
```

ANY/SOME比较的是集合里的最小值，ALL是最大值

```
IN的使用：
(SELECT cc FROM B)求得一个cc集合，然后求A表中在集合中的CC
SELECT * FROM A WHERE cc IN (SELECT cc FROM B)
```

四、子查询作为计算字段

```
SELECT 子查询 AS ... FROM ... WHERE ...
```















