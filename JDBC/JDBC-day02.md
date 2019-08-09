# JDBC

[TOC]



## 一、SQL注入（了解）

通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。具体来说，它是利用现有应用程序，将（恶意的）SQL命令注入到后台数据库引擎执行的能力，它可以通过在Web表单中输入（恶意）SQL语句得到一个存在安全漏洞的网站上的数据库，而不是按照设计者意图去执行SQL语句。 

```
user: tom  
password:aaaaaaa  
login
select * from user where name='tom' and password='aaaaaa';
# 注入写法
select * from user where name='mm' or '1'='1' and password='mm' or '1'='1';
```

Statement存不存在SQL注入问题?

**存在**



## 二、PreparedStatement 

PreparedStatement接口是Statement接口的子接口，它直接继承并重载了Statement的方法。 

创建PreparedStatement对象形式如下：

  ```
PreparedStatement psm=con.prepareStatement("INSERT INTO users(u_name,u_pass) VALUES(?,?)");  （占位符）
  ```

```
#  ?占位符表示的是一些条件值，不能表示列名
select pwd from sysuser where username='admin';
```



输入参数的赋值

  PreparedStatement中提供了大量的setXXX方法对输入参数进行赋值。根据输入参数的SQL类型应选用合适的setXXX方法。

如下更新代码示例

```
public int updateSysUser(SysUser user) {
		// 用来执行sql语句的
		PreparedStatement stmt = null;
		// 获取数据库连接
		Connection connection = ConnectionUtil.getConnection();
		int count = 0;
		try {
			String sql ="update sysuser set username=?,pwd=?,nickname=? where id=?";
			stmt = connection.prepareStatement(sql);
			stmt.setString(1, user.getUserName());
			stmt.setString(2, user.getPwd());
			stmt.setString(3, user.getNickName());
			stmt.setInt(4, user.getId());
			// 返回值为受影响的行数
			count = stmt.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			ConnectionUtil.closeAll(null, stmt, connection);

		}
		return count;
	}
```

setObject与封装

## 三、事务

事务（Transaction）是并发控制的单位，是用户定义的一个操作序列（一组sql语句）。**这些操作要么都做，要么都不做**，是一个不可分割的工作单位。 程序和事务是两个不同的概念。一般而言：一段程序中可能包含多个事务。

事务具备四个特性：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）、和持续性（Durability）。这四个特性也简称为ACID性。、

原子性 (Atomictiy)：事务的原子性是指一个事务中包含的一条语句或者多条语句构成了一个完整的逻辑单元，这个逻辑单元具有不可再分的原子性。这个逻辑单元要么**一起提交执行全部成功，要么一起提交执行全部失败**。

已支付订单生成

扣款成功记录

库存减少修改

这三个操作是一个完整的逻辑单元，原子性



一致性 (Consistency)：可以理解为**数据的完整性，事务的提交要确保在数据库上的操作没有破坏数据的完整性**，比如说不要违背一些约束的数据插入或者修改行为。一旦破坏了数据的完整性，数据库就会回滚（将之前的操作擦除）这个事务来确保数据库中的数据是一致的。

 

隔离性(Isolation)：与数据库中的事务隔离级别以及锁相关，多个用户可以对同一数据并发访问而又不破坏数据的正确性和完整性。但是，并行事务的修改必须与其它并行事务的修改相互独立，隔离。 但是在不同的隔离级别下，事务的读取操作可能得到的结果是不同的。**事务之间互不影响**

脏读

可重复读

序列化

 

持久性(Durability)：数据持久化，**事务一旦对数据的操作完成并提交后，数据修改就已经完成**，即使服务重启这些数据也不会改变。相反，如果在事务的执行过程中，系统服务崩溃或者重启，那么事务所有的操作就会被回滚，即回到事务操作之前的状态。事务一旦提交，**sql执行结果已更新到数据库，这就是持久性**

 

数据库的事务由下列语句组成：

​       一组DML(数据操作语言)语句，经过这组DML修改后数据将保持较好的一致性。  insert update delete 

​        一个 DDL（数据定义语言）语句。  create  drop

​         一个 DCL(数据控制语言) 语句。  权限赋予， grant  revoke

​       DDL和DCL语句最多只能有一个，因为DDL和DCL语句都会导致事务立即提交

**提交就是执行所定义的sql语句**



（了解）

事务开始：

insert

update

insert

create  导致事务理解提交

insert

update

事务提交



事务提交有两种方式：显式提交和自动提交。

  显式提交：使用commit。

  自动提交(隐式)：执行DDL或DCL，或者程序正常退出。

 当事务所包含的任意一个数据库操作执行失败后，应该回滚（rollback）事务，**使该事务中所作的修改全部失效**。事务回滚有两种方式：显式回滚和自动回滚。

显式回滚：使用rollback，让之前的所有事务操作全部失效。

自动回滚(隐式)：系统错误或者强行退出 sql语句执行发生异常。



事务支持

Connection的setAutoCommit方法来关闭自动提交，开启事务，如下SQL语句所示：

conn.setAutoCommit(false);

程序可以调用Connection的commit方法来提交事务，如下代码所示：

conn.commit();

如果任意一条SQL语句执行失败，我们应该用Connection的rollback来回滚事务，如下代码所示：

conn.rollback();

**事务案例**

创建表

```
create table good(
id int PRIMARY key auto_increment,
gname varchar(30)
)

create table gorder(
id int PRIMARY key auto_increment,
gid int,
bcount int,
FOREIGN key(gid) REFERENCES good(id)
)

create table sto(
id int PRIMARY key auto_increment,
gid int,
gcout int,
FOREIGN key(gid) REFERENCES good(id)
)
```

代码

```
public int updateTranGood(int buycount) {
		PreparedStatement prst = null;
		Connection conn = ConnectionUtil.getConnection();
		// 默认情况下，执行的一条SQL语句就是一个事务，事务只有提交才会持久化到数据库
		// 默认情况下，事务是自动提交的，代码执行后，就持久化到了数据库
		// 当设置conn.setAutoCommit(false);之后，事务改为手动提交
		int count = 0;
		int count1 = 0;
		try {
			conn.setAutoCommit(false);
			// 生成订单
			String sql = "insert into gorder values(null,1,?)";
			prst = conn.prepareStatement(sql);
			prst.setObject(1, buycount);
			count = prst.executeUpdate();
			System.out.println("c = " + count);
			// conn.rollback(); // 事务回滚，之前所有的操作均失效
			// 修改库存
			String sql2 = "update sto set gcout=gcout-? where gid=1";
			prst = conn.prepareStatement(sql2);
			prst.setObject(1, buycount);
			count1 = prst.executeUpdate();
			System.out.println("c1 = " + count1);

			conn.commit();// 事务提交，是修改持久化到数据库
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			ConnectionUtil.closeAll(null, prst, conn);
		}
		return count + count1;
	}
```









## 四、数据库连接池

数据库连接池的解决方案是：当应用程序启动时，系统主动建立足够的**数据库连接**，并将这些连接组成一个连接池。每次应用程序请求数据库连接时，无需重新打开连接，而是从池中取出已有的连接使用，使用完后，不再关闭数据库连接，而是直接将该连接归还给连接池。通过使用连接池，将大大提高程序运行效率。

数据库连接池的常用参数有如下：

数据库的初始连接数。  5

连接池的最大连接数。    20

连接池的最小连接数。   3   

连接池的每次增加的容量。   2

等待被杀死的空闲时间     60秒



**连接池连接的关闭是将连接放回连接池，连接池的连接用完也要关掉**





##五、作业

使用PreparedStatement完成对SysUser表的如下功能

1. 根据用户名进行模糊查找
2. 根据用户名或昵称进行模糊查找
3. 向表中插入一条数据
4. 按照昵称降序排列，查询表中的第二页数据，每页2条（选做）