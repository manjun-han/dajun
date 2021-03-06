####MySQL获取数据库和表的信息

命令行方式下，操作数据库，需要知道数据库的信息，如，当前使用数据库的名称，数据库中有多少个表以及表的名称，每个表的表结构是什么，表中有几个字段，每个字段的名称和数据类型是什么，我们都需要进行查看和定位，进而进行下一步的操作。承接上篇创建的`simpleDB`继续，假设创建一个保存人员信息的`Person`表，包含了姓名，出生日期，性别

	mysql> create table Person(
		-> name varchar(100),
		-> birthday datetime,
		-> sex varchar(4)
		-> );
	Query OK, 0 rows affected (0.53 sec)

	mysql> 

####1. `select database()`获取当前使用的数据库名称
如，

	mysql> select database();
	+------------+
	| database() |
	+------------+
	| simpledb   |
	+------------+
	1 row in set (0.00 sec)

如果未选择任何数据库，select database()的结果为NULL；


	mysql> select database();
	+------------+
	| database() |
	+------------+
	| NULL       |
	+------------+
	1 row in set (0.00 sec)	
	mysql>

####2. `show tables`,找出当前数据库中包含的表
如

	mysql> show tables;
	+--------------------+
	| Tables_in_simpledb |
	+--------------------+
	| Person             |
	+--------------------+
	1 row in set (0.00 sec)
	
或，`show tables from db_name`,如

	mysql> show tables from simpleDB;
	+--------------------+
	| Tables_in_simpledb |
	+--------------------+
	| Person             |
	+--------------------+
	1 row in set (0.04 sec)

	mysql> 
		
或，使用模糊匹配表名称,`show tables from db_name like 'pattern'`,如

	mysql> show tables from simpleDB like 'p%';
	+-------------------------+
	| Tables_in_simpledb (p%) |
	+-------------------------+
	| Person                  |
	+-------------------------+
	1 row in set (0.03 sec)

	mysql> 

####3. `describe table_name`,或`desc table_name`命令获取指定名称的表的结构
如

	mysql> describe Person;
	+----------+--------------+------+-----+---------+-------+
	| Field    | Type         | Null | Key | Default | Extra |
	+----------+--------------+------+-----+---------+-------+
	| name     | varchar(100) | YES  |     | NULL    |       |
	| birthday | datetime     | YES  |     | NULL    |       |
	| sex      | varchar(4)   | YES  |     | NULL    |       |
	+----------+--------------+------+-----+---------+-------+
	3 rows in set (0.18 sec)

其中`Field`显示列名称，`Type`是列对应的数据类型，`Null`表示列是否可以包含NULL值，`Key`现实列是否被索引，`Default`指定列的默认值，`Extra`包含了对应列的附加信息。

也可以查看表的某一列的信息，前提是知道具体列的名称，如

	mysql> describe Person name;
	+-------+--------------+------+-----+---------+-------+
	| Field | Type         | Null | Key | Default | Extra |
	+-------+--------------+------+-----+---------+-------+
	| name  | varchar(100) | YES  |     | NULL    |       |
	+-------+--------------+------+-----+---------+-------+
	1 row in set (0.08 sec)

	mysql> 
如果不确定列的具体名称，也可以对列名使用模糊匹配，如，查询列名中包含字母`e`的列信息，

	mysql> desc Person '%e%';
	+-------+--------------+------+-----+---------+-------+
	| Field | Type         | Null | Key | Default | Extra |
	+-------+--------------+------+-----+---------+-------+
	| name  | varchar(100) | YES  |     | NULL    |       |
	| sex   | varchar(4)   | YES  |     | NULL    |       |
	+-------+--------------+------+-----+---------+-------+
	2 rows in set (0.06 sec)

	mysql> 

其实`describe`命令是`show columns`语句的快捷版,使用`show columns`语句也可以来获取表的结构，,简单语法格式如下,

	SHOW COLUMNS FROM tbl_name [FROM db_name] [LIKE 'pattern']
如：

	mysql> show columns from person;
	+----------+--------------+------+-----+---------+-------+
	| Field    | Type         | Null | Key | Default | Extra |
	+----------+--------------+------+-----+---------+-------+
	| name     | varchar(100) | YES  |     | NULL    |       |
	| birthday | datetime     | YES  |     | NULL    |       |
	| sex      | varchar(4)   | YES  |     | NULL    |       |
	+----------+--------------+------+-----+---------+-------+
	3 rows in set (0.10 sec)

	mysql> show columns from person from simpleDB;
	+----------+--------------+------+-----+---------+-------+
	| Field    | Type         | Null | Key | Default | Extra |
	+----------+--------------+------+-----+---------+-------+
	| name     | varchar(100) | YES  |     | NULL    |       |
	| birthday | datetime     | YES  |     | NULL    |       |
	| sex      | varchar(4)   | YES  |     | NULL    |       |
	+----------+--------------+------+-----+---------+-------+
	3 rows in set (0.01 sec)

	mysql> show columns from person like '%e%';
	+-------+--------------+------+-----+---------+-------+
	| Field | Type         | Null | Key | Default | Extra |
	+-------+--------------+------+-----+---------+-------+
	| name  | varchar(100) | YES  |     | NULL    |       |
	| sex   | varchar(4)   | YES  |     | NULL    |       |
	+-------+--------------+------+-----+---------+-------+
	2 rows in set (0.01 sec)

另外，也可以使用`db_name.tbl_name`代替`tbl_name from db_name`,所以

	mysql> show columns from person from simpleDB;
	
等价于		

	mysql> show columns from simpleDB.person;


