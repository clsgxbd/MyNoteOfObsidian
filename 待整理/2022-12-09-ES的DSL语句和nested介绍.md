# 回顾: 
## 1. FutureFutureTask 

- [ ] 1.5的FutureTask 封装Callable 方式创建线程; 因为get返回值存在阻塞问题 ; 所以1.8出来 ComplateableFuture 异步回调

```
Future future = new FutureFutureTask(new Callable(){
	Intager call(){
		return 1;
	}
})

Intager i =  future.get();
sout(i);  // 结果为 1

```
## 2. 首页分类列表

- [ ] 渲染形式
- [ ] 页面生成形式

## 3. 全文检索
ElasticSearch 倒排索引 概述



# 今日
## 1. 根据业务搭建数据结构
### ( 1 )建立mapping
- [x] ES非关系型数据库 #Elasticsearch
- [x] 分词过滤
- [x] Kibana是什么 
- [x] 分词器
- [ ] 哪些字段需要分词
- [ ] 用哪些些字段进行过滤
- [ ] 需要查询哪些数据 4个
- [ ] index: 首先定义索引库
- [ ] type: 类型弱化了
- [ ] document: 文档 : 一行数据
- [ ] field: 展示的字段
- [ ] Es默认index 是true 有些不需要:比如图片路径
- [ ] 英文分词器 standard : 一个单词一个
- [ ] 中文分词器
- [ ] attrs
- [ ] 

### ( 2 ) nested介绍
- [ ] 维护object 对象彼此独立的存储
- [ ] kibana演示 默认数据对象存储的结构特点和问题  步骤:
- [ ] 1. 建立一个index
![](image/Pasted%20image%2020221209092618.png)
运行
存储成功: ![](image/Pasted%20image%2020221209092640.png)
- [ ] 2. 查看索引库
![](image/Pasted%20image%2020221209092710.png)
![](image/Pasted%20image%2020221209092716.png)
- [ ] 3. match_all查询所有
![](image/Pasted%20image%2020221209092741.png)
![](image/Pasted%20image%2020221209092748.png)

![](image/Pasted%20image%2020221209092842.png)
![](image/Pasted%20image%2020221209092848.png)

- [ ] 条件查询根据first和last查询
bool多条件查询
must多个条件之间的关系是并且
![](image/Pasted%20image%2020221209093103.png)
![](image/Pasted%20image%2020221209093113.png)
john white 为什么也能查出来?
原因: 默认情况下, es对于对象类型的数据存储会进行"扁平化" 处理
![](image/Pasted%20image%2020221209093521.png)
解决办法: nested: 允许对象数组彼此独立的进行索引和查询

- [ ] 删除my_index
```
	DELETE /my-index
```

- [ ] 查询索引库
```
	GET /_cat/indices?v
```
没有库 会根据数据结构类型自动创建库
![](image/Pasted%20image%2020221209093909.png)
![](image/Pasted%20image%2020221209093918.png)

错误的演示: 
![](image/Pasted%20image%2020221209094005.png)

正确的演示: 
因为nested对存储的数据进行了独立的存储, 所以查询时, 需要进行指定说明字段的类型
![](image/Pasted%20image%2020221209094313.png)

- [ ] 查看mapping结构改变了吗
![](image/Pasted%20image%2020221209094354.png)
![](image/Pasted%20image%2020221209094400.png)



## 商品上下架

- [ ] 搭建service-list模块
- [ ] pom
- [ ] bootstrap.properties
- [ ] nacos配置中心
- [ ] 启动类 com.atguigu.gmall.list.ServiceListApplication
- [ ] @SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
- [ ] 注册中心注册
- [ ] 扫描包 ComponentScan
- [ ] 启动类名过长: 解决办法 // 漏
- [ ] 显示 8203端口


![](image/Pasted%20image%2020221209101219.png)
- [ ] 分析业务操作过程
- [ ] ORM
- [ ] Spring 官网: Spring Data for ElasticSearch
- [ ] Mysql也是可以通过实体类创建表的
- [ ] 根据实体类创建索引库
- [ ] @Document(indexName="goods", shards = 3, replicas = 2)
![](image/Pasted%20image%2020221209101516.png)

id: 文档数据区别唯一
![](image/Pasted%20image%2020221209101554.png)
![](image/Pasted%20image%2020221209101539.png)

![](image/Pasted%20image%2020221209101627.png)
![](image/Pasted%20image%2020221209101606.png)

默认建立索引
![](image/Pasted%20image%2020221209101713.png)
![](image/Pasted%20image%2020221209101736.png)

![](image/Pasted%20image%2020221209101802.png)
![](image/Pasted%20image%2020221209101858.png)
![](image/Pasted%20image%2020221209101821.png)

![](image/Pasted%20image%2020221209101924.png)

![](image/Pasted%20image%2020221209101946.png)
![](image/Pasted%20image%2020221209101932.png)

- [ ] 创建实体类
- [ ] ListApiController
- [ ] @RestController
- [ ] 注入 ElasticsearchRestTemplate 

- [ ] 创建索引库和建立mapping
- [ ] /createIndex
- [ ] 调用两个过时的方法  // 漏
![](image/Pasted%20image%2020221209110547.png)
- [ ] 完成 重启服务
- [ ] 先查寻索引库 没搭建集群会有警告
- [ ] 请求 /createIndex
- [ ] 再次查询索引库
- [ ] 查看goods 要提交哪些数据
- [ ] 这些数据来自哪些表, 调用哪些接口
- [ ] list模块 从 product模块取出数据, 存入 es

- [ ]  // 漏  挂机了一会: 11:00 - 11:04

- [ ] ElasticsearchRepository 接口
- [ ] GoodsRepository 继承 ElasticsearchRepository
- [ ] SearchServiceImpl 注入GoodsRepository
- [ ] 封装Goods对象
- [ ] goodsRepository.save(Goods对象)



## 商品热度排名

- [ ] 热度排名分析 // 漏
- [ ] 商品热度排名接口 // 漏
- [ ] service-list-client 暴露接口
- [ ] DSL语句 :Query DSL又叫查询表达式，是一种非常灵活又富有表现力的查询语言，采用JSON接口的方式实现丰富的查询，并使你的查询语句更灵活、更精确、更易读且易调试
- [ ] 商品详情 
- [ ] 用户到达搜索界面后 可能会进行哪些查询?
- [ ] 1. 搜索框 - 关键词查询 - 匹配查询
- [ ] 2. 过滤查询  - 分类过滤
- [ ] 3. 过滤查询 - 品牌过滤
- [ ] 4. 过滤查询 - 平台属性查询 -nested 
- [ ] 5. 聚合查询 - 品牌聚合
- [ ] 6. 聚合查询 - 平台属性聚合 -nested
- [ ] 7. 排序查询 - 价格排序
- [ ] 8. 排序查询 - 热度排序 hotScore
- [ ] 9. 分页查询
- [ ] 10. 返回值处理 有的字段不要
- [ ] ES(DSL语句) -- MySQL(SQL)
- [ ] 查小米手机为啥查出了华为手机: 原因: 分词
- [ ] 正确查询方式:  ![](image/Pasted%20image%2020221209152749.png)
- [ ] ![](image/Pasted%20image%2020221209153007.png)
- [ ] ![](image/Pasted%20image%2020221209153137.png)
- [ ] ![](image/Pasted%20image%2020221209153417.png)
- [ ] 漏 ![](image/Pasted%20image%2020221209154240.png)
- [ ] ![](image/Pasted%20image%2020221209155124.png)
- [ ] ![](image/Pasted%20image%2020221209161123.png)
- [ ] 

```Elasticsearch
	// POST /索引/默认方式_type(6版本以后可以省略)/查询关键字_search
	POST /person/_search
	{
		"query": {
			"bool": {
				// must 相当于 and
				// shoud 相当于 or
				"must": [
					{
						"match": {
							"last_name": "Smith"
						}
					},
					{
						"match": {
							"about": "basketball"
						}
					}
				]
			}
		}
	}
```
- [ ] 
- [ ] shardingspare
- [ ] 
- [ ] 
- [ ] 



# todo
- [ ] 项目模块功能总结
- [ ] 