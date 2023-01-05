#MySQL条件值为null

where 条件中判空 用is 不用=
```
例子:
	-- **需求4：**查询`没有加入任何部门的员工`（先查询所有员工，再过滤掉包含部门的数据）
	
	select * from t_emp e left join t_dept d on e.deptId = d.id where e.deptId is null

```
