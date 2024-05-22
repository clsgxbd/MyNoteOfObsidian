#MySQL

[MySQL必知必会](../资料书/MySQL必知必会.pdf)

## sql语句书写时要注意什么?
#sql语句书写时要注意什么
1. 查询Sql不使用select * ，使用具体字段
原因：只取需要的字段，节省资源、减少网络开销
select * 进行查询时，很可能就不会使用到覆盖索引了，就会造成回表查询。
2. 如果知道查询结果只有一条，或者只要最大/最小一条记录，建议用 limit 1
理由：加上 limit 1 后,只要找到了对应的一条记录,就不会继续向下扫描了，效率将会大大提高；
3. 应尽量避免在 where 子句中使用 or 来连接条件
理由：对于or+没有索引的情况，需要散步过程，全表扫描+索引扫描+合并
4. 优化 limit 分页
日常做分页需求时，一般会用 limit 实现。但是当偏移量特别大的时候，查询效率就变低。
– 方案一 ：返回上次查询的最大记录(偏移量)select id，name from employee where id>10000 limit 10;
– 方案二：order by + 索引select id，name from employee order by id limit 10000，10;
– 方案三：在业务允许的情况下限制页数：
理由：当偏移量最大的时候，查询效率就会越低。因为 MySQL 并非是跳过偏移量直接去取后面的数据，而是先把偏移量+要取的条数，然后再把前面偏移量这一段的数据抛弃掉再返回的
5. 优化 like 语句
日常开发中，如果用到模糊关键字查询很容易想到 like，但是 like 很可能让索引失效。
优化：将%放到关键字后面不会造成索引失效，‘123%’
6. 使用 where 条件限定要查询的数据，避免返回多余的行
理由：需要什么数据，就去查什么数据，避免返回不必要的数据，节省开销。
7.尽量避免在索引列上使用 MySQL 的内置函数
理由：索引列上使用 MySQL 的内置函数导致索引失效
反例：select userId,loginTime from loginuser where Date_ADD(loginTime,Interval 7 DAY) >=now();
正例：explain select userId,loginTime from loginuser where loginTime >= Date_ADD(NOW(),INTERVAL - 7 DAY);
8. 应尽量避免在 where 子句中对字段进行表达式操作，这将导致系统放弃使用索引而进行全表扫
理由：虽然 age 加了索引，但是因为对它进行运算，索引直接迷路了
9. Inner join 、left join、right join，优先使用Inner join，如果是 left join，左边表结果尽量小
理由：如果 inner join 是等值连接，或许返回的行数比较少，所以性能相对会好一点；
同理，使用了左连接，左边表数据结果尽量小，条件尽量放到左边处理，意味着返回的行数可能比较少。
10. 应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描
11. 使用联合索引时，注意索引列的顺序，一般遵循最左匹配原则
理由：当我们创建一个联合索引的时候，如 (k1,k2,k3)，相当于创建了 (k1)、(k1,k2) 和 (k1,k2,k3) 三个索引，这就是最左匹配原则。
12. 对查询进行优化，应考虑在 where 及 order by 涉及的列上建立索引，尽量避免全表扫描
13. 在适当的时候，使用覆盖索引
覆盖索引能够使得你的 SQL 语句不需要回表，仅仅访问索引就能够得到所有需要的数据，大大提高了查询效率。
14. 慎用 distinct 关键字
distinct 关键字一般用来过滤重复记录，以返回不重复的记录。在查询一个字段或者很少字段的情况下使用时，给查询带来优化效果。但是在字段很多的时候使用，却会大大降低查询效率。
15. where 子句中考虑使用默认值代替 null
理由：并不是说使用了 is null 或者 is not null 就会不走索引了，这个跟 MySQL 版本以及查询成本都有关
如果把 null 值换成默认值，很多时候让走索引成为可能。同时，表达意思会相对清晰一点
16. 尽量用 union all 替换 union
如果检索结果中不会有重复的记录，推荐 union all 替换 union
理由：如果使用 union，不管检索结果有没有重复，都会尝试进行合并。然后在输出最终结果前进行排序。如果已知检索结果没有重复记录，使用 union all 代替 union，这样会提高效率。
17. 索引不适合建在有大量重复数据的字段上，如性别这类型数据库字段
因为 SQL 优化器是根据表中数据量来进行查询优化的，如果索引列有大量重复数据，MySQL 查询优化器推算发现不走索引的成本更低，很可能就放弃索引了。
18. 使用 explain 分析 SQL 的执行计划
日常开发写 SQL 的时候，尽量养成一个习惯吧。用 explain 分析一下你写的 SQL，尤其是走不走索引这一块。
————————————————
版权声明：本文为CSDN博主「永恒的僾^O^」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44186791/article/details/120989599

### 备注:
1. 查询语句后面加上 `\G` 可以每行展示一个字段和字段值, 便于查看字段存储的数据

## MySQL函数
#MySQL函数

### 聚合函数(常用)
#聚合函数

 ```
 分组函数
	-   AVG(x) ：求平均值
	-   SUM(x)：求总和
	-   MAX(x) ：求最大值
	-   MIN(x) ：求最小值
	-   COUNT(x) ：统计记录数
 ```
| 函数          | 用法                                                         |
| ------------- | ------------------------------------------------------------ |
| ABS(x)        | 返回x的绝对值                                                |
| CEIL(x)       | 返回大于x的最小整数值                                        |
| FLOOR(x)      | 返回小于x的最大整数值                                        |
| MOD(x,y)      | 返回x/y的模                                                  |
| RAND()        | 返回0~1的随机值                                              |
| ROUND(x,y)    | 返回参数x的四舍五入的有y位的小数的值                         |
| TRUNCATE(x,y) | 返回数字x截断为y位小数的结果                                 |
| FORMAT(x,y)   | 强制保留小数点后y位，整数部分超过三位的时候以逗号分割，并且返回的结果是文本类型的 |
| SQRT(x)       | 返回x的平方根                                                |
| POW(x,y)      | 返回x的y次方                                                 |


### 其它函数

#### CONCAT
	CONCAT('字符串1',"字符串2",'字符串3')  字符串拼接
```sql
CONCAT("%","医院","%")  等同于 %医院% 
	-- 动态sql语句中对应 <CONCAT>标签
```

#### LEFT
	LEFT(s,n)  返回字符串 s 的前 n 个字符
```sql
	-- 返回字符串 runoob 中的前两个字符：
	SELECT LEFT('runoob',2) -- ru
```

#### GROUP_CONCAT
	GROUP_CONCAT(字段 separator '分隔符')
```sql
	select id,group_concat(price separator ';') from goods group by id;
```

## Windows和Linux中SQL区别
#Windows和Linux中SQL区别

**Windows环境：**
		全部不区分大小写
**Linux环境：**
		1、数据库名、表名、表的别名、变量名`严格区分大小写`；
		2、列名与列的别名`不区分大小写`。
		3、关键字、函数名称`不区分大小写`；


## MySQL严格模式和宽松模式
#MySQL严格模式和宽松模式
	
	严格模式下 分组查询只能查询分组后的字段和聚合函数
	宽松模式下可以查询原来表中的字段,但是查出来的数据可能是有问题的


## MySQL数据库隔离级别
#MySQL数据库隔离级别

	级别一：读未提交 readUncommited  存在数据脏读
	级别二：读已提交 readCommited  存在不可重复读
	级别三：可重复读  ReadReptable   存在虚读，幻读
	级别四：串行化 解决一切读的问题 性能最低







## [[MyISAM和InnoDB的区别]]
## [[MySQL逻辑架构]]
## [[MySQL索引失效]]
## [[MySQL索引下推]]
## [[MySQl主从同步原理]]
## [[MySQL一主多从配置]]

## 其它
### windows下MySql服务路径修改
#### 如何更改服务中MySQL的可执行文件路径：
> 1，打开CMD输入regedit，打开注册表编辑器
> 2，根据路径找到mysql注册表
> 找到以下路径：
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL80，
> 我电脑上的服务名为：MySQL80你们根据自己得实际情况进行查找。
> 修改ImagePath值为正确的路径。
> ![](image/Pasted%20image%2020240522143640.png)
> 
#### 如何更改MYSQL的数据库文件路径:
> 开始--运行--services.msc--找到MySql服务,停止 的默认安装位置:   
> C:\Program Files\MySQL\MySQL Server 5.0 然后，修改my.ini文件。
> 进入MySQL的安装目录，找到my.ini，修改datadir的值。
> datadir="D:/Program Files/MySQL/MySQL Server 5.0/Data/" -- 修改这个值即可 
> 最后，启动MySQL服务。
> 开始--运行--services.msc--找到MySql服务,启动



