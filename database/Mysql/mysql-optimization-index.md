# Mysql优化之索引优化
## 索引
索引用于快速查找具有特定列值的行。如果没有索引，MySQL 必须从第一行开始，然后读取整个表以查找相关行。表越大，成本越高。如果表有相关列的索引，MySQL 可以快速确定要在数据文件中间查找的位置，而无需查看所有数据。这比顺序读取每一行要快得多。

大多数 MySQL 索引（PRIMARY KEY、 UNIQUE、INDEX和 FULLTEXT）都存储在 B-trees中。例外：空间数据类型的索引使用 R-trees；MEMORY 表也​​支持散列索引；InnoDB使用倒排列表作为FULLTEXT索引。

**_索引优化_**

**_B-Tree和哈希索引的比较_**

_B-Tree 索引特征_
B 树索引可用于使用 =、 >、 >=、 <、 <=或BETWEEN运算符的表达式中的列比较。LIKE 如果参数 to LIKE是不以通配符开头的常量字符串，则索引也可用于比较。例如，以下SELECT语句使用索引：
```sql
SELECT * FROM tbl_name WHERE key_col LIKE 'Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE 'Pat%_ck%';
```

以下SELECT语句不使用索引：
```sql
SELECT * FROM tbl_name WHERE key_col LIKE '%Patrick%';
SELECT * FROM tbl_name WHERE key_col LIKE other_col;
```

有时 MySQL 不使用索引，即使索引可用。发生这种情况的一种情况是优化器估计使用索引将需要 MySQL 访问表中很大比例的行。（在这种情况下，表扫描可能会快得多，因为它需要更少的查找。）但是，如果这样的查询LIMIT仅用于检索某些行，那么 MySQL 无论如何都会使用索引，因为它可以更快地找到在结果中返回几行。

_Hash索引特征_

哈希索引与刚才讨论的有些不同：

- 它们仅用于使用 =or<=> 运算符（但速度非常快）的相等比较。它们不用于比较运算符，例如 <查找值范围。依赖这种类型的单值查找的系统被称为“键值存储”；要将 MySQL 用于此类应用程序，请尽可能使用哈希索引。
- 优化器不能使用哈希索引来加速 ORDER BY操作。（这种类型的索引不能用于按顺序搜索下一个条目。）
- MySQL 无法确定两个值之间大约有多少行（范围优化器使用它来决定使用哪个索引）。如果将MyISAMor InnoDB表更改为散列索引 MEMORY表，这可能会影响某些查询。
- 只能使用整个键来搜索行。（使用 B 树索引，键的任何最左边的前缀都可用于查找行。）

## MySQL不走索引的原因
1. 如果MySQL估计使用索引比全表扫描更慢，则不使用索引。
2. 如果使用MEMORY/HEAP表，并且where条件中不使用“=”进行索引列，那么不会用到索引，head表只有在“=”的条件下才会使用索引
3. 用or分隔开的条件，如果or前的条件中的列有索引，而后面的列没有索引，那么涉及到的索引都不会被用到
4. 复合索引，如果索引列不是复合索引的第一部分，则不使用索引（即不符合最左前缀）
5. 如果like是以‘%'开始的，则该列上的索引不会被使用。
6. 



【引用】

简单剖析B树（B-Tree）与Ｂ+树

https://blog.csdn.net/z_ryan/article/details/79685072

B-Tree和哈希索引的比较

https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html

MySQL查询不使用索引汇总

https://www.jb51.net/article/158141.htm