# Dubbo
「 #Dubbo 」

官网: httpshttps://cn.dubbo.apache.org/zh-cn/://cn.dubbo.apache.org/zh-cn/

核心: 

## 概念
**「一句话定义」**
Apache Dubbo 是一款微服务开发框架，它帮助解决微服务开发中的通信问题，同时为构建企业级微服务的提供服务治理能力，Dubbo 不绑定编程语言，它的目标是为所有主流语言提供对等的微服务开发体验。
![](image/Pasted%20image%2020230310140035.png)

Apache Dubbo 是一款 RPC 服务开发框架，用于解决微服务架构下的服务治理与通信问题，官方提供了 Java、Golang 等多语言 SDK 实现。使用 Dubbo 开发的微服务原生具备相互之间的远程地址发现与通信能力， 利用 Dubbo 提供的丰富服务治理特性，可以实现诸如服务发现、负载均衡、流量调度等服务治理诉求。Dubbo 被设计为高度可扩展，用户可以方便的实现流量拦截、选址的各种定制逻辑。

在云原生时代，Dubbo 相继衍生出了 Dubbo3、Proxyless Mesh 等架构与解决方案，在易用性、超大规模微服务实践、云原生基础设施适配、安全性等几大方向上进行了全面升级。


## 基本架构
![](image/Pasted%20image%2020230310140106.png)

Dubbo 从架构图上分为数据面和控制面。在数据面，使用 Dubbo 开发的微服务进程间基于 RPC 协议通信。DubboAdmin 控制面作为服务治理的抽象入口，由一系列可选的服务治理组件构成，负责 Dubbo集群的服务发现、流量管控策略、可视化监测。

## 核心能力
### 提供微服务抽象与框架
![](image/Pasted%20image%2020230313184452.png)
首先, Dubbo 作为服务开发框架解决了业务应用中为服务定义,暴露,通信与治理问题, 为业务应用开发定义了一套为服务编程范式. 具体来说, Dubbo为业务应用提供了为服务开发API,RPC协议,服务治理三大核心能力, 让开发者真正的专注业务逻辑开发. 

![](image/Pasted%20image%2020230313184716.png)
Dubbo不是应用框架的替代者, 它可以很好的工作在每种语言的主流编程框架之上, 以Java为例, Dubbo可以很好的与Spring写作, 并在此基础上提供服务定义, 为服务编程, 服务发现, 负载均衡, 流量管控能力.


### 提供灵活的通信协议切换能力
![](image/Pasted%20image%2020230313184940.png)
在通信方面, Dubbo区别于其他RPC框架的是它不绑定特定协议, 你可以在底层选用HTTP./2, TCP, gRPC,REST, Hessian 等任意通信协议, 同时享受统一的api, 以及对等的服务治理能力.

### 一切皆可扩展
![](image/Pasted%20image%2020230313185147.png)
Dubbo 的另一个优势在于其可扩展性设计，从流量管控、协议编码、诊断调优、再到服务治理，你都可以去扩展，满足企业级微服务开发与运维的所有诉求。

### 丰富的生态
![](image/Pasted%20image%2020230313185314.png)

基于扩展能力 Dubbo 官方提供了丰富的生态适配，涵盖了所有主流的开源微服务组件


### 服务网格
![](image/Pasted%20image%2020230313185416.png)
对于服务网格架构，Dubbo也可以轻松接入原生 Istio 体系； 在数据面支持与 Envoy 部署的 Proxy 模式，也支持无 Envoy 的 Proxyless 模式，提供更灵活的数据面选择。

## 名词概念
#todo

## 简单使用
### 第一步 
使用官方脚手架快速创建项目模板，只需要选择依赖的版本、组件，点击 “获取代码” 即可
![](image/Pasted%20image%2020230313185606.png)

### 第二步
将模板项目导入 IDE 开发环境。 定义 Java 接口作为 Dubbo 服务。
![](image/Pasted%20image%2020230313185709.png)


开发 Dubbo 服务端，实现接口并完成业务逻辑编码，通过一条简单的注解配置完成服务发布。
![](image/Pasted%20image%2020230313185732.png)

开发Dubbo 客户端，通过注解声明 Dubbo 服务，然后就可以发起远程方法调用了。至此，开发工作完成。

### 第三步
进入部署环节，我们选择 Kubernetes 作为部署环境。 #Kubernetes

首先，通过一条命令安装 dubbo-admin 等服务治理组件，安装成功之后，我们查看部署状态。接下来，开始部署业务应用，随后查看确认直到应用已经正常启动
![](image/Pasted%20image%2020230322143831.png)

然后，我们就可以打开 Admin 控制台查看服务部署与调用情况了。这里是 Dubbo Admin 控制台的页面显示效果，可以看到刚才启动的 Dubbo 服务部署状态；除此之外，Admin 还提供了更详细的流量监控监测，点击服务统计，可进入监控页面
![](image/Pasted%20image%2020230322143900.png)

你可以在此了解Dubbo 集群的详细运行状态，包括每个应用对外服务和调用服务的情况，QpS、成功率等，还可以查看每个实例的资源健康状况。
![](image/Pasted%20image%2020230322143938.png)

![](image/Pasted%20image%2020230322143953.png)

### 第四步
进行流量管控. 当应用已经品文运行后, 进一步控制流量的访问行为,包括实现金丝雀发布,全链路灰度,动态调整超时时间,调整权重,按比例流量发布,参数路由等.控制台提供了可视化的流量治理规则操作入口, 在这里可以直接下发流量规则.
![](image/Pasted%20image%2020230322144230.png)

以一个线上环境的灰度隔离示例, 通过DUbbo流量管控机制, 我们可以给每个应用的一部分机器打上gray标签, 接下来, 对于入口为gray的流量, 就可以控制确保它只在有gray标记的Dubbo示例内流转, 实现了全链路的逻辑隔离效果, 对于隔离于多套开发环境, 线上灰度测试等场景都非常有用.
![](image/Pasted%20image%2020230322144501.png)

对于同区域优先调用的场景, 这里有两个应用做了多区域部署, 紫色是杭州区域, 蓝色是北京区域, 部署在橙色区域的应用会优先访问同区域的应用, 以此降低访问延迟, 蓝色区域部署的服务亦是如此.

![](image/Pasted%20image%2020230322144637.png)

当应用在同区域部署的实例不可用时, 调用会自动跨区域切换到其他可用的区域, 确保整体的可用性.



## 实战入门

## 高级用法


## 胡柴分享:
### Java的SPI和Dubbo的SPI的区别? 
#JavaSPI
JavaSPI和Dubbo都是服务提供者接口的机制, 都允许开发者在运行时动态的加载实现类并替换默认实现. 它们的主要区别在于以下几点: 
1. 配置文件格式不同: Java SPI要求在META-INFO/services目录下创建以扩展点接口权限定名为名称的文件, 文件内容为实现类的全限定名. 而Dubbo SPI要求在META-INF/dubbo目录下创建以扩展点接口全限定名为名称的文件, 文件内容为实现类的全限定名.
2. 实现方式不同: Java SPI是基于Java标准的SPI机制实现的, 使用ClassLoader来加载实现类. Dubbo SPI是在Java标准SPI机制的基础上进行了扩展和优化, 使用Adaptive机制来动态生成代理类, 并使用ExtensionLoader来加载和管理实现类.
3. 扩展性不同: Dubbo SPI相比Java SPI更加灵活, 提供了更多的扩展点和扩展机制, 同时还支持动态配置和动态扩展. Dubbo SPI还支持对扩展点实现类的自动注入和依赖注入, 同时提供了更加完善的SPI扩展文档和示例, 方便开发者使用和扩展.
4. 功能不同: Dubbo SPI 不仅提供了服务提供者接口机制, 还提供了服务消费者接口机制, 支持服务注册, 发现, 负载均衡, 容错, 路由哦, 过滤器等功能. 同时, Dubbo SPI还提供了更加完善的配置机制, 支持多协议, 多注册中心, 多负载均衡等复杂场景下的灵活配置.
综上所述, 虽然Java SPI和Dubbo SPI 都是服务提供者接口的机制, 但Dubbo SPI相比Java SPI更加灵活, 功能更加完善,扩展性更强, 是一种更加适合于大型分布式系统开发的SPI机制.

#### 还有什么实现方式?
除了Java的SPI和Dubbo的SPI，还可以使用反射机制实现在运行时动态加载实现类并替换默认实现。具体来说，可以通过以下步骤实现：  
1.  在需要动态加载实现类的地方，获取默认实现类的实例对象，并记录其类名。  
2.  根据需要加载的实现类类名，使用Class.forName()方法加载对应的Class对象。  
3.  判断加载的Class对象是否为默认实现类的子类，如果是，则使用反射机制创建实现类对象并替换默认实现类对象。  
4.  调用实现类的方法，完成需要的操作。  
需要注意的是，使用反射机制动态加载实现类需要谨慎处理异常，防止出现类找不到、无法创建实例等异常情况。
例子: 假设我们有一个接口`MessageSender`和其默认实现类`DefaultMessageSender`，现在需要在运行时动态加载一个名为`AlternativeMessageSender`的实现类来替换默认实现类。以下是一个简单的例子，实现步骤如下：

```java
public interface MessageSender {
    void sendMessage(String message);
}

public class DefaultMessageSender implements MessageSender {
    @Override
    public void sendMessage(String message) {
        System.out.println("DefaultMessageSender: " + message);
    }
}

public class AlternativeMessageSender implements MessageSender {
    @Override
    public void sendMessage(String message) {
        System.out.println("AlternativeMessageSender: " + message);
    }
}
```

  1.  获取默认实现类的实例对象，并记录其类名。
     
```java
MessageSender defaultSender = new DefaultMessageSender();
String defaultClassName = defaultSender.getClass().getName();
```

2.  根据需要加载的实现类类名，使用Class.forName()方法加载对应的Class对象。
   
```java
String alternativeClassName = "AlternativeMessageSender";
Class<?> alternativeClass = Class.forName(alternativeClassName);
```

3.  判断加载的Class对象是否为默认实现类的子类，如果是，则使用反射机制创建实现类对象并替换默认实现类对象。
   
```java
if (defaultClassName.equals(alternativeClass.getSuperclass().getName())) {
    MessageSender alternativeSender = (MessageSender) alternativeClass.newInstance();
    defaultSender = alternativeSender;
}
```

4.  调用实现类的方法，完成需要的操作。
   
```java
defaultSender.sendMessage("Hello World!"); // 输出 "AlternativeMessageSender: Hello World!"
```

需要注意的是，这里只是一个简单的示例，实际应用中需要考虑更多的异常处理情况，如类找不到、无法创建实例等异常。同时，在使用反射机制时也需要谨慎考虑性能和安全问题。



Dubbo3新特性

服务发现

Triple协议

  

  

自定义二进制协议

4 1 2 2 

  

  

  

重试带来的问题?

  

容错

  

异步调用

  

上下文

log , userID-方法名

  

泛化调用  
写一个接口 规定规则  其他服务去按照规则实现


## 学习参考文档

[官方文档](https://cn.dubbo.apache.org/zh-cn/overview/home/)
[一文帮你快速了解 Dubbo 核心能力](https://cn.dubbo.apache.org/zh-cn/blog/2023/02/23/%E4%B8%80%E6%96%87%E5%B8%AE%E4%BD%A0%E5%BF%AB%E9%80%9F%E4%BA%86%E8%A7%A3-dubbo-%E6%A0%B8%E5%BF%83%E8%83%BD%E5%8A%9B/)

[Dubbo快速入门](http://t.csdn.cn/xYbZZ)

