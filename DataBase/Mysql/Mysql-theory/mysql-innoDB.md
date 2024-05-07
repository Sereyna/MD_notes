# InnoDB
> 摘要: MySQL有多种存储引擎，每种存储引擎有各自的优缺点，可以择优选择使用：MyISAM、InnoDB、MERGE、MEMORY(HEAP)、BDB(BerkeleyDB)、EXAMPLE、FEDERATED、ARCHIVE、CSV、BLACKHOLE。

比较常用的是 MyISAM 和 InnoBD

InnoDB是新表的默认存储引擎。在实践中，高级 InnoDB性能特性意味着 InnoDB表通常优于简单MyISAM表，尤其是对于繁忙的数据库。
## 1 innoDB架构
![innoDB-architecture](../img/innodb-architecture.png "innoDB-architecture")

**_缓冲池（Buffer pool）_**

缓冲池是主内存中的一个区域，用于在 InnoDB访问表和索引数据时对其进行缓存。缓冲池允许直接从内存中访问经常使用的数据，从而加快处理速度。在专用服务器上，多达 80% 的物理内存通常分配给缓冲池。

为了提高大容量读取操作的效率，缓冲池被划分为可能包含多行的页面。为了缓存管理的效率，缓冲池被实现为页链表；很少使用的数据使用最近最少使用 (LRU) 算法的变体从缓存中老化。

_缓冲池 LRU 算法_

缓冲池使用 LRU 算法的变体作为列表进行管理。当需要空间来向缓冲池添加新页面时，最近最少使用的页面将被逐出，并将新页面添加到列表的中间。此中点插入策略将列表视为两个子列表：

- 在头部，最近访问 的新（ “年轻” ）页面的子列表
- 在尾部，最近访问较少的旧页面的子列表
该算法将经常使用的页面保留在新的子列表中。旧的子列表包含不常用的页面；这些页面是驱逐的候选者。

_缓冲池配置_

您可以配置缓冲池的各个方面以提高性能。

- 理想情况下，您将缓冲池的大小设置为尽可能大的值，从而为服务器上的其他进程留出足够的内存来运行而不会出现过多的分页。缓冲池越大，InnoDB就越像内存数据库，从磁盘读取一次数据，然后在后续读取期间从内存中访问数据。
- 在具有足够内存的 64 位系统上，您可以将缓冲池拆分为多个部分，以最大程度地减少并发操作之间的内存结构争用。
- 您可以将经常访问的数据保留在内存中，而不管操作的活动突然高峰会将大量不经常访问的数据带入缓冲池。
- 您可以控制执行预读请求的方式和时间，以异步将页面预取到缓冲池中，以应对即将到来的需求。
- 您可以控制何时发生后台刷新以及是否根据工作负载动态调整刷新速率。
- 您可以配置InnoDB保留当前缓冲池状态以避免服务器重新启动后的长时间预热。

**_更改缓冲区（Change Buffer）_**

更改缓冲区是一种特殊的数据结构，当这些页面不在缓冲池中时 ，它会缓存对二级索引页面的更改。

**_自适应哈希索引（Adaptive Hash Index）_**


【引用】

MySQL存储引擎InnoDB与Myisam的六大区别

https://www.runoob.com/w3cnote/mysql-different-nnodb-myisam.html

Mysql 8.0 interface 
Chapter 15 The InnoDB Storage Engine

https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html