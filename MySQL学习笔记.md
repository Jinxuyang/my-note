---

title: MySQL学习笔记
date: 2020-05-06 18:42:04
tags:
	- mysql
	- 学习笔记
categories: 
	- 数据库
---

[TOC]



### 登录登出



```bash
mysql -u用户名 -p密码 -P端口号 -h服务器名称
```

```bash
mysql > exit;退出
```

<!--more-->

### 修改MySQL提示符

登陆时

```bash
mysql -u用户名 -p密码 -P端口号 -h服务器名称 --prompt 提示符
```

登陆后

```mysql
prompt 提示符
```

```
\D完整日期 \d当前数据库 \h服务器名称 \u当前用户
```

### MySQL常用命令

- 关键字与函数名全部大写
- 数据库名，表名称，字段名称全部小写
- MySQL语句必须以分号结尾

### 创建数据库

```mysql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name 
```







### 数据库三范式

#### 第一范式：列不可再分

如果每列都是不可再分的最小数据单元（也叫作最小的原子单元），则满足第一范式

#### 第二范式：属性完全依赖于主键

第二范式在第一范式的基础之上更进一层。第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。也就是说在一个数据库表中，一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。

#### 第三范式：属性不依赖于其它非主属性

满足第一范式和第二范式，并且表中的列不存在对非主键列的传递依赖。



三大范式只是一般设计数据库的基本理念，可以建立冗余较小、结构合理的数据库。如果有特殊情况，当然要特殊对待，数据库设计最重要的是看需求跟性能，需求>性能>表结构。所以不能一味的去追求范式建立数据库。

# MySQL

## MySQL基本架构

![img](D:\Note\MySQL学习笔记.assets\0PF]]6FBO3]W7L1CH_`550N.png)



MySQL 可以分为 Server 层和存储引擎层两部分

Server层涵盖MySQL大多数核心服务，以及所有内置函数

存储引擎负责数据的存储和提取。插件式。支持InnoDB，MyISAM，Memory

从MySQL 5.5.5开始InnoDB成为默认存储引擎

### 连接器

连接器负责与客户端**建立连接、获取权限、维持和管理连接**

```bash
mysql -h$ip -P$port -u$user -p
```

命令中的mysql就是客户端工具，用户密码认证通过后连接器会到权限表里面查出你拥有的权限。之后，这个连接里面的权限判断逻辑，都将依赖于此时读到的权限。

连接完成后，如果你没有后续的动作，这个连接就处于空闲状态，可以在 show processlist 命令中看到它。

![img](data:image/png;base64,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)

客户端如果太长时间没动静，连接器就会自动将它断开。这个时间是由参数 wait_timeout 控制的，默认值是 8 小时。

数据库里面，长连接是指连接成功后，如果客户端持续有请求，则一直使用同一个连接。短连接则是指每次执行完很少的几次查询就断开连接，下次查询再重新建立一个。

建立连接的过程通常是比较复杂的，在使用中要尽量减少建立连接的动作，也就是尽量使用长连接。

MySQL 在执行过程中临时使用的内存是管理在连接对象里面的。因为使用长连接，会导致MySQL内存占用变大，这些被占用的内存会在连接断开的时候才能释放。

怎么解决这个问题呢？

1. 定期断开长连接。使用一段时间，或者程序里面判断执行过一个占用内存的大查询后，断开连接，之后要查询再重连。
2. 如果你用的是 MySQL 5.7 或更新版本，可以在每次执行一个比较大的操作后，通过执行 mysql_reset_connection 来重新初始化连接资源。这个过程不需要重连和重新做权限验证，但是会将连接恢复到刚刚创建完时的状态。

### 查询缓存

MySQL 拿到一个查询请求后，会先到查询缓存看看，之前是不是执行过这条语句。之前执行过的语句及其结果可能会以 key-value 对的形式，被直接缓存在内存中。key 是查询的语句，value 是查询的结果。如果你的查询能够直接在这个缓存中找到 key，那么这个 value 就会被直接返回给客户端。

如果语句不在查询缓存中，就会继续后面的执行阶段。执行完成后，执行结果会被存入查询缓存中。

**缺点** 查询缓存失效快，只要有对一个表的更新，这个表上的所有查询缓存都会被清空。对于更新压力大的数据库来说，查询缓存的命中率会非常低。

**MySQL 8.0 版本直接将查询缓存的整块功能删掉了**

### 分析器

分析器先会做“词法分析”。

比如

```sql
select id from user;
```

MySQL 从你输入的"select"这个关键字识别出来，这是一个查询语句。它也要把字符串“user”识别成“表名 user”，把字符串“id”识别成“列 ID”。

做完了这些识别以后，就要做“语法分析”。根据词法分析的结果，语法分析器会根据语法规则，判断你输入的这个 SQL 语句是否满足 MySQL 语法。

### 优化器

经过了分析器，MySQL 就知道你要做什么了

重写查询、决定表的读取顺序、选择合适的索引

优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联（join）的时候，决定各个表的连接顺序

### 执行器

开始执行的时候，要先判断一下你对这个表 T 有没有执行查询的权限

如果命中查询缓存，会在查询缓存返回结果的时候，做权限验证。查询也会在优化器之前调用 precheck 验证权限

如果有权限，就打开表继续执行。打开表的时候，执行器就会根据表的引擎定义，去使用这个引擎提供的接口。

## 存储引擎

常用的存储引擎主要有MyISAM、InnoDB、Memory、Archive和Federated

### MyISAM

不支持数据库事务、行级锁和外键，因此在INSERT（插入）或UPDATE（更新）数据即写操作时需要锁定整个表，效率较低。

MyIASM的特点是执行读取操作的速度快，且占用的内存和存储资源较少。它在设计之初就假设数据被组织成固定长度的记录，并且是按顺序存储的。在查找数据时，MyIASM直接查找文件的OFFSET，定位比InnoDB要快（InnoDB寻址时要先映射到块，再映射到行）。总体来说，MyIASM的缺点是更新数据慢且不支持事务处理，优点是查询速度快。

### InnoDB

InnoDB为MySQL提供了事务（Transaction）支持、回滚（Rollback）、崩溃修复能力（Crash Recovery Capabilities）、多版本并发控制（Multi-versionedConcurrency Control）、事务安全（Transaction-safe）的操作。

InnoDB的底层存储结构为B+树，B+树的每个节点都对应InnoDB的一个Page, Page大小是固定的，一般被设为16KB。其中，非叶子节点只有键值，叶子节点包含完整的数据

InnoDB适用于有以下需求的场景。

- 经常有数据更新的表，适合处理多重并发更新请求。

- 支持事务。
- 支持灾难恢复（通过bin-log日志等）。

- 支持外键约束，只有InnoDB支持外键。

- 支持自动增加列属性auto_increment。

### Memory

Memory表使用内存空间创建。每个Memory表实际上都对应一个磁盘文件用于持久化。Memory表因为数据是存放在内存中的，因此访问速度非常快，通常使用Hash索引来实现数据索引。Memory表的缺点是一旦服务关闭，表中的数据就会丢失。

## 日志

与查询流程不一样的是，更新流程涉及两个重要的日志模块，redo log（重做日志）和 binlog（归档日志）。

### redo log

如果每一次的更新操作都需要写进磁盘，然后磁盘也要找到对应的那条记录，然后再更新，整个过程 IO 成本、查找成本都很高。

解决：WAL（Write-Ahead Logging）预写日志 ，关键点就是先写日志，再写磁盘

具体来说，当有一条记录需要更新的时候，InnoDB 引擎就会先把记录写到 redo log里面，并更新内存，这个时候更新就算完成了。同时，InnoDB 引擎会在适当的时候，将这个操作记录更新到磁盘里面，而这个更新往往是在系统比较空闲的时候做

InnoDB 的 redo log 是固定大小的

从头开始写，写到末尾就又回到开头循环写，

![img](D:\Note\MySQL学习笔记.assets\M04B0XLGCBW7D@5JFK{[UJR.png)

write pos 是当前记录的位置，一边写一边后移，写到第 3 号文件末尾后就回到 0 号文件开头。checkpoint 是当前要擦除的位置，也是往后推移并且循环的，擦除记录前要把记录更新到数据文件。

write pos 和 checkpoint 之间的是还空着的部分，可以用来记录新的操作。如果 write pos 追上 checkpoint，表示redo log，这时候不能再执行新的更新，得停下来先擦掉一些记录，把 checkpoint 推进一下。

有了 redo log，InnoDB 就可以保证即使数据库发生异常重启，之前提交的记录都不会丢失，这个能力称为**crash-safe**。

### binlog

redo log 是 InnoDB 引擎特有的日志，而 Server 层也有自己的日志，称为 binlog（归档日志）

这两种日志有以下三点不同。

1. redo log 是 InnoDB 引擎特有的；binlog 是 MySQL 的 Server 层实现的，所有引擎都可以使用。
2. redo log 是物理日志，记录的是“在某个数据页上做了什么修改”；binlog 是逻辑日志，记录的是这个语句的原始逻辑，比如“给 ID=2 这一行的 c 字段加 1 ”。
3. redo log 是循环写的，空间固定会用完；binlog 是可以追加写入的。“追加写”是指 binlog 文件写到一定大小后会切换到下一个，并不会覆盖以前的日志。 

## 数据库事务

数据库事务执行一系列基本操作，这些基本操作组成一个逻辑工作单元一起向数据库提交，要么都执行，要么都不执行。事务是一个不可分割的工作逻辑单元。

事务必须具备以下4个属性，简称ACID属性。

- 原子性（Atomicity）：事务是一个完整操作，参与事务的逻辑单元要么都执行，要么都不执行。

- 一致性（Consistency）：在事务执行完毕时（无论是正常执行完毕还是异常退出），数据都必须处于一致状态。

- 隔离性（Isolation）：对数据进行修改的所有并发事务都是彼此隔离的，它不应以任何方式依赖或影响其他事务。

- 永久性（Durability）：在事务操作完成后，对数据的修改将被持久化到永久性存储中。

## 事务隔离

ACID（Atomicity、Consistency、Isolation、Durability，即原子性、一致性、隔离性、持久性）

https://www.cnblogs.com/yubaolee/p/10398633.html

### 脏读、不可重复读、幻读

当数据库上有多个事务同时执行的时候，就可能出现脏读（dirty read）、不可重复读（non-repeatable read）、幻读（phantom read）的问题，为了解决这些问题，就有了“隔离级别”的概念。

A事务执行过程中，B事务读取了A事务的修改。但是由于某些原因，A事务可能没有完成提交，发生RollBack了操作，则B事务所读取的数据就会是不正确的。这个未提交数据就是脏读（Dirty Read）。

B事务读取了两次数据，在这两次的读取过程中A事务修改了数据，B事务的这两次读取出来的数据不一样。B事务这种读取的结果，即为不可重复读（Nonrepeatable Read）。

不可重复读有一种特殊情况，两个事务更新同一条数据资源，后完成的事务会造成先完成的事务更新丢失。这种情况就是大名鼎鼎的**第二类丢失更新**。主流的数据库已经默认屏蔽了第一类丢失更新问题（即：后做的事务撤销，发生回滚造成已完成事务的更新丢失），但我们编程的时候仍需要特别注意第二类丢失更新。

B事务读取了两次数据，在这两次的读取过程中A事务添加了数据，B事务的这两次读取出来的集合不一样，即为幻读（Phantom Read）

看起来和不可重复读差不多，但幻读强调的集合的增减，而不是单独一条数据的修改

### 隔离级别

**读未提交**是指，一个事务还没提交时，它做的变更就能被别的事务看到。

**读提交**是指，一个事务提交之后，它做的变更才会被其他事务看到。

**可重复读**是指，一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。当然在可重复读隔离级别下，未提交变更对其他事务也是不可见的。

**串行化**，顾名思义是对于同一行记录，“写”会加“写锁”，“读”会加“读锁”。当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。

## 索引 

排好序的快速查找数据结构 

索引的出现其实就是为了提高数据查询的效率，就像书的目录一样

- 索引大大减少了服务器需要扫描的数据量。

- 索引可以帮助服务器避免排序和临时表。

- 索引可以将随机I/O变为顺序I/O。

  

索引并不总是最好的工具。总的来说，只有当索引帮助存储引擎快速查找到记录带来的好处大于其带来的额外工作时，索引才是有效的。对于非常小的表，大部分情况下简单的全表扫描更高效。对于中到大型的表，索引就非常有效。但对于特大型的表，建立和使用索引的代价将随之增长。这种情况下，则需要-种技术可以直接区分出查询需要的一-组数据，而不是一条记录一条记录地匹配。例如可以使用分区
技术。

### 索引的常见模型

 #### 哈希表

哈希表是一种以键 - 值（key-value）存储数据的结构，我们只要输入待查找的值即 key，就可以找到其对应的值即 Value。

![1](D:\Note\MySQL学习笔记.assets\1.png)

图中四个 ID_card_n 的值并不是递增的，这样做的好处是增加新的 User 时速度会很快，只需要往后追加。但缺点是，因为不是有序的，所以哈希索引做区间查询的速度是很慢的。

**哈希表这种结构适用于只有等值查询的场景**

#### 有序数组

**有序数组在等值查询和范围查询场景中的性能就都非常优秀**

如果你要查 ID_card_n2 对应的名字，用二分法就可以快速得到，这个时间复杂度是 O(log(N))

你要查身份证号在 [ID_card_X, ID_card_Y] 区间的 User，可以先用二分法找到 ID_card_X（如果不存在 ID_card_X，就找到大于 ID_card_X 的第一个 User），然后向右遍历，直到查到第一个大于 ID_card_Y 的身份证号

![img](D:\Note\MySQL学习笔记.assets\ADYVJM}NBC_]JOB}74NQX}U.png)

但是，在需要更新数据的时候就麻烦了，你往中间插入一个记录就必须得挪动后面所有的记录，成本太高

**有序数组索引只适用于静态存储引擎**

#### 搜索树

![2](D:\Note\MySQL学习笔记.assets\2.png)

二叉搜索树的特点是：每个节点的左儿子小于父节点，父节点又小于右儿子。这样如果你要查 ID_card_n2 的话，按照图中的搜索顺序就是按照 UserA -> UserC -> UserF -> User2 这个路径得到。这个时间复杂度是 O(log(N))

为了维持 O(log(N)) 的查询复杂度，就需要保持这棵树是平衡二叉树。为了做这个保证，更新的时间复杂度也是 O(log(N))

二叉树是搜索效率最高的，但是实际上大多数的数据库存储却并不使用二叉树。其原因是，索引不止存在内存中，还要写到磁盘上

你可以想象一下一棵 100 万节点的平衡二叉树，树高 20。一次查询可能需要访问 20 个数据块。在机械硬盘时代，从磁盘随机读一个数据块需要 10 ms 左右的寻址时间。也就是说，对于一个 100 万行的表，如果使用二叉树来存储，单独访问一个行可能需要 20 个 10 ms 的时间。

为了让一个查询尽量少地读磁盘，就必须让查询过程访问尽量少的数据块。那么，我们就不应该使用二叉树，而是要使用“N 叉”树。这里，“N 叉”树中的“N”取决于数据块的大小。

以 InnoDB 的一个整数字段索引为例，这个 N 差不多是 1200。这棵树高是 4 的时候，就可以存 1200 的 3 次方个值，这已经 17 亿了。考虑到树根的数据块总是在内存中的，一个 10 亿行的表上一个整数字段的索引，查找一个值最多只需要访问 3 次磁盘。其实，树的第二层也有很大概率在内存中，那么访问磁盘的平均次数就更少了。

### B-Tree索引

MySQL的InnoDB引擎使用B+树（平衡多叉查找树，每有一个叶子节点都包含指向下一个叶子节点的指针）

B-Tree索引能够加快访问数据的速度，因为存储引擎不再需要进行全表扫描来获取需要的数据，取而代之的是从索引的根节点开始进行搜索。

通过比较节点页的值和要查找的值可以找到合适的指针进入下层子节点，这些指针实际上定义了子节点页中值的上限和下限。最终存储引擎要么是找到对应的值，要么该记录不存在。

叶子节点比较特别，它们的指针指向的是被索引的数据，而不是其他的节点页。

### 哈希索引

只有精确匹配索引所有列的查询才有效

对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码，哈希码是一个较小的值，不同键值的行计算出的哈希码也不同。哈希索引将所有的哈希码存储在索引中，同时在哈希表中保存指向每个数据行的指针

- 哈希索引只包含哈希值和行指针，而不存储字段值，所以不能使用索引中的值来避免读取行。不过，访问内存中的行的速度很快，所以大部分情况下这一点对性能的影响并不明显
- 哈希索引数据并不是按照索引值顺序存储的，所以也就无法用于排序。
- 哈希索引也不支持部分索引列匹配查找，因为哈希索引始终是使用索引列的全部内容来计算哈希值的。例如,在数据列(A,B) .上建立哈希索引,如果查询只有数据列A,则无法使用该索引。
- 哈希索引只支持等值比较查询，包括=. IN(). <=> (注意<>和<=>是不同的操作)。也不支持任何范围查询，例如WHERE price > 100.
- 访问哈希索引的数据非常快，除非有很多哈希冲突(不同的索引列值却有相同的哈希值)。当出现哈希冲突的时候，存储引擎必须遍历链表中所有的行指针，逐行进行比较，直到找到所有符合条件的行。
- 如果哈希冲突很多的话，一些索引维护操作的代价也会很高。例如，如果在某个选择性很低(哈希冲突很多)的列上建立哈希索引，那么当从表中删除一行时，存储引擎需要遍历对应哈希值的链表中的每- -行， 找到并删除对应行的引用，冲突越多，代价越大。

### 索引创建原则

- 选择唯一性索引：唯一性索引一般基于Hash算法实现，可以快速、唯一地定位某条数据。

- 为经常需要排序、分组和联合操作的字段建立索引。

- 为常作为查询条件的字段建立索引。

- 限制索引的数量：索引越多，数据更新表越慢，因为在数据更新时会不断计算和添加索引。

- 尽量使用数据量少的索引：如果索引的值很长，则占用的磁盘变大，查询速度会受到影响。

- 尽量使用前缀来索引：如果索引字段的值过长，则不但影响索引的大小，而且会降低索引的执行效率，这时需要使用字段的部分前缀来作为索引。

- 删除不再使用或者很少使用的索引。

- 尽量选择区分度高的列作为索引：区分度表示字段值不重复的比例。

- 索引列不能参与计算：带函数的查询不建议参与索引。

- 尽量扩展现有索引：联合索引的查询效率比多个独立索引高。

### InnoDB的索引模型

在 InnoDB 中，表都是根据主键顺序以索引的形式存放的，这种存储方式的表称为索引组织表

每一个索引在 InnoDB 里面对应一棵 B+ 树。

索引类型分为主键索引和非主键索引。

主键索引的叶子节点存的是整行数据。在 InnoDB 里，主键索引也被称为聚簇索引（clustered index）。

非主键索引的叶子节点内容是主键的值。在 InnoDB 里，非主键索引也被称为二级索引（secondary index）。

基于非主键索引的查询需要多扫描一棵索引树。

### 高性能索引策略

#### 独立的列

我们通常会看到以些查询不当地使用索引，或者使得MySQL无法使用已有的索引。如果查询中的列不是独立的，则MySQL就不会使用索引。‘‘独立的列” 是指索引列不能是表达式的一部分，也不能是函数的参数。
例如，下面这个查询无法使用actor_ id 列的索引:

```sql
mysq1> SELECT actor_ id FROM sakila.actor WHERE actor. _id + 1 = 5;
```


凭肉眼很容易看出WHERE中的表达式其实等价于actor_ id = 4， 但是MySQL无法自动解析这个方程式。这完全是用户行为。我们应该养成简化WHERE条件的习惯，始终将索引列单独放在比较符号的一侧。

#### 前缀索引和索引的选择性

有时候需要索引很长的字符列，这回导致索引变得大且慢，一个方法是使用哈希索引，那还可以做什么呢？

可以索引开始的部分字符，这样可以大大节约索引空间，提高索引效率，但也会降低索引的选择性（索引的选择性值，不重复的索引值和数据表的数据总数的比值），索引的选择性越高查询效率越高，唯一索引的选择性是1，这是做好的索引选择行，性能也是最好的。

前缀的长度要足够长保证较高的选择性同时又不能太长。

前缀索引是一种能是索引更小、更快的有效办法，但MySQL无法使用前缀索引做ORDER BY和GROUOP BY， 也无法使用前缀索引做覆盖扫描

创建前缀索引

```sql
ALTER TABLE xxx.xx ADD KEY (x(7))
```

#### 联合索引（多列索引）

为每个列都创建独立的索引或按章错误的顺序创建多列索引是常见的错误。

在多个列上创建独立的索引大部分情况下不能提高MySQL的查询性能。但MySQL5.0和更新版引入了“索引合并（index merge）”的策略，一定程度上可以使用表上的多个单列索引来定位指定的行，查询能够同时使用这两个单列索引进行扫描，并将结果进行合并。这种算法有三个变种: OR条件的联合(union) ,AND条件的相交(intersection),组合前两种情况的联合及相交。

索引合并册罗有时候是一种优化的结果，但实际上更多时候说明表上的索引建的很糟糕：

- 当出现服务器对多个索引做相交操作时(通常有多个AND条件)，通常意味着需要一个包含所有相关列的多列索引，而不是多个独立的单列索引。
- 当服务器需要对多个索引做联合操作时(通常有多个OR条件)，通常需要耗费大量CPU和内存资源在算法的缓存、排序和合并操作上。特别是当其中有些索引的选择性不高，需要合并扫描返回的大量数据的时候。
- 更重要的是，优化器不会把这些计算到“查询成本”(cost) 中，优化器只关心随机页面读取。这会使得查询的成本被“低估”,导致该执行计划还不如直接走全表扫描。这样做不但会消耗更多的CPU和内存资源，还可能会影响查询的并发性，但如果是单独运行这样的查询则往往会忽略对并发性的影响。通常来说，还不如像在MySQL4.1或者更早的时代一样，将查询改写成UNION的方式往往更好。



索引的顺序（待续）

#### 聚簇索引

聚簇编故事数据行和相邻的键值紧凑地存储在一起、

聚簇索引就是数据行实际存放在索引的叶子页中

因为无法同时吧数据行存放在两个不同的地方，所以一个表只能有一个聚簇索引（覆盖索引可以模拟多个聚簇索引的情况）

索引有存储引擎实现，所以不是所有引擎都支持聚簇索引（InnoDB支持聚簇索引）

注意：叶子页包含行的全部数据，但节点页只包含索引列



一些数据库服务器（不知道是什么东西）允许选择那个索引作为聚簇所引，目前MySQL内建的存储引擎还不支持。InnoDB默认通过主键聚集数据。



如果没有定义主键，InnoDB会选择一个唯一的非空索引代替。如果没有这样的索引，InnoDB会隐式定义一个主键来作为聚簇索引。

优点



- 可以把相关数据保存在一起。例如实现电子邮箱时，可以根据用户ID来聚集数据,这样只需要从磁盘读取少数的数据页就能获取某个用户的全部邮件。如果没有使用聚簇索引，则每封邮件都可能导致一次磁盘I/O。
- 数据访问更快。聚簇索引将索引和数据保存在同一个B-Tree中，因此从聚簇索引中获取数据通常比在非聚簇索引中查找要快。
- 使用覆盖索引扫描的查询可以直接使用页节点中的主键值。

缺点:

- 聚簇数据最大限度地提高了I/O密集型应用的性能，但如果数据全部都放在内存中,则访问的顺序就没那么重要了，聚簇索引也就没什么优势了。
- 插入速度严重依赖于插入顺序。按照主键的顺序插入是加载数据到InnoDB表中速度最快的方式。但如果不是按照主键顺序加载数据，那么在加载完成后最好使用0PTIMIZE TABLE命令重新组织一下表。
- 更新聚簇索引列的代价很高，因为会强制InnoDB将每个被更新的行移动到新的位置。
- 基于聚簇索引的表在插入新行，或者主键被更新导致需要移动行的时候，可能面临“页分裂(page split)”的问题。当行的主键值要求必须将这一行插入到某个已满的页中时，存储引擎会将该页分裂成两个页面来容纳该行，这就是一次页分裂操作。页分裂会导致表占用更多的磁盘空间。
- 聚簇索引可能导致全表扫描变慢，尤其是行比较稀疏，或者由于页分裂导致数据存储不连续的时候。
- 二级索引(非聚簇索引)可能比想象的要更大，因为在二级索引的叶子节点包含了引用行的主键列
- 二级索引访问需要两次索引查找



## 锁

数据库锁设计的初衷是处理并发问题。作为多用户共享的资源，当出现并发访问的时候，数据库需要合理地控制资源的访问规则。

**根据加锁的范围，MySQL 里面的锁大致可以分成全局锁、表级锁和行锁三类**。

### 全局锁

全局锁就是对整个数据库实例加锁。

MySQL 提供了一个加全局读锁的方法，命令是 

```sql
Flush tables with read lock 
```

加锁之后其他线程的以下语句会被阻塞：数据更新语句（数据的增删改）、数据定义语句（包括建表、修改表结构等）和更新类事务的提交语句

**全局锁的典型使用场景是，做全库逻辑备份**。也就是把整库每个表都 select 出来存成文本。

为什么备份需要加锁呢？

假设备份期间，有一个用户，购买了一门课程，业务逻辑里就要扣掉他的余额，然后往已购课程里面加上一门课

如果时间顺序上是先备份账户余额表 (u_account)，然后用户购买，然后备份用户课程表 (u_course)，就会导致用户 A 的数据状态是“账户余额没扣，但是用户课程表里面已经多了一门课”。

也就是说，不加锁的话，备份系统备份的得到的库不是一个逻辑时间点，这个视图是逻辑不一致的。



我们可以在**可重复读**这个隔离级别下开启事务从而拿到一致性的视图。

官方自带的逻辑备份工具是 mysqldump。当 mysqldump 使用参数–single-transaction 的时候，导数据之前就会启动一个事务，来确保拿到一致性视图。而由于 MVCC 的支持，这个过程中数据是可以正常更新的。

那为什么还需要全局锁呢，因为可重复读这个隔离级别需要存储引擎支持，MyISAM就不支持，**single-transaction 方法只适用于所有的表使用事务引擎的库**

### 表级锁（？？？）

表级锁指对当前操作的整张表加锁，它的实现简单，资源消耗较少，被大部分存储引擎支持。最常使用的MyISAM与InnoDB都支持表级锁定。

MySQL 里面表级别的锁有两种：一种是表锁，一种是元数据锁（meta data lock，MDL)。

**表锁的语法是 lock tables … read/write**

当对一个表做增删改查操作的时候，加 MDL 读锁；当要对表做结构变更操作的时候，加 MDL 写锁

- 读锁之间不互斥，因此你可以有多个线程同时对一张表增删改查。
- 读写锁之间、写锁之间是互斥的，用来保证变更表结构操作的安全性。因此，如果有两个线程要同时给一个表加字段，其中一个要等另一个执行完才能开始执行。

### 行锁

在执行以下数据库操作时，数据库会自动应用行级锁。

- INSERT、UPDATE、DELETE、SELECT … FOR UPDATE \[OF columns\] \[WAIT n| NOWAIT]。
- SELECT … FOR UPDATE语句允许用户一次针对多条记录执行更新

**在 InnoDB 事务中，行锁是在需要的时候才加上的，但并不是不需要了就立刻释放，而是要等到事务结束时才释放。**

### 死锁

当并发系统中不同线程出现循环资源依赖，涉及的线程都在等待别的线程释放资源时，就会导致这几个线程都进入无限等待的状态，称为死锁

![img](D:\Note\MySQL学习笔记.assets\ZA]XB{UGT5XUI[TVOI6%U0M.png)

这时候，事务 A 在等待事务 B 释放 id=2 的行锁，而事务 B 在等待事务 A 释放 id=1 的行锁。 事务 A 和事务 B 在互相等待对方的资源释放，就是进入了死锁状态。

- 一种策略是，直接进入等待，直到超时。这个超时时间可以通过参数 innodb_lock_wait_timeout 来设置。
- 另一种策略是，发起死锁检测，发现死锁后，将持有最小行级排他锁的事务进行回滚，让其他事务得以继续执行。将参数 innodb_deadlock_detect 设置为 on，表示开启这个逻辑。

但对于第二种方法来说

每当一个事务被锁的时候，就要看看它所依赖的线程有没有被别人锁住，如此循环，最后判断是否出现了循环等待，也就是死锁。

那如果是我们上面说到的所有事务都要更新同一行的场景呢？

每个新来的被堵住的线程，都要判断会不会由于自己的加入导致了死锁，这是一个时间复杂度是 O(n) 的操作。假设有 1000 个并发线程要同时更新同一行，那么死锁检测操作就是 100 万这个量级的。虽然最终检测的结果是没有死锁，但是这期间要消耗大量的 CPU 资源。因此，你就会看到 CPU 利用率很高，但是每秒却执行不了几个事务。

## 优化

### 数据类型

- 更小的通常更好

  尽量使用可以正确存储数据的最小数据类型，但要确保没有低估需要存储的值的范围

- 简单就好

  简单数据类型的操作通常需要更少的CPU周期，例如整形比字符串操作的代价更低，

  因为字符集和校对规则(排序规则)使字符比较比整型比较更复杂。这里有两个例子: 一个是应该使用MySQL内建的类型生而不是字符串来存储日期和时间，另外一个是应该用整型存储IP地址。

- 避免NULL

  最好指定NOT NULL 除非真的要存储NULL，因为如果查询中包含NULL的列，对MySQL来说更难优化，因为可为NULL的列使得索引、索引统计和值比较都更复杂。可为NULL的列会使用更多的存储空间，在MySQL中也需要特殊处理。当可为NULL的列被索引时，每个索引记录需要一个额外的字节，在MyISAM里甚至还可能导致固定大小的索引(例如只有一个整数列的索引)变成可变大小的索引。
  通常把可为NULL的列改为NOT NULL 带来的性能提升比较小，所以(调优时)没有必要首先在现有schema中查找并修改掉这种情况，除非确定这会导致问题。但是，如果计划在列上建索引，就应该尽量避免设计成可为NULL的列。当然也有例外，例如值得一提的是，InnoDB 使用单独的位(bit) 存储NULL值，所以对于稀疏数据（很多值为NULL，少数不为NULL）有很好的空间效率。但这一点不适用于MyISAM。

## Explain 执行计划

作用

查看：

- 表的读取顺序
- 数据读取操作的操作类型
- 那些索引可以使用
- 那些索引被实际引用
- 表之间的引用
- 每张表有多少行被优化器查询

explain + sql

```sql
mysql> explain select * from pms_category ;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: pms_category
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 1425
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```

**概要描述：**
	id:选择标识符
	select_type:表示查询的类型。
	table:输出结果集的表
	partitions:匹配的分区
	type:表示表的连接类型
	possible_keys:表示查询时，可能使用的索引
	key:表示实际使用的索引
	key_len:索引字段的长度
	ref:列与索引的比较
	rows:扫描出的行数(估算的行数)
	filtered:按表条件过滤的行百分比
	Extra:执行情况的描述和说明

### id

select 查询的序列号,包含一组数字，表示查询中执行 select 子句或操作表的顺序

- id相同，执行的顺序由上至下

- id 不同，如果是子查询，id的序号会递增,id越大越先执行

- 有相同也有不同

  id 如果相同，可以认为是一组，从上往下顺序执行；

  在所有组中，id 值越大，优先级越高

### select_type

(1) SIMPLE(简单SELECT，不使用UNION或子查询等)

(2) PRIMARY(子查询中最外层查询，查询中若包含任何复杂的子部分，最外层的select被标记为PRIMARY)

(3) UNION(UNION中的第二个或后面的SELECT语句,若UNION包含在FROM子句的子查询中,外层SELECT将被标记为：DERIVED)

(4) DEPENDENT UNION(UNION中的第二个或后面的SELECT语句，取决于外面的查询)

(5) UNION RESULT(UNION的结果，union语句中第二个select开始后面所有select)

(6) SUBQUERY(子查询中的第一个SELECT，结果不依赖于外部查询)

(7) DEPENDENT SUBQUERY(子查询中的第一个SELECT，依赖于外部查询)

(8) DERIVED(派生表的SELECT, FROM子句的子查询)

(9) UNCACHEABLE SUBQUERY(一个子查询的结果不能被缓存，必须重新评估外链接的第一行)

### table

显示这一步所访问数据库中表名称（显示这一行的数据是关于哪张表的），有时不是真实的表名字，可能是简称，例如上面的e，d，也可能是第几步执行的结果的简称

### type

对表访问方式，表示MySQL在表中找到所需行的方式，又称“访问类型”。

常用的类型有： **ALL、index、range、 ref、eq_ref、const、system、****NULL（从左到右，性能从差到好）**

ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行

index: Full Index Scan，index与ALL区别为index类型只遍历索引树

range:只检索给定范围的行，使用一个索引来选择行

ref: 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

eq_ref: 类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件

const、system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量，system是const类型的特例，当查询的表只有一行的情况下，使用system

NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。

### **possible_keys**

**指出MySQL能使用哪个索引在表中找到记录，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用（该查询可以利用的索引，如果没有任何索引显示 null）**

该列完全独立于EXPLAIN输出所示的表的次序。这意味着在possible_keys中的某些键实际上不能按生成的表次序使用。
如果该列是NULL，则没有相关的索引。在这种情况下，可以通过检查WHERE子句看是否它引用某些列或适合索引的列来提高你的查询性能。如果是这样，创造一个适当的索引并且再次用EXPLAIN检查查询

### key

**key列显示MySQL实际决定使用的键（索引），必然包含在possible_keys中**

如果没有选择索引，键是NULL。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。

### **key_len**

**表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）**

不损失精确性的情况下，长度越短越好 

### **ref**

**列与索引的比较，表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值**

### **rows**

**估算出结果集行数，表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数**

### **Extra**

**该列包含MySQL解决查询的详细信息,有以下几种情况：**

Using where:不用读取表中所有信息，仅通过索引就可以获取所需数据，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤

Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询，常见 group by ; order by

Using filesort：当Query中包含 order by 操作，而且无法利用索引完成的排序操作称为“文件排序”

```
-- 测试Extra的filesort
explain select * from emp order by name;
```

Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。

Impossible where：这个值强调了where语句会导致没有符合条件的行（通过收集统计信息不可能存在结果）。

Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行

No tables used：Query语句中使用from dual 或不含任何from子句

```
-- explain select now() from dual;
```

## 慢查询优化

通常来说，查询的生命周期大致可以按照顺序来看: 从客户端，到服务器，然后在服务器上进行解析，生成执行计划，执行，并返回结果给客户端。其中“执行”可以认为是整个生命周期中最重要的阶段，这其中包括了大量为了检索数据到存储引擎的调用以及调用后的数据处理，包括排序、分组等。在完成这些任务的时候，查询需要在不同的地方花费时间，包括网络，CPU计算，生成统计信息和执行计划、锁等待(互斥等待)等操作，尤其是向底层存储引擎检索数据的调用操作,这些调用需要在内存操作、CPU操作和内存不足时导致的I/O操作上消耗时间。根据存储引擎不同，可能还会产生大量的上下文切换以及系统调用。

### 是否向数据库请求了不需要的数据

有些查询会请求超过实际需求的数据，让会这些多余的数据会被应用程序丢弃。这回给MySQL服务器带来额外的负担，并增加网络开销，也会消耗服务器的CPU和内存

经典案例

- 查询不需要的记录

- 多表关联时返回全部列

  如果你想查询所有在电影Academy Dinosaur中出现的演员，千万不要按下面的写法
  编写查询:

  ```sql
  mysql> SELECT * FROM sakila. actor
  -> INNER J0IN sakila. film_ _actor USING(actor_ _id)
  -> INNER J0IN sakila. film USING(film _id)
  -> WHERE sakila. film.title = ' Academy Dinosaur' ;
  ```

  这将返回这三个表的全部数据列。正确的方式应该是像下面这样只取需要的列:

  ```sql
  mysql> SELECT sakila.actor.* FROM sakila.actor...;
  ```

- 总是取出所有列

  避免select *

- 重复查询相同的数据

### MySQL是否在扫描额外的记录

对于MySQL，最简单的衡量查询开销的三个指标如下:

- 响应时间
- 扫描的行数
- 返回的行数

没有哪个指标能够完美地衡量查询的开销，但它们大致反映了MySQL在内部执行查询时需要访问多少数据，并可以大概推算出查询运行的时间。这三个指标都会记录到MySQL的慢日志中，所以检查慢日志记录是找出扫描行数过多的查询的好办法。

