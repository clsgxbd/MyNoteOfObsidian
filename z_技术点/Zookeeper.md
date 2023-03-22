# Zookeeper
#Zookeeper 

官网: https://zookeeper.apache.org/

核心: 
	简单来说zookeeper=文件系统+监听通知机制

## 概念
- 它是一个分布式服务框架，是Apache Hadoop 的一个子项目，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，如：统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等。
- ZooKeeper 是一个分布式的，开源的分布式应用程序协同服务。
- ZooKeeper 的设计目标是将那些复杂且容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语集，并以一系列简单易用的接口提供给用户使用。

## 应用场景
典型应用场景：
- 配置管理（configurationmanagement） 
- DNS服务
- 组成员管理（groupmembership）
- 各种分布式锁
ZooKeeper 适用于存储和协同相关的关键数据，不适合用于大数据量存储。


## 结构
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






## 名词概念



## 基本操作/常用命令


## 简单使用

## 实战入门

## 高级用法


## 学习参考文档
docker安装zookeeper:  https://www.cnblogs.com/caoweixiong/p/12325410.html



