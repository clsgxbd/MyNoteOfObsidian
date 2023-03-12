# 动态SQL
#动态SQL

## 概念

## 常用标签
#todo 动态sql常用标签


### foreach 循环遍历集合

```xml
<delete id="deleteByIds" parameterType="java.util.List">
	delete from 
		tableName
	where 
		id in
			<foreach collection="list" item="id" open="(" separator="," close=")">
				#{id}
			</foreach>
</delete>
```

属性: 
(1) collection:需要迭代的集合，必填项
(2) item: 集合中的每一个元素，必填项
(3) index:可选项，表示当前迭代的次数，从  开始
(4) open: 可选项，表示 foreach 开始的字符串，可以为空
(5) close: 可选项，表示 foreach 结束的字符串，可以为空
(6) separator: 可选项，表示当前元素与下一个元素之间的分隔符，可以为空。


### trim 添加前缀后缀
#trim

用法: 
```xml
<trim prefix="(" suffix=")">  
	<if test="C1Id != null and C1Id != 0">  
		(bai.category_id=#{C1Id} and bai.category_level=1)  
	</if>  
	<if test="C2Id != null and C2Id != 0">  
		or (bai.category_id=#{C2Id} and bai.category_level=2)  
	</if>  
	<if test="C3Id != null and C3Id != 0">  
		or (bai.category_id=#{C3Id} and bai.category_level=3)  
	</if>  
</trim>  
```




## 学习参考文档









