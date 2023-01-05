#MySQL
#索引失效 

## 索引失效是怎么产生的?
	我们创建索引后,用不用索引,最终是优化器说了算的.优化器会基于开销(CPU, IO)选择索引, 怎么开销小就怎么来. 不是基于规则,也不是基于语义.
	另外SQL语句是否使用索引, 和数据库的版本, 数据量, 数据选择度(查询中选择的列数)运行环境都有关.

```
-- 创建索引: 
	CREATE INDEX idx_name ON emp(`name`);
```

## 单表索引失效案例
#单表索引失效案例
1. 计算,函数导致索引失效
	```sql
	-- 显示查询分析
	EXPLAIN SELECT * FROM emp WHERE emp.name  LIKE 'abc%';
	EXPLAIN SELECT * FROM emp WHERE LEFT(emp.name,3) = 'abc'; -- 索引失效
								-- LEFT(s,n)函数 返回字符串s的前n个字符
	```
2. LIKE 以 % 开头索引失效
	```sql
	EXPLAIN SELECT * FROM emp WHERE name LIKE '%ab%'; --索引失效
	
	-- 拓展：Alibaba《Java开发手册》
	-- 【强制】页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。
	```
3. 不等于( != 或者 <>)索引失效
   ```sql
	EXPLAIN SELECT * FROM emp WHERE emp.name = 'abc' ;
	EXPLAIN SELECT * FROM emp WHERE emp.name <> 'abc' ; --索引失效
	```
4. IS NOT NULL 和 IS NULL 索引失效
   ```sql
	EXPLAIN SELECT * FROM emp WHERE emp.name IS NULL;
	EXPLAIN SELECT * FROM emp WHERE emp.name IS NOT NULL; --索引失效
		-- 如果name字段的值大部分都是null , IS NOT NULL 索引也不会失效
	```
5. 类型转换导致索引失效
   ```sql
	EXPLAIN SELECT * FROM emp WHERE name='123'; 
	EXPLAIN SELECT * FROM emp WHERE name= 123; --索引失效
	-- name 字段是 VARCHAR 类型, 123 不加单引号是 INT 类型, 这里进行了隐式类型转换, 导致了索引失效
	-- 如果在索引列上使用了类似于-计算-或者-隐式类型转换-之类的操作会导致索引失效
	```
6. 全值匹配我最爱
   
	**准备：**
	```sql
	-- 首先删除之前创建的索引
	CALL proc_drop_index("atguigudb","emp");
	```
	
	**问题：**为以下查询语句创建哪种索引效率最高
	```sql
	-- 查询分析
	EXPLAIN SELECT * FROM emp WHERE emp.age = 30 and deptid = 4 AND emp.name = 'abcd';
	-- 执行SQL
	SELECT * FROM emp WHERE emp.age = 30 and deptid = 4 AND emp.name = 'abcd';
	-- 查看执行时间
	SHOW PROFILES;
	```
	
	**创建索引并重新执行以上测试：**
	```sql
	-- 创建索引：分别创建以下三种索引的一种，并分别进行以上查询分析
	CREATE INDEX idx_age ON emp(age);
	CREATE INDEX idx_age_deptid ON emp(age,deptid);
	CREATE INDEX idx_age_deptid_name ON emp(age,deptid,`name`);
	```
	
	**结论：**可以发现最高效的查询应用了联合索引 `idx_age_deptid_name` 
7. 最佳左前缀法则
	   
	带头大哥不能死，中间小弟不能断
	
	**准备：**
	
	```sql
	-- 首先删除之前创建的索引
	CALL proc_drop_index("atguigudb","emp");
	-- 创建索引
	CREATE INDEX idx_age_deptid_name ON emp(age,deptid,`name`);
	```
	
	**问题：**以下这些SQL语句能否命中 `idx_age_deptid_name` 索引，可以匹配多少个索引字段
	
	**测试：**
	
	- 如果索引了多列，要遵守最左前缀法则。即查询从`索引的最左前列`开始并且不跳过索引中的列。
	- 过滤条件要使用索引，必须按照`索引建立时的顺序，依次满足`，一旦跳过某个字段，索引后面的字段都无法被使用。
	
	```sql
	EXPLAIN SELECT * FROM emp WHERE emp.age=30 AND emp.name = 'abcd' ;
	-- EXPLAIN结果：
	-- key_len：5 只使用了age索引
	-- 索引查找的顺序为 age、deptid、name，查询条件中不包含deptid，无法使用deptid和name索引
	
	EXPLAIN SELECT * FROM emp WHERE emp.deptid=1 AND emp.name = 'abcd';
	-- EXPLAIN结果：
	-- type： ALL， 执行了全表扫描
	-- key_len： NULL， 索引失效
	-- 索引查找的顺序为 age、deptid、name，查询条件中不包含age，无法使用整个索引
	
	EXPLAIN SELECT * FROM emp WHERE emp.age = 30 AND emp.deptid=1 AND emp.name = 'abcd';
	-- EXPLAIN结果：
	-- 索引查找的顺序为 age、deptid、name，匹配所有索引字段
	
	EXPLAIN SELECT * FROM emp WHERE emp.deptid=1 AND emp.name = 'abcd' AND emp.age = 30;
	-- EXPLAIN结果：
	-- 索引查找的顺序为 age、deptid、name，匹配所有索引字段
	
	```
8. 索引中范围条件右边的列失效
	
	**准备：**
	
	```sql
	-- 首先删除之前创建的索引
	CALL proc_drop_index("atguigudb","emp");
	```
	
	**问题：**为以下查询语句创建哪种索引效率最高
	
	```sql
	EXPLAIN SELECT * FROM emp WHERE emp.age=30 AND emp.deptId>1000 AND emp.name = 'abc'; 
	```
	
	**测试1：**
	
	```sql
	-- 创建索引并执行以上SQL语句的EXPLAIN
	CREATE INDEX idx_age_deptid_name ON emp(age,deptid,`name`);
	-- key_len：10， 只是用了 age 和 deptid索引，name失效
	```
	
	**注意：**当我们修改deptId的范围条件的时候，例如deptId>100，那么整个索引失效，MySQL的优化器基于成本计算后认为没必要使用索引了，所以就进行了全表扫描。`（注意：因为表中的数据是随机生成的，因此实际测试中根据具体数据的不同测试的结果也会不一样，最终是否使用索引由优化器决定）`
	
	![image-20220711215826013](image/MySQL8高级-架构和优化/image-20220711215826013.png)
	
	**测试2：**
	
	```sql
	-- 创建索引并执行以上SQL语句的EXPLAIN（将deptid索引的放在最后）
	CREATE INDEX idx_age_name_deptid ON emp(age,`name`,deptid);
	-- 使用了完整的索引
	```
	
	![image-20220712080350120](image/MySQL8高级-架构和优化/image-20220712080350120.png)
	
	**补充：**以上两个索引都存在的时候，MySQL优化器会自动选择最好的方案
### 一般性建议

* 对于单例索引，尽量选择针对当前query过滤性更好的索引(比如name字段的过滤性就比sex字段更好)
* 在选择组合索引的时候, 当前query中过滤性最好的字段在索引顺序汇中，位置越靠前越好
* 在选择组合索引的时候，尽量选择能够包含当前query中的where子句中更多字段的索引
* 在选择组合索引的时候，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面

*总之，书写SQL语句时，尽量避免造成索引失效的情况**



## MySQL索引失效
#MySQL索引失效
> **以下三种情况不走索引：**
>
> 1. 无过滤，不索引
>    order by想使用索引，必须有过滤条件，索引才能生效，没过滤条件就不会走索引,实在没条件就加limit,limit也可以看作是过滤条件
> 2. 顺序错，不索引
>	查询条件的顺序应该和复合索引中的列顺序一致
		order by 中的字段走了索引也不在key_len中体现
> 1. 方向反，不索引
> 	排序条件和索引一致才可以使用索引 ASC


