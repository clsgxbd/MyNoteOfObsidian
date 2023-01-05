1. 可能三个一致没做到
2. mapper.xml文件放置位置, 
   resourcs-mapper下面是可以自动加载的,
   src-main-java目录下,默认只编译.java文件不可以自动加载xml文件


解决办法
方案一: 
	直接把xml文件放到resources下面
方案二:
1. 添加依赖
![](image/Pasted%20image%2020221107095356.png)
   2. 需要修改配置文件application
      ![](image/Pasted%20image%2020221107095505.png)
      
