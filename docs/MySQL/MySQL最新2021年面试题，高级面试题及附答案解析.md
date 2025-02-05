# MySQL最新2021年面试题，高级面试题及附答案解析

### 其实，博主还整理了，更多大厂面试题，直接下载吧

### 下载链接：[高清172份，累计 7701 页大厂面试题  PDF](https://github.com/souyunku/DevBooks/blob/master/docs/index.md)



### 1、MYSQL数据库服务器性能分析的方法命令有哪些?

**Show status, 一些值得监控的变量值：**

**1、** Bytes_received和Bytes_sent 和服务器之间来往的流量。

**2、** Com_*服务器正在执行的命令。

**3、** Created_*在查询执行期限间创建的临时表和文件。

**4、** Handler_*存储引擎操作。

**5、** Select_*不同类型的联接执行计划。

**6、** Sort_*几种排序信息。

**Show profiles 是MySql用来分析当前会话SQL语句执行的资源消耗情况**


### 2、超键、候选键、主键、外键分别是什么？

**1、** 超键：在关系模式中，能唯一知标识元组的属性集称为超键。

**2、** 候选键：是最小超键，即没有冗余元素的超键。

**3、** 主键：数据库表中对储存数据对象予以唯一和完整标识的数据列或属性的组合。一个数据列只能有一个主键，且主键的取值不能缺失，即不能为空值（Null）。

**4、** 外键：在一个表中存在的另一个表的主键称此表的外键。。


### 3、MySQL中int(10)和char(10)以及varchar(10)的区别

**1、** int(10)的10表示显示的数据的长度，不是存储数据的大小；chart(10)和varchar(10)的10表示存储数据的大小，即表示存储多少个字符。

**2、** char(10)表示存储定长的10个字符，不足10个就用空格补齐，占用更多的存储空间

**3、** varchar(10)表示存储10个变长的字符，存储多少个就是多少个，空格也按一个字符存储，这一点是和char(10)的空格不同的，char(10)的空格表示占位不算一个字符


### 4、一条SQL语句在MySQL中如何执行的？

先看一下MySQL的逻辑架构图吧~

![](https://user-gold-cdn.xitu.io/2020/5/23/1724037179201fe9?w=661&h=500&f=png&s=289132#alt=)

**查询语句：**

**1、** 先检查该语句是否有权限

**2、** 如果没有权限，直接返回错误信息

**3、** 如果有权限，在 MySQL8.0 版本以前，会先查询缓存。

**4、** 如果没有缓存，分析器进行词法分析，提取 sql 语句select等的关键元素。然后判断sql 语句是否有语法错误，比如关键词是否正确等等。

**5、** 优化器进行确定执行方案

**6、** 进行权限校验，如果没有权限就直接返回错误信息，如果有权限就会调用数据库引擎接口，返回执行结果。


### 5、MySQL里记录货币用什么字段类型好

NUMERIC和DECIMAL类型被MySQL实现为同样的类型，这在SQL92标准允许。他们被用于保存值，该值的准确精度是极其重要的值，例如与金钱有关的数据。当声明一个类是这些类型之一时，精度和规模的能被(并且通常是)指定。

例如：

salary DECIMAL(9,2)

在这个例子中，9(precision)代表将被用于存储值的总的小数位数，而2(scale)代表将被用于存储小数点后的位数。

因此，在这种情况下，能被存储在salary列中的值的范围是从-9999999.99到9999999.99。


### 6、简述在MySQL数据库中MyISAM和InnoDB的区别

**MyISAM：**

不支持事务，但是每次查询都是原子的；

支持表级锁，即每次操作是对整个表加锁；

**存储表的总行数；**

一个MYISAM表有三个文件：索引文件、表结构文件、数据文件；

采用菲聚集索引，索引文件的数据域存储指向数据文件的指针。辅索引与主索引基本一致，但是辅索引不用保证唯一性。

**InnoDb：**

支持ACID的事务，支持事务的四种隔离级别；

支持行级锁及外键约束：因此可以支持写并发；

**不存储总行数：**

一个InnoDb引擎存储在一个文件空间（共享表空间，表大小不受操作系统控制，一个表可能分布在多个文件里），也有可能为多个（设置为独立表空，表大小受操作系统文件大小限制，一般为2G），受操作系统文件大小的限制；

主键索引采用聚集索引（索引的数据域存储数据文件本身），辅索引的数据域存储主键的值；因此从辅索引查找数据，需要先通过辅索引找到主键值，再访问辅索引；最好使用自增主键，防止插入数据时，为维持B+树结构，文件的大调整。


### 7、InnoDB与MyISAM的区别

**1、** InnoDB支持事务，MyISAM不支持事务

**2、** InnoDB支持外键，MyISAM不支持外键

**3、** InnoDB 支持 MVCC(多版本并发控制)，MyISAM 不支持

**4、** select count(*) from table时，MyISAM更快，因为它有一个变量保存了整个表的总行数，可以直接读取，InnoDB就需要全表扫描。

**5、** Innodb不支持全文索引，而MyISAM支持全文索引（5.7以后的InnoDB也支持全文索引）

**6、** InnoDB支持表、行级锁，而MyISAM支持表级锁。

**7、** InnoDB表必须有主键，而MyISAM可以没有主键

**8、** Innodb表需要更多的内存和存储，而MyISAM可被压缩，存储空间较小，。

**9、** Innodb按主键大小有序插入，MyISAM记录插入顺序是，按记录插入顺序保存。

**10、** InnoDB 存储引擎提供了具有提交、回滚、崩溃恢复能力的事务安全，与 MyISAM 比 InnoDB 写的效率差一些，并且会占用更多的磁盘空间以保留数据和索引


### 8、视图有哪些特点？

**视图的特点如下:**

**1、** 视图的列可以来自不同的表，是表的抽象和在逻辑意义上建立的新关系。

**2、** 视图是由基本表(实表)产生的表(虚表)。

**3、** 视图的建立和删除不影响基本表。

**4、** 对视图内容的更新(添加，删除和修改)直接影响基本表。

**5、** 当视图来自多个基本表时，不允许添加和删除数据。

视图的操作包括创建视图，查看视图，删除视图和修改视图。


### 9、关心过业务系统里面的sql耗时吗？统计过慢查询吗？对慢查询都怎么优化过？

我们平时写Sql时，都要养成用explain分析的习惯。

慢查询的统计，运维会定期统计给我们

**「优化慢查询：」**

**1、** 分析语句，是否加载了不必要的字段/数据。

**2、** 分析SQl执行句话，是否命中索引等。

**3、** 如果SQL很复杂，优化SQL结构

**4、** 如果表数据量太大，考虑分表


### 10、varchar(50)中50的涵义

**1、** 字段最多存放 50 个字符

**2、** 如 varchar(50) 和 varchar(200) 存储 "jay" 字符串所占空间是一样的，后者在排序时会消耗更多内存


### 11、MySQL有关权限的表都有哪几个？
### 12、MyISAM索引与InnoDB索引的区别？
### 13、Myql中的事务回滚机制概述
### 14、什么是存储过程？有哪些优缺点？
### 15、主从同步延迟的解决办法
### 16、实践中如何优化MySQL
### 17、日志的存放形式
### 18、完整性约束包括哪些？
### 19、MySQL中有哪几种锁？
### 20、聚簇索引和非聚簇索引，在查询数据的时候有区别吗？为什么？
### 21、优化UNION查询
### 22、如何在Unix和MySQL时间戳之间进行转换？
### 23、如果要存储用户的密码散列，应该使用什么字段进行存储？
### 24、试述视图的优点？
### 25、如何通俗地理解三个范式？
### 26、UNION与UNION ALL的区别？
### 27、myisamchk是用来做什么的？
### 28、什么情况下设置了索引但无法使用
### 29、数据库自增主键可能遇到什么问题。
### 30、索引能干什么?




## 全部答案，整理好了，直接下载吧

### 下载链接：[全部答案，整理好了](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin-2.png)




## 最新，高清PDF：172份，7701页，最新整理

[![大厂面试题](https://www.souyunku.com/wp-content/uploads/weixin/mst.png "架构师专栏")](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")

[![大厂面试题](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")](https://www.souyunku.com/wp-content/uploads/weixin/githup-weixin.png "架构师专栏")
