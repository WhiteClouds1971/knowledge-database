---
{"dg-publish":true,"permalink":"/001/it//spring-cloud01/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-03-28T15:26:46.000+08:00","updated":"2024-06-01T09:18:49.616+08:00"}
---

#微服务 #注册中心 #负载均衡
# 认识微服务

![Pasted image 20240328161731.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328161731.png)

## 服务架构演变
### 单体架构

![Pasted image 20240328153424.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328153424.png)

### 分布式架构

![Pasted image 20240328153727.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328153727.png)

### 服务治理

![Pasted image 20240328154035.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328154035.png)

### 微服务

> 微服务是一种经过良好架构设计的**分布式**架构方案。微服务本质上也是一种分布式方案

![Pasted image 20240328154756.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328154756.png)

### 小结

**单体架构特点？**
- 简单方便，高度耦合，扩展性差，适合小型项目。例如：学生管理系统

**分布式架构特点？**
- 松耦合，扩展性好，但架构复杂，难度大。适合大型互联网项目，例如：京东、淘宝

**微服务：一种良好的分布式架构方案**
- 优点：拆分粒度更小、服务更独立、耦合度更低
- 缺点：架构非常复杂，运维、监控、部署难度提高
## 微服务技术对比

![Pasted image 20240328162348.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328162348.png)

## SpringCloud

### 技术组件

![Pasted image 20240328162829.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328162829.png)

### spring cloud和spring boot之间的版本兼容

![Pasted image 20240328163053.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328163053.png)

# 微服务拆分及远程调用
### 导入服务拆分Demo

![Pasted image 20240328163910.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328163910.png)

### 两个服务主要代码

OrderService:
```java
@Service
public class OrderService {

    @Autowired
    private OrderMapper orderMapper;

    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
        // 4.返回
        return order;
    }
}
```

UserService:
```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User queryById(Long id) {
        return userMapper.findById(id);
    }
}
```

### 远程调用

#### 需求：
![Pasted image 20240328164940.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328164940.png)

#### RestTemplate:

>实现使用Java代码发送http请求

1. 在order-service的OrderApplication中注册RestTemplate
```java
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

2. 修改order-service中的OrderService的queryOrderById方法：
```java
@Service
public class OrderService {

    @Autowired
    private OrderMapper orderMapper;

    @Autowired
    private RestTemplate restTemplate;

    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
        String url = "http://localhost:8081/user/" + order.getUserId();
        User user = restTemplate.getForObject(url, User.class);
        order.setUser(user);
        // 4.返回
        return order;
    }
}
```

3. 验证
![Pasted image 20240328170745.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240328170745.png)

总结：
- 基于RestTemplate发起的http请求实现远程调用
- http请求做远程调用是与语言无关的调用，只要知道对方的ip、端口、接口路径、请求参数即可。
## 提供者和消费者

>服务提供者：一次业务中，被其它微服务调用的服务。（提供接口给其它微服务）
  服务消费者：一次业务中，调用其它微服务的服务。（调用其它微服务提供的接口）

**服务A调用服务B，服务B调用服务C，那么服务B是什么角色？**
- 服务提供者：暴露接口给其它微服务调用
- 服务消费者：调用其它微服务提供的接口
- 提供者与消费者角色其实是相对的
- 一个服务可以同时是服务提供者和服务消费者
# eureka注册中心
## 解决了什么问题

![Pasted image 20240401112122.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240401112122.png)

## Eureka的工作流程

![Pasted image 20240401112418.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240401112418.png)

1. 消费者该如何获取服务提供者具体信息？
	- 服务提供者启动时向eureka注册自己的信息
	- eureka保存这些信息
	- 消费者根据服务名称向eureka拉取提供者信息
2. 如果有多个服务提供者，消费者该如何选择？
	- 服务消费者利用负载均衡算法，从服务列表中挑选一个
3. 消费者如何感知服务提供者健康状态？
	- 服务提供者会每隔30秒向EurekaServer发送心跳请求，报告健康状态
	- eureka会更新记录服务列表信息，心跳不正常会被剔除
	- 消费者就可以拉取到最新的信息
## 理论总结

1. 在Eureka架构中，微服务角色有两类：
- EurekaServer：服务端，注册中心
	- 记录服务信息
	- 心跳监控
- EurekaClient：客户端
	- Provider：服务提供者，例如案例中的 user-service
		- 注册自己的信息到EurekaServer
		- 每隔30秒向EurekaServer发送心跳
	- consumer：服务消费者，例如案例中的 order-service
		- 根据服务名称从EurekaServer拉取服务列表
		- 基于服务列表做负载均衡，选中一个微服务后发起远程调用
## 动手实践
### 搭建EurekaServer

1. 创建项目，引入spring-cloud-starter-netflix-eureka-server的依赖
```pom
<dependency>    
	<groupId>org.springframework.cloud</groupId>    
	<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

> 上文中的`spring-cloud-starter-netflix-eureka-server`的`starter`表示Eureka服务端的所有服务spring boot都已经帮我们配置好了。我们只需要引入依赖直接使用就好了。

2. 编写启动类，添加@EnableEurekaServer注解

3. 添加application.yml文件，编写下面的配置：
```yml
server:
  port: 10086
spring:  
  application:
    name: eurekaserver
eureka:
  client:
	service-url: 
      defaultZone: http://127.0.0.1:10086/eureka/
```

### 将client注册到Eureka

1. 在user-service项目引入spring-cloud-starter-netflix-eureka-client的依赖
```pom
<dependency>    
	<groupId>org.springframework.cloud</groupId>    
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2. 在application.yml文件，编写下面的配置：
```yml
eureka:  
  client:    
	service-url:      
      defaultZone: http://127.0.0.1:10086/eureka/
```

**启动多个服务实例**
{ #6c4f8b}


> -Dserver.port=8082
![Pasted image 20240401152524.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240401152524.png)

![Pasted image 20240401152912.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240401152912.png)

### 通过Eureka完成远程调用

1. 修改OrderService的代码，修改访问的url路径，用服务名代替ip、端口：
```java
String url = "http://userservice/user/" + order.getUserId();
```

2. 在order-service项目的启动类OrderApplication中的RestTemplate添加负载均衡注解：
```java
@Bean
@LoadBalanced
public RestTemplate restTemplate() {    
	return new RestTemplate();
}
```

3. 在浏览器中访问`http://localhost:8088/order/103`
![Pasted image 20240401155402.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240401155402.png)

# Ribbon负载均衡原理
## 负载均衡的流程

![Pasted image 20240403101629.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240403101629.png)

## 负载均衡策略

> Ribbon的负载均衡规则是一个叫做IRule的接口来定义的，每一个子接口都是一种规则：

![Pasted image 20240403101937.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240403101937.png)

![Pasted image 20240403102010.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240403102010.png)

## 调整负载均衡的策略

通过定义IRule实现可以修改负载均衡规则，有两种方式：

1. 代码方式：在order-service中的OrderApplication类中，定义一个新的IRule：
```java
@Bean
public IRule randomRule(){
	return new RandomRule();
}
```

2. 配置文件方式：在order-service的application.yml文件中，添加新的配置也可以修改规则：
```yml
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #负载均衡规则 
```

> 第一种方案是全局负载均衡方案配置，配置完之后所有的请求都会使用这个方案进行请求
> 第二种方案是先配置了服务名称，在配置了负载均衡方案。这种只会在请求这个服务的时候才会使用本方案。其他的请求不使用本方案。

## 饥饿加载

Ribbon**默认是采用懒加载**，即第一次访问时才会去创建LoadBalanceClient，请求时间会很长。
而饥饿加载则会在项目启动时创建，降低第一次访问的耗时，通过下面配置开启饥饿加载：
```yml
ribbon:
  eager-load:
	enabled: true # 开启饥饿加载
	  clients: userservice # 对userservice这个服务饥饿加载；对一个服务进行饥饿加载
	  # 对多个服务进行饥饿加载的方法
		# - userservice1 
		# - userservice2
```

## 总结

1. Ribbon负载均衡规则
	- 规则接口是IRule
	- 默认实现是ZoneAvoidanceRule，根据zone选择服务列表，然后轮询
2. 负载均衡自定义方式
	- 代码方式：配置灵活，但修改时需要重新打包发布
	- 配置方式：直观，方便，无需重新打包发布，但是无法做全局配置
3. 饥饿加载
	- 开启饥饿加载
	- 指定饥饿加载的微服务名称
# nacos注册中心

Nacos 是阿里巴巴的产品，现在是 SpringCloud 中的一个组件。相比 Eureka 功能更加丰富，在国内受欢迎程度较高。

## 安装 Nacos

Nacos 是使用 Java 开发的注册中心，所以首先需要在配置 Java 的运行环境。

1. 下载并解压 Nacos 安装包：[下载地址](https://github.com/alibaba/nacos)

![Pasted image 20240407110432.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407110432.png)
Nacos 目录结构
- Bin：启动 Nacos 的可执行目录
- Conf：Nacos 的配置文件
- Target：Nacos 的 jar 包

2. 通过命令行执行 bin 目录下的脚本启动 Nacos
```shell
sh startup.sh -m standalone
```
- -m：启动的模式
- Standalone：单机启动

3. 验证

![Pasted image 20240407111340.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407111340.png)
访问http://localhost:8848/nacos/index.html查看Nacos主页：
![Pasted image 20240407111428.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407111428.png)
## 服务注册到 Nacos

1. 在 cloud-demo 父工程中添加 spring-cloud-alilbaba 的管理依赖：
```xml
<!-- springCloud -->  
<dependency>  
    <groupId>com.alibaba.cloud</groupId>  
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>  
    <version>${spring-cloud-alibaba.version}</version>  
    <type>pom</type>  
    <scope>import</scope>  
</dependency>
```

2. 添加 Nacos 客户端依赖
```xml
<dependency>  
    <groupId>com.alibaba.cloud</groupId>  
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>  
</dependency>
```

3. 修改 user-service&order-service 中的 application. Yml 文件，注释 eureka 地址，添加 nacos 地址：
```yml
spring:  
  cloud:  
    nacos:  
      server-addr: localhost:8848
```

4. 启动 client 服务并验证

![Pasted image 20240407114815.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407114815.png)
## Nacos 服务分级存储模型

Nacos 的服务级别为服务-集群-实例；通过这种分级模型可以大大增加应用的容灾能力。
![Pasted image 20240407115734.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407115734.png)

### 服务跨集群调用问题

![Pasted image 20240407120004.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407120004.png)

### 配置 Nacos 的集群

1. 修改 application. yml，添加如下内容
```xml
spring:
  cloud:  
    nacos:  
      discovery:  
        cluster-name: HZ # 集群名称；例如：HZ，杭州
```

2. 验证

![Pasted image 20240407120800.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240407120800.png)
### 根据集群负载均衡

> 相同集群的实例服务，在调用时优先调用本集群下的服务实例

1. 修改 order-service 中的 application.yml，设置集群为 HZ：
```yml
spring:
  cloud:  
    nacos:  
      discovery:  
        cluster-name: HZ
```

2. 然后在 order-service 中设置负载均衡的 IRule 为 NacosRule，这个规则优先会寻找与自己同集群的服务：
```yml
userservice:  
  ribbon:  
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule #负载均衡规则
```

> [!note] 
> NacosRule 负载均衡策略
> 1. 优先选择同集群服务实例列表
> 2. 本地集群找不到提供者，才去其它集群寻找，并且会报警告
> 3. 确定了可用实例列表后，再采用**随机**负载均衡挑选实例
{ #359201}


### 根据权重负载均衡

实际部署中会出现这样的场景：
- 服务器设备性能有差异，部分实例所在机器性能较好，另一些较差，我们希望性能好的机器承担更多的用户请求
Nacos 提供了权重配置来控制访问频率，权重越大则访问频率越高

1. 在 Nacos 控制台可以设置实例的权重值；
2. 将权重设置为 0.1，测试可以发现 8083 被访问到的频率大大降低：

![Pasted image 20240409110634.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409110634.png)

> [!note] 实例的权重控制：
> 1. Nacos 控制台可以设置实例的权重值，0~1 之间
> 2. 同集群内的多个实例，权重越高被访问的频率越高
> 3. 权重设置为 0 则完全不会被访问

**权重为 0 的应用场景：**
{ #768003}


在服务上线升级的过程中，如果不能完全中断服务的情况下，不能重启所有服务，可以使用一下方案进行升级：

1. 先将一个服务的权重调整为 0 后停止该服务
2. 然后升级重启该服务，先为该服务设置一个叫小的权重如 0.1。
3. 对升级的服务进行小范围测试。如果没有问题的话在调整一个较大的权重。
4. 重复流程升级所有服务。
## 环境隔离

> Nacos 也是一个数据管理中心

Nacos 中服务存储和数据存储的最外层都是一个名为 namespace 的东西，用来做最外层隔离
![Pasted image 20240409112311.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409112311.png)
- Namespace： 上文说的集群是根据业务做分类隔离。但是在实际开发过程中还存在开发、测试、生产不同的应用环境。Namespace 就是用来做不同应用环境之间的隔离。所以不同 Namespace 之间数据不互通。
- Group：可以把业务相关度比较高的服务放到一个组里。比如支付和订单服务；一般情况也不会配置 Group
### 设置命名空间

> [!Warning] 不同命名空间下的服务是不通的！
{ #15cf32}


1. 在 Nacos 控制台可以创建 namespace，用来隔离不同环境

![Pasted image 20240409113044.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409113044.png)

2. 修改 order-service 的 application.yml，添加 namespace：
```yml
spring:  
  cloud:  
    nacos:  
      discovery:  
        namespace: 800c8373-69de-4f5d-ba98-57d2cd0d5915 # 命名空间的ID
```

3. 重启 Orderservice 并在浏览器访问测试：

![Pasted image 20240409113556.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409113556.png)

空间之间数据不互通。所以 order service 访问不了 user service：
![Pasted image 20240409113607.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409113607.png)
>[!note] 
>Nacos 环境隔离:
>每个 namespace 都有唯一 id
>服务设置 namespace 时要写 id 而不是名称
>不同 namespace 下的服务互相不可见
## 临时实例和非临时实例

服务注册到 Nacos 时，可以选择注册为临时或非临时实例，通过下面的配置来设置：
```yml
spring:
  cloud:
    nacos:
      discovery:
        ephemeral: false # 设置为非临时实例
```

临时实例宕机时，会从 nacos 的服务列表中剔除，而非临时实例则不会
## Nacos 注册中心原理

![Pasted image 20240409114515.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240409114515.png)

1. Nacos 与 eureka 的共同点
	- 都支持服务注册和服务定时拉取
	- 都支持服务提供者心跳方式做健康检测
2. Nacos 与 Eureka 的区别
	- Nacos 支持服务端主动检测提供者状态：临时实例采用心跳模式，非临时实例采用主动检测模式
	- Nacos临时实例心跳不正常会被剔除，非临时实例则不会被剔除
	- Nacos 支持服务列表变更的消息推送模式，服务列表更新更及时。Eureka 不支持主动推送
	- Nacos 集群默认采用 AP 方式，当集群中存在非临时实例时，采用 CP 模式；Eureka 采用 AP 方式