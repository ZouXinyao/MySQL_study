# 存储过程

存储过程可以说是由 SQL 语句和流控制语句构成的语句集合

- 和视图一样，都是对 SQL 代码进行封装，可以反复利用，清晰、安全，还可以减少网络传输量。
- 与视图不同的是：存储过程是程序化的 SQL，可以直接操作底层数据表。
- 与函数类似，可以接收参数，可以有返回值。

一、创建1个存储过程

```
CREATE PROCEDURE 存储过程名称([参数列表])
BEGIN
    需要执行的语句
END    
```

1、 CREATE PROCEDURE 创建一个存储过程，类似一些语言中函数的关键字(python的def，go的func)，后面是存储过程的名称(函数名)，以及过程所带的参数，可以包括输入参数和输出参数。最后由 BEGIN 和 END 来定义所要执行的语句块(函数的{  })。

```
举例：
1~n累加

CREATE PROCEDURE `add_num`(IN n INT)
BEGIN
       DECLARE i INT;
       DECLARE sum INT;
       
       SET i = 1;
       SET sum = 0;
       WHILE i <= n DO
              SET sum = sum + i;
              SET i = i +1;
       END WHILE;
       SELECT sum;
END

使用CALL add_num(50)求得1~50的值
```

删除存储过程：DROP PROCEDURE

更新存储过程：ALTER PROCEDURE

2、DELIMITER

在MySQL中，需要用 DELIMITER 来临时定义新的结束符。

SQL 采用（；）作为结束符。但是存储过程是一个整体，我们不希望 SQL 逐条执行，而是采用存储过程整段执行的方式，因此我们就需要临时定义新的 DELIMITER，新的结束符可以用（//）或者（$$）

```
举例：

DELIMITER //
CREATE PROCEDURE `add_num`(IN n INT)
BEGIN
       DECLARE i INT;
       DECLARE sum INT;
       
       SET i = 1;
       SET sum = 0;
       WHILE i <= n DO
              SET sum = sum + i;
              SET i = i +1;
       END WHILE;
       SELECT sum;
END //
DELIMITER ;
首先用（//）作为结束符，又在整个存储过程结束后采用了（//）作为结束符号，告诉 SQL 可以执行了，然后再将结束符还原成默认的（;）
```

2、3种参数类型

IN：			入参

OUT：		返回值

INOUT：	既是入参又是返回值，相当于指针或引用做入参

用@修饰OUT类型的参数，就可以实现修改入参值，比如：

```
CALL func(@a);
SELECT @a;
@a是func处理后的值，没有新增变量作为返回值
```

二、流控制语句

- BEGIN…END：BEGIN…END 中间包含了多个语句，每个语句都以（;）号为结束符。
- DECLARE：DECLARE 用来声明变量，使用的位置在于 BEGIN…END 语句中间，而且需要在其他语句使用之前进行变量的声明。
- SET：赋值语句，用于对变量进行赋值。
- SELECT…INTO：把从数据表中查询的结果存放到变量中，也就是为变量赋值。
- IF…THEN…ENDIF：条件判断语句，也可以在 IF…THEN…ENDIF 中使用 ELSE 和 ELSEIF 来进行条件判断。
- CASE：CASE 语句用于多条件的分支判断(switch)
- LOOP、LEAVE 和 ITERATE：LOOP 是循环语句，使用 LEAVE 可以跳出循环，使用 ITERATE 则可以进入下一次循环。可以把 LEAVE 理解为 BREAK，把 ITERATE 理解为 CONTINUE。
- REPEAT…UNTIL…END REPEAT：循环语句，首先会执行一次循环，然后在 UNTIL 中进行表达式的判断，如果满足条件就退出，即 END REPEAT；如果条件不满足，则会就继续执行循环，直到满足退出条件为止。
- WHILE…DO…END WHILE：这也是循环语句，和 REPEAT 循环不同的是，这个语句需要先进行条件判断，如果满足条件就进行循环，如果不满足条件就退出循环。

三、存储过程的缺点

- 可移植性差，存储过程不能跨数据库移植，在一种数据库中写好的，换成其他数据库时都需要重新编写。
- 其次调试困难，只有少数 DBMS 支持存储过程的调试。对于复杂的存储过程来说，开发和维护都不容易。
- 此外，存储过程的版本管理也很困难，比如数据表索引发生变化了，可能会导致存储过程失效。
- 最后它不适合高并发的场景，高并发的场景需要减少数据库的压力，有时数据库会采用分库分表的方式，而且对可扩展性要求很高，在这种情况下，存储过程会变得难以维护，增加数据库的压力，显然就不适用了。











