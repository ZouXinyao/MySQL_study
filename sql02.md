一、DDL

DDL 的英文全称是 Data Definition Language，中文是数据定义语言。它定义了数据库的结构和数据表的结构。

一般创建表都用可视化的数据库管理工具。

常用的功能是增删改，分别对应的命令是 CREATE、DROP 和 ALTER。

定义数据库

```
CREATE DATABASE nba; // 创建一个名为nba的数据库
DROP DATABASE nba; // 删除一个名为nba的数据库
```

定义数据表

```
CREATE TABLE [table_name](字段名 数据类型，......)
```

1. 添加字段，比如我在数据表中添加一个 age 字段，类型为int(11)

   ```
   ALTER TABLE player ADD (age int(11));
   ```

2. 修改字段名，将 age 字段改成player_age

   ```
   ALTER TABLE player RENAME COLUMN age to player_age
   ```

3. 修改字段的数据类型，将player_age的数据类型设置为float(3,1)

   ```
   ALTER TABLE player MODIFY (player_age float(3,1));
   ```

4. 删除字段, 删除刚才添加的player_age字段

   ```
   ALTER TABLE player DROP COLUMN player_age;
   ```

二、数据表的约束

- 主键约束：主键起到唯一标识一条记录的作用，不能重复，不能为空。
- 外键约束：外键是确保了表与表之间引用完整性。一个表中的外键对应的另一张表的主键。外键可以是重复的，也可以为空。
- 唯一性约束：唯一性约束表明了字段在表中的数值是唯一的，即使我们已经有了主键，还可以对其他字段进行唯一性约束。
- NOT NULL约束：字段的为空约束
- DEFAULT：字段默认值
- CHECK约束：用来检查特定字段取值范围的有效性

三、设计数据表的准则——“三少一多”原则的核心就是“简单可复用”

1. 数据表的个数越少越好
2. 数据表中的字段个数越少越好
3. 数据表中联合主键的字段个数越少越好
4. 使用主键和外键越多越好

数据库的设计实际上就是定义各种表，以及各种字段之间的关系。这些关系越多，证明这些实体之间的冗余度越低，利用度越高。

