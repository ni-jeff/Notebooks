# 数据库
### 1. 事务四大特性（ACID）原子性、一致性、隔离性、持久性 
数据库事务 transanction 正确执行的四个基本要素。ACID,原子性(Atomicity)、一致性(Correspondence)、隔离性(Isolation)、持久性(Durability)。
1. 原子性：整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
2. 一致性：在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。
3. 隔离性：隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行 相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，  必须串行化或序列化请 求，使得在同一时间仅有一个请求用于同一数据。
4. 持久性：在事务完成以后，该事务所对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。
### 2. 数据库隔离级别，每个级别会引发什么问题，mysql默认是哪个级别 
1. Serializable (串行化)：可避免  脏读、不可重复读、幻读的发生。
2. Repeatable read (可重复读)：可避免  脏读、不可重复读的发生。
3. Read committed (读已提交)：可避免  脏读的发生。
4. Read uncommitted (读未提交)：最低级别，任何情况都无法保证。  
- 以上四种隔离级别最高的是Serializable级别，最低的是Read uncommitted级别，当然级别越高，执行效率就越低。像Serializable这样的级别，就是以锁表的方式(类似于Java多线程中的锁)使得其他的线程只能在锁外等待，所以平时选用何种隔离级别应该根据实际情况。  
- 在MySQL数据库中默认的隔离级别为Repeatable read (可重复读)。而在Oracle数据库中，只支持Serializable (串行化)级别和Read committed (读已提交)这两种级别，其中默认的为Read committed级别。
### 3. innodb和myisam存储引擎的区别 
1. InnoDB 支持事务，MyISAM 不支持事务。这是 MySQL 将默认存储引擎从 MyISAM 变成 InnoDB 的重要原因之一；
2. InnoDB 支持外键，而 MyISAM 不支持。对一个包含外键的 InnoDB 表转为 MYISAM 会失败；  
3. InnoDB 是聚集索引，MyISAM 是非聚集索引。聚簇索引的文件存放在主键索引的叶子节点上，因此 InnoDB 必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。而 MyISAM 是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。 
4. InnoDB 不保存表的具体行数，执行 select count(*) from table 时需要全表扫描。而MyISAM 用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快；    
5. InnoDB 最小的锁粒度是行锁，MyISAM 最小的锁粒度是表锁。一个更新语句会锁住整张表，导致其他查询和更新都会被阻塞，因此并发访问受限。这也是 MySQL 将默认存储引擎从 MyISAM 变成 InnoDB 的重要原因之一；
6. Innodb不支持全文索引，而MyISAM支持全文索引，查询效率上MyISAM要高
### 4. MYSQL的两种存储引擎区别（事务、锁级别等等），各自的适用场景
1. 是否要支持事务，如果要请选择 InnoDB，如果不需要可以考虑 MyISAM；
2. 如果表中绝大多数都只是读查询，可以考虑 MyISAM，如果既有读写也挺频繁，请使用InnoDB。
3. 系统奔溃后，MyISAM恢复起来更困难，能否接受，不能接受就选 InnoDB；
### 5. 查询语句不同元素（where、jion、limit、group by、having等等）执行先后顺序
- 查询中用到的关键词主要包含六个，并且他们的顺序依次为 select--from--where--group by--having--order by
其中select和from是必须的，其他关键词是可选的，这六个关键词的执行顺序 与sql语句的书写顺序并不是一样的，而是按照下面的顺序来执行:
1. from:需要从哪个数据表检索数据
2. where:过滤表中数据的条件
3. group by:如何将上面过滤出的数据分组
4. having:对上面已经分组的数据进行过滤的条件
5. select:查看结果集中的哪个列，或列的计算结果
6. order by :按照什么样的顺序来查看返回的数据
- from后面的表关联，是自右向左解析 而where条件的解析顺序是自下而上的。  
  也就是说，在写SQL语句的时候，尽量把数据量小的表放在最右边来进行关联（用小表去匹配大表），而把能筛选出小量数据的条件放在where语句的最左边 （用小表去匹配大表）
### 6. 数据库的优化（从sql语句优化和索引两个部分回答）
[Reference](https://tech.meituan.com/2014/06/30/mysql-index.html)  
1. 索引优化
    1. 建立聚集索引
    2. 常查询数据建立索引
    3. 最左前缀原则  
对于多列索引，总是从索引的最前面字段开始，接着往后，中间不能跳过。比如创建了多列索引(name,age,sex)，会先匹配name字段，再匹配age字段，再匹配sex字段的，中间不能跳过。
    4. 不要建立无意义的索引
    5. 尽量选择区分度高的列作为索引。
1. SQL语句优化
    1. 查询语句尽可能使用索引
    2. 尽量减少子查询，使用关联查询（left join,right join,inner join）替代
    3. 可以使用 exist 和not exist代替 in和not in；对连续数值可以使用between；
    4. 尽量不要在where子句中对字段进行函数操作或计算操作；
    5. 尽量不要在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。
    6. 尽量不要在 where 子句中对字段进行 null 值判断；
    7. 尽量不要使用select * from table这种方式；把要查询的具体字段列出来，不要返回任何用不到的字段；
    8. 尽可能的使用 varchar/nvarchar 代替 char/nchar ，因为首先变长字段存储空间小，可以节省存储空间；
    9. or 的查询尽量用 union或者union all 代替(在确认没有重复数据或者不用剔除重复数据时，union all会更好)
### 7. 索引有B+索引和hash索引，各自的区别 
1. Hash索引仅仅能满足"=","IN"和"<=>"查询，不能使用范围查询。因为经过相应的Hash算法处理之后的Hash值的大小关系，并不能保证和Hash运算前完全一样；

2. Hash索引无法被用来避免数据的排序操作。因为Hash值的大小关系并不一定和Hash运算前的键值完全一样；

3. Hash索引不能利用部分索引键查询。对于组合索引，Hash索引在计算Hash值的时候是组合索引键合并后再一起计算Hash值，而不是单独计算Hash值，所以通过组合索引的前面一个或几个索引键进行查询的时候，Hash索引也无法被利用；

4. Hash索引在任何时候都不能避免表扫描。由于不同索引键存在相同Hash值，所以即使取满足某个Hash键值的数据的记录条数，也无法从Hash索引中直接完成查询，还是要回表查询数据； 

5. Hash索引遇到大量Hash值相等的情况后性能并不一定就会比B+树索引高。
### 8. B+索引数据结构，和B树的区别 
1. 有k个子结点的结点必然有k个关键码。
2. 非叶结点仅具有索引作用，跟记录有关的信息均存放在叶结点中。
3. 树的所有叶结点构成一个有序链表，可以按照关键码排序的次序遍历全部记录。

1.  B+树的磁盘读写代价更低
2.  B+树的查询效率更加稳定
3.  B+树更适合基于范围的查询
### 9. 索引的分类（主键索引、唯一索引），最左前缀原则，哪些情况索引会失效
- 索引的分类  
  - 从物理结构上
    1. 聚集索引：索引的键值的逻辑顺序决定了表中相应行的物理顺序。  
    2. 非聚集索引：非聚集索引通过索引记录地址访问表中的数据。索引的逻辑顺序和表中行的物理存储顺序不同。
  - 从内容上分  
    1. 普通索引：是最基本的索引，它没有任何限制。
    2. 主键索引：是一种特殊的唯一索引，一个表只能有一个主键，不允许有空值。一般是在建表的时候同时创建主键索引。
    3. 唯一索引：与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。
    4. 组合索引：指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。
    5. 全文索引：主要用来查找文本中的关键字，而不是直接与索引中的值相比较。fulltext索引跟其它索引大不相同，它更像是一个搜索引擎，而不是简单的where语句的参数匹配。fulltext索引配合match against操作使用，而不是一般的where语句加like。它可以在create table，alter table ，create index使用，不过目前只有char、varchar，text 列上可以创建全文索引。值得一提的是，在数据量较大时候，现将数据放入一个没有全局索引的表中，然后再用CREATE index创建fulltext索引，要比先为一张表建立fulltext然后再将数据写入的速度快很多。
  - 从数据结构上分
    1. 哈希索引
    2. B+树索引
- 最左前缀原则   
对于多列索引，总是从索引的最前面字段开始，接着往后，中间不能跳过。比如创建了多列索引(name,age,sex)，会先匹配name字段，再匹配age字段，再匹配sex字段的，中间不能跳过。
- 索引失效  
![索引失效](https://github.com/ni-jeff/Notebooks/blob/main/Interview/Java_Backend/index_not_match.png)
### 10. 聚集索引和非聚集索引区别。
1. 聚集索引：索引的键值的逻辑顺序决定了表中相应行的物理顺序。  
2. 非聚集索引：非聚集索引通过索引记录地址访问表中的数据。索引的逻辑顺序和表中行的物理存储顺序不同。
### 11. 有哪些锁（乐观锁悲观锁），select时怎么加排它锁 
- 有哪些锁  
  - 悲观锁：  
    顾名思义，很悲观，就是每次拿数据的时候都认为别的线程会修改数据，所以在每次拿的时候都会给数据上锁。上锁之后，当别的线程想要拿数据时，就会阻塞，直到给数据上锁的线程将事务提交或者回滚。传统的关系型数据库里就用到了很多这种锁机制，比如行锁，表锁，共享锁，排他锁等，都是在做操作之前先上锁。
    - 行锁：
    - 表锁：
    - 页锁：
    - 共享锁：  
    共享锁又称为读锁，一个线程给数据加上共享锁后，其他线程只能读数据，不能修改。
    - 排他锁：  
    排他锁又称为写锁，和共享锁的区别在于，其他线程既不能读也不能修改。
  - 乐观锁：  
  乐观锁其实不会上锁。顾名思义，很乐观，它默认别的线程不会修改数据，所以不会上锁。只是在更新前去判断别的线程在此期间有没有修改数据，如果修改了，会交给业务层去处理。
- select时怎么加排它锁  
```select * from table where id = ? for update```
### 12. 关系型数据库和非关系型数据库区别 
- 关系型数据库：指采用了关系模型来组织数据的数据库。关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。  
- 非关系型数据库：指非关系型的，分布式的，且一般不保证遵循ACID原则的数据存储系统。  
  - 非关系型数据库结构  
非关系型数据库以键值对存储，且结构不固定，每一个元组可以有不一样的字段，每个元组可以根据需要增加一些自己的键值对，不局限于固定的结构，可以减少一些时间和空间的开销。
  - 优点
    1. 用户可以根据需要去添加自己需要的字段，为了获取用户的不同信息，不像关系型数据库中，要对多表进行关联查询。仅需要根据key取出相应的value就可以完成查询。
    2. 适用于SNS(Social Networking Services)中，例如facebook，微博。系统的升级，功能的增加，往往意味着数据结构巨大变动，这一点关系型数据库难以应付，需要新的结构化数据存储。由于不可能用一种数据结构化存储应付所有的新的需求，因此，非关系型数据库严格上不是一种数据库，应该是一种数据结构化存储方法的集合。
- 对比  
  1. 成本：Nosql数据库简单易部署，基本都是开源软件，不需要像使用Oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
  2. 查询速度：Nosql数据库将数据存储于缓存之中，而且不需要经过SQL层的解析，关系型数据库将数据存储在硬盘中，自然查询速度远不及Nosql数据库。
  3. 存储数据的格式：Nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
  4. 扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。Nosql基于键值对，数据之间没有耦合性，所以非常容易水平扩展。
  5. 持久存储：Nosql不使用于持久存储，海量数据的持久存储，还是需要关系型数据库
  6. 数据一致性：非关系型数据库一般强调的是数据最终一致性，不像关系型数据库一样强调数据的强一致性，从非关系型数据库中读到的有可能还是处于一个中间态的数据，
  7. Nosql不提供对事务的处理。
### 13. 数据库三范式，根据某个场景设计数据表（可以通过手绘ER图） 
1. 第一范式：原子性
2. 第二范式：属性完全依赖于主键
3. 第三范式：不存在对非主键列的传递依赖
### 14. 数据库的读写分离、主从复制
- 目的：为了提高数据库的并发性能 & 保证MySQL服务的高可用
- 原理：  
  在数据库集群架构中，让主库负责处理事务性查询，而从库只负责select查询，让两者分工明确达到提高数据库整体读写能力。  
  MySQL没有自带读写分离，必须依靠第三方中间件或者使用Spring实现读写分离。
### 15. 使用explain优化sql和索引
- 语法：explain select … from … [where …]
- 关键字：
  1. id：这是SELECT的查询序列号
  2. select_type：select_type就是select的类型
  3. table：显示这一行的数据是关于哪张表的
  4. **type**：这列最重要，显示了连接使用了哪种类别,有无使用索引，是使用Explain命令分析性能瓶颈的关键项之一。
  5. possible_keys：列指出MySQL能使用哪个索引在该表中找到行
  6. key：显示MySQL实际决定使用的键（索引）。如果没有选择索引，键是NULL
  7. key_len：显示MySQL决定使用的键长度。如果键是NULL，则长度为NULL。使用的索引的长度。在不损失精确性的情况下，长度越短越好
  8. ref：显示使用哪个列或常数与key一起从表中选择行。
  9. rows：显示MySQL认为它执行查询时必须检查的行数。
  10. Extra：包含MySQL解决查询的详细信息，也是关键参考项之一。
### 16. long_query怎么解决 
- 慢SQL一般有以下特征：
  1. 数据库CPU负载高。一般是查询语句中有很多计算逻辑，导致数据库cpu负载。
  2. IO负载高导致服务器卡住。这个一般和全表查询没索引有关系。
  3. 查询语句正常，索引正常但是还是慢。如果表面上索引正常，但是查询慢，需要看看是否索引没有生效。
- 优化基本步骤：
  1.  先运行看看是否真的很慢，注意设置SQL_NO_CACHE
  2. where条件单表查，锁定最小返回记录表。这句话的意思是把查询语句的where都应用到表中返回的记录数最小的表开始查起，单表每个字段分别查询，看哪个字段的区分度最高
  3. explain查看执行计划，是否与1预期一致（从锁定记录较少的表开始查询）
  4. order by limit 形式的sql语句让排序的表优先查
  5. 了解业务方使用场景
  6. 加索引时参照建索引的几大原则
  7. 观察结果，不符合预期继续从1开始分析
### 17. 内连接、外连接、交叉连接、笛卡儿积等  
1. 内连接（inner join）：取得两张表中满足存在连接匹配关系的记录。  
MySQL语法：左表 join 右表 on 匹配条件
2. 外连接（outer join）：取得两张表中满足存在连接匹配关系的记录，以及某张表（或两张表）中不满足匹配关系的记录。具体分为：左外连接，右外连接，全外连接。  
     1. 左外连（left outer join）：除显示两表满足匹配关系的记录，还显示左边表不满住匹配关系的记录；  
     MySQL语法：左表 left join 右表 on 匹配条件  
     1. 右外接（right outer join）：除显示两表满足匹配关系的记录，还显示右边表不满住匹配关系的记录；  
     MySQL语法：左表 right join 右表 on 匹配条件  
     1. 全外连（full outer join）：除显示两表满足匹配关系的记录，还显示左右两边表不满住匹配关系的记录；  
     MySQL语法：MySQL不支持全外连接语法，可以用一条左外语句union一条右外语句得到同样效果。  
3. 交叉连接/笛卡尔积（cross join）：显示两张表所有记录的一一对应，没有匹配关系进行筛选。  
MySQL语法：左表 join 右表 或 左表,右表
### 18. 死锁判定原理和具体场景，死锁怎么解决  
[Reference](https://blog.csdn.net/XiaHeShun/article/details/81393796)  
- 锁的种类
  | 锁种类 | 开销           | 死锁 | 颗粒度 | 锁冲突   | 并发度 |
  | :----- | :------------- | :--- | :----- | :------- | :----- |
  | 行级锁 | 开销大，加锁慢 | 会   | 最小   | 概率最低 | 最高   |
  | 页级锁 | 中等           | 会   | 中等   | 中等     | 中等   |
  | 表级锁 | 开销小，加锁快 | 不会 | 大     | 概率最高 | 最低   |


  在数据库实现资源锁定的过程中，随着锁定资源颗粒度的减小，锁定相同数据量的数据所需要消耗的内存数量是越来越多的，实现算法也会越来越复杂。不过，随着锁定资源颗粒度的减小，应用程序的访问请求遇到锁等待的可能性也会随之降低，系统整体并发度也随之提升。  
- 发生情况  
  1. 事务之间对资源访问顺序的交替  
   此类常见于两个用户分别首先访问A表和B表，并锁住当前表，但是双方都需要访问对方锁住的表，都在等待对方释放锁，也就是经典的刀叉问题。  
   像这样的死锁，基本上就是程序员的编程逻辑出现了问题，需要调整程序的逻辑。
  2. 并发修改同一记录  
   此类常见于用户A查询一条纪录，然后修改该条纪录；这时用户B修改该条纪录，这时用户A的事务里锁的性质由查询的共享锁企图上升到独占锁，而用户B里的独占锁由于A有共享锁存在所以必须等A释放掉共享锁，而A由于B的独占锁而无法上升的独占锁也就不可能释放共享锁，于是出现了死锁。这种死锁由于比较隐蔽，但在稍大点的项目中经常发生。  
   像这样的解决办法就是使用乐观锁，在表的字段中增加version字段，更新时进行查询version是否为当前取到version。乐观锁可以避免长事务中的数据库加锁开销。最不推荐的是采用悲观锁进行控制，悲观锁会使排队等待的时间相当漫长，会发生灾难性的后果。
  3. 索引不当导致全表扫描  
   此类常见于在事务中执行了一条不满足的语句，执行全表扫描，把行级锁上升为表级锁，多个这样的事务执行之后，就很容易产生死锁和阻塞。类似的情况还有当表中的数据量非常庞大而索引建的过少或不合适的时候，使得经常发生全表扫描，最终应用系统会越来越慢，最终发生阻塞或死锁。  
   解决其办法有，SQL语句中不要使用太复杂的关联多表的查询；使用“执行计划”对SQL语句进行分析，对于有全表扫描的SQL语句，建立相应的索引进行优化。
  4. 事务封锁范围大且相互等待  
   在同一数据库中并发执行多个需要长时间运行的事务时通常发生死锁。事务运行时间越长，其持有排它锁或更新锁的时间也就越长，从而堵塞了其它活动并可能导致死锁。  
   保持事务在一个批处理中，可以最小化事务的网络通信往返量，减少完成事务可能的延迟并释放锁。  
   确定事务是否能在更低的隔离级别上运行。执行提交读允许事务读取另一个事务已读取（未修改）的数据，而不必等待第一个事务完成。使用较低的隔离级别（例如提交读）而不使用较高的隔离级别（例如可串行读）可以缩短持有共享锁的时间，从而降低了锁定争夺
### 19. varchar和char的使用场景。 

||char(32)|varchar(32)|
|:--|:--|:--|
|占用空间|固定32字符（如果数据长度不够32将用空格补齐）|跟随实际存储内容长度，但不超过32|
|空格处理|检索时会去掉尾部空格（数据本身有空白符也会被去掉）|不会对空格处理|
|是否记录字段长度|否|是。额外拿出1或2个额外字节记录字段数据长度（字符数）|
|性能||在update时时可能使行变得更长，需要额外的工作，容易产生碎片|
|适用场景|存储的数据长度基本一致，不需要空格，eg 手机号、UUID、密码加密后的密文|数据长度不一定，长度范围变化较大的场景；字符串列的最大长度比平均长度大很多；列的更新很少，所以碎片不是问题；使用了像UTF-8这样复杂的字符集，每个字符都使用不同的字节数进行存储|
### 20. mysql并发情况下怎么解决（通过事务、隔离级别、锁） 
- 隔离
- 锁
- MVCC  
  多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的 InnoDB 存储引擎实现隔离级别的一种具体方式，用于实现提交读和可重复读这两种隔离级别。
### 21. 数据库崩溃时事务的恢复机制（REDO日志和UNDO日志）
- UNDO Log  
  - 生成方式
    1. 用排他锁锁定该行
    2. 记录redo log
    3. 把该行修改前的值Copy到undo log，即上图中下面的行
    4. 修改当前行的值，填写事务编号，使回滚指针指向undo log中的修改前的行
  - 长度  
  如果undo log一直不删除，则会通过当前记录的回滚指针回溯到该行创建时的初始内容，所幸的时在Innodb中存在purge线程，它会查询那些比现在最老的活动事务还早的undo log，并删除它们，从而保证undo log文件不至于无限增长。  
  - 回滚方式  
  回滚时把insert undo log丢弃即可，而update undo log则根据当前回滚指针从undo log中找出事务修改前的版本。
- REDO LOG
  - 作用  
  Redo log的主要作用是用于数据库的崩溃恢复（事务的持久性）
  - 生成方式
    1. 先将原始数据从磁盘中读入内存中来，修改数据的内存拷贝
    2. 生成一条重做日志并写入redo log buffer，记录的是数据被修改后的值
    3. 当事务commit时，将redo log buffer中的内容刷新到 redo log file，对 redo log file采用追加写的方式
    4. 定期将内存中修改的数据刷新到磁盘中

- 对比  
  undo log是逻辑日志，对事务回滚时，只是将数据库逻辑地恢复到原来的样子，而redo log是物理日志，记录的是数据页的物理变化