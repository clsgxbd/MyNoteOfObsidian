#MySQL

[MySQL必知必会](MySQL必知必会(资料书).pdf)

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