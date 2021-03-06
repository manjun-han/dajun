####MySQL服务器的线程状况


了解MySQL的当前线程信息对于掌握MySQL的运行状况如何是很有帮助的，尤其是当出现并发连接数量超过上限引起“too many connections”错误时，我们需要了解当前服务器所发生的情况。

####show processlist语句

SQL中的`show processlist`语句用来想我们展示当前服务器有哪些线程正在运行，以及每个线程的运行状态。拥有[PROCESS](http://dev.mysql.com/doc/refman/5.6/en/privileges-provided.html#priv_process)权限的用户可以通过该语句查看服务器的所有线程，否则只能查看当前账户本身关联对应的线程。默认情况下`show processlist`语句只能查看每个结果的`Info`字段的前100个字符，可以使用`FULL`关键字查看所有信息，如：`show full processlist`

> 附注:
> 
> + `performance_schema.threads`表中包含了所有服务器的线程信息,具体描述参见官网。
> + [`mysqladmin processlist`](5)命令也可以用来完成与`show processlist`语句同样的功能。

####示例

首先我本地Mac OS下安装了MySQL服务器，并正常运行中。

其次，因为我本地同时安装了Java开发环境，并建有使用数据库的Java Web工程，提供数据库连接池功能，所以为了方便说明，我将本地的连接池数量初始化为10，并更改数据库的配置信息连接到本地MySQL服务器的`test`数据库，之后运行我的Web工程，初始化数据库连接。

之后，在系统的命令行终端，连接我本地的MySQL服务器，查看使用`show processlist`的显示结果,如

	
	192:bin manjun.han$ mysql -u root -h localhost -p
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 3000
	Server version: 5.6.15 MySQL Community Server (GPL)

	Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

	Oracle is a registered trademark of Oracle Corporation and/or its
	affiliates. Other names may be trademarks of their respective
	owners.

	Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

	mysql> show processlist;
	+------+------+-----------------+------+---------+------+-------+------------------+
	| Id   | User | Host            | db   | Command | Time | State | Info             |
	+------+------+-----------------+------+---------+------+-------+------------------+
	| 2559 | root | localhost       | NULL | Sleep   | 9079 |       | NULL             |
	| 2987 | root | localhost:53130 | test | Sleep   |  505 |       | NULL             |
	| 2988 | root | localhost:53131 | test | Sleep   |  505 |       | NULL             |
	| 2989 | root | localhost:53132 | test | Sleep   |  505 |       | NULL             |
	| 2990 | root | localhost:53133 | test | Sleep   |  505 |       | NULL             |
	| 2991 | root | localhost:53134 | test | Sleep   |  505 |       | NULL             |
	| 2992 | root | localhost:53135 | test | Sleep   |  505 |       | NULL             |
	| 2993 | root | localhost:53136 | test | Sleep   |  505 |       | NULL             |
	| 2994 | root | localhost:53137 | test | Sleep   |  505 |       | NULL             |
	| 2995 | root | localhost:53138 | test | Sleep   |  505 |       | NULL             |
	| 2996 | root | localhost:53139 | test | Sleep   |  505 |       | NULL             |
	| 2997 | root | localhost:53140 | test | Sleep   |  505 |       | NULL             |
	| 2998 | root | localhost:53141 | test | Sleep   |  505 |       | NULL             |
	| 3000 | root | localhost       | NULL | Query   |    0 | init  | show processlist |
	+------+------+-----------------+------+---------+------+-------+------------------+
	14 rows in set (0.00 sec)

	mysql> 
	
以上字段的说明如下：

+ Id - 连接标识符	。
+ User - 线程所属用户。
+ Host - 连接线程的客户端主机名。
+ db - 默认连接数据库名称。未选择的话，值为NULL。
+ Command - 线程执行命令的类型，`Sleep`代表当前线程正等待客户端发送语句，`Query`代表当前线程正在执行查询，更多类型说明参见[Thread Command Values](4)。
+ Time - 当前线程保持目前状态的时间(单位:秒)。
+ State - 表示线程执行中发生的行为，事件或状态，多数状态多对应于某些快速的操作，如果线程长时间呆在某个状态，这事可能发生了错误，需要仔细调查。参见[Examining Thread Information][1]。对于`show processlist`语句查询，该字段值为NULL。
+ Info - 线程执行的语句描述，如果未有语句执行，此字段为NULL。如标识符为3000的最后一条记录是我们执行`show processlist`语句的线程信息。

####Too many connections错误说明

连接MySQL服务器，发生Too many connections的错误，意味着服务器当前所有连接都被其它客户端占用。

MySQL服务器的最大连接数是通过[max_connections](2)系统变量设置的。MySQL5.6版本默认最大连接数是150，之前的MySQL版本，如MySQL5.1的最大连接数为100.通过为[max_connections](2)变量设置更大的值，可以设置MySQL服务器支持更大的并发连接数。

事实上MySQL服务器在设置[max_connections](2)的值，做了个隐式的特殊处理，会在实际的数量之上多添加一个连接，即最大连接数量实际为[max_connections+1](2)。假如你设置[max_connections](2)＝150，那么实际服务器的[max_connections](2)＝151。这个额外的连接是为了留给拥有[SUPER](3)权限的账户使用的。一般情况下只有管理员账户拥有[SUPER](3)权限，但普通的账户却没有。这样即使是发生Too many connections错误，管理员账户也可以连接到MySQL服务器，使用`show processlist`语句来诊断问题。

MySQL支持的最大连接数依赖于其运行平台的线程库(thread library)的质量，总的可用RAM的大小，每个连接消耗的RAM大小，每个连接的负载(workload)，预期相应时间。Linux或Solaris平台通常可以支持500到1000的并发连接，如果有足够的RAM，每个连接的负载较低，并且对响应时间要求不高，那么最大的并发连接可以到10000。

> [MySQL : Too many connection errors.](http://www.microshell.com/database/mysql/mysql-too-many-connection-errors/)提及，服务器磁盘的爆满，也可能引起Too many connections错误。

####参考

+ [SHOW PROCESSLIST 语法(MySQL5.6)](http://dev.mysql.com/doc/refman/5.6/en/show-processlist.html)
+ [The threads Table(MySQL5.6)](http://dev.mysql.com/doc/refman/5.6/en/threads-table.html)
+ [ Too many connections(MySQL5.6)](http://dev.mysql.com/doc/refman/5.6/en/too-many-connections.html)
+ [A.2.6. 连接数过多(MySQL5.1)](http://dev.mysql.com/doc/refman/5.1/zh/problems.html#too-many-connections)
+ [Examining Thread Information][1]
+ [Thread Command Values](4)

[1]: http://dev.mysql.com/doc/refman/5.6/en/thread-information.html
[2]: http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_max_connections
[3]: http://dev.mysql.com/doc/refman/5.6/en/privileges-provided.html#priv_super
[4]: http://dev.mysql.com/doc/refman/5.6/en/thread-commands.html
[5]: http://dev.mysql.com/doc/refman/5.6/en/mysqladmin.html