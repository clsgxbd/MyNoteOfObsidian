trim标签 添加前缀后缀
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

