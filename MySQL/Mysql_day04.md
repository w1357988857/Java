# MYSQL

[TOC]



## 视图(了解)  虚表

实表：create table 创建出来的表

从用户角度来看，一个视图是从一个特定的角度来查看数据库中的数据 。从数据库系统内部来看，一个视图是由SELECT语句组成的查询定义的**虚拟表** 。从数据库系统内部来看，视图是由一张或多张表中的数据组成的，从数据库系统外部来看，视图就如同一张表 一样，对表能够进行的一般操作都可以应用于视图，例如查询，插入，修改，删除操作等。

**从虚表中增删改数据，对实表的影响是无法预知的，所以虚表仅用查询，不允许增删改**



2、视图的用途

- 筛选表中的行(可以将指定的列通过视图释放出来，也可以隐藏一些比较隐私的列)
- 防止未经许可的用户访问敏感数据(视图权限)
- 降低数据库的复杂程度（视图体现的是逻辑，也是业务）
-  将多个物理数据库抽象为一个逻辑数据库（业务）

使用视图可以给用户和开发人员带来很多好处。具体为：

A. 对最终用户的好处

（ 1 ）结果更容易理解

创建视图时，可以将列名改为有意义的名称，使用户更容易理解列所代表的内容。在视图中修改列名不会影响基表的列名。

（ 2 ）获得数据更容易

很多人对 SQL 不太了解，因此对他们来说创建对多个表的复杂查询很困难。可以通过创建视图来方便用户访问多个表中的数据。

B. 对开发人员的好处

（ 1 ）限制数据检索更容易

开发人员有时需要隐藏某些行或列中的信息。通过使用视图，用户可以灵活地访问他们需要的数据，同时保证同一个表或其他表中的其他数据的安全性。要实现这一目标，可以在创建视图时将要对用户保密的列排除在外。

（ 2 ）维护应用程序更方便

​      调试视图比调试查询更容易。跟踪视图中过程的各个步骤中的错误更为容易，这是因为所有的步骤都是视图的组成部分 

从一个或者多个表或视图中导出的虚拟表，**其结构和数据是建立在对表的查询基础上的**。

理论上它可以像普通的物理表（实表）一样使用，例如增、删、改、查等，修改视图中的数据实际上是修改原始数据表。因为修改视图有许多限制，**所以在实际开发中一般视图仅做查询使用**。 

```
create view v_detail 
as 
select * from table
```

**实表的改变，会实时反映到虚表上**



## 索引（了解）  index

**用途**:我们对数据查询及处理速度已成为衡量应用系统成败的标准，而采用索引来加快数据处理速度通常是最普遍采用的**优化方法。**

**索引是什么**:数据库中的索引类似于一本书的目录，在一本书中使用目录可以快速找到你想要的信息，而不需要读完全书。在数据库中，数据库程序使用索引可以定向到表中的数据，而**不必扫描整个表**。书中的目录是一个字词以及各字词所在的页码列表，数据库中的索引是表中的值以及各值**存储位置的列表**。

**索引的利弊**：查询执行的大部分开销是I/O，使用索引提高性能的一个主要目标是**避免全表扫描**，因为全表扫描需要从磁盘上读取表的每一个数据页，如果有索引指向数据值，则查询只需要读少数次的磁盘就行啦。所以**合理的使用索引能加速数据的查询**。但是索引并不总是提高系统的性能，带索引的表需要在数据库中**占用更多的存储空间，同样用来增删数据的命令运行时间以及维护索引所需的处理时间会更长**。所以我们要合理使用索引，及时更新去除次优索引。

**系统会为primary key和unique自动创建索引**

是否适合创建索引的列：

1. 该列数据是否经常变化，如果经常变化，不适合创建索引
2. 查询使用频繁的列，适合创建索引



**拓展阅读**

[索引建立原则参考](http://www.cnblogs.com/knowledgesea/p/3672099.html)

创建索引语法

```
CREATE INDEX可对表增加普通索引或UNIQUE索引。
CREATE INDEX index_name ON table_name (column_list)
CREATE UNIQUE INDEX index_name ON table_name (column_list)
```

```
# 对course表的cname列创建索引，索引的名字为index_cname
create index index_cname 
on course(cname);
# 从course表中将索引index_cname删除
drop index index_cname on course;
```



##流程控制函数

**CASE-WHEN-THEN-ELSE-END**

CASE 
		WHEN 条件1 THEN 结果1 
		[WHEN 条件2 THEN 结果2 ...] 
		[ELSE 最终结果]       # 如果没有匹配的结果值，则返回结果为ELSE后的结果，如果没有ELSE部分，则返回值为 NULL 
END



查询学生的成绩信息，学号，姓名，课程名，成绩，以及成绩的等级

90-100优秀   75-89 良好 60-75及格  60分以下不及格

```
select s.sno as '学号',sname as '姓名',cname '课程名',grade '成绩',
			case 
			when grade>=90 then '优秀'
			when grade>=75 then '良好'
			when grade>=60 then '及格'
			ELSE '不及格'
			END AS '等级'    # END 表示case语句的结束  '等级'列的别名

from student s,sc, course c
where s.sno=sc.sno and c.cno=sc.cno
```

**系名练习**

查询学生信息，如果系名为CS，则显示计算机科学，如果系名为IS，则显示信息系统，如果系名为MA，则显示数学

```

```



**IF**

IF(条件,条件为真时执行,条件为假时执行);     #类似于java中的 boolean ? A : B 三目运算符

输出学生信息，如果性别为**F**则输出**女**，如果为**M**，则输出**男**

```
select sno 学号,sname 姓名,if(ssex='M','男','女') as 性别,sdept 所在系
from student
```

查询学生信息，如果系名为CS，则显示计算机科学，如果系名为IS，则显示信息系统，如果系名为MA，则显示数学

```
select sno 学号,sname 姓名,if(ssex='M','男','女') as 性别,
if(sdept='CS','计算机科学',if(sdept='MA','数学','信息系统')) 所在系
from student
```



**IFNULL**

IFNULL(EXP1,EXP2)

EXP1如果为空，则显示EXP2的内容，如果不为空，就显示其本身内容

查询课程的先修课，如果先修课为**null**，则显示**无**

```
select cno 课程号,cname 课程名,IFNULL(CPNO,'无') 先修课,ccredit 学分
from course
```



## 存储过程

存储过程（Stored Procedure）是**一组**为了完成特定功能的**SQL语句集**，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它。

存储过程是可编程的函数，在数据库中创建并保存，可以由**SQL语句和控制结构**组成。当想要在不同的应用程序或平台上执行相同的函数，或者封装特定功能时，存储过程是非常有用的。

**语法**
CREATE PROCEDURE 存储过程名 (参数列表)
  BEGIN 
		-- 代码
  END

**参数**

存储过程根据需要可能会有输入、输出、输入输出参数，多个参数用","分割开。共有三种参数类型,IN,OUT,INOUT:

**IN**参数的值必须在调用存储过程时**指定**，在存储过程中可以修改该参数的值，但不能被返回

**OUT**该值可在存储过程内部被改变，并**返回**

**INOUT**调用时**指定**，并且可被改变和**返回**

**调用**

调用存储过程: CALL 存储过程名(参数列表);

**删除**

删除存储过程: DROP PROCEDURE 存储过程名;



**实例1**

输入参数有多个

定义

```
DROP PROCEDURE IF EXISTS p1;# 如果存储过程p1存在则删除
# 创建存储过程
CREATE PROCEDURE p1(IN a INT, IN b VARCHAR(20))
BEGIN
	select a;
	select b;
	set a=20;
	set b='hello world';
	select a;
	select b;
END;
```

调用1

```
call p1(10,'abcd')
```

调用2

```
set @x=12;
set @y='abc';
call p1(@x,@y);

select @x;
select @y;
```

@@:表示系统变量
@:表示自定义变量

**实例2**

带有传出参数

```
DROP PROCEDURE IF EXISTS p2;

CREATE PROCEDURE p2(out a INT, out b VARCHAR(20))
BEGIN
	select a;   # 会输出null，参数值传不进来
	select b;   # 会输出null，参数值传不进来
	set a=20;
	set b='hello world';
	select a;
	select b;
END;
```

```
set @m=12;
set @n='abc';
call p2(@m,@n);
select @m;   # 显示存储过程中设置的值  20
select @n;   # 显示存储过程中设置的值  hello world
```

**实例3**

输入输出参数

```
DROP PROCEDURE IF EXISTS p3;

CREATE PROCEDURE p3(INOUT a INT, INOUT b VARCHAR(20))
BEGIN
	select a;
	select b;
	set a=20;
	set b='hello world';
	select a;
	select b;
END;
```

调用

```
set @m=12;
set @n='abc';
CALL p3(@m,@n);
```



**实例4**

变量声明

```
DROP PROCEDURE IF EXISTS p4;

CREATE PROCEDURE p4(INOUT str VARCHAR(50))
BEGIN
  # DECLARE 变量名[,...] 变量类型 [DEFAULT 默认值]
  # 局部变量声明必须在最上面,并且中间还不能有任何其他代码
	DECLARE a VARCHAR(32);
	DECLARE b VARCHAR(32);
	DECLARE c VARCHAR(32) DEFAULT 'hello';

  select sno,sname into a,b from student where sno='200215121';

	set str=CONCAT(a,b,c);

END;
```

调用

```
set @s='';
CALL p4(@s);
select @s;
```



**实例5**

IF-THEN--ELSEIF-THEN...--ELSE-END IF

```
DROP PROCEDURE IF EXISTS p5;

CREATE PROCEDURE p5(IN stu VARCHAR(10), IN cou INT)
BEGIN
    DECLARE DEG  INT;
    SELECT GRADE INTO DEG FROM sc WHERE SNO=stu and cno=cou;
    
    IF DEG>90
					THEN SELECT '优秀';
			ELSEIF DEG>85
					THEN SELECT '良好';
			ELSEIF  DEG>60
					THEN SELECT '及格';
			ELSE   select '不及格';
    END IF;
END;
```

调用

```
CALL p5('200215121',1)
```



**实例6**

[循环名:] **LOOP**
    要循环的代码
**END LOOP** [循环名]

**LEAVE** 循环名:这个语句被用来退出任何被标注的流程控制构造 (跳出某个循环)
**ITERATE** 循环名:跳出某个循环,进入下一次循环

创建表，通过存储过程向表中插入100条数据

```
create table t1(
id int PRIMARY key auto_increment
)
```

存储过程

```
create PROCEDURE p6()
BEGIN
DECLARE i  INT DEFAULT 1;

	sta:LOOP
			 IF i>100
				 THEN LEAVE sta;
			 END IF;
			 
			 INSERT into t1 VALUES(null);
			 SET i = i + 1;
	END LOOP sta;

end;
```

调用

```
call p6();
```

```
# 清空表中的数据：删除表，重建表，可知id自增长序列，从开开始
TRUNCATE TABLE t1;
# 清空表中的数据：将数据删除，可知id自增长序列，一直在增加
delete from t1;
```



**拓展学习**

存储过程的游标**cursor**，遍历表中的每一行数据





