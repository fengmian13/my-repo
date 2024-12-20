# 

## 反射

它赋予了我们在运行时分析类以及执行类中方法的能力。通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些方法和属性。

**反射**可以让代码更加灵活、（为各种框架提供开箱即用的功能提供了便利），一般我们写业务代码接触到直接使用反射机制的场景不多，但是在Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制。

### 简述进程和线程的关系，区别和优缺点？

一个进程中可以有多个线程，多个线程共享进程的**堆**和**方法区** 资源，但是每个线程有自己的\**程序计数器**、**虚拟机栈** 和 **本地方法栈**。

**总结：** 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。线程执行开销小，但不利于资源的管理和保护；而进程正相反。

### 事务

### [何谓事务？](#何谓事务)

我们设想一个场景，这个场景中我们需要插入多条相关联的数据到数据库，不幸的是，这个过程可能会遇到下面这些问题：

- 数据库中途突然因为某些原因挂掉了。
- 客户端突然因为网络原因连接不上数据库了。
- 并发访问数据库时，多个线程同时写入数据库，覆盖了彼此的更改。
- ……

上面的任何一个问题都可能会导致数据的不一致性。为了保证数据的一致性，系统必须能够处理这些问题。事务就是我们抽象出来简化这些问题的首选机制。事务的概念起源于数据库，目前，已经成为一个比较广泛的概念。

**何为事务？** 一言蔽之，**事务是逻辑上的一组操作，要么都执行，要么都不执行。**

事务最经典也经常被拿出来说例子就是转账了。假如小明要给小红转账 1000 元，这个转账会涉及到两个关键操作，这两个操作必须都成功或者都失败。

1. 将小明的余额减少 1000 元
2. 将小红的余额增加 1000 元。

事务会把这两个操作就可以看成逻辑上的一个整体，这个整体包含的操作要么都成功，要么都要失败。这样就不会出现小明余额减少而小红的余额却并没有增加的情况。

**那数据库事务有什么作用呢？**

简单来说，数据库事务可以保证多个对数据库的操作（也就是 SQL 语句）构成一个逻辑上的整体。构成这个逻辑上的整体的这些数据库操作遵循：**要么全部执行成功,要么全部不执行** 。

```sql
# 开启一个事务
START TRANSACTION;
# 多条 SQL 语句
SQL1,SQL2...
## 提交事务
COMMIT;
```

#### 关系型数据库（例如：`MySQL`、`SQL Server`、`Oracle` 等）事务都有 **ACID** 特性：

![ACID](https://oss.javaguide.cn/github/javaguide/mysql/ACID.png)ACID

1. **原子性**（`Atomicity`）：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
2. **一致性**（`Consistency`）：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；
3. **隔离性**（`Isolation`）：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
4. **持久性**（`Durability`）：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

这里要额外补充一点：**只有保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。也就是说 A、I、D 是手段，C 是目的！**

### 并发事务带来了哪些问题?

#### 1、脏读

一个事务读取数据并且对数据进行了修改，这个修改对其他事务来说是可见的，即使当前事务没有提交。这时另外一个事务读取了这个还未提交的数据，但第一个事务突然回滚，导致数据并没有被提交到数据库，那第二个事务读取到的就是脏数据，这也就是脏读的由来。

例如：事务 1 读取某表中的数据 A=20，事务 1 修改 A=A-1，事务 2 读取到 A = 19,事务 1 回滚导致对 A 的修改并未提交到数据库， A 的值还是 20。

![脏读](https://javaguide.cn/assets/concurrency-consistency-issues-dirty-reading-C1rL9lNt.png)

#### 2、丢失修改

在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。

例如：事务 1 读取某表中的数据 A=20，事务 2 也读取 A=20，事务 1 先修改 A=A-1，事务 2 后来也修改 A=A-1，最终结果 A=19，事务 1 的修改被丢失。

![丢失修改](https://javaguide.cn/assets/concurrency-consistency-issues-missing-modifications-D4pIxvwj.png)

#### 3、不可重复读（重点：内容修改）

指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

#### 4、幻读（重点：内容增加）

幻读与不可重复读类似。它发生在一个事务读取了几行数据，接着另一个并发事务插入了一些数据时。在随后的查询中，第一个事务就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

#### 不可重复读和幻读的区别？

- 不可重复读的重点是内容修改或者记录减少比如多次读取一条记录发现其中某些记录的值被修改；
- 幻读的重点在于记录新增比如多次执行同一条查询语句（DQL）时，发现查到的记录增加了。

幻读其实可以看作是不可重复读的一种特殊情况，单独把幻读区分出来的原因主要是解决幻读和不可重复读的方案不一样。

### 并发事务的控制方式有哪些？

锁和MVCC

#### 锁

控制方式下会通过锁来显式控制共享资源而不是通过调度手段，MySQL 中主要是通过 **读写锁** 来实现并发控制。

#### 共享锁（S锁）

又称读锁，事务在读取记录的时候获取共享锁，允许多个事务同时获取（锁兼容）。

#### 排他锁（X锁）

又称写锁/独占锁，事务在修改记录的时候获取排他锁，不允许多个事务同时获取。如果一个记录已经被加了排他锁，那其他事务不能再对这条记录加任何类型的锁（锁不兼容）。

#### MVCC

多版本并发控制方法，即对一份数据会存储多个版本，通过事务的可见性来保证事务能看到自己应该看到的版本。通常会有一个全局的版本分配器来为每一行数据设置版本号，版本号是唯一的。

### [SQL 标准定义了哪些事务隔离级别?](#sql-标准定义了哪些事务隔离级别)

SQL 标准定义了四个隔离级别：

- **READ-UNCOMMITTED(读取未提交)** ：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
- **READ-COMMITTED(读取已提交)** ：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。
- **REPEATABLE-READ(可重复读)** （<u>默认级别</u>）：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- **SERIALIZABLE(可串行化)** ：最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。

|     隔离级别     | 脏读 | 不可重复读 | 幻读 |
| :--------------: | :--: | :--------: | :--: |
| READ-UNCOMMITTED |  √   |     √      |  √   |
|  READ-COMMITTED  |  ×   |     √      |  √   |
| REPEATABLE-READ  |  ×   |     ×      |  √   |
|   SERIALIZABLE   |  ×   |     ×      |  ×   |

### [共享锁和排他锁呢？](#共享锁和排他锁呢)

不论是表级锁还是行级锁，都存在共享锁（Share Lock，S 锁）和排他锁（Exclusive Lock，X 锁）这两类：

- **共享锁（S 锁）**：又称读锁，事务在读取记录的时候获取共享锁，允许多个事务同时获取（锁兼容）。
- **排他锁（X 锁）**：又称写锁/独占锁，事务在修改记录的时候获取排他锁，不允许多个事务同时获取。如果一个记录已经被加了排他锁，那其他事务不能再对这条事务加任何类型的锁（锁不兼容）。

排他锁与任何的锁都不兼容，共享锁仅和共享锁兼容。

|      | S 锁   | X 锁 |
| :--- | :----- | :--- |
| S 锁 | 不冲突 | 冲突 |
| X 锁 | 冲突   | 冲突 |

**当前读** （一致性锁定读）就是给行记录加 X 锁或 S 锁。

**快照读**：通常指的是一种读取数据库记录的方式，这种方式可以提供数据在某一特定时间点的一致性视图。这种读取操作不会受到其他并发事务修改数据的影响，因此可以避免读取到中间状态的数据，保证数据的一致性。依赖于<u>MVCC</u>

### [MySQL 如何存储 IP 地址？](#mysql-如何存储-ip-地址)

可以将 IP 地址转换成整形数据存储，性能更好，占用空间也更小。

MySQL 提供了两个方法来处理 ip 地址

- `INET_ATON()`：把 ip 转为无符号整型 (4-8 位)
- `INET_NTOA()` :把整型的 ip 转为地址

插入数据前，先用 `INET_ATON()` 把 ip 地址转为整型，显示数据时，使用 `INET_NTOA()` 把整型的 ip 地址转为地址显示即可

### [如何实现读写分离？](#如何实现读写分离)

不论是使用哪一种读写分离具体的实现方案，想要实现读写分离一般包含如下几步：

1. 部署多台数据库，选择其中的一台作为主数据库，其他的一台或者多台作为从数据库。
2. 保证主数据库和从数据库之间的数据是实时同步的，这个过程也就是我们常说的**主从复制**。
3. 系统将写请求交给主数据库处理，读请求交给从数据库处理。

常用的方式有两种：

**1. 代理方式**

![代理方式实现读写分离](https://oss.javaguide.cn/github/javaguide/high-performance/read-and-write-separation-and-library-subtable/read-and-write-separation-proxy.png)

我们可以在应用和数据中间加了一个代理层。应用程序所有的数据请求都交给代理层处理，代理层负责分离读写请求，将它们路由到对应的数据库中。

提供类似功能的中间件有 **MySQL Router**（官方， MySQL Proxy 的替代方案）、**Atlas**（基于 MySQL Proxy）、**MaxScale**、**MyCat**。<u>在 MySQL 8.2 的版本中，MySQL Router 能自动分辨对数据库读写/操作并把这些操作路由到正确的实例上。</u>

**组件方式**

在这种方式中，我们可以通过引入第三方组件来帮助我们读写请求。

这也是我比较推荐的一种方式。这种方式目前在各种互联网公司中用的最多的，相关的实际的案例也非常多。如果你要采用这种方式的话，推荐使用 `sharding-jdbc` ，直接引入 jar 包即可使用，非常方便。同时，也节省了很多运维的成本。

### 常见的 SQL 优化手段：

1. 合理创建索引：
   - 选择经常用于查询、连接、排序和分组操作的列创建索引。
   - 避免过度创建索引，过多的索引会影响数据插入、更新和删除的性能。
2. 优化查询语句：
   - 避免使用 `SELECT *`，只选择需要的列。
   - 减少子查询的使用，尽量使用连接（JOIN）操作。
   - 避免在 `WHERE` 子句中使用函数对列进行操作，这可能导致索引无法使用。
3. 优化表结构：
   - 适当进行表分区，将数据分散到不同的物理存储区域，提高查询性能。
   - 规范表设计，消除冗余列，确保数据的一致性和完整性。
4. 缓存查询结果：
   - 对于频繁执行且结果相对稳定的查询，可以使用缓存机制来提高性能。
5. 调整数据库参数：
   - 根据数据库服务器的硬件资源和应用需求，合理配置内存、缓冲区大小等参数。
6. 优化连接操作：
   - 确保连接条件的列上有适当的索引。
   - 选择合适的连接类型（内连接、外连接等）。
7. 避免全表扫描：
   - 通过合适的索引和查询条件，尽量让数据库只扫描需要的数据。
8. 定期维护数据库：
   - 包括数据清理、索引重建、统计信息更新等。
9. 分解复杂查询：
   - 将复杂的查询分解为多个简单的查询，逐步获取结果。
10. 监控和分析执行计划：
    - 使用数据库提供的工具查看查询的执行计划，分析耗时的操作和资源消耗情况，针对性地进行优化。

[sql优化的15个小技巧（必知五颗星），面试说出七八个就有了_sql优化常用的15种方法-CSDN博客](https://blog.csdn.net/guoqi_666/article/details/122484535)

为什么不建议采用uuid作为主键

因为uuid是无序

但是很多时候我们不希望使用mysql自增的id

那么你也要保证你的id的自增性，这个在分布式系统中很常见-snowflake 