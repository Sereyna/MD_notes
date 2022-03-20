# 基础理论

**联结查询**

sql之left join、right join、inner join的区别
- left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录
- right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录
- inner join(等值连接) 只返回两个表中联结字段相等的行

**最左原则**

最左匹配原则就是指在联合索引中，如果你的 SQL 语句中用到了联合索引中的最左边的索引，那么这条 SQL 语句就可以利用这个联合索引去进行匹配。例如某表现有索引(a,b,c)，现在你有如下语句：
```sql
select * from t where a=1 and b=1 and c =1;     #这样可以利用到定义的索引（a,b,c）,用上a,b,c

select * from t where a=1 and b=1;     #这样可以利用到定义的索引（a,b,c）,用上a,b

select * from t where b=1 and a=1;     #这样可以利用到定义的索引（a,b,c）,用上a,b（mysql有查询优化器）

select * from t where a=1;     #这样也可以利用到定义的索引（a,b,c）,用上a

select * from t where b=1 and c=1;     #这样不可以利用到定义的索引（a,b,c）

select * from t where a=1 and c=1;     #这样可以利用到定义的索引（a,b,c），但只用上a索引，b,c索引用不到
```
值得注意的是，当遇到范围查询(>、<、between、like)就会停止匹配。也就是：
```sql
select * from t where a=1 and b>1 and c =1; #这样a,b可以用到（a,b,c），c索引用不到 
```
但是如果是建立(a,c,b)联合索引，则a,b,c都可以使用索引，因为优化器会自动改写为最优查询语句
```sql
select * from t where a=1 and b >1 and c=1;  #如果是建立(a,c,b)联合索引，则a,b,c都可以使用索引
#优化器改写为
select * from t where a=1 and c=1 and b >1;
```

这也是最左前缀原理的一部分，索引index1:(a,b,c)，只会走a、a,b、a,b,c 三种类型的查询，其实这里说的有一点问题，a,c也走，但是只走a字段索引，不会走c字段。

另外还有一个特殊情况说明下，select * from table where a = '1' and b > ‘2’ and c='3' 这种类型的也只会有 a与b 走索引，c不会走。

像select * from table where a = '1' and b > ‘2’ and c='3' 这种类型的sql语句，在a、b走完索引后，c肯定是无序了，所以c就没法走索引，数据库会觉得还不如全表扫描c字段来的快。

以index （a,b,c）为例建立这样的索引相当于建立了索引a、ab、abc三个索引。一个索引顶三个索引当然是好事，毕竟每多一个索引，都会增加写操作的开销和磁盘空间的开销。

【引用】

数据库系统（期末重点）

https://blog.csdn.net/qq_39232265/article/details/85066099?spm=1001.2014.3001.5501

MYSQL | 最左匹配原则

https://www.cnblogs.com/-mrl/p/13230006.html