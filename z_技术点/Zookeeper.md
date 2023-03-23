# Zookeeper
#Zookeeper 

官网: https://zookeeper.apache.org/

核心: 
	ZooKeeper= 文件系统 + 监听通知机制 + ACL

## 概念
- 它是一个分布式服务框架，是Apache Hadoop 的一个子项目，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，如：统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等。
- ZooKeeper 是一个分布式的，开源的分布式应用程序协同服务。
- ZooKeeper 的设计目标是将那些复杂且容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语集，并以一系列简单易用的接口提供给用户使用。

## 应用场景
> ①数据发布/订阅 ；②负载均衡；③命名服务；④分布式协调/通知；⑤集群管理；⑥Master 选举；⑦分布式锁；⑧分布式队列。

典型应用场景：
- 配置管理（configurationmanagement） 
- DNS服务
- 组成员管理（groupmembership）
- 各种分布式锁
ZooKeeper 适用于存储和协同相关的关键数据，不适合用于大数据量存储。



## 数据结构
ZooKeeper维护一个类似文件系统的数据结构（如下官方示意图），每一个子目录项（如app1）都被称作为znode（目录节点），和文件系统一样，我们能够自由的对一个znode进行CRUD，也可以在znode下进行子znode的CRUD，唯一不同的是，znode是可以存储数据的。
 ![](image/Pasted%20image%2020230322191054.png)
### znode类型
(1) 持久节点(persistent node)节点会被持久化.
(2) 临时节点(ephemeral node), 客户端断开链接后, Zookeeper会自动删除临时节点.
(3) 顺序节点(sequential node), 每次创建顺序节点时, Zookeeper都会在路径后面添加上10位数的数字, 从1开始, 最大的是2147483647(2^32-1)

每个顺序节点都有一个单独的计数器, 并且单调递增的, 由Zookeeper的leader实例维护.
Znode实际上有四种形式, 默认是persistent.
(1) PERSISTENT 持久节点: ：如 create/test/a "hello", 通过create参数指定为持久节点.
(2) PERSISTENT_SEQUENTIAL（持久顺序节点/s0000000001），通过 create -s 参数指定为顺序节点
(3) EPHEMERAL 临时节点, 通过 create -e 参数指定为顺序节点
(4) EPHEMERAL_SEQUENTIAL（临时顺序节点/s0000000001），通过 create -s -e 参数指定为临时及顺序节点
 ![](image/Pasted%20image%2020230322192149.png)ZooKeeper3.5.x 中引入了 container 节点 和 ttl 节点（不稳定）
（1）container 节点用来存放子节点，如果container节点中的子节点为0 ，则container节点在未来会被服务器删除，定时任务默认60秒执行一次。
（2）ttl 节点默认禁用，需要通过配置开启，如果ttl 节点没有子节点，或者 ttl 节点在 指定的时间内没有被修改则会被服务器删除。


### Znode属性
> - Znode兼具文件和目录两种特点. 既像文件, 又像目录一样可以作为路径标识的一部分. Znode维护着:
> 	 - data: Znode存储的数据信息;
> 	 - ACL: 记录Znode的访问权限;
> 	 - stat: 包含Znode的各种元数据, 比如事务ID,版本号,时间戳,大小等等.
> 	 - child: 当前节点的子节点引用;
> 	把以上的几类属性细化, 又可以得到以下属性的细节:
> 	 - cZxid: 表示节点被创建时的事务ID;
> 	 - mZxid: 表示节点最后一次被修改时的事务ID;
> 	 - ctime: 表示节点创建时间;
> 	 - mtime: 表示节点最后一次被修改的时间;
> 	 - pZxid: 表示该节点的子节点列表最后一次被修改时的事务ID. 只有子节点列表变更才会更新pZxid, 子节点内容变更不会更新;
> 	 - cversion: 表示子节点的版本号;
> 	 - dataVersion: 表示内容版本号;
> 	 - dataLength: 表示数据长度;
> 	 - numChildren: 表示子节点数;
> 	 - ephemeralOwner: 表示创建该临时节点时的会话sessionID, 如果是持久节点那么为0;

### Zxid
>  致使ZooKeeper节点状态改变的每一个操作都将使节点接收到一个Zxid格式的时间戳，并且这个时间戳全局有序。也就是说，每个对节点的改变都将产生一个唯一的Zxid。如果Zxid1的值小于Zxid2的值，那么Zxid1所对应的事件发生在Zxid2所对应的事件之前。实际上，ZooKeeper的每个节点维护着两个Zxid值，为别为：cZxid、mZxid。
>  - cZxid: 是节点的创建时间所对应的Zxid格式时间戳
>  - mZxid: 是节点的修改时间所对应的Zxid格式的时间戳
>  现实中Zxid是一个64位的数字, 它的高32位是epoch用来标识Leader关系是否改变, 每次一个Leader被选出来, 它都会有一个新的epoch. 低32位是个递增计数器.

### 版本号
>  版本号是用来记录节点数据或者是节点的子节点列表或者是权限信息的修改次数。如果一个节点的version是1，那就代表说这个节点从创建以来被修改了一次。对节点的每一个操作都将致使这个节点的版本号增加。每个节点维护着三个版本号，分别为：
> 	 1. dataVersion: 节点数据版本号
> 	 2. cversion: 子节点版本号
> 	 3. aversion: 节点所拥有的ACL版本号
>  它通过对这些数据的管理来让缓存生效并且令协调更新. 每当Znode中的数据更新后它所维护的版本号将增加.


### ZK数据结构特点小结
1. 每个字目录都被称为znode, 每个znode是它所在路径的唯一标识, 如test1这个znode的标识为/test/test1。
2. znode可以有子目录, 每个znode都可以存储数据。
3. znode可以是临时节点， 如果客户端和服务器连接的session失效，znode将会被删除（这里注意，并不是立马删除，而是有一个30秒左右的延迟）。
4. znode节点中数据如果被修改，可以被监控到，并通知对应的客户端 #监听通知机制

## 监听通知机制
#todo zookeeper监听通知机制
[zookeeper监听通知机制](https://juejin.cn/post/6927241708652806158)
## 基本操作/常用命令
- 连接ZK服务: bin/zkCli.sh

**1. PERSISTENT 持久节点**
- 创建格式: create
	例: 
	 创建持久化节点 test:
		 create /test 'hello'
	 创建子节点:
		 create /test/test1 'hello1'
		 create /test/test2 'hello2'
		 create /test/test3 'hello3'
- 查看节点: ls
	ls -R 可以遍历节点的目录结构;
	get 可以获取到节点的数据;
	例:
	 ls /test
	 ls /test/test1
	 ls -R /test
- 修改数据: set
  例:
	 set /test/test1 'hello update'
- 删除数据
  例: 
	  delete /test/test1

**2. PERSISTENT_SEQUENTIAL 持久顺序节点**
- 创建格式: create -s
  例:
	 先创建父节点: create /seq
	 然后创建持久顺序节点:
		 create -s /seq/order-
		 create -s /seq/order-
		 create -s /seq/order-
 - 注:
	- 如果有父节点的话, 需要先创建父节点, 不然直接创建会报错;
	- 当然不带前缀也是可以的, 直接create -s /seq/, 这里最后的斜杠是要加的, 否则seq就会作为一个前缀来处理;
- 查看, 修改, 删除操作和持久节点的操作一致

**3. EPHEMERAL 临时节点**
- 创建格式: create -e
  例: create -e /ephemeral 'hello e'
 - 注:
	 - 在mac下 ctrl+c 断开客户端链接, 然后在连接上, 通过 ls / 查看, 节点 /ephemeral 在断开连接之后被自动删除了;
	 - 节点的生命周期是和当前session绑定的;
	 - 并不是当前session断开连接就立马被删除了, 这个大概是30s的延迟, 所以立马登陆上还是能够看到刚刚创建的临时节点, 但是过了30s之后还是会被删除掉的;
	 - 临时节点下不能有子节点, 不管是什么类型的节点都不行, 创建会报错: Ephemerals cannot have children: /ephemeral/test;
- 查看, 修改, 删除操作和持久节点的操作一致

**4. EPHEMERAL_SEQUENTIAL 临时顺序节点
- 创建格式: create -e -s
  例: 
	  先创建父节点: create /ephemeral-seq
	  然后创建临时顺序节点:
		  create -e -s /ephemeral-seq/order-
		  create -e -s /ephemeral-seq/order-
		  create -e -s /ephemeral-seq/order-
- 查看, 修改, 删除操作和持久节点的操作一致

**5. container 容器节点**
 container节点用来存放子节点, 如果container节点中的子节点为0, 则container节点在未来会被服务器删除, 定时任务默认60s执行一次.
- 创建格式: create -c
  例: create -c /container 

- 查看Znode属性: ls -s
  例: ls -s /test



## 简单使用
### 注册中心



## 实战入门

## 高级用法


## 学习参考文档
[什么是zookeeper](https://mp.weixin.qq.com/s?__biz=MzA4ODIyMzEwMg==&mid=2447535886&idx=1&sn=c7bbf5923dda56fa79c6fea823fcb09a&chksm=843bb31fb34c3a095cce135a2dcb66705087b567963b3588261db1fcc2c1d8a219a67231df67&scene=21#wechat_redirect)

[docker安装zookeeper](https://www.cnblogs.com/caoweixiong/p/12325410.html)

[ZooKeeper数据结构和实操](https://juejin.cn/post/6959754013323919391)

[zookeeper监听通知机制](https://juejin.cn/post/6927241708652806158)

#todo 稀土掘金: 小飞机yt的主页有zooleeper全套总结
