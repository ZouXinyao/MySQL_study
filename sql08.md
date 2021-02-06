# 视图

视图，又名虚拟表，本身是不具有数据的。视图作为一张虚拟表，封装了底层与数据表的接口。它相当于是一张表或多张表的数据结果集。虚拟表的创建连接了一个或多个数据表，不同的查询应用都可以建立在虚拟表之上。

视图的作用：

- 视图一方面可以帮我们使用表的一部分而不是所有的表，另一方面也可以针对不同的用户制定不同的查询视图。
- 视图隐藏了底层的表结构，简化了数据访问操作，客户端不再需要知道底层表的结构及其之间的关系。
- 视图提供了一个统一访问数据的接口。（即可以允许用户通过视图访问数据的安全机制，而不授予用户直接访问底层表的权限），从而加强了安全性，使用户只能看到视图所显示的数据。
- 视图还可以被嵌套，一个视图中可以嵌套另一个视图。

注意：视图总是显示最新的数据！每当用户查询视图时，数据库引擎通过使用视图的 SQL 语句重建数据。

通常情况下，小型项目的数据库可以不使用视图，但是在大型项目中，以及数据表比较复杂的情况下，视图的价值就凸显出来了，它可以帮助我们把经常查询的结果集放到虚拟表中，提升使用效率。

一、创建、更新、删除视图

1、创建视图：CREATE VIEW

```
CREATE VIEW view_name AS
SELECT column1, column2
FROM table
WHERE condition
```

就是在 SQL 查询语句的基础上封装了视图 VIEW，这样就会基于 SQL 语句的结果集形成一张名为view_name的虚拟表。创建出来的视图可以像正常的表一样进行查询。

2、嵌套视图

当我们创建好一张视图之后，还可以在它的基础上继续创建视图。相当于在现有的视图上又封装了一个视图。

3、修改视图：ALTER VIEW

```
ALTER VIEW view_name AS
SELECT column1, column2
FROM table
WHERE condition
```

修改视图就是对原视图的更新

4、删除视图：DROP VIEW

```
DROP VIEW view_name
```

我们可以将视图理解为对SELECT的二次封装，形成新的虚拟表，方便我们重用他们。

二、视图的应用

1、完成复杂的连接

先建立根据条件建立一个虚拟表，然后每次都可以在虚拟表中进行查询。

```
根据player和height_grades求球员对应的身高等级：
1、先创建一个视图，
CREATE VIEW player_height_grades AS
SELECT p.player_name, p.height, h.height_level
FROM player as p JOIN height_grades as h
ON height BETWEEN h.height_lowest AND h.height_highest
2、每次查询身高等级都可以在这个视图中进行查询：
SELECT * FROM player_height_grades WHERE height >= 1.90 AND height <= 2.08
```

2、利用视图对数据进行格式化

当我们像输出某个格式的内容，可以利用视图

```
输出球员姓名和对应的球队，对应格式为 player_name(team_name)
1、创建1个视图，
CREATE VIEW player_team AS 
SELECT CONCAT(player_name, '(' , team.team_name , ')') AS player_team FROM player JOIN team WHERE player.team_id = team.team_id
2、后序的查询
SELECT * FROM player_team
```

3、简化处理数据

先在创建视图的时候进行数据处理，然后每次需要这种复杂的计算时，直接进行查询就可以了，免去每次查询都需要计算的麻烦。

三、临时表与视图

- 视图是虚拟表；临时表是真实存在的数据表，不过它不用于长期存放数据；
- 临时表只在当前连接存在，关闭连接后，临时表就会自动释放。







