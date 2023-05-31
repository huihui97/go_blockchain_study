# 数据库概述

## 数据库

**数据库**：database，简称为DB，是按照一定格式存储数据的一些文件的组合。其实就是：存储数据的仓库，实际上是一堆文件。这些文件中存储了特定格式的数据。



## 数据库管理系统

**数据库管理系统**：database management system，简称为DBMS。是专门用来管理数据库中的数据，数据库管理系统可以对数据库当中的数据进行增删改查。

**常用的数据库管理系统**：MySQL、Oracle、MS、SqlServer、DB2、sybase等



## SQL

**sql**：结构化查询语言，程序员通过学习sql语句，编写sql语句，然后DBMS负责执行sql语句，最终完成数据库中的增删改查操作。



## DB、DBMS和SQL三者关系

DBMS--执行-->sql语句--操作-->DB中的数据



## MySQL完美卸载

+ 第一步：双击安装包进行卸载
+ 第二步：删除目录
  + 删除 C:\Program Files (x86)\MySQL
  + 打开隐藏目录，删除 C:\ProgramData\MySQL
  + 删除安装目录：D:\MySQL\MySQL         D:\MySQL\MySQLData





## MySQL服务

启动服务两种方式：

+ 第一种：可以通过：计算机-->右键-->管理-->服务和应用程序-->服务-->找到mysql服务
  + mysql服务，默认是“启动”状态，只有启动了mysql才能用。
  + 默认情况下是“自动”启动
  + 方式：可以在mysql服务上点击右键：有启动、重启服务、停止服务等选项



+ 使用命令来启动和关闭mysql服务
  + 语法：net stop 服务名称          net start 服务名称
  + 其他服务的启动和停止同样采用以上的命令





## MySQL常用命令（不区分大小写）

+ 登录mysql ：`mysql -u root -p`

+ 退出mysql：`quit/exit`

+ 查看mysql中有哪些数据库：`show databases;`  ==注意：以分号结尾，英文形式==

  ```go
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sakila             |
  | sys                |
  | world              |
  +--------------------+
  ```

  mysql 默认自带了6个数据库。

+ 使用某个数据库：`use sys;`

  ```go
  mysql> use sys;
  Database changed  //表示正在使用一个名字叫做sys的数据库
  ```



+ 创建一个数据库：`create database test;`

  ```go
  mysql> use sys;
  Database changed
  mysql> create database test;  //表示创建了一个名为test的数据库
  Query OK, 1 row affected (0.00 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sakila             |
  | sys                |
  | test               |          //这就是新建的数据库
  | world              |
  +--------------------+
  7 rows in set (0.00 sec)
  ```



+ 删除一个数据库：`drop database test;`

  ```go
  mysql> drop database test;  //表示删除test数据库
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sakila             |  //可以看到之前创建的test数据库已经被删除了
  | sys                |
  | world              |
  +--------------------+
  6 rows in set (0.00 sec)
  ```



+ 查看数据库下有哪些表：`show tabales;`

  ```go
  mysql> use mysql  //表示：使用哪个数据库
  Database changed
  
  
  mysql> show tables;  //表示：查看该数据库下有哪些表
  +------------------------------------------------------+
  | Tables_in_mysql                                      |
  +------------------------------------------------------+
  | columns_priv                                         |
  | component                                            |
  | db                                                   |
  | default_roles                                        |
  | engine_cost                                          |
  | func                                                 |
  | general_log                                          |
  | global_grants                                        |
  | gtid_executed                                        |
  | help_category                                        |
  | help_keyword                                         |
  | help_relation                                        |
  | help_topic                                           |
  | innodb_index_stats                                   |
  | innodb_table_stats                                   |
  | ndb_binlog_index                                     |
  | password_history                                     |
  | plugin                                               |
  | procs_priv                                           |
  | proxies_priv                                         |
  | replication_asynchronous_connection_failover         |
  | replication_asynchronous_connection_failover_managed |
  | replication_group_configuration_version              |
  | replication_group_member_actions                     |
  | role_edges                                           |
  | server_cost                                          |
  | servers                                              |
  | slave_master_info                                    |
  | slave_relay_log_info                                 |
  | slave_worker_info                                    |
  | slow_log                                             |
  | tables_priv                                          |
  | time_zone                                            |
  | time_zone_leap_second                                |
  | time_zone_name                                       |
  | time_zone_transition                                 |
  | time_zone_transition_type                            |
  | user                                                 |
  +------------------------------------------------------+
  38 rows in set (0.00 sec)
  ```

  

+ 导入数据命令：`source sql文件绝对路径`

  ```go
  mysql> source D:\bjpowernode.sql  //导入sql文件，即数据库表
  ```

+ 查看表结构：`desc 表名;`

  ```go
  mysql> desc emp;
  ```

+ 查看表结构完整信息，包括注释，默认值等

  ```go
  mysql> show full columns from users_test;
  ```

+ 查看mysql数据库版本号：`select version();`

  ```go
  mysql> select version();
  +-----------+
  | version() |
  +-----------+
  | 8.0.31    |
  +-----------+
  1 row in set (0.00 sec)
  ```

+ 查看当前使用哪个数据库：`select database()`

  ```go
  mysql> select database();  //在mysql中，不见分号不执行，分号表示语句结束
  +-------------+
  | database()  |
  +-------------+
  | bjpowernode |  //表示：当前正在使用bjpowernode数据库
  +-------------+
  1 row in set (0.00 sec)
  ```

+ mysql中终止命令输入：`\c`

  ```go
  mysql> show
      ->
      -> \c  //表示：终止一条命令的输入
  mysql>
  ```

  





## 数据库基本单元-->表

==数据库当中最基本的单元是表：table==

### 什么是表

如下形式就是表，以表格的形式去存储数据的

| 姓名 | 性别 | 年龄 |
| ---- | ---- | ---- |
| 张三 | 男   | 20   |
| 李四 | 女   | 19   |
| 王五 | 男   | 21   |
|      |      |      |

### 为什么用表存储数据

数据库当中是以表格的形式存储数据的，因为表 比较直观。



任何一张表都有行和列：

+ 行（row）：被称为数据/记录
+ 列（column）：被称为字段  （比如上面有：姓名字段、性别字段、年龄字段，==每个字段都是有：字段名、数据类型、约束等属性==）
  + 字段名：就是普通名字，见名知意就行。
  + 数据类型：字符串、数字、日期等...
  + 约束：有很多约束，如唯一性约束：添加该约束后，该字段中的数据不能重复





## SQL分类

sql语句有很多，需要分门别类进行记忆。

+ DQL：数据查询语言（凡是带有select关键字的都是查询语句）
+ DML：数据操作语言（凡是对表当中的数据进行增删改的都是DML），insert、delete、update，主要操作表中的数据。
+ DDL：数据定义语言（凡是带有create、drop、alter都是DDL），主要操作表的结构（如：删掉表的某个字段、删除整个表或数据库等），而不是表中的数据
+ TCL：事务控制语言，包括：事务提交commit，事务回滚rollback。
+ DCL：数据控制语言：如授权grant，撤销权限revoke等。





## 导入数据

+ 导入数据命令：`source sql文件绝对路径`

```go
mysql> use bjpowernode //切换到需要导入表的数据库
Database changed

mysql> source D:\bjpowernode.sql  //导入sql文件，即数据库表
```

```go
mysql> show tables;        //查看bjpowernode数据库，可以看到数据库中包含了三个表:dept,emp,salgrade
+-----------------------+
| Tables_in_bjpowernode |
+-----------------------+
| dept                  |
| emp                   |
| salgrade              |
+-----------------------+
3 rows in set (0.00 sec)
```



+ 查看表中的数据：

```go
select * from 表名;   //这个是SQL语句，表示；从表中查询所有数据，其中*表示所有
```

```go
mysql> select * from emp;  //emp表中内容，员工表
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)
```

```go
mysql> select * from dept; //dept表中内容，部门表
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)
```

```go
mysql> select * from salgrade; //salgrade表中内容，工资等级表
+-------+-------+-------+
| GRADE | LOSAL | HISAL |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+
5 rows in set (0.00 sec)
```



+ 不看表中的数据，只看表的结构：`desc 表名;`  

```go
mysql> desc emp;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| EMPNO    | int         | NO   | PRI | NULL    |       |
| ENAME    | varchar(10) | YES  |     | NULL    |       |
| JOB      | varchar(9)  | YES  |     | NULL    |       |
| MGR      | int         | YES  |     | NULL    |       |
| HIREDATE | date        | YES  |     | NULL    |       |
| SAL      | double(7,2) | YES  |     | NULL    |       |
| COMM     | double(7,2) | YES  |     | NULL    |       |
| DEPTNO   | int         | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
8 rows in set (0.00 sec)
```

```go
mysql> desc dept;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| DEPTNO | int         | NO   | PRI | NULL    |       |
| DNAME  | varchar(14) | YES  |     | NULL    |       |
| LOC    | varchar(13) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

```go
mysql> desc salgrade; 
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| GRADE | int  | YES  |     | NULL    |       |
| LOSAL | int  | YES  |     | NULL    |       |
| HISAL | int  | YES  |     | NULL    |       |
+-------+------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```



## SQL脚本

```go
xxx.sql文件被称为sql脚本文件
sql脚本文件中编写了大量的sql语句，可以通过执行sql脚本文件来执行文件中的所有sql语句,实现批量处理sql语句。
用法：source sql脚本文件的绝对路径
	如： mysql> source D:\course\vip.sql

在实际工作中，第一天到公司，项目经理会给你一个 xxx.sql 脚本文件 ，执行该脚本文件来获取数据库的数据。
```





## MySQL注释

```mysql
//第一种单行注释： #  这是mysql特有的

#这里是注释内容，查询database_name库table_name表里面的数据
SELECT * FROM database_name.table_name;

//第二种单行注释：--，注意；--注释符后需要加一个空格，注释才能生效 ，这是sql语句通用的单行注释

-- 这里是注释内容，删除database_name库table_name表里面的数据
DELETE FROM database_name.table_name;

# 多行注释，/*...*/,多行注释跟go语言注释一致  ，这是sql语句通用的多行注释

/* 这里是注释
这里依然还是注释，查询database_name库table_name表里面的数据*/
SELECT * FROM database_name.table_name;
```



## mysql中常用数据类型

```go
//常见数据类型
varchar(最长255)  可变长度字符串，会根据实际数据长度动态分配空间 ，必须给出长度，否则报错啊！！！！！！！！！！！

//char可以不写建议长度，系统不会报错，但是varchar必须给出建议长度，否则系统报错
char(最长255)   定长字符串，不管实际数据长度是多少，分配固定长度的数据空间存储数据，使用不恰当的时候，可能会导致空间浪费，

char和varchar如何选择：比如性别是定长数据可以选择char，姓名是变长数据则可以选择varchar


longtext存储的是字符串，字符长度是0~4294967295，mediumtext长度是0~1677215 字节，所以当超过mediumtext存储长度可以使用longtext类型


int(最长11)  数字中的整型 ，可以不用给出具体长度，不会报错
bigint  数据中的长整型，默认20
float 单精度浮点型数据
double 双精度浮点型数据
tinyint(1)  mysql中没有bool类型，tinyint(1)只有两个值0和1，相当于bool类型
			当把一个数据设置成bool类型的时候,数据库会自动转换成tinyint(1)的数据类型


date  短日期类型 
datetime  长日期类型
clob  字符大对象，最多可以存储4G的字符串，比如可以存储一篇文章，超过255个字符都需要采用clob
blob 二进制大对象，专门用来存储图片、声音、视频等流媒体数据，往blob类型的字段上插入数据时，例如插入图片、视频等需要使用IO流
```







# 简单查询

==注意：对于SQL语句来说是通用的，所有SQL语句都是以`;`结尾，SQL语句是不区分大小写。==

+ 查看一个字段：`select 字段名 from 表名;`

  ```go
  mysql> select dname from dept;  //dname可以不区分大小写的，表示：查询某个字段
  +------------+
  | dname      |
  +------------+
  | ACCOUNTING |
  | RESEARCH   |
  | SALES      |
  | OPERATIONS |
  +------------+
  4 rows in set (0.00 sec)
  
  mysql> SELECT DNAME FROM DEPT;  //可以看到大小写都可以
  +------------+
  | DNAME      |
  +------------+
  | ACCOUNTING |
  | RESEARCH   |
  | SALES      |
  | OPERATIONS |
  +------------+
  4 rows in set (0.00 sec)
  ```



+ 查询两个或多个字段：`select 字段名1，字段名2，... from 表名;`

  对于查看多个字段名，用逗号隔开即可。

  ```go
  mysql> select dname,loc from dept; //表示：查询其中多个字段
  +------------+----------+
  | dname      | loc      |   //因为，查询时用小写，所以显示也是小写
  +------------+----------+
  | ACCOUNTING | NEW YORK |
  | RESEARCH   | DALLAS   |
  | SALES      | CHICAGO  |
  | OPERATIONS | BOSTON   |
  +------------+----------+
  4 rows in set (0.00 sec)
  ```



+ 查询所有字段：

  + 第一种方式：把所有字段名都写上 如：`select deptno,dname,loc from dept;`

    ```go
    mysql> select deptno,dname,loc from dept;  //写出所有字段名进行查询，效率高
    +--------+------------+----------+
    | deptno | dname      | loc      |
    +--------+------------+----------+
    |     10 | ACCOUNTING | NEW YORK |
    |     20 | RESEARCH   | DALLAS   |
    |     30 | SALES      | CHICAGO  |
    |     40 | OPERATIONS | BOSTON   |
    +--------+------------+----------+
    4 rows in set (0.00 sec)
    ```

    

  + 第二种方式：`select * from dept;`

    ```go
    mysql> select * from dept;  //用*代替所有字段名，效率低，可读性差，因为最后也是将*转换为所有字段名，不推荐使用。
    +--------+------------+----------+
    | DEPTNO | DNAME      | LOC      |
    +--------+------------+----------+
    |     10 | ACCOUNTING | NEW YORK |
    |     20 | RESEARCH   | DALLAS   |
    |     30 | SALES      | CHICAGO  |
    |     40 | OPERATIONS | BOSTON   |
    +--------+------------+----------+
    4 rows in set (0.00 sec)
    ```

    

+ 给查询的列（字段）起别名：`用as关键字起别名 或者 直接用空格`

  ```go
  mysql> select dname as deptname,loc from dept;  //用as来起别名
  +------------+----------+
  | deptname   | loc      |  //可以看到 dname-->deptname
  +------------+----------+
  | ACCOUNTING | NEW YORK |
  | RESEARCH   | DALLAS   |
  | SALES      | CHICAGO  |
  | OPERATIONS | BOSTON   |
  +------------+----------+
  4 rows in set (0.00 sec)
  
  mysql> select dname deptname,loc from dept; //用空格起别名
  +------------+----------+
  | deptname   | loc      |   //可以看到dname-->deptname
  +------------+----------+
  | ACCOUNTING | NEW YORK |
  | RESEARCH   | DALLAS   |
  | SALES      | CHICAGO  |
  | OPERATIONS | BOSTON   |
  +------------+----------+
  4 rows in set (0.00 sec)
  
  //如果在起别名时，别名中存在空格，直接运行则会报错，需要用单引号或双引号将该别名括起来。
  mysql> select dname dept name,loc from dept;
  ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'name,loc from dept' at line 1
  
  mysql> select dname 'dept name',loc from dept; //可以看到，别名中存在空格，此时需要用单引号括起来，也可以用双引号
  +------------+----------+
  | dept name  | loc      |
  +------------+----------+
  | ACCOUNTING | NEW YORK |
  | RESEARCH   | DALLAS   |
  | SALES      | CHICAGO  |
  | OPERATIONS | BOSTON   |
  +------------+----------+
  4 rows in set (0.00 sec)
  ```
  
  ==注意：只是将显示的查询结果列名显示为别名，原表的列名没有发生变化，select语句永远都不会进行修改操作==
  
  ==**注意：在所有数据库中，字符串统一使用单引号，单引号是标准，双引号在oracle数据库中无法使用，在mysql数据库中是可以使用的**。==



+ 列参与数学运算

  + 如计算员工工资：`mysql> select ename,sal*12 from emp;`

    ```go
    mysql> select ename,sal from emp;  //查看员工工资
    +--------+---------+
    | ename  | sal     |
    +--------+---------+
    | SMITH  |  800.00 |
    | ALLEN  | 1600.00 |
    | WARD   | 1250.00 |
    | JONES  | 2975.00 |
    | MARTIN | 1250.00 |
    | BLAKE  | 2850.00 |
    | CLARK  | 2450.00 |
    | SCOTT  | 3000.00 |
    | KING   | 5000.00 |
    | TURNER | 1500.00 |
    | ADAMS  | 1100.00 |
    | JAMES  |  950.00 |
    | FORD   | 3000.00 |
    | MILLER | 1300.00 |
    +--------+---------+
    14 rows in set (0.00 sec)
    
    mysql> select ename,sal*12 from emp;  //这里直接可以对工资×12个月操作，是可以的，只是显示而原内容没有发生变化
    +--------+----------+
    | ename  | sal*12   |
    +--------+----------+
    | SMITH  |  9600.00 |
    | ALLEN  | 19200.00 |
    | WARD   | 15000.00 |
    | JONES  | 35700.00 |
    | MARTIN | 15000.00 |
    | BLAKE  | 34200.00 |
    | CLARK  | 29400.00 |
    | SCOTT  | 36000.00 |
    | KING   | 60000.00 |
    | TURNER | 18000.00 |
    | ADAMS  | 13200.00 |
    | JAMES  | 11400.00 |
    | FORD   | 36000.00 |
    | MILLER | 15600.00 |
    +--------+----------+
    14 rows in set (0.00 sec)
    
    mysql> select ename,sal*12 as yearsal from emp;  //这里起了一个别名来代替 sal*12
    +--------+----------+
    | ename  | yearsal  |
    +--------+----------+
    | SMITH  |  9600.00 |
    | ALLEN  | 19200.00 |
    | WARD   | 15000.00 |
    | JONES  | 35700.00 |
    | MARTIN | 15000.00 |
    | BLAKE  | 34200.00 |
    | CLARK  | 29400.00 |
    | SCOTT  | 36000.00 |
    | KING   | 60000.00 |
    | TURNER | 18000.00 |
    | ADAMS  | 13200.00 |
    | JAMES  | 11400.00 |
    | FORD   | 36000.00 |
    | MILLER | 15600.00 |
    +--------+----------+
    14 rows in set (0.00 sec)
    
    mysql> select ename,sal*12 as '年薪' from emp;  //别名是中文，需要用单引号括起来
    +--------+----------+
    | ename  | 年薪     |
    +--------+----------+
    | SMITH  |  9600.00 |
    | ALLEN  | 19200.00 |
    | WARD   | 15000.00 |
    | JONES  | 35700.00 |
    | MARTIN | 15000.00 |
    | BLAKE  | 34200.00 |
    | CLARK  | 29400.00 |
    | SCOTT  | 36000.00 |
    | KING   | 60000.00 |
    | TURNER | 18000.00 |
    | ADAMS  | 13200.00 |
    | JAMES  | 11400.00 |
    | FORD   | 36000.00 |
    | MILLER | 15600.00 |
    +--------+----------+
    14 rows in set (0.00 sec)
    ```

    ==**结论：字段可以使用数学表达式，即可以对字段进行加减乘除：+ - * /**==

    **==注意：别名是中文，必须使用单引号括起来==**





# 条件查询

## 定义：

条件查询：不是将表中所有数据都查出来，而是根据符合条件来查询数据。



## 语法格式

`select 字段1,字段2,字段3 from 表名 where 条件;`

==条件查询需要用到 where 语句，where 必须放到 from 语句表的后面==



## 支持的运算符

| 运算符          | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| =               | 等于  ，go语言中是==，在sql语句中是=                         |
| <>或!=          | 不等于                                                       |
| <               | 小于                                                         |
| <=              | 小于等于                                                     |
| >               | 大于                                                         |
| >=              | 大于等于                                                     |
| between …and …. | 两个值之间，等同于 >= and <=，  使用between and 必须遵循***左小右大***原则 |
| is null         | 为 null（is not null 不为空），对于null只能用is ，不能用等号 |
| and             | 并且                                                         |
| or              | 或者                                                         |
| in              | 包含，相当于多个 or（not in 不在这个范围中）                 |
| not             | not可以取非，主要用在 is 或 in 中                            |
| like            | like 称为模糊查询，支持%或下划线匹配，% 匹配任意个字符，_  下划线只匹配一个字符 |

==注意：==

+ ==在数据库中，null不能使用等号查询，只能使用 is ，因为数据库中的null代表什么都没有，不是一个值，所以不能使用等号衡量。==
+ ==and比or的优先级高，先执行and，再执行or==
+ ==in相当于多个or，in不是一个区间，in后面跟具体的值，相当于离散值的集合，是否属于该集合==
+ ==%和_ 都是一个特殊的符号==







```go
mysql> select ename,sal from emp where sal = 3000;  //查询工资为3000的员工姓名和该员工工资
+-------+---------+
| ename | sal     |
+-------+---------+
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
+-------+---------+
2 rows in set (0.00 sec)

mysql> select ename,sal from emp where ename='smith';  //也可以通过字符串来查询，ename字段名为字符串类型
+-------+--------+
| ename | sal    |
+-------+--------+
| SMITH | 800.00 |
+-------+--------+
1 row in set (0.00 sec)

mysql> select ename,sal from emp where sal between 2450 and 3000;
//等价于：mysql> select ename,sal from emp where sal >=2450 and sal <= 3000;
+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
+-------+---------+
5 rows in set (0.00 sec)

mysql> select ename,sal,comm from emp where comm = null;  //针对null，用等号无法查询出来
Empty set (0.00 sec)

mysql> select ename,sal,comm from emp where comm is null;  //针对null 只能通过is来查询
+--------+---------+------+
| ename  | sal     | comm |
+--------+---------+------+
| SMITH  |  800.00 | NULL |
| JONES  | 2975.00 | NULL |
| BLAKE  | 2850.00 | NULL |
| CLARK  | 2450.00 | NULL |
| SCOTT  | 3000.00 | NULL |
| KING   | 5000.00 | NULL |
| ADAMS  | 1100.00 | NULL |
| JAMES  |  950.00 | NULL |
| FORD   | 3000.00 | NULL |
| MILLER | 1300.00 | NULL |
+--------+---------+------+
10 rows in set (0.00 sec)

mysql> select ename,sal,comm from emp where comm is not null;  //not主要用于is和in中
+--------+---------+---------+
| ename  | sal     | comm    |
+--------+---------+---------+
| ALLEN  | 1600.00 |  300.00 |
| WARD   | 1250.00 |  500.00 |
| MARTIN | 1250.00 | 1400.00 |
| TURNER | 1500.00 |    0.00 |
+--------+---------+---------+
4 rows in set (0.00 sec)

mysql> select empno,ename,job from emp where job='manager' or job='salesman';
+-------+--------+----------+
| empno | ename  | job      |
+-------+--------+----------+
|  7499 | ALLEN  | SALESMAN |
|  7521 | WARD   | SALESMAN |
|  7566 | JONES  | MANAGER  |
|  7654 | MARTIN | SALESMAN |
|  7698 | BLAKE  | MANAGER  |
|  7782 | CLARK  | MANAGER  |
|  7844 | TURNER | SALESMAN |
+-------+--------+----------+
7 rows in set (0.00 sec)

//找出工资大于2500并且部门编号为10或20的员工
mysql> select * from emp where sal > 2500 and (deptno=10 or deptno=20);  //小括号优先级比and高，括号内先执行
+-------+-------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+-------+-----------+------+------------+---------+------+--------+
|  7566 | JONES | MANAGER   | 7839 | 1981-04-02 | 2975.00 | NULL |     20 |
|  7788 | SCOTT | ANALYST   | 7566 | 1987-04-19 | 3000.00 | NULL |     20 |
|  7839 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
|  7902 | FORD  | ANALYST   | 7566 | 1981-12-03 | 3000.00 | NULL |     20 |
+-------+-------+-----------+------+------------+---------+------+--------+
4 rows in set (0.00 sec)

//in相当于多个or，in不是一个区间，in后面跟具体的值，相当于离散值的集合，是否属于该集合
mysql> select empno,ename,job from emp where job in ('manager','salesman');
+-------+--------+----------+
| empno | ename  | job      |
+-------+--------+----------+
|  7499 | ALLEN  | SALESMAN |
|  7521 | WARD   | SALESMAN |
|  7566 | JONES  | MANAGER  |
|  7654 | MARTIN | SALESMAN |
|  7698 | BLAKE  | MANAGER  |
|  7782 | CLARK  | MANAGER  |
|  7844 | TURNER | SALESMAN |
+-------+--------+----------+
7 rows in set (0.00 sec)

//not in 表示不在这几个值中的数据
mysql> select empno,ename,job from emp where job not in ('manager','salesman');
+-------+--------+-----------+
| empno | ename  | job       |
+-------+--------+-----------+
|  7369 | SMITH  | CLERK     |
|  7788 | SCOTT  | ANALYST   |
|  7839 | KING   | PRESIDENT |
|  7876 | ADAMS  | CLERK     |
|  7900 | JAMES  | CLERK     |
|  7902 | FORD   | ANALYST   |
|  7934 | MILLER | CLERK     |
+-------+--------+-----------+
7 rows in set (0.00 sec)
```



**模糊查询：**

+ %表示任意个字符
+ _表示一个任意字符

```go
//找出名字含有O的
mysql> select ename from emp where ename like '%O%';  //两个%之间，表示含有，之所以加单引号，是因为ename字段为字符串
+-------+
| ename |
+-------+
| JONES |
| SCOTT |
| FORD  |
+-------+
3 rows in set (0.00 sec)

//找出以K开头的
mysql> select ename from emp where ename like 'K%';//等价select ename from emp where substr(ename,1,1)='K';
+-------+
| ename |
+-------+
| KING  |
+-------+
1 row in set (0.00 sec)

//找出以T结尾的
mysql> select ename from emp where ename like '%T';
+-------+
| ename |
+-------+
| SCOTT |
+-------+
1 row in set (0.00 sec)

//找出第二个字母为A的
mysql> select ename from emp where ename like '_A%';  //_ 代表了一个字母
+--------+
| ename  |
+--------+
| WARD   |
| MARTIN |
| JAMES  |
+--------+
3 rows in set (0.00 sec)

//找出第三个字母为R的
mysql> select ename from emp where ename like '__R%'; //这里是两个下划线
+--------+
| ename  |
+--------+
| WARD   |
| MARTIN |
| TURNER |
| FORD   |
+--------+
4 rows in set (0.00 sec)

//找出第一个字母为W 第三个字母为R的
mysql> select ename from emp where ename like 'W_R%';
+-------+
| ename |
+-------+
| WARD  |
+-------+
1 row in set (0.00 sec)

//找出开头为W，包含R的
mysql> select ename from emp where ename like 'W%R%';
+-------+
| ename |
+-------+
| WARD  |
+-------+
1 row in set (0.00 sec)



如果存在一个名字中带有下划线的，怎么找出，由于下划线是特殊符号，因此需要转义 \
如：select name from student where name like '%\_%'  //必须使用\将下划线转义为普通字符。
```







# 排序数据

排序关键字：`order by`



## 单个字段排序

+ 默认升序：

  ```go
  mysql> select ename,sal from emp order by sal;  //不加任何东西，默认为升序
  +--------+---------+
  | ename  | sal     |
  +--------+---------+
  | SMITH  |  800.00 |
  | JAMES  |  950.00 |
  | ADAMS  | 1100.00 |
  | WARD   | 1250.00 |
  | MARTIN | 1250.00 |
  | MILLER | 1300.00 |
  | TURNER | 1500.00 |
  | ALLEN  | 1600.00 |
  | CLARK  | 2450.00 |
  | BLAKE  | 2850.00 |
  | JONES  | 2975.00 |
  | SCOTT  | 3000.00 |
  | FORD   | 3000.00 |
  | KING   | 5000.00 |
  +--------+---------+
  14 rows in set (0.00 sec)
  ```



+ 指定降序：

  ```go
  mysql> select ename,sal from emp order by sal desc;  //在后面添加desc，代表下降descend
  +--------+---------+
  | ename  | sal     |
  +--------+---------+
  | KING   | 5000.00 |
  | SCOTT  | 3000.00 |
  | FORD   | 3000.00 |
  | JONES  | 2975.00 |
  | BLAKE  | 2850.00 |
  | CLARK  | 2450.00 |
  | ALLEN  | 1600.00 |
  | TURNER | 1500.00 |
  | MILLER | 1300.00 |
  | WARD   | 1250.00 |
  | MARTIN | 1250.00 |
  | ADAMS  | 1100.00 |
  | JAMES  |  950.00 |
  | SMITH  |  800.00 |
  +--------+---------+
  14 rows in set (0.00 sec)
  ```

  

+ 指定升序：

  ```go
  mysql> select ename,sal from emp order by sal asc;   //在后面添加asc，代表上升ascend
  +--------+---------+
  | ename  | sal     |
  +--------+---------+
  | SMITH  |  800.00 |
  | JAMES  |  950.00 |
  | ADAMS  | 1100.00 |
  | WARD   | 1250.00 |
  | MARTIN | 1250.00 |
  | MILLER | 1300.00 |
  | TURNER | 1500.00 |
  | ALLEN  | 1600.00 |
  | CLARK  | 2450.00 |
  | BLAKE  | 2850.00 |
  | JONES  | 2975.00 |
  | SCOTT  | 3000.00 |
  | FORD   | 3000.00 |
  | KING   | 5000.00 |
  +--------+---------+
  14 rows in set (0.00 sec)
  ```

  



## 多个字段排序

```go
//查询员工名字和工资，按照工资升序排，如果工资一样，再按照名字升序排
//by后面跟了两个字段名，先按照第一个字段排，再按照第二个字段排，即有先后顺序
mysql> select ename, sal from emp order by sal asc ,ename asc ;  //多个字段，用逗号隔开
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| MARTIN | 1250.00 |  //可以看到，按照工资排时，工资相同时再按照姓名排
| WARD   | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| FORD   | 3000.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
```



## 根据字段位置排序

```go
//了解以下，在开发中不建议这么写，因为不健壮
//注意：2表示你查询字段的第二个，而不是原始表中的第二个字段
mysql> select ename, sal from emp order by 2; //这里2表示第二列，即为sal，
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| JAMES  |  950.00 |
| ADAMS  | 1100.00 |
| WARD   | 1250.00 |
| MARTIN | 1250.00 |
| MILLER | 1300.00 |
| TURNER | 1500.00 |
| ALLEN  | 1600.00 |
| CLARK  | 2450.00 |
| BLAKE  | 2850.00 |
| JONES  | 2975.00 |
| SCOTT  | 3000.00 |
| FORD   | 3000.00 |
| KING   | 5000.00 |
+--------+---------+
14 rows in set (0.00 sec)
```





## 综合案例

```go
//找出工资在1250到3000之间的员工信息，要求按照薪资降序排列
//关键字顺序不能变，只能按照：select...from ... where ... order by ...;

mysql> select empno,ename,sal from emp where sal between 1250 and 3000 order by sal desc;
+-------+--------+---------+
| empno | ename  | sal     |
+-------+--------+---------+
|  7788 | SCOTT  | 3000.00 |
|  7902 | FORD   | 3000.00 |
|  7566 | JONES  | 2975.00 |
|  7698 | BLAKE  | 2850.00 |
|  7782 | CLARK  | 2450.00 |
|  7499 | ALLEN  | 1600.00 |
|  7844 | TURNER | 1500.00 |
|  7934 | MILLER | 1300.00 |
|  7521 | WARD   | 1250.00 |
|  7654 | MARTIN | 1250.00 |
+-------+--------+---------+
10 rows in set (0.00 sec)
```

==语句执行顺序：==

+ 第一步：from
+ 第二步：where
+ 第三步：select
+ 第四步：order by （排序总是在最后执行）





# 数据处理函数

**数据处理函数又被称为单行处理函数：一个输入对应一个输出**



## 常见单行处理函数

| 函数名      | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| lower       | 转换为小写                                                   |
| upper       | 转换为大写                                                   |
| substr      | 取子串（substr（被截取的字符串，起始下标，截取长度））//**在MySQL中字符串的起始下标从1开始** |
| length      | 取长度                                                       |
| trim        | 去掉前后空格                                                 |
| concat      | 字符串拼接，concat(str1, str2, ...)                          |
| str_to_date | 将字符串转换为日期                                           |
| date_format | 将日期转换为特定格式的字符串                                 |
| format      | 对数字进行格式化，设置千分位                                 |
| round       | round(x,d) ,x指要处理的数,d是指保留几位小数 ，如果没有d，则默认为保留0位，如果为负数，指定小数点左边的d位整数位为0 |
| rand()      | 格式就是：rand()，作用：生成小于1的随机数                    |
| ifnull      | 空处理函数，专门处理空null，可以将 null 转换成一个具体值     |

==注意：**sql字符串中的下标从1开始，而不是从0开始**，与go语言是有区别的，go语言的字符串是从0开始的==

==注意：**在数据库中，只要有null参与的数学运算都为null**==



## 示例

```go
//转小写
mysql> select lower(ename) from emp;  //一行一行数据进行处理，14个输入对应14个输出
+--------------+
| lower(ename) |
+--------------+
| smith        |
| allen        |
| ward         |
| jones        |
| martin       |
| blake        |
| clark        |
| scott        |
| king         |
| turner       |
| adams        |
| james        |
| ford         |
| miller       |
+--------------+
14 rows in set (0.00 sec)

//取子串
mysql> select substr(ename,1,1) from emp;
+-------------------+
| substr(ename,1,1) |
+-------------------+
| S                 |
| A                 |
| W                 |
| J                 |
| M                 |
| B                 |
| C                 |
| S                 |
| K                 |
| T                 |
| A                 |
| J                 |
| F                 |
| M                 |
+-------------------+
14 rows in set (0.00 sec)
 
mysql> select concat(ename,'--',sal)from emp;  //concat函数为字符串拼接
+------------------------+
| concat(ename,'--',sal) |
+------------------------+
| SMITH--800.00          |
| ALLEN--1600.00         |
| WARD--1250.00          |
| JONES--2975.00         |
| MARTIN--1250.00        |
| BLAKE--2850.00         |
| CLARK--2450.00         |
| SCOTT--3000.00         |
| KING--5000.00          |
| TURNER--1500.00        |
| ADAMS--1100.00         |
| JAMES--950.00          |
| FORD--3000.00          |
| MILLER--1300.00        |
+------------------------+
14 rows in set (0.00 sec)

mysql> select length(ename) from emp;  //获取字符串长度
+---------------+
| length(ename) |
+---------------+
|             5 |
|             5 |
|             4 |
|             5 |
|             6 |
|             5 |
|             5 |
|             5 |
|             4 |
|             6 |
|             5 |
|             5 |
|             4 |
|             6 |
+---------------+
14 rows in set (0.00 sec)

mysql> select * from emp where ename = ' King ';
Empty set (0.00 sec)

mysql> select * from emp where ename =trim(' King '); //去掉字符串前后空格
+-------+-------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+-------+-----------+------+------------+---------+------+--------+
|  7839 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
+-------+-------+-----------+------+------------+---------+------+--------+
1 row in set (0.00 sec)

//一个无法解释的现象，如果select 后面跟的不是一个字段名而是一个字面值
mysql> select 'abc' from emp;  //用单引号括起来表示一个字面值，没有单引号则会当成一个字段名取查找，如果没有就报错
+-----+
| abc |
+-----+
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
| abc |
+-----+
14 rows in set (0.00 sec)

mysql> select round(3.56,1) from emp;  //round根据保留位数向上取
+---------------+
| round(3.56,1) |
+---------------+
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
|           3.6 |
+---------------+
14 rows in set (0.00 sec)

mysql> select round(32555.5611,-1) from emp;  //将小数点左边第d位置为0，然后四舍五入
+----------------------+
| round(32555.5611,-1) |  //可以看到小数点左边第一位变为0了，小数点左边第二位变为6，还是因为向上取
+----------------------+
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
|                32560 |
+----------------------+
14 rows in set (0.00 sec)

mysql> select rand() from emp;  //生成小于1的随机数
+---------------------+
| rand()              |
+---------------------+
|  0.8427907301511529 |
| 0.25274003879422324 |
|  0.7353280342513026 |
|  0.9184200856074878 |
|  0.3861163560171764 |
|  0.1753215539970636 |
|  0.7182587326674338 |
| 0.06532903207962311 |
|  0.1718689931294592 |
|  0.6633579001420716 |
|  0.8011825520556257 |
| 0.01583909058555876 |
|  0.6756478274945615 |
|  0.3307218964497763 |
+---------------------+
14 rows in set (0.00 sec)


```



ifnull 用法：`ifnull(数据,被当作哪个值)`，即如果某个数据为空，则将该数学当作指定的值来使用。

```go
mysql> select ename,sal+comm from emp;  //有null参与的数学运算都为空null
+--------+----------+
| ename  | sal+comm |
+--------+----------+
| SMITH  |     NULL |
| ALLEN  |  1900.00 |
| WARD   |  1750.00 |
| JONES  |     NULL |
| MARTIN |  2650.00 |
| BLAKE  |     NULL |
| CLARK  |     NULL |
| SCOTT  |     NULL |
| KING   |     NULL |
| TURNER |  1500.00 |
| ADAMS  |     NULL |
| JAMES  |     NULL |
| FORD   |     NULL |
| MILLER |     NULL |
+--------+----------+
14 rows in set (0.00 sec)

mysql> select ename, sal+ifnull(comm,0) from emp;  //如果comm为null，则将其视为0来执行
+--------+--------------------+
| ename  | sal+ifnull(comm,0) |
+--------+--------------------+
| SMITH  |             800.00 |
| ALLEN  |            1900.00 |
| WARD   |            1750.00 |
| JONES  |            2975.00 |
| MARTIN |            2650.00 |
| BLAKE  |            2850.00 |
| CLARK  |            2450.00 |
| SCOTT  |            3000.00 |
| KING   |            5000.00 |
| TURNER |            1500.00 |
| ADAMS  |            1100.00 |
| JAMES  |             950.00 |
| FORD   |            3000.00 |
| MILLER |            1300.00 |
+--------+--------------------+
14 rows in set (0.00 sec)
```



==**case...end函数格式：case...when...then...when...then...else...end**==，该函数同样不修改数据库，只是针对显示结果。

```go
示例：当员工工作岗位为manager，工资上调10%，当员工工作岗位为salesman，工资上调50%，其他正常。

//可以看到：case后面的job与when后面的值进行匹配，匹配成功则执行then后面的语句，该函数相当于go语言中的switch语句用法

mysql> select ename,job,sal as oldsal,(case job when 'manager' then sal*1.1 when 'salesman' then sal*1.5 else sal end) as newsal from emp;  //起别名时需要将case..end内容括起来，否则无法区分
+--------+-----------+---------+---------+
| ename  | job       | oldsal  | newsal  |
+--------+-----------+---------+---------+
| SMITH  | CLERK     |  800.00 |  800.00 |
| ALLEN  | SALESMAN  | 1600.00 | 2400.00 |
| WARD   | SALESMAN  | 1250.00 | 1875.00 |
| JONES  | MANAGER   | 2975.00 | 3272.50 |
| MARTIN | SALESMAN  | 1250.00 | 1875.00 |
| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
| CLARK  | MANAGER   | 2450.00 | 2695.00 |
| SCOTT  | ANALYST   | 3000.00 | 3000.00 |
| KING   | PRESIDENT | 5000.00 | 5000.00 |
| TURNER | SALESMAN  | 1500.00 | 2250.00 |
| ADAMS  | CLERK     | 1100.00 | 1100.00 |
| JAMES  | CLERK     |  950.00 |  950.00 |
| FORD   | ANALYST   | 3000.00 | 3000.00 |
| MILLER | CLERK     | 1300.00 | 1300.00 |
+--------+-----------+---------+---------+
14 rows in set (0.00 sec)
                                       
                                       
//或者case后面不接字段，when后面为判断表达式也可以
mysql> select ename ,job,sal as oldsal, (case when job='manager' then sal*1.1 when job='salesman' then sal*.15 end) as newsal from emp; //这里存在一个问题就是：只有满足when条件的才会计算，不满足when不会计算，直接为null
+--------+-----------+---------+---------+
| ename  | job       | oldsal  | newsal  |
+--------+-----------+---------+---------+
| SMITH  | CLERK     |  800.00 |    NULL |
| ALLEN  | SALESMAN  | 1600.00 |  240.00 |
| WARD   | SALESMAN  | 1250.00 |  187.50 |
| JONES  | MANAGER   | 2975.00 | 3272.50 |
| MARTIN | SALESMAN  | 1250.00 |  187.50 |
| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
| CLARK  | MANAGER   | 2450.00 | 2695.00 |
| SCOTT  | ANALYST   | 3000.00 |    NULL |
| KING   | PRESIDENT | 5000.00 |    NULL |
| TURNER | SALESMAN  | 1500.00 |  225.00 |
| ADAMS  | CLERK     | 1100.00 |    NULL |
| JAMES  | CLERK     |  950.00 |    NULL |
| FORD   | ANALYST   | 3000.00 |    NULL |
| MILLER | CLERK     | 1300.00 |    NULL |
+--------+-----------+---------+---------+
14 rows in set (0.00 sec)          
                                         

//如果case后面什么都不带，那么就要添加else语句                                         
mysql> select ename ,job,sal as oldsal, (case when job='manager' then sal*1.1 when job='salesman' then sal*.15 else sal end) as newsal from emp;
+--------+-----------+---------+---------+
| ename  | job       | oldsal  | newsal  |
+--------+-----------+---------+---------+
| SMITH  | CLERK     |  800.00 |  800.00 |
| ALLEN  | SALESMAN  | 1600.00 |  240.00 |
| WARD   | SALESMAN  | 1250.00 |  187.50 |
| JONES  | MANAGER   | 2975.00 | 3272.50 |
| MARTIN | SALESMAN  | 1250.00 |  187.50 |
| BLAKE  | MANAGER   | 2850.00 | 3135.00 |
| CLARK  | MANAGER   | 2450.00 | 2695.00 |
| SCOTT  | ANALYST   | 3000.00 | 3000.00 |
| KING   | PRESIDENT | 5000.00 | 5000.00 |
| TURNER | SALESMAN  | 1500.00 |  225.00 |
| ADAMS  | CLERK     | 1100.00 | 1100.00 |
| JAMES  | CLERK     |  950.00 |  950.00 |
| FORD   | ANALYST   | 3000.00 | 3000.00 |
| MILLER | CLERK     | 1300.00 | 1300.00 |
+--------+-----------+---------+---------+
14 rows in set (0.00 sec)
```

















# 分组函数

**分组函数又称为多行处理函数：多个输入对应一个输出**

## 常用函数：

| 函数名 | 功能   |
| ------ | ------ |
| count  | 计数   |
| sum    | 求和   |
| avg    | 平均值 |
| max    | 最大值 |
| min    | 最小值 |

==注意：分组函数在使用的时候必须先分组，然后才能使用。如果没有对数据分组，则整张表默认为一组==

## 示例

```go
//找出最高工资
mysql> select ename,max(sal) from emp; //select后面不能加ename是因为默认将整张表当成一组，并没有将ename来分组
ERROR 1140 (42000): In aggregated query without GROUP BY, expression #1 of SELECT list contains nonaggregated column 'bjpowernode.emp.ENAME'; this is incompatible with sql_mode=only_full_group_by

mysql> select max(sal) from emp;
+----------+
| max(sal) |
+----------+
|  5000.00 |
+----------+
1 row in set (0.00 sec)

//找出最低工资
mysql> select min(sal) from emp;
+----------+
| min(sal) |
+----------+
|   800.00 |
+----------+
1 row in set (0.00 sec)

//计算工资和
mysql> select sum(sal) from emp;
+----------+
| sum(sal) |
+----------+
| 29025.00 |
+----------+
1 row in set (0.00 sec)

//计算平均工资
mysql> select avg(sal) from emp;
+-------------+
| avg(sal)    |
+-------------+
| 2073.214286 |
+-------------+
1 row in set (0.00 sec)

//计算员工数量
mysql> select count(ename) from emp;
+--------------+
| count(ename) |
+--------------+
|           14 |
+--------------+
1 row in set (0.00 sec)
```



## 分组函数注意事项：

+ ==**分组函数自动忽略null，不需要提前对null进行处理**==

  ```go
  mysql> select comm from emp;  
  +---------+
  | comm    |
  +---------+
  |    NULL |
  |  300.00 |
  |  500.00 |
  |    NULL |
  | 1400.00 |
  |    NULL |
  |    NULL |
  |    NULL |
  |    NULL |
  |    0.00 |
  |    NULL |
  |    NULL |
  |    NULL |
  |    NULL |
  +---------+
  14 rows in set (0.00 sec)
  
  mysql> select sum(comm) from emp;  //可以看到分组处理函数计算时会忽略null，不像单行处理函数需要对null进行处理
  +-----------+
  | sum(comm) |
  +-----------+
  |   2200.00 |
  +-----------+
  1 row in set (0.00 sec)
  
  mysql> select count(comm) from emp; //可以看到count也忽略了null
  +-------------+
  | count(comm) |
  +-------------+
  |           4 |
  +-------------+
  1 row in set (0.00 sec)
  ```

+ 分组函数中`count(*)`和`count(具体字段)`区别：

  + `count(具体字段)`：统计该字段下所有不为null的元素的数量
  + `count(*)`：统计表中的总行数。（只要有一行记录，count就++，因为一行记录不可能都为null，只要一行中某个数据不为null，则该行记录有效）

  ```go
  mysql> select count(comm) from emp; //表示：统计该字段下所有不为null的元素的数量
  +-------------+
  | count(comm) |
  +-------------+
  |           4 |
  +-------------+
  1 row in set (0.00 sec)
  
  mysql> select count(*) from emp;
  +----------+
  | count(*) |
  +----------+
  |       14 |
  +----------+
  1 row in set (0.01 sec)
  ```

+ **分组函数不能直接使用在where子句中。**

  ```go
  //找出比最低工资高的员工信息
  mysql> select ename,sal from emp where sal>min(sal);
  ERROR 1111 (HY000): Invalid use of group function
  
  //报错原因：因为分组函数在使用时必须先分组才能使用，where执行时还没有分组，所以where后面不能出现分组函数。
  ```

+ 所有分组函数可以组合起来一起使用

  ```go
  mysql> select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp;
  //这种情况为什么分组函数能使用？
  //因为：select是在group by 之后才执行的，而且默认分为一组。
  
  +----------+----------+----------+-------------+----------+
  | sum(sal) | min(sal) | max(sal) | avg(sal)    | count(*) |
  +----------+----------+----------+-------------+----------+
  | 29025.00 |   800.00 |  5000.00 | 2073.214286 |       14 |
  +----------+----------+----------+-------------+----------+
  1 row in set (0.00 sec)
  ```

  



# 分组查询

## 定义：

在实际应用中，需要先进行分组，然后对每一组的数据进行操作。此时需要使用分组查询。

```go
格式：
	select ... from ... group by ...

示例：
1，计算每个部门的工资和         //每个部门即为分组
2，计算每个工作岗位的平均工资    //每个工作岗位
3，找出每个工作岗位的最高薪资。  
4....
```



## 执行顺序

```go
select 
	... 
from
	...
where
	... 
group by 
	...
having    //having子句对分组后的结果进行过滤的——不可以单独的出现，涉及的字段必须是select中出现的，或者是select中的别名
	... 
order by
	...


第一步：from   //选择表
第二步：where    //根据条件过滤数据
第三步：group by  //分组
第四步：having    //having必须跟group by联合使用，可以对分组后进一步过滤，having后面只能跟分组函数和某些字段
				//（某些字段为----可以出现在select后面的字段，一般为分组字段，或者分组函数起的别名字段）
第五步：select     //选出数据
第六步：order by  //对选出的数据排序
```

==**注意**：通过测试，实际上执行顺序：`from--> join on --> where --> group by --> select --> having --> distinct -->order by --> limit`==



## 示例

```go
//找出每个岗位的工资和
mysql> select job,sum(sal) from emp group by job;  //注意：select后面可以写job，是因为按照job分组了
+-----------+----------+
| job       | sum(sal) |
+-----------+----------+
| CLERK     |  4150.00 |
| SALESMAN  |  5600.00 |
| MANAGER   |  8275.00 |
| ANALYST   |  6000.00 |
| PRESIDENT |  5000.00 |
+-----------+----------+
5 rows in set (0.00 sec)

//执行顺序：先从emp表中查询数据，根据job字段进行分组，然后对每一组的数据进行sum(sal)

```

==**结论：在一条select语句当中，如果有group by 语句的话，select后面只能跟：参加分组的字段，以及分组函数。其他一律不能跟**。==



```go
//找出每个部门的最高工资
mysql> select deptno,max(sal) from emp group by deptno;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     20 |  3000.00 |
|     30 |  2850.00 |
|     10 |  5000.00 |
+--------+----------+
3 rows in set (0.00 sec)

//找出每个部门不同工作岗位的最高工资：
//两个字段联合起来分组，其实group by 后面可以跟多个字段
mysql> select deptno,job,max(sal) from emp group by deptno,job;  //这是将两个字段视为一个字段来进行分组
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     20 | CLERK     |  1100.00 |
|     30 | SALESMAN  |  1600.00 |
|     20 | MANAGER   |  2975.00 |
|     30 | MANAGER   |  2850.00 |
|     10 | MANAGER   |  2450.00 |
|     20 | ANALYST   |  3000.00 |
|     10 | PRESIDENT |  5000.00 |
|     30 | CLERK     |   950.00 |
|     10 | CLERK     |  1300.00 |
+--------+-----------+----------+
9 rows in set (0.00 sec)

mysql> select deptno,job,max(sal) from emp group by job,deptno order by deptno;  //可以最后按照部门编号排序
+--------+-----------+----------+
| deptno | job       | max(sal) |
+--------+-----------+----------+
|     10 | MANAGER   |  2450.00 |
|     10 | PRESIDENT |  5000.00 |
|     10 | CLERK     |  1300.00 |
|     20 | CLERK     |  1100.00 |
|     20 | MANAGER   |  2975.00 |
|     20 | ANALYST   |  3000.00 |
|     30 | SALESMAN  |  1600.00 |
|     30 | MANAGER   |  2850.00 |
|     30 | CLERK     |   950.00 |
+--------+-----------+----------+
9 rows in set (0.00 sec)

//要求找出每个部门最高薪资，要求显示最高薪资大于3000的
mysql> select deptno,max(sal) from emp where sal>3000 group by deptno;
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     10 |  5000.00 |
+--------+----------+
1 row in set (0.00 sec)

//先分组，分组后根据having条件对数据进行过滤，最后显示出来
mysql> select deptno,max(sal) from emp group by deptno having max(sal)>3000;  
+--------+----------+
| deptno | max(sal) |
+--------+----------+
|     10 |  5000.00 |
+--------+----------+
1 row in set (0.00 sec)
//第一种方式是先where（筛选）然后分组，其实只是进行了一次筛选操作
//第二种方式：是先分组，然后根据having条件对每一组进行筛选，进行了多次筛选操作，因此第二种方式效率更低
```

==注意：**where 和having，优先选择where，where完成不了的，再考虑having**==



```go
//如：找出每个部门的平均薪资，要求显示平均薪资高于2000的
//此时，where无法解决问题，考虑having

mysql> select deptno,avg(sal) from emp group by deptno having avg(sal)>2000;
+--------+-------------+
| deptno | avg(sal)    |
+--------+-------------+
|     20 | 2175.000000 |
|     10 | 2916.666667 |
+--------+-------------+
2 rows in set (0.00 sec)
```





## 总结

```go
select ...（字段）
from ...（表）
where ...（条件）
group by ...（字段）
having ...（条件）
order by ...（字段）

顺序：from --> where --> group by --> having --> select --> order by
理解：从某张表中查询数据，先经过where条件筛选出符合条件的数据，然后对这些数据进行分组，分组之后可以使用having继续筛选，最后排序输出

注意：在mysql中其实select语句中的别名可以在having后面使用，但是在oracle中select语句中的别名不能在having后面使用。


综合示例：
如：找出每个岗位的平均薪资，要求显示平均薪资大于1500的，除manager岗位之外，要求按照平均薪资降序排
mysql> select job,avg(sal) from emp where job <> 'manager' group by job having avg(sal)>1500 order by avg(sal) desc;                  //order by 是最后执行的，所以可以使用 avg(sal)
+-----------+-------------+
| job       | avg(sal)    |
+-----------+-------------+
| PRESIDENT | 5000.000000 |
| ANALYST   | 3000.000000 |
+-----------+-------------+
2 rows in set (0.00 sec)

mysql> select job,avg(sal)as avgsal from emp where job <> 'manager' group by job having avg(sal)>1500 order by avgsal desc;                   //select先于order by 执行，所以order by 后面可以跟select 字段后面的别名
+-----------+-------------+
| job       | avgsal      |
+-----------+-------------+
| PRESIDENT | 5000.000000 |
| ANALYST   | 3000.000000 |
+-----------+-------------+
2 rows in set (0.00 sec)

mysql> select job,avg(sal)as avgsal from emp where job <> 'manager' group by job having avg(sal)>1500 order by avg(sal) desc;             //可以看到，不跟也没关系，因为order by后面最终还是avg(sal) 
+-----------+-------------+
| job       | avgsal      |
+-----------+-------------+
| PRESIDENT | 5000.000000 |
| ANALYST   | 3000.000000 |
+-----------+-------------+
2 rows in set (0.00 sec)
```







# 去除重复记录

去重需要使用一个关键字：`distinct`

```go
mysql> select job from emp;
+-----------+
| job       |
+-----------+
| CLERK     |
| SALESMAN  |
| SALESMAN  |
| MANAGER   |
| SALESMAN  |
| MANAGER   |
| MANAGER   |
| ANALYST   |
| PRESIDENT |
| SALESMAN  |
| CLERK     |
| CLERK     |
| ANALYST   |
| CLERK     |
+-----------+
14 rows in set (0.00 sec)

mysql> select distinct job from emp;  //在字段前加上关键字distinct可以去重
+-----------+
| job       |
+-----------+
| CLERK     |
| SALESMAN  |
| MANAGER   |
| ANALYST   |
| PRESIDENT |
+-----------+
5 rows in set (0.00 sec)

mysql> select ename,distinct job from emp;  //这里报错
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'distinct job from emp' at line 1


//distinct只能出现字段最前方，表示连个字段联合起来去除重复记录
//将job和deptno看为一个整体，然后进行去重
mysql> select distinct job ,deptno from emp;  //即distinct只能用于第一个字段的前面
+-----------+--------+
| job       | deptno |
+-----------+--------+
| CLERK     |     20 |
| SALESMAN  |     30 |
| MANAGER   |     20 |  //是将job和deptno联合起来去除重复记录
| MANAGER   |     30 |
| MANAGER   |     10 |
| ANALYST   |     20 |
| PRESIDENT |     10 |
| CLERK     |     30 |
| CLERK     |     10 |
+-----------+--------+
9 rows in set (0.00 sec)


//例题：统计工作岗位的数量：
mysql> select count(distinct job) from emp;  //分组函数可以跟distinct一起使用
+---------------------+
| count(distinct job) |
+---------------------+
|                   5 |
+---------------------+
1 row in set (0.00 sec)
```





# 连接查询

## 连接查询定义：

从一张表中单独查询，称为单表查询。

连接查询：比如从emp表和dept表中联合起来查询数据，从emp表中取员工名字，从dept表中取部门名字，这种跨表查询，多张表联合起来查询数据，即				称为连接查询。

## 连接查询分类

+ 根据语法的年代查询：
  + SQL92：1992年出现的语法
  + SQL99：1999年的出现的语法

+ 根据表的连接方式分类：
  + 内连接
    + 等值连接
    + 非等值连接
    + 自连接
  + 外连接
    + 左外连接（左连接）
    + 右外连接（右连接）
  + 全连接



## 现象



### 笛卡尔积现象

当两张表进行查询时，没有任何条件的限制会发生什么现象？

```go
//查询每个员工的部门名称：
mysql> select ename,deptno from emp; //看到，在emp表中只有部门编号，没有部门名称
+--------+--------+
| ename  | deptno |
+--------+--------+
| SMITH  |     20 |
| ALLEN  |     30 |
| WARD   |     30 |
| JONES  |     20 |
| MARTIN |     30 |
| BLAKE  |     30 |
| CLARK  |     10 |
| SCOTT  |     20 |
| KING   |     10 |
| TURNER |     30 |
| ADAMS  |     20 |
| JAMES  |     30 |
| FORD   |     20 |
| MILLER |     10 |
+--------+--------+
14 rows in set (0.00 sec)


mysql> select * from dept;   //部门表
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)


//两张表连接没有条件限制：
mysql> select ename,dname from emp,dept;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | OPERATIONS |
| SMITH  | SALES      |
| SMITH  | RESEARCH   |
| SMITH  | ACCOUNTING |
| ALLEN  | OPERATIONS |
| ALLEN  | SALES      |
| ALLEN  | RESEARCH   |
| ALLEN  | ACCOUNTING |
...
+--------+------------+
56 rows in set (0.00 sec)  //14 * 4，即拿第一表中每个ename和第二张表中的dname都匹配一次

```

==现象：当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数是两张表条数的乘积，这种现象被称为：笛卡尔积现象。==





### 避免笛卡尔积现象

==如何避免：即在连接时添加条件，满足某个条件的记录被筛选出来。==

```go
//注意：在多张表连接时，在字段名出现冲突时，可以在字段名前添加相应的表名，即  表名.字段名
//不仅可以在where后面也可以在select后面，只要在多张表连接时字段名出现冲突时
//其实在单张表查询时，也可以使用 表名.字段名  但是这没有必要
mysql> select ename , dname from emp,dept where emp.deptno = dept.deptno;  //表名.字段名
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
//匹配此时并没有减少，还是匹配了14*4=56次，四选一
```

==注意：通过以上方式可以避免笛卡尔积现象，但是匹配次数并没有减少==

==注意：`select ename , dname from emp,dept;`，ename不仅会从emp表中查询，还会从dept表中查询，dname同样也会从两张表中查询，此时效率较低，可以通过在字段前添加表名来限制从某张表中查询`select emp.ename,dept.dname from emp,dept;`这样就限制了ename只能从emp表中查询，dname只能从dept表中查询。==

```go
mysql> select emp.ename,dept.dname from emp,dept where emp.deptno=dept.deptno;  //这样写效率高
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)


//可以看到，也可以给表起别名
//注意：不仅可以在select后面给字段起别名，同样可以在from 后面给表起别名
//from..where..select，因为是在from后面给表起别名，所以后面可以跟 别名.字段名
mysql> select e.ename,d.dname from emp as e,dept as d where e.deptno=d.deptno;  //SQL92语法
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
```

==注意：通过笛卡尔积现象得出：表的连接次数越多，匹配次数越多，效率越低，应尽量避免表的连接次数。==



## 内连接

**内连接：两张表之间没有主次之分**

### 等值连接

```go
//示例；查询每个员工所在部门，显示员工名和部门名

//SQL92语法：
mysql> select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
//sql92缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件都放在了where后面。


//SQL99语法：join表连接，on为连接条件
//SQL99的话，join..on .. where .. 作用：on在连接时进行条件筛选，where可以在连接完后再进一步筛选
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;  //连接条件和where后续筛选条件分离
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)

//这里inner可以省略，加上更好，可以直接看到是内连接
mysql> select e.ename,d.dname from emp e inner join dept d on e.deptno=d.deptno; 
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)


//sql99优点：表连接的条件是独立的，连接之后，如果还要进一步筛选，可以在后面添加where进一步筛选。
//sel99语法格式：select...from ...join ..on ..where...group by ... order by ..
```





### 非等值连接

```go
//找出每个员工的员工名、薪资、薪资等级
mysql> select e.ename,e.sal,s.grade from emp e inner join salgrade s on e.sal between s.losal and s.hisal;
//条件不是一个等量关系，称为非等值连接，inner可以省略
+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+
14 rows in set (0.00 sec)
```



### 自连接

**技巧：一张表看成两张表来进行    内连接—自连接**

```go
//查询员工的上级领导，要求显示员工名和对应的领导名
mysql> select * from emp;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+

//自己连接自己，即同一张表进行连接，称为：自连接
//内在顺序：select e1.ename  根据on条件,从emp e2 中找到对应的字段
mysql> select e1.ename as '员工名' ,e2.ename '领导名' from emp as e1 inner join emp e2 on e1.mgr = e2.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
13 rows in set (0.00 sec) //13条记录，没有King
```



## 外连接

==内连接和外连接区别：**在外连接中，两张表之间存在主次关系，而在内连接中，两张表没有主次关系**==



```go
//内连接：
mysql> select e.ename,d.dname from emp e inner join dept d on e.deptno=d.deptno; 
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)


//外连接:
//由于dept表是在emp表的右边，我们想将的dept表中达到匹配条件和没有达到匹配条件数据都显示出来，所以在join前添加right
mysql> select e.ename ,d.dname from emp e right join dept d on e.deptno = d.deptno; //右外连接
//right表示含义：将join关键字右边的表看成主表，将主表的数据全部查询出来，捎带根据on后面的条件关联左边的表
+--------+------------+
| ename  | dname      |
+--------+------------+
| MILLER | ACCOUNTING |
| KING   | ACCOUNTING |
| CLARK  | ACCOUNTING |
| FORD   | RESEARCH   |
| ADAMS  | RESEARCH   |
| SCOTT  | RESEARCH   |
| JONES  | RESEARCH   |
| SMITH  | RESEARCH   |
| JAMES  | SALES      |
| TURNER | SALES      |
| BLAKE  | SALES      |
| MARTIN | SALES      |
| WARD   | SALES      |
| ALLEN  | SALES      |
| NULL   | OPERATIONS |  //可以看到，这里没有满足on后面的匹配条件也显示出来了，称为外连接
+--------+------------+
15 rows in set (0.00 sec)

//也可以改成左外连接，只要将两张调换以下，可以达到相同的效果
mysql> select e.ename ,d.dname from dept d left join emp e on e.deptno = d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| MILLER | ACCOUNTING |
| KING   | ACCOUNTING |
| CLARK  | ACCOUNTING |
| FORD   | RESEARCH   |
| ADAMS  | RESEARCH   |
| SCOTT  | RESEARCH   |
| JONES  | RESEARCH   |
| SMITH  | RESEARCH   |
| JAMES  | SALES      |
| TURNER | SALES      |
| BLAKE  | SALES      |
| MARTIN | SALES      |
| WARD   | SALES      |
| ALLEN  | SALES      |
| NULL   | OPERATIONS |
+--------+------------+
15 rows in set (0.00 sec)

//外连接在join前加上一个outer 表示外连接，同样和内连接一样可以省略 ，带着可读性强
mysql> select e.ename ,d.dname from emp e right outer join dept d on e.deptno = d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| MILLER | ACCOUNTING |
| KING   | ACCOUNTING |
| CLARK  | ACCOUNTING |
| FORD   | RESEARCH   |
| ADAMS  | RESEARCH   |
| SCOTT  | RESEARCH   |
| JONES  | RESEARCH   |
| SMITH  | RESEARCH   |
| JAMES  | SALES      |
| TURNER | SALES      |
| BLAKE  | SALES      |
| MARTIN | SALES      |
| WARD   | SALES      |
| ALLEN  | SALES      |
| NULL   | OPERATIONS |
+--------+------------+
15 rows in set (0.00 sec)
```

==注意：带有right称为右外连接或右连接，带有left称为左外连接或左连接；外连接在join前加上一个outer 表示外连接，同样和内连接一样可以省略 ，带着可读性强==



==**注意：外连接的查询条数一定是 >= 内连接的查询条数**==





```go
//查询每个员工的上级领导，要求显示所有员工的名字和领导名
mysql> select e1.ename '员工名', e2.ename '领导名' from emp e1 left join emp e2 on e1.mgr = e2.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| KING   | NULL   |  //可以看到King也显示出来，因为king无法匹配条件，所以领导名直接为null
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
14 rows in set (0.00 sec)

//从上述可以看出：外连接是先将主表显示出来，然后考虑条件，如果符合条件则根据条件来匹配，如果不符合条件则直接显示为空null
```





## 全连接

**全连接：两张表都是主表**

全连接何少用，自行了解





## 多张表连接

```go
语法:
select ...
from a
join b
on a和b的连接条件
join c
on a和c的连接条件
join d
on a和d的连接条件

注意：一条SQL语句中可以将内连接和外连接结合使用



示例：找出每个员工的部门名称以及工资等级，要求显示员工名、部门名、薪资、薪资等级
mysql> select e.ename,d.dname,e.sal,s.grade from emp e join dept d on e.deptno=d.deptno join salgrade s on e.sal between s.losal and hisal;
+--------+------------+---------+-------+
| ename  | dname      | sal     | grade |
+--------+------------+---------+-------+
| SMITH  | RESEARCH   |  800.00 |     1 |
| ALLEN  | SALES      | 1600.00 |     3 |
| WARD   | SALES      | 1250.00 |     2 |
| JONES  | RESEARCH   | 2975.00 |     4 |
| MARTIN | SALES      | 1250.00 |     2 |
| BLAKE  | SALES      | 2850.00 |     4 |
| CLARK  | ACCOUNTING | 2450.00 |     4 |
| SCOTT  | RESEARCH   | 3000.00 |     4 |
| KING   | ACCOUNTING | 5000.00 |     5 |
| TURNER | SALES      | 1500.00 |     3 |
| ADAMS  | RESEARCH   | 1100.00 |     1 |
| JAMES  | SALES      |  950.00 |     1 |
| FORD   | RESEARCH   | 3000.00 |     4 |
| MILLER | ACCOUNTING | 1300.00 |     2 |
+--------+------------+---------+-------+
14 rows in set (0.00 sec)

//这里用外连接也可以得到同样的效果，因为是每个员工，我们可以将emp看作为主表，此时就出现主次，可以用外连接
mysql> select e.ename,d.dname,e.sal,s.grade from emp e left join dept d on e.deptno=d.deptno left join salgrade s on e.sal between s.losal and hisal;
+--------+------------+---------+-------+
| ename  | dname      | sal     | grade |
+--------+------------+---------+-------+
| SMITH  | RESEARCH   |  800.00 |     1 |
| ALLEN  | SALES      | 1600.00 |     3 |
| WARD   | SALES      | 1250.00 |     2 |
| JONES  | RESEARCH   | 2975.00 |     4 |
| MARTIN | SALES      | 1250.00 |     2 |
| BLAKE  | SALES      | 2850.00 |     4 |
| CLARK  | ACCOUNTING | 2450.00 |     4 |
| SCOTT  | RESEARCH   | 3000.00 |     4 |
| KING   | ACCOUNTING | 5000.00 |     5 |
| TURNER | SALES      | 1500.00 |     3 |
| ADAMS  | RESEARCH   | 1100.00 |     1 |
| JAMES  | SALES      |  950.00 |     1 |
| FORD   | RESEARCH   | 3000.00 |     4 |
| MILLER | ACCOUNTING | 1300.00 |     2 |
+--------+------------+---------+-------+
14 rows in set (0.00 sec)




示例：找出每个员工的部门名称以及工资等级和上级领导，要求显示员工名、领导名、部门名、薪资、薪资等级
//内连接和外连接可以混用
mysql> select e1.ename '员工名',e2.ename '领导名',d.dname,e1.sal,s.grade from emp e1 left join emp e2 on e1.mgr=e2.empno join dept d on e1.deptno = d.deptno join salgrade s on e1.sal between s.losal and s.hisal;
+--------+--------+------------+---------+-------+
| 员工名 | 领导名 | dname      | sal     | grade |
+--------+--------+------------+---------+-------+
| SMITH  | FORD   | RESEARCH   |  800.00 |     1 |
| ALLEN  | BLAKE  | SALES      | 1600.00 |     3 |
| WARD   | BLAKE  | SALES      | 1250.00 |     2 |
| JONES  | KING   | RESEARCH   | 2975.00 |     4 |
| MARTIN | BLAKE  | SALES      | 1250.00 |     2 |
| BLAKE  | KING   | SALES      | 2850.00 |     4 |
| CLARK  | KING   | ACCOUNTING | 2450.00 |     4 |
| SCOTT  | JONES  | RESEARCH   | 3000.00 |     4 |
| KING   | NULL   | ACCOUNTING | 5000.00 |     5 |
| TURNER | BLAKE  | SALES      | 1500.00 |     3 |
| ADAMS  | SCOTT  | RESEARCH   | 1100.00 |     1 |
| JAMES  | BLAKE  | SALES      |  950.00 |     1 |
| FORD   | JONES  | RESEARCH   | 3000.00 |     4 |
| MILLER | CLARK  | ACCOUNTING | 1300.00 |     2 |
+--------+--------+------------+---------+-------+
14 rows in set (0.00 sec)
```







# 子查询

## 定义

子查询：select语句中嵌套select语句，被嵌套的select语句称为子查询。

子查询出现的位置：

```go
select ...(select)...
from ... (select)...  //将select返回的临时表作为需要的表
where ... (select)... //将select返回的数据作为判断条件中的一个数据

//子查询可以出现在select、from、where后面
```

**注意：select子句必须用括号括起来，否则会报错**



## where子句中出现子查询

**==select子查询 用在where后面只能是一个值==**

```go
//示例：找出比最低工资高的员工姓名和工资
//子查询：(select min(sal) from emp)返回的是800


//select子查询 用在where后面只能是一个值

mysql> select ename,sal from emp where sal > (select min(sal) from emp); //避开了在没有分组情况下，使用分组函数
+--------+---------+
| ename  | sal     |
+--------+---------+
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| FORD   | 3000.00 |
| MILLER | 1300.00 |
+--------+---------+
13 rows in set (0.00 sec)
```





## from子句中的子查询

==注意：from后面的子查询，**可以将子查询的查询结果当做一张临时表，但是临时表必须要起别名，select后面的查询字段必须带上别名**。==

```go
//找出每个岗位的平均工资的薪资等级
//先(select job,avg(sal) as avgsal from emp group by job)将每个岗位的平均工资的表格返回，然后将该返回的表格和工资等级表格连接
mysql> select t.job,t.avgsal,s.grade from (select job,avg(sal) as avgsal from emp group by job) as t join salgrade as s on t.avgsal between s.losal and s.hisal;
+-----------+-------------+-------+
| job       | avgsal      | grade |
+-----------+-------------+-------+
| CLERK     | 1037.500000 |     1 |
| SALESMAN  | 1400.000000 |     2 |
| MANAGER   | 2758.333333 |     4 |
| ANALYST   | 3000.000000 |     4 |
| PRESIDENT | 5000.000000 |     5 |
+-----------+-------------+-------+
5 rows in set (0.00 sec)
```



## select后面出现的子查询（了解）

```go
//找出每个员工的部门名称，要求显示员工名、部门名
//（select d.dname from dept as d where d.deptno=e.deptno) 返回的是一列查询结果，这里select是返回一列，如果返回是多列结果其实就可以视为一张表格了，这是就出现问题了，所以select子查询只能返回一列结果。


//记住：可以将from理解为：from相当于import，将表格导入进来
mysql> select e.ename,(select d.dname from dept as d where d.deptno = e.deptno ) as dname from emp e;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)



//注意：可以看到select子查询只能返回一列，返回多列就会报错
mysql> select e.ename,(select d.dname ,d.loc from dept as d where d.deptno = e.deptno ) as dname from emp e;
ERROR 1241 (21000): Operand should contain 1 column(s)

//错误：子查询返回多行，也会报错
mysql> select e.ename ,(select dname from dept) from emp e;
ERROR 1242 (21000): Subquery returns more than 1 row

//总结：在select后面的子查询只能每次返回一行一列的数据
//其实，select语句是一条记录一条记录地查询。




//当然可以用内连接来查询
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;
+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| WARD   | SALES      |
| JONES  | RESEARCH   |
| MARTIN | SALES      |
| BLAKE  | SALES      |
| CLARK  | ACCOUNTING |
| SCOTT  | RESEARCH   |
| KING   | ACCOUNTING |
| TURNER | SALES      |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| FORD   | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
14 rows in set (0.00 sec)
```

==注意：**对于select后面的子查询来说：这个子查询只能一次返回1条结果，多于1条就报错**==



==**总结：在select后面的子查询只能每次返回一行一列的数据**
其实，select语句是一条记录一条记录地查询。==









# union

**union：**在mysql中，==**union用于将多个select语句的结果组合到一个结果集中，并删除结果集中的重复数据**。==

```go
//示例：查询工作岗位是manger和salesman的员工
mysql> select ename,job from emp where job = 'manager' or job = 'salesman';
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)

mysql> select ename,job from emp where job in ('manager','salesman');
+--------+----------+
| ename  | job      |
+--------+----------+
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| JONES  | MANAGER  |
| MARTIN | SALESMAN |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)

//也可以利用union将两个查询集合并到一张结果集中：可以看到，select语句的优先级高于union的，先执行两边再执行union
mysql> select ename,job from emp where job = 'manager' union select ename ,job from emp where job = 'salesman';
+--------+----------+
| ename  | job      |
+--------+----------+
| JONES  | MANAGER  |
| BLAKE  | MANAGER  |
| CLARK  | MANAGER  |  //这是两张表进行合并，所以必须保证两张列数相同，如图中，我们必须保证第二张表和第一张表都是两列才行
| ALLEN  | SALESMAN |
| WARD   | SALESMAN |
| MARTIN | SALESMAN |
| TURNER | SALESMAN |
+--------+----------+
7 rows in set (0.00 sec)

```



==注意：在表连接时，union的效率更高，对于表连接：每连接一次新表，则匹配的次数满足笛卡尔积现象。union可以减少匹配的次数，在减少匹配次数的情况下，还可以完成两个结果集的拼接==



==union使用时注意事项：==

+ **union在进行结果集合并时，要求两个结果集的==列数相同，在mysql中并不要求是相同的字段==**
+ union在合并时最好保证列的数据类型一致（在MySQL中不一致也可以，但是在oracle中不行，oracle语言更严格）
+ **union会在合并时==删除结果集中重复的数据==**







# limit

## 定义

**limit**：是将查询结果集的一部分取出来，通常使用在分页查询当中。

分页的作用：是为了提高用户的体验，因为一次全部都查出来，用户体验差。

## limit使用

limit用法：

```go
完整用法：
	limit startIndex , length   //stratIndex表示起始下标（针对表而言从0开始），length表示长度
省略用法：
	limit length  //表示取前几个数据
```



```go
//示例：
mysql> select ename,sal from emp order by sal desc limit 0,5;  //对于表来说，下标从0开始
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
5 rows in set (0.00 sec)

mysql> select ename,sal from emp order by sal desc limit 5;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
5 rows in set (0.00 sec)
```



**==注意：在mysql中limit是在order by之后执行！！！==**



```go
//示例：取出工资排名在【3-5】名的员工
mysql> select ename ,sal from emp order by sal desc limit 2,3;
+-------+---------+
| ename | sal     |
+-------+---------+
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
3 rows in set (0.00 sec)
```





## 分页

```go
比如：每页显示3条记录
第一页：limit 0,3; [0,1,2]
第二页：limit 3,3; [3,4,5]
第三页：limit 6,3; [6,7,8]

其实每页显示 pagesize 条记录：
第pageno页：limit (pageno -1)*pagesize , pagesize; 
```





## 完整DQL语句

```go
select ...
from ...(select 、join on )
where ...
group by ...
having ...
order by ...
limit ...
```





# 表



## 表的创建

DDL语句：包括create drop alter

建表语法：

```go
create table 表名(字段名1 数据类型,字段名2 数据类型,字段名3 数据类型);

表名：建议以 t_  或者 tbl_ 开始，可读性强。
字段名：见名知意
表名和字段名都是属于标识符
```

==注意：**数据库中的命名规范：所有的标识符全部都是小写，单词和单词之间使用下划线进行衔接**==



## 关于mysql中的数据类型

```go
//常见数据类型
varchar(最长255)  可变长度字符串，会根据实际数据长度动态分配空间 ，必须给出长度，否则报错啊！！！！！！！！！！！

//char可以不写建议长度，系统不会报错，但是varchar必须给出建议长度，否则系统报错
char(最长255)   定长字符串，不管实际数据长度是多少，分配固定长度的数据空间存储数据，使用不恰当的时候，可能会导致空间浪费，

char和varchar如何选择：比如性别是定长数据可以选择char，姓名是变长数据则可以选择varchar

int(最长11)  数字中的整型 ，可以不用给出具体长度，不会报错
bigint  数据中的长整型，默认20
float 单精度浮点型数据
double 双精度浮点型数据

date  短日期类型 
datetime  长日期类型
clob  字符大对象，最多可以存储4G的字符串，比如可以存储一篇文章，超过255个字符都需要采用clob
blob 二进制大对象，专门用来存储图片、声音、视频等流媒体数据，往blob类型的字段上插入数据时，例如插入图片、视频等需要使用IO流
```



```go
//学生表，包括 学号、姓名、性别、年龄、邮箱地址
create table t_student(no int,name varchar(32),sex char(1),age int(3), email varchar(255));  //在varchar、char和int后面添加数字为建议长度，超过该长度也不会报错，这只是一个建议长度

mysql> create table t_student(no int,name varchar(32),sex char(1),age int(3),email varchar(255));
Query OK, 0 rows affected, 1 warning (0.09 sec)

mysql> show tables;
+-----------------------+
| Tables_in_bjpowernode |
+-----------------------+
| dept                  |
| emp                   |
| salgrade              |
| t_student             |  //可以看到表已经建好了
+-----------------------+
4 rows in set (0.00 sec)

mysql> desc t_student;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| no    | int          | YES  |     | NULL    |       |
| name  | varchar(32)  | YES  |     | NULL    |       |
| sex   | char(1)      | YES  |     | NULL    |       |
| age   | int          | YES  |     | NULL    |       |
| email | varchar(255) | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```





## 表删除

语法格式：`drop table 表名;`

```go
mysql> drop table t_student;    //删除表，如果该表不存在时，会报错
Query OK, 0 rows affected (0.02 sec)

mysql> show tables;
+-----------------------+
| Tables_in_bjpowernode |
+-----------------------+
| dept                  |
| emp                   |
| salgrade              |  //可以看到刚才新建的t_student表已经被删除了
+-----------------------+
3 rows in set (0.00 sec)

//另一种写法：
mysql> drop table if exists t_student;  //如果表存在就删除，这种写法可以有效避免报错
Query OK, 0 rows affected, 1 warning (0.00 sec)
```





## 插入insert

**DML语句包括insert delete  update**

### 插入单条记录

```go
插入记录语法格式：
	insert into 表名(字段名1,字段名2,字段名3,...) values(值1,值2,值3);
注意：字段名和值要一一对应，即数量和数据类型要对应。

//示例：no,name,sex,age,email这个字段顺序没有必要和创建表的顺序一致，是通过字段名对应存储数据的
mysql> insert into t_student(no,name,sex,age,email)values(11,'zhangsan','男',23,'123@qq.com');
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_student;
+------+----------+------+------+------------+
| no   | name     | sex  | age  | email      |
+------+----------+------+------+------------+
|   11 | zhangsan | 男   |   23 | 123@qq.com |
+------+----------+------+------+------------+
1 row in set (0.00 sec)


mysql> insert into t_student(no) values(22);  //也可以存入一个字段的数据
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_student;
+------+----------+------+------+------------+
| no   | name     | sex  | age  | email      |
+------+----------+------+------+------------+
|   11 | zhangsan | 男   |   23 | 123@qq.com |
|   22 | NULL     | NULL | NULL | NULL       |
+------+----------+------+------+------------+
2 rows in set (0.00 sec)
```

==注意：insert语句是添加一条记录，没有给其他字段指定值的话，则默认值为null==



```go
//在创建表时可以通过default关键字来指定默认值。

mysql> create table t_student (no int,name varchar(32),sex char(1) default '男',age int(3),email varchar(255));
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> desc t_student;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| no    | int          | YES  |     | NULL    |       |
| name  | varchar(32)  | YES  |     | NULL    |       |
| sex   | char(1)      | YES  |     | 男      |       |
| age   | int          | YES  |     | NULL    |       |
| email | varchar(255) | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
  
                               
mysql> insert into t_student (no) values(11);   
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_student;
+------+------+------+------+-------+
| no   | name | sex  | age  | email |
+------+------+------+------+-------+
|   11 | NULL | 男   | NULL | NULL  |   
+------+------+------+------+-------+
1 row in set (0.00 sec)                              
```



==注意：**insert语句中的字段名是可以省略的，但是前面字段名省略相当于都存在，因此需要根据建表时字段名顺序将值全部写上**，否则报错==

```go
mysql> insert into t_student values(22);  //报错
ERROR 1136 (21S01): Column count doesn't match value count at row 1


mysql> insert into t_student values(22,'lisi','女',23,'lisi@qq.com');  //必须按照建表时字段顺序，将值全部写上
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_student;
+------+------+------+------+-------------+
| no   | name | sex  | age  | email       |
+------+------+------+------+-------------+
|   11 | NULL | 男   | NULL | NULL        |
|   22 | lisi | 女   |   23 | lisi@qq.com |
+------+------+------+------+-------------+
2 rows in set (0.00 sec)
```



### 插入日期

==注意：**日期可以用于排序，也可以用于比较**==

#### 数字格式化

格式化数字：`format(数字,'格式')   //格式为：$999,999`

```go
mysql> select ename,format(sal,'$999,999')from emp;
+--------+------------------------+
| ename  | format(sal,'$999,999') |
+--------+------------------------+
| SMITH  | 800                    |  //可以看到加入了千分位
| ALLEN  | 1,600                  |
| WARD   | 1,250                  |
| JONES  | 2,975                  |
| MARTIN | 1,250                  |
| BLAKE  | 2,850                  |
| CLARK  | 2,450                  |
| SCOTT  | 3,000                  |
| KING   | 5,000                  |
| TURNER | 1,500                  |
| ADAMS  | 1,100                  |
| JAMES  | 950                    |
| FORD   | 3,000                  |
| MILLER | 1,300                  |
+--------+------------------------+
14 rows in set, 14 warnings (0.00 sec)
```



#### str_to_date

`str_to_date`：将字符串varchar类型转换成date类型

```go
语法格式：
	str_to_date('字符串日期','日期格式') //日期格式必须和字符串格式一致，因为二者是对应解析的，否则会报错
其中，mysql中的日期格式：%Y 年  %m 月 %d 日 %h 时 %i %s 秒

mysql> insert into t_user(id,name,birth)values(1111,'wangwu',str_to_date('1997-03-01','%Y-%m-%d'));
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |  //str_to_date将varchar类型转换成date日期类型数据，通常使用在insert里
+------+--------+------------+
1 row in set (0.00 sec)

```



==注意：**如果字符串日期恰好是 `%Y-%m-%d` 这种形式，此时会自动将这种格式的字符串转换成日期类型**，此时就可以省略`str_to_date`函数。==

```go
mysql> insert into t_user(id,name,birth) values(22,'lisi','1998-01-02');  
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |
|   22 | lisi   | 1998-01-02 |
+------+--------+------------+
2 rows in set (0.00 sec)

mysql> insert into t_user(id,name,birth) values(22,'lisi','1998-1-2');  //这里省略也行
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
+------+--------+------------+
3 rows in set (0.00 sec)

mysql> insert into t_user(id,name,birth) values(22,'lisi','98-01-02');  //这里省略也行
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
+------+--------+------------+
4 rows in set (0.00 sec)

mysql> insert into t_user(id,name,birth) values(22,'lisi','08-1-2');  //这里省略也行
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 2008-01-02 |
+------+--------+------------+
5 rows in set (0.00 sec)

mysql> insert into t_user(id,name,birth) values(22,'lisi','98.1.02');  //这里-可以换成其他字符来分隔
Query OK, 1 row affected, 1 warning (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 2008-01-02 |
|   22 | lisi   | 1998-01-02 |
+------+--------+------------+
6 rows in set (0.00 sec)
//只要字符串日期形式 %Y-%m-%d，只要日期是年月日 这种形式就行，中间的分隔符其实没有关系
//但是如果日期不是年月日 这种形式，此时利用str_to_date()，格式必须和字符串日期一一对应，包括分隔符也要对应。
```



#### date_format

`date_format`：将date类型转换成具有一定格式的varchar字符串类型

```go
date_format(日期类型数据，需要转成的格式)  //格式可以自己定义，如 %d/%m/%Y  只要是日期格式即可
//该函数通常使用在查询日期方面，设置展示的日期格式


mysql> select id,name,date_format(birth,'%d/%m/%Y') from t_user;  //这里可以通过format定义日期格式
+------+--------+-------------------------------+
| id   | name   | date_format(birth,'%d/%m/%Y') |
+------+--------+-------------------------------+
| 1111 | wangwu | 01/03/1997                    |
|   22 | lisi   | 02/01/1998                    |
|   22 | lisi   | 02/01/1998                    |
|   22 | lisi   | 02/01/1998                    |
|   22 | lisi   | 02/01/2008                    |
|   22 | lisi   | 02/01/1998                    |
+------+--------+-------------------------------+
6 rows in set (0.00 sec)


//这种写法，是自动将表中日期数据转换成mysql默认的日期格式的字符串进行展示
mysql> select id,name,birth from t_user;
+------+--------+------------+
| id   | name   | birth      |
+------+--------+------------+
| 1111 | wangwu | 1997-03-01 |  //这里是自动将日期类型的数据，以mysql默认的日期格式形式，转换成字符串显示出来的
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 1998-01-02 |
|   22 | lisi   | 2008-01-02 |
|   22 | lisi   | 1998-01-02 |
+------+--------+------------+
6 rows in set (0.00 sec)
```



#### date和datetime两个类型的区别

```go
date  --> 短日期，只包含年月日信息
datetime --> 长日期，包括年月日时分秒信息

date  在mysql中的默认日期格式：%Y-%m-%d
datetime  在mysql中的默认日期格式：%Y-%m-%d %h:%i:%s


mysql> create table t_user(id int,name varchar(32),birth date,create_time datetime);
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t_user(id,name,birth,create_time)values(11,'zhangsan','1997-02-03','2022-12-23 19:01:23');  //按照mysql默认格式插入日期字符串，会自动转换成默认格式的日期
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|   11 | zhangsan | 1997-02-03 | 2022-12-23 19:01:23 |
+------+----------+------------+---------------------+
1 row in set (0.00 sec)
```



==在mysql存在一个**函数`now()`来获取系统当前时间**，并且获取的**时间带有时分秒信息**，是datetime日期类型==

```go
//通过now()来自动获取系统当前时间信息，包含时分秒
mysql> insert into t_user(id,name,birth,create_time)values(22,'lisi','1997-03-03',now());  
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|   11 | zhangsan | 1997-02-03 | 2022-12-23 19:01:23 |
|   22 | lisi     | 1997-03-03 | 2022-12-23 19:06:06 |
+------+----------+------------+---------------------+
2 rows in set (0.00 sec)

//通过使用curdate()来自动获取系统当前日期，不包含时分秒
mysql> select curdate();
+------------+
| curdate()  |
+------------+
| 2022-12-27 |
+------------+
1 row in set (0.00 sec)

//通过now()来自动获取系统当前时间信息，包含时分秒
mysql> select now();
+---------------------+
| now()               |
+---------------------+
| 2022-12-27 17:29:32 |
+---------------------+
1 row in set (0.00 sec)



//求时间差函数 timestampdiff(unit,begin,end);
//其中，begin和end可以为date或datetime，如果其中一个为date，另一个为datetime，会自动将date转换为datetime进行运算
//unit为end-begin的返回值单位，单位有 year（年），quarter（季），month（月），week（周），day（日），hour（时），minute（分），second（秒），microseconde（毫秒）

mysql> select timestampdiff(year,'2018-12-01',now());
+----------------------------------------+
| timestampdiff(year,'2018-12-01',now()) |
+----------------------------------------+
|                                      4 |
+----------------------------------------+
1 row in set (0.00 sec)

mysql> select timestampdiff(quarter,'2018-12-01',now());
+-------------------------------------------+
| timestampdiff(quarter,'2018-12-01',now()) |
+-------------------------------------------+
|                                        16 |
+-------------------------------------------+
1 row in set (0.00 sec)
```





### 插入多条记录

```go
insert 插入多条记录语法格式：
insert into 表名(字段名1,字段名2,字段名3)values(值1,值2,值3),(值1,值2,值3),...,(值1,值2,值3);

mysql> insert into t_user (id,name,birth,create_time)values(11,'zhangsan','1992-03-05',now()),(22,'lisi','1998-08-23',now());  //这里一下子插入两条记录
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from t_user;
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|   11 | zhangsan | 1992-03-05 | 2022-12-23 22:08:30 |
|   22 | lisi     | 1998-08-23 | 2022-12-23 22:08:30 |
+------+----------+------------+---------------------+
2 rows in set (0.00 sec)
```



### 快速创建表

#### 表复制

```go
//将一张表的查询结果当作一张表新建！！！！
//该方法可以实现表的复制！！！！

mysql> create table emp2 as select * from emp;   //as既可以用来起别名，还可以查询结果作为一张表来新建
Query OK, 14 rows affected, 2 warnings (0.02 sec)
Records: 14  Duplicates: 0  Warnings: 2

mysql> select * from emp2;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)
```



#### 将部分字段查询结果作为表的新建

```go
mysql> create table emp3 as select ename,sal from emp;  //将某些字段的查询结果作为一张表，新建出来
Query OK, 14 rows affected, 1 warning (0.01 sec)
Records: 14  Duplicates: 0  Warnings: 1

mysql> select * from emp3;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SMITH  |  800.00 |
| ALLEN  | 1600.00 |
| WARD   | 1250.00 |
| JONES  | 2975.00 |
| MARTIN | 1250.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| SCOTT  | 3000.00 |
| KING   | 5000.00 |
| TURNER | 1500.00 |
| ADAMS  | 1100.00 |
| JAMES  |  950.00 |
| FORD   | 3000.00 |
| MILLER | 1300.00 |
+--------+---------+
14 rows in set (0.00 sec)
```



#### 将查询结果插入到一张表中

```go
mysql> select * from dept_bak;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
4 rows in set (0.00 sec)

//插入必须两站表的列数相同

mysql> insert into dept_bak select * from dept;  //将查询结果插入一张表中，当然要保证列数相同才能插入，此时没有用到as
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from dept_bak;
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
8 rows in set (0.00 sec)
```





## update 更新

**==注意：没有条件限制会导致所有数据都更新，将会产生灾难性后果。==**

```go
update 语法格式：
	update 表名 set 字段名1=值1,字段名2=值2,字段名3=值3,... where 条件;


mysql> select * from t_user;  //原始内容
+------+----------+------------+---------------------+
| id   | name     | birth      | create_time         |
+------+----------+------------+---------------------+
|   11 | zhangsan | 1997-02-03 | 2022-12-23 19:01:23 |
|   22 | lisi     | 1997-03-03 | 2022-12-23 19:06:06 |
+------+----------+------------+---------------------+
2 rows in set (0.00 sec)

mysql> update t_user set name='wangwu',id = 33;  //没有条件情况下进行更新
Query OK, 2 rows affected (0.00 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> select * from t_user;  //没有条件更新后的结果，可以看到所有记录都修改了
+------+--------+------------+---------------------+
| id   | name   | birth      | create_time         |
+------+--------+------------+---------------------+
|   33 | wangwu | 1997-02-03 | 2022-12-23 19:01:23 |
|   33 | wangwu | 1997-03-03 | 2022-12-23 19:06:06 |
+------+--------+------------+---------------------+
2 rows in set (0.00 sec)


mysql> update t_user set id =44,name='zhaoliu' where birth = '1997-02-03';  //根据日期条件，进行修改
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0  //可以看到，匹配到一个，修改了一个

mysql> select * from t_user;  //修改后的结果
+------+---------+------------+---------------------+
| id   | name    | birth      | create_time         |
+------+---------+------------+---------------------+
|   44 | zhaoliu | 1997-02-03 | 2022-12-23 19:01:23 |
|   33 | wangwu  | 1997-03-03 | 2022-12-23 19:06:06 |
+------+---------+------------+---------------------+
2 rows in set (0.00 sec)
```









## delete 删除

 ==**根据条件来删除数据，没有条件将会导致将整张表的数据删除，会产生灾难性后果**==

```go
delete语法格式：
	delete from 表名 where 条件;  //根据条件删除某些记录

注意：如果没有条件，整张表的数据会全部删除


mysql> select * from t_user;
+------+---------+------------+---------------------+
| id   | name    | birth      | create_time         |
+------+---------+------------+---------------------+
|   44 | zhaoliu | 1997-02-03 | 2022-12-23 19:01:23 |
|   33 | wangwu  | 1997-03-03 | 2022-12-23 19:06:06 |
+------+---------+------------+---------------------+
2 rows in set (0.00 sec)

mysql> delete from t_user where id =44;  //根据id为44的这个条件来删除表中的记录
Query OK, 1 row affected (0.00 sec)  //一行被删除了

mysql> select * from t_user;  //删除后的结果
+------+--------+------------+---------------------+
| id   | name   | birth      | create_time         |
+------+--------+------------+---------------------+
|   33 | wangwu | 1997-03-03 | 2022-12-23 19:06:06 |
+------+--------+------------+---------------------+
1 row in set (0.00 sec)




mysql> insert into t_user values(11,'lisi','1993-09-02','2000-09-08 12:23:32');
Query OK, 1 row affected (0.00 sec)

mysql> select * from t_user;
+------+--------+------------+---------------------+
| id   | name   | birth      | create_time         |
+------+--------+------------+---------------------+
|   33 | wangwu | 1997-03-03 | 2022-12-23 19:06:06 |
|   11 | lisi   | 1993-09-02 | 2000-09-08 12:23:32 |
+------+--------+------------+---------------------+
2 rows in set (0.00 sec)

mysql> delete from t_user;  //将所有记录全部删除
Query OK, 2 rows affected (0.00 sec)

mysql> select * from t_user; //可以看到表中为空
Empty set (0.00 sec)
```



## 快速删除表中的数据

```go
delete from 表名;  可以删除表中的全部数据，但是效率较低   //属于DML操作

delete 删除数据的原理：
	它会将表中的数据一个一个删除，但是这个数据所占的真实空间不会释放。
	缺点：删除效率低
	优点：支持回滚，即删除后可以恢复数据
```



```go
用法：truncate table 表名;  //属于DDL操作  和delete用法区分开

truncate语句删除数据原理：
	它删除效率较高，表被一次截断，物理删除。
	缺点：不支持回滚
	优点：快速

//表非常大，上亿条记录时，使用delete删除可能需要好几个小时，可以选择使用truncate删除表中的数据，只需要不到1秒中就删除数据，效率高，但是truncate 删除是不可恢复的，所以要仔细询问客户是否真的要删除。


mysql> truncate table dept_bak;  //尽管truncate是ddl语句，但是这是删除表中的数据，表还在
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-----------------------+
| Tables_in_bjpowernode |
+-----------------------+
| dept                  |
| dept_bak              |  //表还在
| emp                   |
| emp2                  |
| emp3                  |
| salgrade              |
| t_student             |
| t_user                |
+-----------------------+
8 rows in set (0.00 sec)
```

**==注意：delete可以删除表中的所有数据，也可以根据where条件删除表中的某些记录，但是truncate只能删除表中所有数据==**





## 删除表

```go
删除表 drop table 表名; 
	******这是直接删除表，而delete和truncate是删除表中的数据，表还在。*****

mysql> drop table dept_bak;  //直接将表删除了
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-----------------------+
| Tables_in_bjpowernode |
+-----------------------+
| dept                  |
| emp                   |
| emp2                  |
| emp3                  |
| salgrade              |
| t_student             |
| t_user                |
+-----------------------+
7 rows in set (0.00 sec)

```

==注意：drop不仅可以删除表，还可以删除一个数据库==

```go
//创建数据库：create datebase 数据库名;
//删除数据库: drop datebase 数据库名;

//创建表： create table 表名(字段名1 数据类型,字段名2 数据类型,字段名3 数据类型);
//删除表中数据：delete from 表名;    truncate table 表名;
//删除表：drop table 表名;
```



## 对表结构增删改

表结构修改：就是 添加一个字段，删除一个字段，修改一个字段！！！

对表结构修改需要使用`alter`

```go
ALTER TABLE <表名> [修改选项]

新增表字段
ALTER TABLE <表名> ADD COLUMN column <类型>
修改字段名（重命名）
ALTER TABLE <表名> CHANGE COLUMNoldcolumn column <新列类型>
相当于modify
ALTER TABLE <表名> ALTER COLUMN column { SET DEFAULT <默认值> | DROP DEFAULT }
修改字段（类型和约束）
ALTER TABLE <表名> MODIFY COLUMN column <类型>
删除字段
ALTER TABLE <表名> DROP COLUMN column
重命名表名
ALTER TABLE <表名> RENAME TO <新表名>
```

在实际开发中，一旦需求确定后，表一旦设计后之后，很少会对表结构进行修改。因为在开发中修改表的结构成本较高，对应的代码需要大量修改，这个责任应该由设计人员来承担。如果真的需要修改表的结构，可以使用工具来修改。



## 约束

#### 定义

约束（constraint）：可以在创建表的时候，可以给表中的字段加上一些约束，来保证这个表中数据的完整性、有效性。

作用：保证表中的数据有效！！！

#### 常见的约束

+ 非空约束：not null，只有列级约束，没有表级约束

  ```go
  非空约束 not null 约束的字段不能为null
  
  create table t_vip(id int,name varchar(255) not null);  //直接在字段后面添加 not null
  
  mysql> insert into t_vip(id,name)values(11,'lisi');  //正常插入
  Query OK, 1 row affected (0.00 sec)
  
  mysql> insert into t_vip(id)values(22);  //插入报错，必须给非空约束的字段插入数据，否则报错。
  ERROR 1364 (HY000): Field 'name' doesn't have a default value
  
  ```

  

+ 唯一性约束：unique，有列级约束和表级约束

  ```go
  唯一性约束 unique，约束的字段不能重复，但是可以为null
  
  mysql> create table t_vip(id int,name varchar(32) unique);
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> insert into t_vip(id,name)values(11,'zhangsan'); 
  Query OK, 1 row affected (0.00 sec)
  
  //约束字段为unique时，该字段不能出现重复值，否则会报错
  mysql> insert into t_vip(id,name)values(22,'zhangsan'); 
  ERROR 1062 (23000): Duplicate entry 'zhangsan' for key 't_vip.name'
  
  mysql> insert into t_vip(id)values(22);  //看到可以为null ，列级约束
  Query OK, 1 row affected (0.00 sec)
  
  mysql> select * from t_vip;
  +------+----------+
  | id   | name     |
  +------+----------+
  |   11 | zhangsan |
  |   22 | NULL     |
  +------+----------+
  2 rows in set (0.00 sec)
  ```

  ```go
  //如果name和email两个字段联合起来具有唯一性?
  
  //使用unique(字段1,字段2,...) 表示多个字段联合起来唯一，形式如下
  create table t_vip(id int,name varchar(255),email varchar(255),unique(name,email));//注意：unique前的逗号
  //注意：约束添加在列后面的称为列约束，而没有添加在列后的，这种约束称为表级约束！！
  //当我们需要对多个字段联合起来，添加某一个约束时，可以使用表级约束。
  
  mysql> create table t_vip(id int,name varchar(255),email varchar(255),unique(name,email));
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> insert into t_vip (id,name,email)values(1,'zhangsan','zhangsan@123.com'); 
  Query OK, 1 row affected (0.00 sec)
  
  mysql> insert into t_vip (id,name,email)values(1,'zhangsan','zhangsan@sina.com');
  Query OK, 1 row affected (0.00 sec)
  
  mysql> select * from t_vip;
  +------+----------+-------------------+
  | id   | name     | email             |
  +------+----------+-------------------+
  |    1 | zhangsan | zhangsan@123.com  | //可以看到两个数据都插入进来了，因为是name和email联合唯一，而不是各自唯一
  |    1 | zhangsan | zhangsan@sina.com |
  +------+----------+-------------------+
  2 rows in set (0.00 sec)
  
  mysql> insert into t_vip (id,name,email)values(2,'zhangsan','zhangsan@sina.com');
  ERROR 1062 (23000): Duplicate entry 'zhangsan-zhangsan@sina.com' for key 't_vip.name'
  ```

  ```go
  //unique和not null 可以联合使用
  mysql> create table t_vip(id int,name varchar(255) not null unique); //not null 和 unique 同时使用
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> desc t_vip;
  +-------+--------------+------+-----+---------+-------+
  | Field | Type         | Null | Key | Default | Extra |
  +-------+--------------+------+-----+---------+-------+
  | id    | int          | YES  |     | NULL    |       |
  | name  | varchar(255) | NO   | PRI | NULL    |       |  //PRI表示主键
  +-------+--------------+------+-----+---------+-------+
  2 rows in set (0.00 sec)
  
  //在mysql当中，如果一个字段同时被not null和unique约束的话，该字段会自动变为主键字段。（在oracle中是不一样的）
  ```

+ 主键约束：primary key  简称PK，可以使用表级约束和列级约束，其中表级约束主要给多个字段联合起来添加约束

  + 主键约束：是一种约束

  + 主键字段：该字段上添加了主键约束，这样的字段称为主键字段

  + 主键值：主键字段中的每一个值都称为主键值，主键值是每一行记录的==唯一标识==，如身份证号一样。 

  + ==**任何一张表都应该有主键，没有主键表无效**==

  + ==主键特征：not null + unique  即主键值不能为null，同时也不能重复。==

    ```go
    
    mysql> create table t_vip(id int primary key,name varchar(32)); //一个字段上添加主键约束，称为单一主键
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into t_vip(id,name)values(11,'zhangsan'),(2,'lisi');
    Query OK, 2 rows affected (0.00 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    mysql> select * from t_vip;
    +----+----------+
    | id | name     |
    +----+----------+
    |  2 | lisi     |
    | 11 | zhangsan |
    +----+----------+
    2 rows in set (0.00 sec)
    
    mysql> insert into t_vip(id,name)values(11,'lisi'); //主键不能重复
    ERROR 1062 (23000): Duplicate entry '11' for key 't_vip.PRIMARY'
    
    mysql> insert into t_vip(name)values('wangwu'); //主键不能为空
    ERROR 1364 (HY000): Field 'id' doesn't have a default value
    
    
    
    //多个字段联合起来作为主键，称为复合主键，在实际开发中不建议使用复合主键，使用单一主键即可
    mysql> create table t_vip(id int,name varchar(32),primary key(id,name));  
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into t_vip (id,name) values(11,'zhangsan');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into t_vip(id,name)values(22,'zhangsan');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert  into t_vip(id,name)values(11,'lisi');  
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into t_vip(id,name)values(11,'zhangsan');  //报错
    ERROR 1062 (23000): Duplicate entry '11-zhangsan' for key 't_vip.PRIMARY'
    
    mysql> select * from t_vip;
    +----+----------+
    | id | name     |
    +----+----------+
    | 11 | lisi     |
    | 11 | zhangsan |
    | 22 | zhangsan |
    +----+----------+
    3 rows in set (0.00 sec)
    ```

  + **一张表中主键约束只能有一个**。

    ```go
    //可以看到不能同时给多个字段添加主键约束，上面的联合约束跟这种形式是不一样的，联合约束是将多个字段视为一个字段添加约束
    mysql> create table t_vip(id int primary key,name varchar(32) primary key);
    ERROR 1068 (42000): Multiple primary key defined
    ```

  + **主键值建议使用：int bigint char 类型，不建议使用： varchar类型。 主键值一般为定长的**

  + 主键除了可以用单一主键和复合主键分类外，还可以分为：自然主键和业务主键

    + 自然主键：主键值是一个自然数，和业务没有关系

    + 业务主键：主键值和业务紧密相关，例如将银行卡号作为主键值

    + 结论：在实际开发中，自然主键使用更多

    + **在mysql中，可以使用`auto_increment`自动维护一个主键值**

      ```go
      //auto_increment 自增，默认值从1开始，每次+1
      //可以通过 alter table 表名 auto_increment = 2  来修改auto_increment的默认值。
      
      mysql> create table t_vip(id int primary key auto_increment,name varchar(32));
      Query OK, 0 rows affected (0.01 sec)
      
      mysql> insert into t_vip(name)values('zhangsan'),('lisi'),('wangwu');
      Query OK, 3 rows affected (0.00 sec)
      Records: 3  Duplicates: 0  Warnings: 0
      
      mysql> select * from t_vip;
      +----+----------+
      | id | name     |
      +----+----------+
      |  1 | zhangsan |  //自动维护一个主键值
      |  2 | lisi     |
      |  3 | wangwu   |
      +----+----------+
      3 rows in set (0.00 sec)
      ```
      
      

+ 外键约束：foreign key   简称FK

  + 外键约束：一种约束foreign key

  + 外键字段：字段上添加了外键约束

  + 外键值：外键字段当中的每一个值

    ```go
    //外键约束示例：
    
    mysql> create table t_class(classno int primary key,classname varchar(255));  //新建一张班级表
    Query OK, 0 rows affected (0.01 sec)
    
    mysql> insert into t_class(classno,classname)values(101,'黄梅县第五中学1班'),(102,'黄梅县第五中学2班');
    Query OK, 2 rows affected (0.00 sec)  //向班级表中插入两条记录
    Records: 2  Duplicates: 0  Warnings: 0
    
    mysql> select * from t_class;  //班级表中内容
    +---------+-------------------+
    | classno | classname         |  //classno被学生表引用，不要求classno是主键，但至少是unique约束。
    +---------+-------------------+
    |     101 | 黄梅县第五中学1班 |  //即作为外键被引用，至少要保证具有唯一性
    |     102 | 黄梅县第五中学2班 |
    +---------+-------------------+
    2 rows in set (0.00 sec)
    
     //新建一种学生表，并将该表中的cno字段添加外键约束，使其引用班级表中的classno
    mysql> create table t_student (id int primary key auto_increment,name varchar(255),cno int ,foreign key (cno) references t_class(classno)); 
    Query OK, 0 rows affected (0.01 sec)
    
    //由于给学生表中的cno字段添加了外键约束，因此，在添加数据时必须保证cno字段的值是属于班级表中的classno值
    mysql> insert into t_student (name,cno)values('zhangsan',101),('lisi',102);
    Query OK, 2 rows affected (0.00 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    //由于班级表中的103不存在，因此，向学生表中插入103时会报错，这就是外键约束
    mysql> insert into t_student (name,cno)values('wangwu',103);
    ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`bjpowernode`.`t_student`, CONSTRAINT `t_student_ibfk_1` FOREIGN KEY (`cno`) REFERENCES `t_class` (`classno`))
    
    mysql> select * from t_student;
    +----+----------+------+
    | id | name     | cno  |
    +----+----------+------+
    |  1 | zhangsan |  101 |
    |  2 | lisi     |  102 |
    +----+----------+------+
    2 rows in set (0.00 sec)
    
    mysql> insert into t_student(name) values('zhaoliu');  //可以看到外键值可以为空
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from t_student;
    +----+----------+------+
    | id | name     | cno  |
    +----+----------+------+
    |  1 | zhangsan |  101 |
    |  2 | lisi     |  102 |
    |  4 | zhaoliu  | NULL |  //可以看到外键值可以为空
    +----+----------+------+
    3 rows in set (0.00 sec)
    ```

  + ==对于外键约束需要注意==的：

    + 删除表的顺序：先删除子表，再删除父表
    + 创建表的顺序：先创建父表，再创建子表
    + 删除数据的顺序：先删除子数据，再删除父数据
    + 插入数据的顺序：先插入父数据，再插入子数据

  + **==子表中的外键引用的父表中的某个字段，该被引用的字段不一定是主键，但至少具有unique约束==**

  + ==**外键值可以为空**==

+ 检查约束：check （MySQL不支持，oracle支持）



# 存储引擎

## 定义

存储引擎：是mysql中特有的一个术语，其他数据库中没有。（oracle中有，但是不叫这个名字）

​				**实际上存储引擎是一个表存储/组织数据的方式**，不同的存储引擎，表的存储数据方式不同。



## 如何给表指定存储引擎

==可以在建表的时候，给表增加存储引擎==

```go
show create table t_student;  //表示：展现当时创建t_student这张表时的sql语句

mysql> show create table t_student;
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table     | Create Table                                                                                                                                                                                                                                                                                                                                     |
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| t_student | CREATE TABLE `t_student` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `cno` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `cno` (`cno`),
  CONSTRAINT `t_student_ibfk_1` FOREIGN KEY (`cno`) REFERENCES `t_class` (`classno`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

//ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci 
//这里默认存储引擎为 InnoDB , 默认的编码方式 utf8mb4
```



==我们可以在创建表的时候，在最后小括号后面，用engine来指定存储引擎，用charset来指定这张表的编码方式==

```go
//示例：
create table t_product(id int primary key,name varchar(255))engine=InnoDB default charset=gbk;
//通过engine=存储引擎 和 charset=编码方式 来创建表
```





## mysql支持哪些存储引擎

```go
mysql> show engines \G  //查看mysql支持哪些存储引擎
*************************** 1. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 2. row ***************************
      Engine: MRG_MYISAM
     Support: YES
     Comment: Collection of identical MyISAM tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 3. row ***************************
      Engine: CSV
     Support: YES
     Comment: CSV storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 4. row ***************************
      Engine: FEDERATED
     Support: NO
     Comment: Federated MySQL storage engine
Transactions: NULL
          XA: NULL
  Savepoints: NULL
*************************** 5. row ***************************
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 6. row ***************************
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 7. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES
          XA: YES
  Savepoints: YES
*************************** 8. row ***************************
      Engine: ndbinfo
     Support: NO
     Comment: MySQL Cluster system information storage engine
Transactions: NULL
          XA: NULL
  Savepoints: NULL
*************************** 9. row ***************************
      Engine: BLACKHOLE
     Support: YES
     Comment: /dev/null storage engine (anything you write to it disappears)
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 10. row ***************************
      Engine: ARCHIVE
     Support: YES
     Comment: Archive storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 11. row ***************************
      Engine: ndbcluster
     Support: NO
     Comment: Clustered, fault-tolerant tables
Transactions: NULL
          XA: NULL
  Savepoints: NULL
11 rows in set (0.00 sec)
```



## mysql常用的存储引擎

+ MyISAM存储引擎：使用三个文件表示一张表，可被转换为压缩、只读表来节省空间，不支持事务，安全性级别低
+ MEMORY存储引擎：数据存储在内存当中，查询效率高，不能存储图片、声音、视频。不安全，关机后数据消失。
+ InnoDB存储引擎：mysql默认存储引擎，支持事务，支持数据库崩溃后自动恢复机制，最要的特点是非常安全，效率较低。





# 事务

## 定义

事务(transaction)：一个事务就是一个完整的业务逻辑，是一个最小的工作单元，不可再分。

事务本质：一个事务其实就是多条DML语句同时成功，或者同时失败。

```go
//例如：转账业务，从A账户向B账户中转账1000
包含两步：将A账户的钱减去1000，将B账户的钱加上1000，这就是一个完整的业务逻辑。

上面的操作就是一个最小的工作单元，要么同时成功，要么同时失败，不可再分。


正是因为处理某件事时，需要多条DML语句联合起来才能完成，所以需要事务的存在。如果任何一件事只需要一条DML语句就能完成，那么事务就没有存在的价值。
```



==注意：**只有DML语句才有事务**，即insert 、update、delete 跟事务有关，因为只有以上三个操作是对数据库表中的增、删、改。只要操作涉及到数据的增删改，首先保证数据安全问题。==



## 事务提交和回滚



***事务是如何做到多条DML语句同时成功和同时失败的？***

+ 由于InnoDB存储引擎：提供了一组用来记录事务性活动的日志文件。

+ 事务开启到事务结束，一系列的（insert、delete、upadate）DML语句的操作都会记录到“事务性活动的日志文件”中。

+ 在事务的执行过程中，我们可以提交事务，也可以回滚事务。

  + 提交事务：**清空事务性活动的日志文件，将数据全部彻底持久化到数据库中**。提交事务标志事务的结束，并且是全部成功的结束。

    ```go
    提交事务语句：commit
    
    先执行：start transaction;    //执行该语句后，不会自动提交事务了，需要手动提交事务
    再执行：commit;
    
    
    
    mysql> delete from dept_bak;
    Query OK, 1 row affected (0.00 sec)
    
    mysql> select * from dept_bak;  //表为空
    Empty set (0.00 sec)
    
    mysql> start transaction;   //开启事务
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(20,'manager','huangmei'); //插入数据
    Query OK, 1 row affected (0.00 sec)
    
    mysql> commit ;  //提交事务，如果没有提交，数据并没有真正保存在数据库中。
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;  //可以看到表中存在数据
    +--------+---------+----------+
    | DEPTNO | DNAME   | LOC      |
    +--------+---------+----------+
    |     20 | manager | huangmei |
    +--------+---------+----------+
    1 row in set (0.00 sec)
     
    mysql> rollback;   //回滚事务
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak; //可以看到表中数据仍存在，因为只能回滚到上次提交点。
    +--------+---------+----------+
    | DEPTNO | DNAME   | LOC      |
    +--------+---------+----------+
    |     20 | manager | huangmei |
    +--------+---------+----------+
    1 row in set (0.00 sec)
    ```

    

  + 回滚事务：**将之前所有的DML操作全部撤销，并且清空事务性活动的日志文件**。回滚事务标志事务的结束，并且全部是失败的结束。					==**回滚事务只能回滚到上一次提交事务的提交点**。==

    ```go
    回滚事务：rollback;
    
    mysql> select * from dept_bak;  //表为空
    Empty set (0.00 sec)
    
    mysql> start transaction;  //开启事务
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(10,'sales','beijing');  //插入数据
    Query OK, 1 row affected (0.00 sec)
    
    mysql> rollback;  //回滚上一次提交点
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;  //可以看到，回滚后，表为空
    Empty set (0.00 sec)
    ```

+ ==**在mysql中，是默认提交事务的，即每执行一条DML语句，则提交一次事务**。==



==注意：**如果开启事务（即关闭了默认的每一条DML语句就会自动commit事务）**，在这之后，每执行一条DML语句或多条DML语句时需要手动提交，即在执行一条或多条DML语句后，需要添加一条commit命令==



## 事务特性

+ A：原子性-->说明事务是最小的工作单元，不可再分。
+ C：一致性-->在一个事务当中，所有的操作必须同时成功，或者同时失败，以保证数据的一致性。
+ I：隔离性--> 一个事务和另一个事务之间具有一定的隔离。
+ D：持久性--> 事务最终结束的一个保障。事务提交，相当于将没有保存到硬盘上的数据保存到硬盘上。





## 事务隔离级别

重点研究一下事务的隔离性。

比如：A教室和B教室中间有一道墙，这道墙被称为隔离。这道墙可以很厚，也可以很薄。薄厚程度被称为隔离级别。

### 事务和事务之间的隔离级别

```go
查看隔离级别：select @@transaction_isolation;

mysql> select @@transaction_isolation;  //查看当前默认的事务隔离级别
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)

设置事务隔离级别：set [global 全局范围 /session 会话范围 ] transaction_isolation = ‘隔离级别’

mysql> set global transaction_isolation = 'read-uncommitted';  //这里用 - 连接
Query OK, 0 rows affected (0.00 sec)

//重新进入mysql，就可以发现事务隔离级别修改为：read-uncommitted
mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-UNCOMMITTED        |
+-------------------------+
1 row in set (0.00 sec)
```



+ 读未提交：`read-uncommitted` （最低隔离级别）

  + 事务A可以读取到事务B未提交的数据

  + 这种隔离级别存在的问题：脏读现象，即读到脏数据

  + 这种隔离级别一般都是理论上的，大多数数据库隔离级别是二档起步，即一般使用下面三个隔离级别。

    ```go
    开启两个终端，事务A终端和事务B终端
    事务A：
    mysql> select @@transaction_isolation;
    +-------------------------+
    | @@transaction_isolation |
    +-------------------------+
    | READ-UNCOMMITTED        |
    +-------------------------+
    1 row in set (0.00 sec)
    
    mysql> use bjpowernode;
    Database changed
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> select * from dept_bak;
    +--------+-------+---------+
    | DEPTNO | DNAME | LOC     |
    +--------+-------+---------+
    |     10 | sales | beijing |
    +--------+-------+---------+
    1 row in set (0.00 sec)
    
    mysql> select * from dept_bak;
    +--------+-------+---------+
    | DEPTNO | DNAME | LOC     |
    +--------+-------+---------+
    |     10 | sales | beijing |
    |     10 | sales | beijing |
    +--------+-------+---------+
    2 rows in set (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    ```

    ```go
    事务B：
    mysql> use bjpowernode;
    Database changed
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(10,'sales','beijing');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> insert into dept_bak values(10,'sales','beijing');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> rollback;
    Query OK, 0 rows affected (0.00 sec)
    
    
    从事务B中向同一张表dept_bak中插入数据，但是在事务B中还没有提交，在事务A就可以查看出事务B向表中添加的数据。
    当事务B回滚交易时，此时在事务A中可以看到表中的数据已经不存在了
    可以得出：事务B对表中进行操作时，尽管还没有提交，但是在事务A中已经可以查看到事务B添加的数据
    ```

    ![image-20221225181137306](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225181137306.png)

+ 读已提交:  `read-committed`

  + 事务A只能读取到事务B已经提交的数据。

  + 这种隔离级别解决了脏读现象。

  + 该级别存在不可重复读取数据。不可重复读取数据是指事务开启后且没有结束前，可能每次读取的数据是不同的，因为可能还有数据在提交。

  + 这种隔离级别，每次读取到的数据都是绝对真实的。

  + oracle数据默认采用的隔离级别为：read committed

    ```go
    //打开两个终端，分别用于事务A和事务B
    
    事务A终端内容：
    mysql> select @@transaction_isolation;
    +-------------------------+
    | @@transaction_isolation |
    +-------------------------+
    | READ-COMMITTED          |
    +-------------------------+
    1 row in set (0.00 sec)
    
    mysql> use bjpowernode;
    Database changed
    mysql> select *from dept_bak;
    Empty set (0.00 sec)
    
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> select * from dept_bak;
    +--------+---------+---------+
    | DEPTNO | DNAME   | LOC     |
    +--------+---------+---------+
    |     10 | manager | tianjin |
    +--------+---------+---------+
    1 row in set (0.00 sec)
    
    
    事务B终端内容：
    mysql> use bjpowernode;
    Database changed
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(10,'manager','tianjin');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)
    
    
    
    //可以看到事务B尽管插入数据，但是在没有提交前，事务A是无法查看数据的，表dept_bak仍为空，只有当事务B已经提交事务后，在事务A中才可以查看到数据。
    ```

    

    ![image-20221225182037312](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225182037312.png)

+ 可重复读:  `repeatable-read`

  + 事务A开启之后，不管多久，每次在事务A中读取到的数据都是一致的。即使事务B将数据已修改并提交了，事务A读到的数据仍不会发生变化。

  + 这种隔离级别解决了不可能重复读取数据的问题。

  + 该级别存在的问题：可能出现幻影读。每次读取的数据都是幻想，不够真实。

  + 其实就是：只要事务A不结束，读到的数据不会发生变化，仍是事务A开启前的数据，其它事务对数据修改对事务A没有影响。

  + mysql中默认的事务级别：repeatable read

    ```go
    //打开两个终端
    
    //事务A终端：
    mysql> select @@transaction_isolation;
    +-------------------------+
    | @@transaction_isolation |
    +-------------------------+
    | REPEATABLE-READ         |
    +-------------------------+
    1 row in set (0.00 sec)
    
    mysql> use bjpowernode;
    Database changed
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> select * from dept_bak;
    Empty set (0.00 sec)
    
    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;
    +--------+-------+------+
    | DEPTNO | DNAME | LOC  |
    +--------+-------+------+
    |    100 | abc   | xxx  |
    +--------+-------+------+
    1 row in set (0.00 sec)
    
    
    
    //事务B终端：
    mysql> use bjpowernode;
    Database changed
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(100,'abc','xxx');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)
    
    
    可以得出结论：在事务A开始之后，之后事务A未结束之前，事务A中查看的数据为事务A开始时的数据。事务B在事务A之后修改的数据，哪怕事务B已经提交了，但是事务A仍然查询不到。只有当事务A结束之后，事务A才能查看到修改的数据。
    ```

    ![image-20221225185015729](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225185015729.png)

+ 序列化/串行化：`serializable` （最高隔离级别）

  + 这高隔离级别，效率最低，解决所有问题。

  + 这种隔离级别表示事务排队进行，不能并发

  + 每次读取的数据是最真实的，并且效率最低

    ```go
    //打开两个终端：
    
    //事务A终端：
    mysql> use bjpowernode;
    Database changed
    mysql> select * from dept_bak;
    +--------+-------+------+
    | DEPTNO | DNAME | LOC  |
    +--------+-------+------+
    |    100 | abc   | xxx  |
    +--------+-------+------+
    1 row in set (0.00 sec)
    
    mysql> start transaction;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into dept_bak values(20,'efg','www');
    Query OK, 1 row affected (0.00 sec)
    
    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select @@transaction_isolation;
    +-------------------------+
    | @@transaction_isolation |
    +-------------------------+
    | SERIALIZABLE            |
    +-------------------------+
    1 row in set (0.00 sec)
    
    
    
    //事务B终端：
    mysql> use bjpowernode;
    Database changed
    mysql> start transaction ;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from dept_bak;
    ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
    mysql> select * from dept_bak;
    +--------+-------+------+
    | DEPTNO | DNAME | LOC  |
    +--------+-------+------+
    |    100 | abc   | xxx  |
    |     20 | efg   | www  |
    +--------+-------+------+
    2 rows in set (4.51 sec)
    
    
    //可以得出结论：事务A在对一个表进行操作时，只要还未提交，事务B是无法操作这张表的，只有当事务A提交后，事务B才能操作这张表。当事务A在进行时，如果事务B也操作这张表，那么事务B的命令处于阻塞状态，只有事务A提交后，事务B的命令才能正常进行。 
    ```

    ![image-20221225185832655](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225185832655.png)

    ![image-20221225190029015](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225190029015.png)









# 索引

## 定义

索引（index）：是在数据库表的字段上添加的，是一种为了提高查询效率而存在的一种机制。一张表的每个字段上都可以添加一个索引，或者多个字段联合起来也可以添加索引。

索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。



**mysql在查询方面主要是两种方式：**

+ 全表扫描，就是一条一条地比对
+ 根据索引检索，就是where条件字段上有索引，会利用索引来查找



## 索引实现原理

+ 在mysql数据库中，索引也是需要排序的，并且索引排序和TreeSet数据结构相同。TreeSet底层是一个自平衡二叉树。所以，在mysql当中索引是一个B-Tree数据结构，遵循左小右大原则存放，采用中序遍历方式遍历数据
+ **==在任何数据库中，主键字段都会自动添加索引，另外在mysql中，如果一个字段的约束为unique，也会自动添加索引==**
+ 在任何数据库中，任何一张表的任何一条记录在硬盘存储上都有一个硬盘的物理编号
+ 在mysql中，索引是一个单独的对象，索引在不同的存储引擎以不同的形式存在。在MyISAM存储引擎中，索引存储在一个`.MYI`文件中。在InnoDB存储引擎中，索引存储在一个逻辑名称为`tablespace`当中。在MEMORY存储引擎当中，索引被存储在内存当中。不管索引存储在哪里，索引在mysql中都是以 自平衡二叉树 的形式存在



## 什么时候添加索引

**满足以下条件需要考虑添加索引：**

+ 数据量庞大（根据硬件环境来决定）
+ 该字段经常出现在where后面，以条件的形式存在，即该字段总是被扫描
+ 该字段很很少进行DML操作。因为DML之后，索引需要重新排序。



==建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。建议通过主键查询，通过unique约束的字段查询，效率相对较高的。==





## 索引创建、删除、检索

### 索引创建

```go
//创建索引
//在表emp的ename字段上创建索引，索引名为emp_ename_index
create index emp_ename_index on emp(ename)  


mysql> create index emp_ename_index on emp(ename);
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0
```



### 索引删除

```go
//删除索引
//从表中删除索引名为emp_ename_index 的索引
drop index emp_ename_index on emp;


mysql> drop index emp_ename_index on emp;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
```



### 如何查看一个sql语句是否使用了索引检索

```go
使用explain来解释某条sql语句是否使用索引来检索

mysql> explain select * from emp where ename ='king';
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | emp   | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   14 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)

//当我们给ename 添加索引后，再进行查询可以发现：此时是通过索引查询数据的。

mysql> create index emp_ename_index on emp(ename);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain select * from emp where ename ='king';
+----+-------------+-------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys   | key             | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | emp   | NULL       | ref  | emp_ename_index | emp_ename_index | 43      | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```

![image-20221225214837436](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225214837436.png)

![image-20221225215003867](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225215003867.png)





## 索引失效

```go
//第一种情况：
//示例：
select * from emp where ename like '%T';  //模糊查询，哪怕在ename上添加索引，也不会根据索引查询

//原因：是因为在模糊查询中使用了 % 开头了，尽量避免模糊查询时以 % 开始，否则无法使用索引查询，只能通过全局扫描。


mysql> explain select * from emp where ename like '%T';  //可以看到type为all，即代表全表扫描，扫描了14行
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | emp   | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   14 |    11.11 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
```

![image-20221225220651057](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225220651057.png)



```go
//第二种情况：
使用or的时候，索引也会失效。如果使用or时，必须保证or两边的字段都需要有索引，此时才会以索引查询，否则就是全表扫描。

原因：如果or其中的一边的字段没有索引，那么or的另一个字段的索引也会失效。
这种情况下，可以使用union，即 一边是索引查询，另一边是全表扫描，相比两边都是全表扫描，效率会更高一些。
```



```go
//第三种情况：
//使用复合索引时，没有使用左侧的列 来查询，此时索引失效

复合索引：两个字段或多个字段联合起来 添加一个索引，称为复合索引。

mysql> create index emp_job_sal_index on emp(job,sal);  //添加复合索引的方式
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

![image-20221225223023394](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225223023394.png)





```go
//第四种情况：
在where当中，索引字段参加了运算，此时索引失效
```

![image-20221225223403853](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225223403853.png)





```go
//第五种情况：
在where当中，索引字段使用了函数，此时索引失效
```

![image-20221225223701144](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225223701144.png)







## 索引分类

==**索引是各种数据库进行优化的重要手段。优化的时候优先考虑的因素就是索引**==。索引在数据库种分了很多类。

+ 单一索引：一个字段上添加索引
+ 复合索引：两个字段或多个字段上添加索引
+ 主键索引：主键上添加索引
+ 唯一性索引：具有unique约束的字段上添加索引。
+ ....

**==注意：唯一性比较弱的字段上添加索引用处不大==**









# 视图

## 定义

**视图（view）：视图是站在不同角度看待==同一份数据==。**



## 视图创建和删除

+ 创建视图：`create view 视图名 as select ...   //as后面必须跟select语句，即DQL语句才行`

  ```go
  //创建视图：只有DQL语句才能以view的形式创建
  mysql> create view  dept_bak_view as select * from dept_bak;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> show tables;
  +-----------------------+
  | Tables_in_bjpowernode |
  +-----------------------+
  | dept                  |
  | dept_bak              |
  | dept_bak_view         |  //dept_bak视图
  | emp                   |
  | salgrade              |
  | t_class               |
  | t_student             |
  +-----------------------+
  7 rows in set (0.00 sec)
  ```

+ 删除视图：`drop view 视图名`

  ```go
  //删除视图：
  mysql> drop view  dept_bak_view;
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> show tables;
  +-----------------------+
  | Tables_in_bjpowernode |
  +-----------------------+
  | dept                  |
  | dept_bak              |
  | emp                   |
  | salgrade              |
  | t_class               |
  | t_student             |
  +-----------------------+
  6 rows in set (0.00 sec)
  ```

  

## 视图作用

+ ==**我们可以面向视图对象进行增删改查，对视图对象进行的增删改查，会导致原表被操作**。==

  ```go
  mysql> create view  dept_bak_view as select * from dept_bak; //创建视图
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> select * from dept_bak; //原表数据
  +--------+-------+------+
  | DEPTNO | DNAME | LOC  |
  +--------+-------+------+
  |    100 | abc   | xxx  |
  |     20 | efg   | www  |
  +--------+-------+------+
  2 rows in set (0.00 sec)
  
  mysql> insert into dept_bak_view(deptno,dname,loc)values(60,'yui','ppp'); //对视图对象插入数据
  Query OK, 1 row affected (0.00 sec)
  
  mysql> select * from dept_bak;  //可以看到原表的数据发生变化了
  +--------+-------+------+
  | DEPTNO | DNAME | LOC  |
  +--------+-------+------+
  |    100 | abc   | xxx  |
  |     20 | efg   | www  |
  |     60 | yui   | ppp  |
  +--------+-------+------+
  3 rows in set (0.00 sec)
  ```

+ **多张表关联创建的视图，对视图数据进行操作也会导致原表发生改变**。

+ 视图对象在实际开发到底有什么用？

  + **视图对象来代替一条select语句**。

    ```go
    比如，在开发中，有一条非常复杂的select语句，但是又需要多次使用时，可以创建一个视图来表示该sql语句，下次使用就不需要重复使用该复杂的select语句了。从而简化开发，提高效率，也便于后期维护。
    ```

  + 在面向视图开发时，**可以像使用表一样对视图进行增删改查（**又称为CRUD，其中C-->create 增，R-->retrive 查、检索，U-->update 改，D-->delete 删）操作，视图对象也是存储在硬盘上的，不会消失。





# DBA命令

掌握数据导入和导出（数据备份）命令：

+ 数据导出：

  ```go
  //在window dos命令窗口中执行以下命令：
  
  //导出整个数据库：
  mysqldump 数据库名>导出的绝对路径的文件名 -u 用户名 -p 密码 
  
  C:\Users\18390>mysqldump bjpowernode> D:\bjpowernode.sql -u root -p
  Enter password: ***********
  
  
  //导出数据库中的表：
  mysqldump 数据库名 表名>导出的绝对路径的文件名 -u 用户名 -p 密码 
  
  C:\Users\18390>mysqldump bjpowernode emp> D:\bjpowernode_emp.sql -u root -p
  Enter password: ***********
  ```

  ![image-20221225233152357](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221225233152357.png)

+ 数据导入：

  ```go
  //先登录数据库
  //然后创建数据库：create database 数据库名;
  //再使用数据库：use 数据库名;
  //初始化数据库：source 导出的sql文件;
  
  mysql> show databases;  //可以看到此时没有bjpowernode数据库
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | performance_schema |
  | sakila             |
  | sys                |
  | world              |
  +--------------------+
  6 rows in set (0.00 sec)
  
  mysql> create database bjpowernode;  //创建bjpowernode数据库
  Query OK, 1 row affected (0.00 sec)
  
  mysql> use bjpowernode;  //使用bjpowernode数据库
  Database changed
  
  mysql> source D:\bjpowernode.sql  //导入数据库的sql文件
  
  mysql> source D:\bjpowernode_emp.sql  //导入数据库中表的sql文件
  
  ```

  









# 主从复制



### 简介

**主从复制**：主从复制是用来建立一个和主数据库完全一样的数据库环境称为从数据库；



**主从复制的作用**：

+ 如果主数据库**出现单点故障**问题，我们可以直接将服务**切换到从数据库**，从而保证服务立马可用
+ 如果并发请求特别大时，我们可以**进行读写分离操作**，主数据库负责写，从数据库负责读。
+ 如果主数据库数据丢失，从数据库中还保留了一份，减少了数据丢失的风险。





### 原理

![image-20230227142816538](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20230227142816538.png)

**该图主要分成了三步：**

+ 第一步：master服务器将更新事件（update、insert、delete）按照顺序写入二进制bin-log日志中，当slave连接到master后，master会为slave开启bin-log dump线程，该线程会读取bin-log日志
+ slave服务器连接master后，slave会在一定时间间隔内对master二进制日志进行探测是否发生改变，如果发生改变，则开启一个I/O Thread 请求master bin-log日志。
+ master为每个I/O线程启动一个dump线程，用于向其发送二进制bin-log日志中的事件，然后slave将读取的bin-log日志写入从数据库的中继日志（relay log）中，slave将启动SQL线程从relay log中读取二进制事件日志，在本地重放，使得从数据库中的数据和主数据库中的数据保持一致，最后slave的I/O Thread和SQL Thread将进入睡眠状态，等待下一次被唤醒。



**总结：**

+ master必须开启bin-log功能，将更新事件记录到bin-log日志中。
+ 整个mysql主从复制一共开启了3个线程，master开启I/O线程，Slave开启I/O线程和SQL线程。
+ **master和slave交互时，是slave去请求master，而不是master主动推送给slave**。slave通过I/O线程连接master后发起请求，master服务器收到slave I/O线程发来的日志请求信息，I/O线程去将bin-log内容返回给slave I/O线程。



**Mysql主从复制同步方式**：

+ **异步复制**：==**mysql主从同步  —>  默认采用异步复制**==。上面三步中只有第一步是同步（master向bin-log写入日志），主数据库写入bin-log日志后即可成功返回客户端，无需等待bin-log日志传递给从数据库过程。master不关心slave的数据有没有写入成功。因此，如果master和slave之间有网络延迟，就会造成暂时的数据不一致现象。如果master出现故障，而数据日志还没有传递过去，则会造成数据丢失。但该方式效率最高。
+ **同步复制**：相对同步复制而言，master将事件发送给slave主机后会触发一个等待，直到所有的slave节点（如果存在多个slave）返回数据复制成功的信息个master。这种方式最安全，但是效率最低。
+ **半同步复制**：master主机将事件发送给slave主机后会触发一个等待，直到其中一个slave节点（如果存在多个节点）返回数据复制成功的信息给master。半同步复制除了不需要等待所有的slave主机确认事件的接收外，半同步复制不要事件完全执行，因此仍可能看到slave主机上数据复制延迟的发生，如果因为网络延迟等原因造成slave迟迟没有返回复制成功的信息，超过了master设置的时长，半同步复制就变成了异步复制。





### 主从同步延迟

==**Mysql默认采用的异步操作**==，因为它的效率明显是最高的。因为只要写入bin log后事物就结束返回成功了。但由于从库从主库异步拷贝日志 以及串行执行 SQL 的特点，所以从库的数据一定会比主库慢一些，是有延时的。所以经常出现，刚写入主库的数据可能是读不到的，要过几十毫秒，甚至几百毫秒才能读取到。这就是主从同步延时问题。



**如何查看主从延迟时间**：通过监控 show slave status 命令输出的Seconds_Behind_Master参数的值来判断：

```go
mysql> show slave status\G;
    // 状态一
    Seconds_Behind_Master: NULL  //表示io_thread或是sql_thread有任何一个发生故障；
    // 状态二
    Seconds_Behind_Master: 0   //表示主从复制良好；
    // 状态三
    Seconds_Behind_Master: 79  //数字越大表示从库延迟越严重。

```



**影响延迟因素**:

+ 主节点如果执行一个很大的事务，那么就会对主从延迟产生较大的影响
+ 网络延迟，日志较大，slave数量过多
+ 主上多线程写入，从节点只有单线程同步
+ 机器性能问题，从节点是否使用了“烂机器”
+ 锁冲突问题也可能导致从机的SQL线程执行慢



**优化主从复制延迟**:

+ 大事务：将大事务分为小事务，分批更新数据
+ 减少Slave的数量，不要超过5个，减少单次事务的大小
+ MySQL 5.7之后，可以使用多线程复制，使用MGR复制架构
+ 在磁盘、raid卡、调度策略有问题的情况下可能会出现单个IO延迟很高的情况，可用iostat命令查看DB数据盘的IO情况，再进一步判断
+ 针对锁问题可以通过抓去processlist以及查看information_schema下面和锁以及事务相关的表来查看

















# 数据库设计的三范式

数据库设计范式：是数据库表的设计依据，教我们如何对数据库表进行设计。

==**数据库设计范式**：==：

+ 第一范式：**要求任何一张表必须又主键，每一个字段的原子性不可再分**
+ 第二范式：建立在第一范式的基础之上，要求所有非主键的字段完全依赖主键，不要产生部分依赖。
+ 第三范式：建立在第二范式的基础之上，要求所有非主键字段直接依赖主键，不要产生传递依赖。

设计数据库表的时候，按照以上的范式进行，可以避免表中数据的冗余，空间浪费。



## 第一范式

第一范式，是最重要的范式，所有表的设计都需要满足。第一范式要求必须有主键，并且每一个字段都是原子性不可再分。

![image-20221226172246227](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221226172246227.png)

```go
//可以看到，第一张表中，首先没有主键，而且联系方式包含了邮箱和电话，故联系方式不是原子性的可以再分，可以分为 邮箱、电话。

//可以看到，表2，首先添加了主键，然后将联系方式变为两个字段：email和联系电话，此时所有字段都是原子性的不可再分。
```





## 第二范式

第二范式：建立在第一范式的基础之上，要求所有非主键的字段必须完全依赖主键，不要产生部分依赖。

第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。

![image-20221226173011697](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221226173011697.png)

```go
//表1，没有主键，因此，不满足第一范式。

//表2，是将学生编号和教师编号联合起来作为一个主键，称为复合主键。但是产生部分依赖，造成了数据冗余，如“张三”和”王老师“出现重复。
```

![image-20221226174128919](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221226174128919.png)

```go
//复合主键容易出现部分依赖，可以采用多张表的单一主键设计。

//如表3，4，5，三张表的一种多对多设计。

```

==**多对多设计口诀：多对多，三张表，关系表两个外键！！**==



## 第三范式

第三范式：建立在第二范式的基础之上，要求所有非主键字段直接依赖主键，不要产生传递依赖。

第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

![image-20221226175720503](C:\Users\18390\AppData\Roaming\Typora\typora-user-images\image-20221226175720503.png)



```go
//表1，符合第一范式，因为有主键，并且也符合第二范式，因为主键是单一主键而不是复合主键，没有产生部分依赖。

//从表1可以看到：一年一班依赖01，01依赖1001，产生了传递依赖。因此不符合第三范式要求，产生了数据冗余。

//可以采用一对多设计来解决直接依赖。
```



==**一对多口诀：一对多，两张表，多的表加外键！！！**==





## 表设计总结

+ 一对多：一对多，两张表，多的表加外键
+ 多对多：多对多，三张表，关系表加外键
+ 一对一：一对一，外键唯一



==数据库设计三范式是理论上的，实践和理论有时候存在偏差，最终目的都是为了满足客户的需求，有时候会拿冗余换执行速度。因为在sql当中，表与表之间的连接次数越多，效率越低（笛卡尔积现象）。有时候可能会存在冗余，但是为了减少表的连接次数，这样做也是合理的，并且对开发人员来说，sql语句的编写难度也会降低。==

















# 作业

```go
1、取得每个部门最高薪水的人员名称
mysql> select e.ename ,e.deptno,e.sal from emp e join (select deptno,max(sal) as maxsal from emp group by deptno) as t on e.deptno = t.deptno where e.sal = t.maxsal;
+-------+--------+---------+
| ename | deptno | sal     |
+-------+--------+---------+
| BLAKE |     30 | 2850.00 |
| SCOTT |     20 | 3000.00 |
| KING  |     10 | 5000.00 |
| FORD  |     20 | 3000.00 |
+-------+--------+---------+
4 rows in set (0.00 sec)



mysql> select e.ename ,e.deptno,e.sal from emp e join (select deptno,max(sal) as maxsal from emp group by deptno) as t on e.deptno = t.deptno and e.sal = t.maxsal;
+-------+--------+---------+
| ename | deptno | sal     |
+-------+--------+---------+
| BLAKE |     30 | 2850.00 |
| SCOTT |     20 | 3000.00 |
| KING  |     10 | 5000.00 |
| FORD  |     20 | 3000.00 |
+-------+--------+---------+
4 rows in set (0.00 sec)
```



```go
2、哪些人的薪水在部门的平均薪水之上
mysql> select e.ename,t.deptno,e.sal,t.avgsal from emp e join (select deptno,avg(sal) as avgsal from emp group by deptno ) as t on e.deptno=t.deptno and e.sal > t.avgsal;
+-------+--------+---------+-------------+
| ename | deptno | sal     | avgsal      |
+-------+--------+---------+-------------+
| ALLEN |     30 | 1600.00 | 1566.666667 |
| JONES |     20 | 2975.00 | 2175.000000 |
| BLAKE |     30 | 2850.00 | 1566.666667 |
| SCOTT |     20 | 3000.00 | 2175.000000 |
| KING  |     10 | 5000.00 | 2916.666667 |
| FORD  |     20 | 3000.00 | 2175.000000 |
+-------+--------+---------+-------------+
6 rows in set (0.00 sec)
```



```go
3、取得部门中的平均薪水等级，如下：
//注意：表连接完其实相当于已经产生了一个连接表（临时表），可以直接对这临时表分组操作。
mysql> select e.deptno,avg(s.grade) from emp e join salgrade s on e.sal between s.losal and s.hisal group by e.deptno;
+--------+--------------+
| deptno | avg(s.grade) |
+--------+--------------+
|     20 |       2.8000 |
|     30 |       2.5000 |
|     10 |       3.6667 |
+--------+--------------+
3 rows in set (0.00 sec)
```



```go
4、不准用组函数（Max），取得最高薪水（给出两种解决方案）

//第一种方案：
mysql> select ename,sal from emp order by sal desc limit 1;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
+-------+---------+
1 row in set (0.00 sec)

//第二种方案：利用表的自连接
mysql> select ename,sal from emp where sal not in (select distinct e1.sal from emp e1 join emp e2 on e1.sal<e2.sal);
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
+-------+---------+
1 row in set (0.00 sec)
```



```go
5、取得平均薪水最高的部门的部门编号（至少给出两种解决方案）

//第一种：order by 是在select之后执行的
mysql> select deptno,avg(sal)as avgsal from emp group by deptno order by avgsal desc limit 1;
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
1 row in set (0.00 sec)

//第二种：
mysql> select deptno,avg(sal) as avgsal from emp group by deptno having avg(sal) = (select max(t.avgsal) from (select deptno,avg(sal) as avgsal from emp group by deptno) as t);
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
1 row in set (0.00 sec)
```



```go
6、取得平均薪水最高的部门的部门名称
mysql> select t.deptno,t.avgsal ,d.dname from (select deptno ,avg(sal) as avgsal from emp group by deptno) as t join dept as d on t.deptno = d.deptno order by avgsal desc limit 1;
+--------+-------------+------------+
| deptno | avgsal      | dname      |
+--------+-------------+------------+
|     10 | 2916.666667 | ACCOUNTING |
+--------+-------------+------------+
1 row in set (0.00 sec)
```



```go
7、求平均薪水的等级最低的部门的部门名称
mysql> select tt.deptno,tt.grade,d.dname from (select t.deptno,t.avgsal,s.grade from (select deptno ,avg(sal) as avgsal from emp group by deptno) as t join salgrade as s on t.avgsal between s.losal and s.hisal) as tt join dept as d on tt.deptno = d.deptno order by grade limit 1;
+--------+-------+-------+
| deptno | grade | dname |
+--------+-------+-------+
|     30 |     3 | SALES |
+--------+-------+-------+
1 row in set (0.00 sec)
```



```go
8、取得比普通员工(员工代码没有在 mgr 字段上出现的)的最高薪水还要高的领导人姓名
mysql>  select ename,sal from emp where empno in (select mgr from emp where mgr is not null) and sal > (select max(sal) from emp where empno not in (select mgr from emp where mgr is not null));
+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+
6 rows in set (0.00 sec)


mysql> select ename,sal from emp where sal > (select max(sal) from emp where empno not in (select mgr from emp where mgr is not null));
+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+
6 rows in set (0.00 sec)
```



```go
9、取得薪水最高的前五名员工
mysql> select ename,sal from emp order by sal desc limit 5;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| SCOTT | 3000.00 |
| FORD  | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
5 rows in set (0.00 sec)
```



```go
10、取得薪水最高的第六到第十名员工
mysql> select ename,sal from emp order by sal desc limit 5,5;
+--------+---------+
| ename  | sal     |
+--------+---------+
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
+--------+---------+
5 rows in set (0.00 sec)
```



```go
11、取得最后入职的 5 名员工

//日期也可以用于排序还可以用于比较大小
mysql> select ename,hiredate from emp order by hiredate desc limit 5;
+--------+------------+
| ename  | hiredate   |
+--------+------------+
| ADAMS  | 1987-05-23 |
| SCOTT  | 1987-04-19 |
| MILLER | 1982-01-23 |
| JAMES  | 1981-12-03 |
| FORD   | 1981-12-03 |
+--------+------------+
5 rows in set (0.00 sec)
```



```go
12、取得每个薪水等级有多少员工
mysql> select s.grade,count(*) from emp e join salgrade s on e.sal between s.losal and hisal group by s.grade;
+-------+----------+
| grade | count(*) |
+-------+----------+
|     1 |        3 |
|     3 |        2 |
|     2 |        3 |
|     4 |        5 |
|     5 |        1 |
+-------+----------+
5 rows in set (0.00 sec)
```



```go
13、面试题
```



```go
14、列出所有员工及领导的姓名
mysql> select e1.ename as '员工名',e2.ename as '领导名' from emp e1 left join emp e2 on e1.mgr=e2.empno;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| SMITH  | FORD   |
| ALLEN  | BLAKE  |
| WARD   | BLAKE  |
| JONES  | KING   |
| MARTIN | BLAKE  |
| BLAKE  | KING   |
| CLARK  | KING   |
| SCOTT  | JONES  |
| KING   | NULL   |
| TURNER | BLAKE  |
| ADAMS  | SCOTT  |
| JAMES  | BLAKE  |
| FORD   | JONES  |
| MILLER | CLARK  |
+--------+--------+
14 rows in set (0.00 sec)
```



```go
15、列出受雇日期早于其直接上级的所有员工的编号,姓名,部门名称
mysql> select t.empno,t.ename,d.dname from (select e1.empno, e1.ename,e1.hiredate,e1.deptno from emp e1 join emp e2 on e1.mgr = e2.empno and e1.hiredate < e2.hiredate) as t join dept d on t.deptno = d.deptno;
+-------+-------+------------+
| empno | ename | dname      |
+-------+-------+------------+
|  7369 | SMITH | RESEARCH   |
|  7499 | ALLEN | SALES      |
|  7521 | WARD  | SALES      |
|  7566 | JONES | RESEARCH   |
|  7698 | BLAKE | SALES      |
|  7782 | CLARK | ACCOUNTING |
+-------+-------+------------+
6 rows in set (0.00 sec)
```



```go
16、列出部门名称和这些部门的员工信息,同时列出那些没有员工的部门.
mysql> select e.*,d.dname from emp e right join dept d on e.deptno=d.deptno;
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO | dname      |
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 | ACCOUNTING |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 | ACCOUNTING |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 | ACCOUNTING |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 | RESEARCH   |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 | RESEARCH   |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 | RESEARCH   |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 | RESEARCH   |
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 | RESEARCH   |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 | SALES      |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 | SALES      |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 | SALES      |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 | SALES      |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 | SALES      |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 | SALES      |
|  NULL | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |   NULL | OPERATIONS |
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
```



```go
17、列出至少有 5 个员工的所有部门
mysql> select t.dname,count(*) from (select e.*,d.dname from emp e right join dept d on e.deptno=d.deptno) as t group by t.dname having count(*) >=5;
+----------+----------+
| dname    | count(*) |
+----------+----------+
| RESEARCH |        5 |
| SALES    |        6 |
+----------+----------+
2 rows in set (0.00 sec)
```



```go
18、列出薪金比"SMITH"多的所有员工信息.
mysql> select * from emp where sal > (select sal from emp where ename = 'smith');
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
13 rows in set (0.00 sec)
```



```go
19、列出所有"CLERK"(办事员)的姓名及其部门名称,部门的人数
mysql> select t1.ename,t1.job,t1.dname,t1.deptno,t2.deptcount from (select t.* ,d.dname from ( select *  from emp where job ='clerk') as t join dept as d on t.deptno=d.deptno) t1 join (select deptno,count(*) as deptcount from emp group by deptno) t2 on t1.deptno =t2.deptno;
+--------+-------+------------+--------+-----------+
| ENAME  | JOB   | dname      | DEPTNO | deptcount |
+--------+-------+------------+--------+-----------+
| SMITH  | CLERK | RESEARCH   |     20 |         5 |
| ADAMS  | CLERK | RESEARCH   |     20 |         5 |
| JAMES  | CLERK | SALES      |     30 |         6 |
| MILLER | CLERK | ACCOUNTING |     10 |         3 |
+--------+-------+------------+--------+-----------+
4 rows in set (0.00 sec)
```



```go
20、列出最低薪金大于 1500 的各种工作及从事此工作的全部雇员人数.
mysql> select job,count(job) from emp where sal > 1500  group by job;
+-----------+------------+
| job       | count(job) |
+-----------+------------+
| SALESMAN  |          1 |
| MANAGER   |          3 |
| ANALYST   |          2 |
| PRESIDENT |          1 |
+-----------+------------+
4 rows in set (0.00 sec)


//从下面语句中可以看出， sal字段是未识别的，having后面只能跟分组函数
mysql> select job,count(job) from emp group by job having sal > 1500;
ERROR 1054 (42S22): Unknown column 'sal' in 'having clause
```



```go
21、列出在部门"SALES"<销售部>工作的员工的姓名,假定不知道销售部的部门编号.
mysql> select e.ename from emp e  join dept d on e.deptno =d.deptno where d.dname = 'sales';
+--------+
| ename  |
+--------+
| ALLEN  |
| WARD   |
| MARTIN |
| BLAKE  |
| TURNER |
| JAMES  |
+--------+
6 rows in set (0.00 sec)
```



```go
22、列出薪金高于公司平均薪金的所有员工,所在部门,上级领导,雇员的工资等级.
mysql> select t1.ename '员工名',t2.ename '领导名',d.dname,s.grade from (select * from emp where sal > (select avg(sal) from emp)) t1 left join (select * from emp where sal > (select avg(sal) from emp)) t2 on t1.mgr = t2.empno join dept d on t1.deptno =d.deptno join salgrade s on t1.sal between s.losal and s.hisal;
+--------+--------+------------+-------+
| 员工名 | 领导名 | dname      | grade |
+--------+--------+------------+-------+
| FORD   | JONES  | RESEARCH   |     4 |
| SCOTT  | JONES  | RESEARCH   |     4 |
| CLARK  | KING   | ACCOUNTING |     4 |
| BLAKE  | KING   | SALES      |     4 |
| JONES  | KING   | RESEARCH   |     4 |
| KING   | NULL   | ACCOUNTING |     5 |
+--------+--------+------------+-------+
6 rows in set (0.00 sec)
```



```go
23、列出与"SCOTT"从事相同工作的所有员工及部门名称.
mysql> select t.ename,d.dname from (select * from emp where job = (select job from emp where ename = 'scott') and ename != 'scott') as t join dept as d on t.deptno = d.deptno;
+-------+----------+
| ENAME | dname    |
+-------+----------+
| FORD  | RESEARCH |
+-------+----------+
1 row in set (0.00 sec)
```





```go
24、列出薪金高于在部门 30 工作的所有员工的薪金的员工姓名和薪金.部门名称
mysql> select t.ename,t.sal,d.dname from (select * from emp where sal > (select max(sal) from emp group by deptno having deptno=30)) t join dept d on t.deptno = d.deptno;
+-------+---------+------------+
| ENAME | SAL     | dname      |
+-------+---------+------------+
| JONES | 2975.00 | RESEARCH   |
| SCOTT | 3000.00 | RESEARCH   |
| KING  | 5000.00 | ACCOUNTING |
| FORD  | 3000.00 | RESEARCH   |
+-------+---------+------------+
4 rows in set (0.00 sec)
```



```go
25、列出在每个部门工作的员工数量,平均工资和平均服务期限
mysql> select d.dname,t.workercount,t.avgsal,t.avgyear from dept d join (select deptno,count(*) workercount , avg(sal) avgsal,avg(timestampdiff(year,hiredate,now())) avgyear from emp group by deptno) t on d.deptno = t.deptno;
+------------+-------------+-------------+---------+
| dname      | workercount | avgsal      | avgyear |
+------------+-------------+-------------+---------+
| ACCOUNTING |           3 | 2916.666667 | 40.6667 |
| RESEARCH   |           5 | 2175.000000 | 38.8000 |
| SALES      |           6 | 1566.666667 | 41.0000 |
+------------+-------------+-------------+---------+
3 rows in set (0.00 sec)
```



```go
26、列出所有员工的姓名、部门名称和工资。
mysql> select e.ename,e.sal,d.dname from emp e join dept d on e.deptno = d.deptno;
+--------+---------+------------+
| ename  | sal     | dname      |
+--------+---------+------------+
| SMITH  |  800.00 | RESEARCH   |
| ALLEN  | 1600.00 | SALES      |
| WARD   | 1250.00 | SALES      |
| JONES  | 2975.00 | RESEARCH   |
| MARTIN | 1250.00 | SALES      |
| BLAKE  | 2850.00 | SALES      |
| CLARK  | 2450.00 | ACCOUNTING |
| SCOTT  | 3000.00 | RESEARCH   |
| KING   | 5000.00 | ACCOUNTING |
| TURNER | 1500.00 | SALES      |
| ADAMS  | 1100.00 | RESEARCH   |
| JAMES  |  950.00 | SALES      |
| FORD   | 3000.00 | RESEARCH   |
| MILLER | 1300.00 | ACCOUNTING |
+--------+---------+------------+
14 rows in set (0.00 sec
```



```go
27、列出所有部门的详细信息和人数
mysql> select d.*,ifnull(t.deptcount,0) from dept d left join (select deptno, count(*) as deptcount from emp group by deptno) t on d.deptno = t.deptno;
+--------+------------+----------+-----------------------+
| DEPTNO | DNAME      | LOC      | ifnull(t.deptcount,0) |
+--------+------------+----------+-----------------------+
|     10 | ACCOUNTING | NEW YORK |                     3 |
|     20 | RESEARCH   | DALLAS   |                     5 |
|     30 | SALES      | CHICAGO  |                     6 |
|     40 | OPERATIONS | BOSTON   |                     0 |
+--------+------------+----------+-----------------------+
4 rows in set (0.00 sec)
```



```go
28、列出各种工作的最低工资及从事此工作的雇员姓名
mysql> select * from emp e join (select job,min(sal) minsal from emp group by job) t on e.job = t.job and e.sal = t.minsal;
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+---------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO | job       | minsal  |
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+---------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 | CLERK     |  800.00 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 | SALESMAN  | 1250.00 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 | SALESMAN  | 1250.00 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 | MANAGER   | 2450.00 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 | ANALYST   | 3000.00 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 | PRESIDENT | 5000.00 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 | ANALYST   | 3000.00 |
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+---------+
7 rows in set (0.00 sec)
```



```go
29、列出各个部门的 MANAGER(领导)的最低薪金
mysql> select deptno,min(sal) from emp where job = 'manager' group by deptno ;
+--------+----------+
| deptno | min(sal) |
+--------+----------+
|     20 |  2975.00 |
|     30 |  2850.00 |
|     10 |  2450.00 |
+--------+----------+
3 rows in set (0.00 sec)
```



```go
30、列出所有员工的年工资,按年薪从低到高排序
mysql> select ename,sal*12 as yearsal from emp order by yearsal asc;
+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| JAMES  | 11400.00 |
| ADAMS  | 13200.00 |
| WARD   | 15000.00 |
| MARTIN | 15000.00 |
| MILLER | 15600.00 |
| TURNER | 18000.00 |
| ALLEN  | 19200.00 |
| CLARK  | 29400.00 |
| BLAKE  | 34200.00 |
| JONES  | 35700.00 |
| SCOTT  | 36000.00 |
| FORD   | 36000.00 |
| KING   | 60000.00 |
+--------+----------+
14 rows in set (0.00 sec)
```



```go
31、求出员工领导的薪水超过 3000 的员工名称与领导名称
mysql> select e1.ename '员工名',e2.ename '领导名' from emp e1 join emp e2 on e1.mgr = e2.empno and e2.sal>3000;
+--------+--------+
| 员工名 | 领导名 |
+--------+--------+
| JONES  | KING   |
| BLAKE  | KING   |
| CLARK  | KING   |
+--------+--------+
3 rows in set (0.00 sec)
```



```go
32、求出部门名称中,带'S'字符的部门员工的工资合计、部门人数
mysql> select d.dname,ifnull(t.sumsal,0),ifnull(t.deptcount,0) from dept d left join (select deptno, sum(sal) sumsal,count(deptno) deptcount from emp group by deptno) t on d.deptno = t.deptno where d.dname like '%S%';
+------------+--------------------+-----------------------+
| dname      | ifnull(t.sumsal,0) | ifnull(t.deptcount,0) |
+------------+--------------------+-----------------------+
| RESEARCH   |           10875.00 |                     5 |
| SALES      |            9400.00 |                     6 |
| OPERATIONS |               0.00 |                     0 |
+------------+--------------------+-----------------------+
3 rows in set (0.00 sec)
```



```go
33、给任职日期超过 30 年的员工加薪 10%.
mysql> select ename ,(case when timestampdiff(year,hiredate,now()) >30 then sal*1.1 else sal end) newsal from emp;
+--------+---------+
| ename  | newsal  |
+--------+---------+
| SMITH  |  880.00 |
| ALLEN  | 1760.00 |
| WARD   | 1375.00 |
| JONES  | 3272.50 |
| MARTIN | 1375.00 |
| BLAKE  | 3135.00 |
| CLARK  | 2695.00 |
| SCOTT  | 3300.00 |
| KING   | 5500.00 |
| TURNER | 1650.00 |
| ADAMS  | 1210.00 |
| JAMES  | 1045.00 |
| FORD   | 3300.00 |
| MILLER | 1430.00 |
+--------+---------+
14 rows in set (0.00 sec)
```

