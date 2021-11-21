# SpringCloud笔记

## 1.架构演进

### 1.1 单体架构

**单体架构:**

​	我们最初的项目开发都是单体架构,也就是说把所有的模块和组件都放在一个应用最终打成一个(war、jar)包放在一个Tomcat服务器上,并且只享有一个数据库;

好处:

1. 易于开发 ：架构简单，技术成本低
2. 易于测试 ：所有功能在一个项目，方便测试
3. 易于部署 ：一个Tomcat就可以实现部署，简单方便

这种架构方式只适合小型项目并发量不是很大的场景,`单体架构出现代码量、用户量和并发量就造成单体瓶颈`;一旦遇到高流量并发那么就会出现很多缺点就随之暴露出来了:

1. 代码臃肿不方便开发维护(代码可读性差)
2. 代码编译系统启动变慢
3. 系统扩展性能变差(牵一发而动全身)
4. 无法针对某一个业务做扩展(集群)
5. 对大数据量,高并发量的处理不占优势
6. 技术选型单一
7. 模块/业务耦合度高

**集群:**

​	就是把应用复制多份部署多份,每个应用都部署在不同的机器上(节点), 集群中的每个应用都是完整的功能, 如果挂掉其中一个或者一部分节点,集群仍然可用!

举个例子:就好比是超市收银一样,之前由于客人不多那么使用一台收银机子那么就可以完全够用,但是如果人流量增多大家都挤着排队等着一台收银机子,那么效率自然就变得很差,客户的使用体验也变差:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/u=952521477,3058193742&fm=26&fmt=auto)

所以就会增加多个收银台来完成`相同`的工作-----这就是集群!

理解:集群就是将应用复制多份然后部署在不同的服务器商,然后每台服务器上完成的功能都是一模一样的,目的就是为了减轻单体应用所带来的的并发压力,并且任何一台服务器挂了也不影响整体的运行!

**负载均衡:**

​	负载均衡说白了就是请求的路由、请求的分发 、就是将请求`均衡的分发`到服务器上，之前说集群就是将服务器复制多份,那么每个服务器都有自己的ip,用户请求并不知道该请求谁,那么这个时候就有负载均衡Nginx,用户只需请求到Nginx,而Nginx里保存了集群每个服务器的ip,然后根据自己的算法来将请求分配到其中一个集群服务器

![mark](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cDovL3E2M2YyZWFjMy5ia3QuY2xvdWRkbi5jb20vYmxvZy8yMDIwMDIyNC8zdmhtTGxJRkhhS2QucG5n)

也就是说集群就是将应用复制多份,每台服务器完成的功能都是一模一样的,可是无论如何最终只有一台服务器来完成请求,那么如何决定呢?这个时候Nginx就来了,那客户端是不知道该访问哪个应用节点的，这个时候我们就需要有一个`请求分发的功能的组件`(负载均衡器)将客户端的请求相对平均的分发多个应用节点上，这就是负载均衡，这个做请求分发的组件就是负载均衡器

理解:说白了Nginx就是将请求拿到之后由它内部的算法来决定将请求分发到哪台服务器上去执行;

负载均衡算法:

- 轮询(round robin):依次将请求分配到各个后台服务器中，默认的负载均衡方式
- 权重(weight) ： 根据权重值将请求分配到后台服务器中，权重值越大，分配比例越高
- IP_HASH：按照ip地址进行hash运算，同一ip的请求会被分配到相同的机器上
- url_hash ： 根据请求的url的hash值将请求分到不同的机器中。
- fair：根据服务器响应时间来分发请求，时间越短分发的请求越多

**总结:**

虽然做了集群可以缓解服务器的压力但是终究还是处于一个单体架构的一种方式,但仍有很多不足:

- 项目本身任然是单体，代码依然臃肿
- 面对海量数据数据库会成为性能瓶颈(需要分库分表)
- 持续交互能力差，代码臃肿，业务复杂，开发维护时间长，新人入手成本高。

所以当单体应用不能满足`复杂的业务`和`海量数据`的系统时，我们需要考虑其他的架构方式。

### 1.2 分布式架构

**分布式:**

​	把一个大的系统, 按业务拆分成多个子系统,每个子系统只负责其中一部分功能,多个子系统一起组合在一起才是一个完整的系统,系统之间需要相互协调共同调用,完成工作,缺一不可!

举个例子: 就好比是在厨房工作为了要给客户上一道佛跳槽这道菜,一个厨师忙里忙外又要切菜、准备菜品、调味等等,一个人不停的干就会一直忙不停如果此时再增加其他菜那么肯定是忙不停的!所以此时就会给增加多个厨师,这个厨师切菜,那个厨师炒菜,这样每个人只会干自己所负责的事情,都不会收到干扰,提高了整体的效率!

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/u=367974782,436195706&fm=253&fmt=auto&app=120&f=JPEG)

注意:如果分布式有个子系统挂了怎么办? 那么单独针对这个子系统做集群, 这样如果有一个节点挂了也不影响整体运行!

理解:多个人在一起做不同的事情，多人和在一起才是一件完整的事情，有点0.5+0.5=1的感觉

**分布式与集群的区别:**

集群:就是将应用复制多份,类似于:1+1=2

分布式:将应用拆分成子应用,类似于:0.5+0.5=1

集群的出现是为了解决高并发的问题,而分布式是为了解决程序耦合的问题

```
自己理解:
	集群就是将一个系统复制多份到多台服务器上运行,这样一个节点挂了也不影响整个系统运行, 目的就是解决服务器的压力而分布式则是将系统中的业务模块拆分成一个一个子系统,而这些子系统组合在一起就形成一个完整的系统,目的就是为了解决程序代码模块耦合的问题, 单体架构不好做模块扩展,比如说手机支付模块并发量大,在单体架构中就不好做扩展,因为是处于一个tomcat服务中,所以分布式则就将支付模块独立拆分出去.成为一个独立的子系统,然后 使用集群的横向扩展,处理并发量的问题!
```

**面向服务的架构SOA:**

​	是分布式上的基础进行演变的,将系统中的系统模块进行拆分成子系统,而在这个子系统中,比如将支付系统拆分成表现层和服务层, 这样请求到表现层,由表现层去调用服务层, 这样的好处就是让系统内互相调用, 拓展性更高!解耦!它的思想是每个子应用可以通过网络通信协议向其他子应用提供服务或者消费服务

理解:

​	就是在分布式将业务拆分成子模块的基础上再一次拆分,也就是说支付模块拆分成Controller与service层, 然后之间通过HTTP网络协议进行调用,当然这样拆分了之后你可以直接调用其他模块,比如支付模块的Controller可以调用用户模块的Service层!总之就是通过网络协议可以调用其他服务或者被调

这样的结构划分提高了应用的`重用性`，业务逻辑也变得`可组合`

SOA架构中有重要的两个角色，`服务消费者`(Consumer)和`服务提供者`(Provider)即服务调用者和服务被调用者，这样的架构优点有:

1. 模块拆分，使用API通信，降低模块之间的耦合度
2. 项目拆分多个子应用，每个子应用业务简单，代码简单，方便维护开发。
3. 不同技术人员可以负责不同的子应用
4. 提高服务之间的重用性，业务逻辑可组合。

其缺点在于:

1. 服务之间的API接口开发增加了工作量;
2. SOA服务之间的网络通信调用对性能有一定的影响(尽管很小)
3. 相对于单体应用来说，技术,人力成本较高。
4. 部署和运维相对麻烦

> 总结:

我们可以得知服务的架构的演变是:`单体架构->集群(负载均衡)->分布式->SOA(面向服务的架构)`



记住:集群就是扩展几台服务器完成相同的功能目的就是解决并发问题!而分布式是将一个应用拆分成几个子应用目的就是为了解决代码的耦合问题和业务模块的扩展!而SOA建立在分布式模块拆分的基础上对模块进行面向服务化,让表示层和业务层之间脱离耦合,两者之间调用使用网络通信协议来进行消费!

### 1.3 微服务架构

微服务架构可以说是在SOA架构上的一种发展!

> 什么是微服务:

​	微服务架构是一种`架构模式`,或者说一种架构风格,它提倡将单一的应用程序划分成一组小的服务,每个服务运行在独立的自己的进程内,服务之间互相协调、互相配置,为用户提供最终价值;而服务之间采用轻量级的通信机制(HTTP的RESTful API),每个服务都围绕着具体的业务进行构建,并且能够被独立的部署到生产环境中,另外,应尽量避免统一的、集中式的服务管理机制,对具体的一个服务而言,应该根据业务上下文,选择合适的语言,而工具(maven)对其进行构建, 可以有一个非常轻量级的集中式管理来协调这些服务,可以使用不同的语言来编写服务,也可以使用不同的数据存储.

```
从以上描述中我们可以找到几个关键字:
	微服务是一种架构模式是一种架构思想,将应用拆分成一组小的服务,而每个服务都独立运行在进程中!服务之间采用HTTP通信机制,并且服务能够独立的部署到生产环境中!每个服务可以用不同的语言来编写并且也可以使用不同的数据库来进行存储,每个服务协调来完成整个系统的业务!
```

再来从技术维度理解下:

​	微服务化的核心就是将传统的一站式应用,根据业务拆分成一个个的服务,彻底的去耦合,每个微服务提供单个业务功能的服务,一个服务做一件事情,从技术角度看就是一种小而独立的处理过程,类似进程的概念,能够自行单独启动或销毁,拥有自己独立的数据库!

tip:一站式的意思就是全套服务啥都做;

> 微服务:

​	强调的是服务的大小,它关注的是某一个点,是具体的解决某一个问题/提供落地对应服务的一个服务应用;狭义来看:可以看作idea中一个个微服务工程或者Moudel。IDEA工具里面使用Maven开发的一个个独立的Moudel,它具体是使用SpringBoot开发的一个小模块,专业的事情交给专业的模块来做,一个模块就做着一件事情;

​	强调的是一个个的个体,每个个体完成一个具体的任务或者功能。

> 特点：

总结下微服务的特点：

1. 由多个服务组成完整的系统
2. 每个服务都是独立的，有自己的进程
3. 服务之间使用HTTP协议通信
4. 不同的服务可以使用不同的编程语言
5. 不同的服务的数据库可以多样化选择
6. 微服务是一个分布式系统

> 优缺点：

在项目规模较大的时候，相对于单体应用来说，微服务具备很多优势，主要体现在如下方面：

1. 单个服务业务简单，代码简单方便开发维护
2. 服务之间无耦合，服务之间升级维护互不影响
3. 轻量级HTTP通信机制，使得的不同的服务可以采用不同的编程语言
4. 微服务有极强的扩展能力,业务量大的服务可以再次拆分服务，也可以进行集群部署，剔除服务也很方便
5. 更大的系统负载能力和容错能力(集群)
6. 对于开发人员来说,通常只需要关注单一服务,新员工上手也比较快
7. 微服务架构对现在流行的敏捷开发支持优化

凡是都有双面性，微服务展示出了他都优势之处，同时也有其不足的地方，主要体现如下方面：

- 分布式事务 ：服务通信机制增加了事务的复杂性，架构师要选择合适的分布式方案(CAP理论)
- 部署麻烦 ：微服务众多，部署麻烦，需要借助容器技术和自动化部署工具，这又增加了开发人员的学习成本。
- 技术成本高 ：微服务架构本身比较复杂,技术成本高，开发人员需要花更多的时间学习相关技术。
- 服务通信对性能的损耗 ： 微服务架构一定要考虑服务通信延迟对服务调用性能的损耗问题，开发人员需要选择合适的通信方式解决这一问题。

> 理解

分布式：一个系统的各个组成部分,分开不住在不同的主机上---重在部署,分布

微服务:服务的划分,服务化的! 可以把你的应用拆分成很小的!

**可以说微服务是分布式的,但是不能说分布式就是微服务!**

1. 分布式部署（数据库一台主机，Redis一台主机，项目一台主机）
2. 分布式项目 （订单一个项目-主机，支付一个项目-主机，用户一个项目-主机）

那么也就是说：

​	微服务 ：一定是分布式项目，也是部署在不同的主机上 ，但是他更注重服务的划分，服务的治理。

理解：分布式则是将订单系统拆分成一个子系统,而在这个子系统中进行更小细粒度的拆分,将下单服务或者查询订单服务再拆分 ,而各种服务之间进行通信,而每个服务采用不同的语言编写,拓展性非常强! 出现问题只需要关注某个服务即可!



总的架构演变过程：

单体架构遇到并发瓶颈-》集群+负载均衡-》代码耦合度高进行分布式拆分-》把服务层独立出来，让请求可重组（SOA）-》微服务：基于SOA进行服务拆分，把一个个小的系统中的服务再度拆分成独立的进程

> SOA和微服务的区别

仔细发现好像SOA和微服务都是将服务再度拆分那么这两者有啥区别呢？

​	其实微服务跟SOA本质上并无太大区别，我倒觉得微服务其实就是随着互联网的发展，复杂的平台、业务的出现，导致SOA架构向`更细粒度`、`更通用化`程度发展，就成了所谓的微服务了。以这种说法做为根据，我觉得SOA与微服务的区别在于如下几个方面：

1. 微服务相比于SOA更加精细，微服务更多的以独立的进程的方式存在，互相之间并无影响；
2. 微服务提供的接口方式更加通用化，例如HTTP RESTful方式，各种终端都可以调用，无关语言、平台限制；
3. 微服务更倾向于分布式去中心化的部署方式，在互联网业务场景下更适合；

> 微服务技术栈有哪些?

|             微服务技术条目             |                           落地技术                           |
| :------------------------------------: | :----------------------------------------------------------: |
|                服务开发                |               SpringBoot、Spring、SpringMVC等                |
|             服务配置与管理             |            Netfix公司的Archaius、阿里的Diamond等             |
|             服务注册与发现             |                 Eureka、Consul、Zookeeper等                  |
|                服务调用                |                       Rest、PRC、gRPC                        |
|               服务熔断器               |                       Hystrix、Envoy等                       |
|                负载均衡                |                       Ribbon、Nginx等                        |
| 服务接口调用(客户端调用服务的简化工具) |                           Fegin等                            |
|                消息队列                |                 Kafka、RabbitMQ、ActiveMQ等                  |
|            服务配置中心管理            |                  SpringCloudConfig、Chef等                   |
|           服务路由(API网关)            |                            Zuul等                            |
|                服务监控                |            Zabbix、Nagios、Metrics、Specatator等             |
|               全链路追踪               |                   Zipkin、Brave、Dapper等                    |
|            数据流操作开发包            | SpringCloud Stream(封装与Redis，Rabbit，Kafka等发送接收消息) |
|              时间消息总栈              |                       SpringCloud Bus                        |
|                服务部署                |               Docker、OpenStack、Kubernetes等                |

> 各微服务框架对比

| 功能点/服务框架 | Netflix/SpringCloud | Motan | gRPC | Thri t | Dubbo/DubboX |
| :------------------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 功能定位 | 完整的微服务框架 | RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册发现 | RPC框架 | RPC框架 | 服务框架 |
| 支持Rest | 是，Ribbon支持多种可拔插的序列号选择 | 否 | 否 | 否 | 否 |
| 支持RPC | 否 | 是(Hession2) | 是 | 是 | 是 |
| 支持多语言 | 是(Rest形式) | 否 | 是 | 是 | 否 |
| 负载均衡 | 是(服务端zuul+客户端Ribbon)，zuul-服务，动态路由，云端负载均衡Eureka（针对中间层服务器） | 是(客户端) | 否 | 否 | 是(客户端) |
| 配置服务 | Netfix Archaius，Spring Cloud Config Server 集中配置 | 是(Zookeeper提供) | 否 | 否 | 否 |
| 服务调用链监控 | 是(zuul)，zuul提供边缘服务，API网关 | 否 | 否 | 否 | 否 |
| 高可用/容错 | 是(服务端Hystrix+客户端Ribbon) | 是(客户端) | 否 | 否 | 是(客户端) |
| 典型应用案例 | Netflix | Sina | Google | Facebook | |
| 社区活跃程度 | 高 | 一般 | 高 | 一般 | 2017年后重新开始维护，之前中断了5年 |
| 学习难度 | 中等 | 低 | 高 | 高 | 低 |
| 文档丰富程度 | 高 | 一般 | 一般 | 一般 | 高 |
| 其他 | Spring Cloud Bus为我们的应用程序带来了更多管理端点 | 支持降级 | Netflix内部在开发集成gRPC | IDL定义 | 实践的公司比较多 |

## 2. Spring Cloud

### 2.1 什么是springCloud?

Spring官网：https://spring.io/

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9zczEuYmRzdGF0aWMuY29tLzcwY0Z1WFNoX1ExWW54R2twb1dLMUhGNmhoeS9pdC91PTM1NzgwMTc3ODAsMTIxODQyMDAyOSZmbT0yNiZncD0wLmpwZw)

![format_png 1](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9zczIuYmRzdGF0aWMuY29tLzcwY0Z2blNoX1ExWW54R2twb1dLMUhGNmhoeS9pdC91PTE2Mzc3ODY4OTcsNzQ1MTM1MjUxJmZtPTI2JmdwPTAuanBn)

SpringCloud下载：

官网：http://projects.spring.io/spring-cloud/

![image-20211117170658773](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117170658773.png)

可以发现SpringCloud没有采用数字编号的方式命名版本号，而是采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序；

比如最早的Realse版本:Angel,第二个Realse版本:Brixaton,然后是Camden、Dalston、Edgware，目前最新的是Hoxton SR4 CURRENT GA通用稳定版。

这里推荐基本自学参考书：

- SpringCloud Netflix 中文文档：https://springcloud.cc/spring-cloud-netflix.html
- SpringCloud 中文API文档(官方文档翻译版)：https://springcloud.cc/spring-cloud-dalston.html
- SpringCloud中国社区：http://springcloud.cn/
- SpringCloud中文网：[https://springcloud.cc](https://springcloud.cc/)

-------

**微服务**:是一种架构思想,是一种架构理论

**SpringCloud**:是微服务的落地实现的解决方案!

简介:

​	Spring cloud是一个基于Spring Boot实现的`服务治理工具包`，用于微服务架构中管理和协调服务的。Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地`简化了分布式系统基础设施的开发`，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。有了SpringCloud之后，让微服务架构的落地变得更简单。	

总之:Cloud只是一个工具包,是基于SpringBoot来实现的,所以我们说SpringBoot是一个微服务框架!通过SpringBoot自动配置封装屏蔽了复杂的配置和实现原理!

**面试题:SpringCloud和SpringBoot的关系?**

1. SpringCloud是基于springBoot的
   - SpringBoot可以离开SpringCloud独立使用，开发项目，但SpringCloud离不开SpringBoot，属于依赖关系；
2. SpringBoot是一个微服务框架,SpringCloud是一个`微服务治理框架`
   - SpringBoot专注于快速、方便的`开发单个个体`微服务，SpringCloud`关注全局的`服务治理框架；
3. SpringBoot开发单个微服务, SpringCloud`协调和管理`各个微服务

理解:核心就这几句话:SpringCloud是基于SpringBoot的,有依赖关系;SpringCloud突出一个关键字"**治理**"!

### 2.2 Dubbo 和 SpringCloud技术选型

分布式+服务治理Dubbo

- 目前成熟的互联网架构，应用服务化拆分 + 消息中间件

Dubbo和SpringCloud对比:

可以看下社区活跃度:

Dubbo:https://github.com/apache/dubbo

SpringCloud:https://github.com/spring-cloud

对比结果：

|                | Dubbo         | SpringCloud                  |
| -------------- | ------------- | ---------------------------- |
| 服务注册中心   | Zookeeper     | Spring Cloud Netfilx Eureka  |
| 服务调用方式   | RPC           | REST API                     |
| 服务监控       | Dubbo-monitor | Spring Boot Admin            |
| 断路器（熔断） | 不完善        | Spring Cloud Netfilx Hystrix |
| 服务网关       | 无            | Spring Cloud Netfilx Zuul    |
| 分布式配置     | 无            | Spring Cloud Config          |
| 服务跟踪       | 无            | Spring Cloud Sleuth          |
| 消息总栈       | 无            | Spring Cloud Bus             |
| 数据流         | 无            | Spring Cloud Stream          |
| 批量任务       | 无            | Spring Cloud Task            |

最大区别：**SpringCloud抛弃了Dubbo的RPC通信,采用的是基于HTTP的REST方式**

严格来说:

​	这两种方式各有优劣。虽然从一定角度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题，而且REST相比RPC更为灵活，服务提供方式和调用方式的依赖只依靠一纸契约，不存在代码级别的强依赖，这个优点在当下强调快速演化的微服务环境下，显得更加合适。

​		Dubbo跟SpringCloud最显著的区别是Dubbo的定位只是一个RPC框架，相比SpringCloud来说它缺少很多功能模块，如：网关，链路追踪等，所以往往在使用Dubbo作为微服务开发框架的时候，还需要去配合其他的框架一起使用，如:加入zookeeper作为注册中心。不过后面Spring Cloud Alibaba 整合了Dubbo!

**通信协议不一样**:

​	Dubbo的通信方式是RPC，基于原声的tcp,性能较好，而SpringCloud的通信方式基于Http协议，虽然底层基于tcp，但是Http的封装过于臃肿，但是使用Http好处在于互相通信的两个服务可以使用不同的变成语言去编写，只要他们都支持Http通信即可互相调用，而Dubbo只支持Java,当然Dubbo交给Apache维护之后做了升级，Dubbo在以后不仅仅支持Java。



两者比起来其实更像一个品牌机和组装机的区别！还有社区支持与更新力度的区别

总结：

​	二者解决的问题域不一样：Dubbo的定位`是一款RPC框架`，而SpringCloud的目标是`微服务架构下的一站式解决方案`！

​	Dubbo只是提供了服务的调用,其他的技术都是不成熟不支持的!SpringCloud为微服务提供了完整了解决生态



> 服务通信协议:

在微服务中服务间通过网络进行通信，那么服务间的远程调用方式有哪些呢？

1. RPC

   ```
   解释:远程过程调用，类似的还有RMI。自定义数据格式，基于原生TCP通信，速度快，效率高。早期的webservice，现在热门的dubbo，都是RPC的典型
   ```

2. HTTP

   ```
   Http其实是一种网络传输协议，基于TCP，规定了数据传输的格式。现在客户端浏览器与服务端通信基本都是采用Http协议。也可以用来进行远程服务调用。缺点是消息封装臃肿。 现在热门的Rest风格，就可以通过http协议来实现
   ```

总结:

RPC:远程过程调用,基于原生TCP原生协议,效率高!

HTTP:SpringCloud基于Http的RESTFul调用



### 2.3 常用组件

SpringCloud为一系列的微服务难题提供了相应的组件来解决:

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200329232256382.png)

- **EurekaServer:服务的注册与发现**
  - 服务之间需要通信,添加或删除都需要调整每个服务之间的通信地址,要非常麻烦,而现在增加了EurekaServer,它会将所有的微服务统计在一起, 统一管理, 可理解为它就是一个班的班主任的角色!总的来说EurekaServer就是服务的注册与发现,它管理了所有的微服务,形成了一个服务清单(保存微服务的ip,端口名....总之服务的信息)
  
- **ConfigServer:配置文件的统一管理**
  - 对整个微服务的配置文件统一管理,这样修改要方便很多!
  
- **zuul:网关**
  - 可理解为小区的大门,所有的请求都需要经过zuul之后才能到达目标服务; 作用: 安全校测、权限控制、监控,微服务的访问路口

- **Feigin\Ribbon:客户端负载均衡**
- Nginx是一个独立的服务,而以上两个都需要集成在项目客户端中;  也就是说整个微服务中,A微服务调用B微服务,而B微服务是一个集群, 那么调用哪个呢?这个时候就会在A微服务装载了一个Ribbon或者Feigin负载均衡来决定

- **Hystrix:熔断机制**
  - 当A服务调用B服务失败了,也就是B服务出现故障了那么就对B服务进行一个熔断机制,不让这个异常延伸到A服务 ,不让这个请求挂掉. 总的来说Hystrix就是解决服务故障的!
- **BUS:消息总线**
  - 是发消息给所有的微服务,类似于广播!
- **Sleuth:微服务链路追踪**
  - 微服务之间的链路关系. 也就是能看到微服务之间调用的关系通过图示来看!链路追踪的作用就是来监控维护之间的调用关系,让程序员方便直观的感受到一个请求经历了哪些微服务，以及服务的请求时间，是否有异常等

 总的来说组件都有这些:

![image-20211118083457513](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118083457513.png) 

## 3.Eureka-服务注册中心

> 什么是Eureka

- Eureka是Netflix的有个子模块，也是核心模块之一。
- Eureka是基于REST的服务，用于定位服务，以实现云端中间件层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务注册与发现，`只需要使用服务的标识符就可以访问到服务`，而不需要修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper.

理解:Eureka的好处就在于A服务调用B服务不需要去频繁的修改服务通信地址,只需要通过该服务的标识符就可调用!就跟我们之前理解的那样:Eureka会管理所有的服务形成一个服务清单!简单理解为`管理服务及通信地址`

> Eureka的工作原理

1. 服务注册

   ```
   微服务在启动的时候就会注册,从EurekaClient往EurekaServer注册自己服务的信息(通信地址如:服务名,ip,端口等)!服务中心形成自己的注册表
   ```

2. 服务发现

   ```
   	调用方称消费者, 被调用称之为提供者,  消费者会每隔30秒从注册中心拉取一次注册清单(提供者也可以拉取清单它也可以是消费者);  拉下来之后就会缓存到本地,然后消费者在调用提供者的时候就会从本地缓存中调用消费清单中拿到ip通信地址,然后发起调用!
   	当一个微服务在向另一个微服务发起调用的时候会根据目标服务的服务名找到其通信地址，然后基于HTTP协议向目标服务发起请求
   	如果每次都要访问注册中心拿取ip通信地址,就会造成性能损耗大!所以先缓存到本地再从本地获取
   ```

3. 服务续约

   ```
   如果有一个微服务挂了那么注册中心的注册清单里是否要删除它呢? 此时就有一个心跳机制, 每30秒就会往注册中心发起一个心跳请求报告给注册中心说我还活着!3次续约失败, 这个服务就会被注册中心剔除
   ```

4. 服务下线

   ```
   自己主动正常下线, 就会往注册中心发起下线请求,然后从注册中心清单中移除掉
   ```

Eureka的思想:

![image-20211118083602390](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118083602390.png)

> Eureka的工作流程

![mark](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cDovL3E2M2YyZWFjMy5ia3QuY2xvdWRkbi5jb20vYmxvZy8yMDIwMDMxOS80dzBTeGppZ2p5eHgucG5n)

解析:支付服务和用户服务在启动的时候就会往注册中心进行"服务注册"将自己的服务名、ip等端口注册进去，注册中心拿到后就会形成服务通信地址清单，这个清单里面就包含每个服务的通信地址等等；然后微服务每隔30秒就会向注册中心拉取注册清单然后缓存到本地中，然后消费者在调用提供者的时候，就会从本地缓存的注册清单中获取地址然后调用!

> 实战

1. eureka-server

   1. pom.xml

      ```xml-dtd
      <!--导包~-->
      <dependencies>
          <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
          <!--导入Eureka Server依赖-->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-eureka-server</artifactId>
              <version>1.4.6.RELEASE</version>
          </dependency>
      </dependencies>
      ```

   2. application.yml

      ```yaml
      server:
        port: 7001
      # Eureka配置
      eureka:
        instance:
          # Eureka服务端的实例名字
          hostname: 127.0.0.1
        client:
          # 表示是否向 Eureka 注册中心注册自己(这个模块本身是服务器,所以不需要)
          register-with-eureka: false
          # fetch-registry如果为false,则表示自己为注册中心,客户端的化为 ture
          fetch-registry: false
          # Eureka监控页面~
          service-url:
            defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      ```

   3. 主启动类

      ```java
      /**
       * @Auther: csp1999
       * @Date: 2020/05/18/10:26
       * @Description: 启动之后，访问 http://127.0.0.1:7001/
       */
      @SpringBootApplication
      // @EnableEurekaServer 服务端的启动类，可以接受别人注册进来~
      @EnableEurekaServer
      public class EurekaServer_7001 {
          public static void main(String[] args) {
              SpringApplication.run(EurekaServer_7001.class,args);
          }
      }
      ```

   4. 启动访问: http://localhost:7001/

      ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130420201.png)

2. eureka-client

   1. 导入依赖:

      ```xml-dtd
      <!--Eureka依赖-->
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
      </dependency>
      ```

   2. application中新增Eureca配置

      ```yaml
      # Eureka配置：配置服务注册中心地址
      eureka:
        client:
          service-url:
            defaultZone: http://localhost:7001/eureka/ #注册中心的注册地址
          registry-fetch-interval-seconds: 5 # 修改客户端拉取注册中心注册表的时间
      ```

   3. 为主启动类添加[@EnableEurekaClient](https://github.com/EnableEurekaClient)注解

      ```java
      /**
       * @Auther: csp1999
       * @Date: 2020/05/17/22:09
       * @Description: 启动类
       */
      @SpringBootApplication
      // @EnableEurekaClient 开启Eureka客户端注解，在服务启动后自动向注册中心注册服务
      @EnableEurekaClient
      public class DeptProvider_8001 {
          public static void main(String[] args) {
              SpringApplication.run(DeptProvider_8001.class,args);
          }
      }
      ```

   4. 先启动7001服务端后启动8001客户端进行测试，然后访问监控页http://localhost:7001/ 产看结果如图，成功

      ![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130507106.png)

> 看到结果说明我们已经将服务注册到服务端注册中心去了,以下就是其他的配置:

1. 修改Eureka上的默认描述信息

   ```yaml
   # Eureka配置：配置服务注册中心地址
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
     instance:
       instance-id: springcloud-provider-dept-8001 #修改Eureka上的默认描述信息
   ```

2. 修改结果:

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130659314.png)

------

1. 如果此时停掉springcloud-provider-dept-8001 等**30s**后 监控会开启保护机制：

![123](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130557974.png)

2. 修改client的yaml:

   ```yaml
   registry-fetch-interval-seconds: 5 # 修改客户端拉取注册中心注册表的时间
   ```

------

1. 此时鼠标悬停地址上就会出现:

   ![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130718511.png)

2. 修改为使用ip注册到Eureka去:

   ```yaml
   eureka:
   	instance: # 修改的是client客户端,服务端不修改
   		prefer-ip-address: true #使用IP注册到Eureaka
   ```

-----

配置关于服务加载的监控信息

pom.xml中添加依赖:

```xml-dtd
<!--actuator完善监控信息-->
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

application.yml中添加配置

```yaml
# info配置
info:
# 项目的名称
  app.name: 源码项目
# 公司的名称
  company.namee: MiaoDaWei之家
# 注意:这里只要是info下面的都可以,名字自己定!
```

然后启动重新刷新点击进入就可以看到显示如下内容:

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521130756988.png)

![image-20211117201646293](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117201646293.png)

> ##### EureKa自我保护机制：好死不如赖活着

一句话总结就是：**某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！**

- 默认情况下，当eureka server在一定时间内没有收到实例的心跳，便会把该实例从注册表中删除（**默认是90秒**），但是，如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制，比如在开发测试时，需要频繁地重启微服务实例，但是我们很少会把eureka server一起重启（因为在开发过程中不会修改eureka注册中心），**当一分钟内收到的心跳数大量减少时，会触发该保护机制**。可以在eureka管理界面看到Renews threshold和Renews(last min)，当后者（最后一分钟收到的心跳数）小于前者（心跳阈值）的时候，触发保护机制，会出现红色的警告：`EMERGENCY!EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT.RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEGING EXPIRED JUST TO BE SAFE.`从警告中可以看到，eureka认为虽然收不到实例的心跳，但它认为实例还是健康的，eureka会保护这些实例，不会把它们从注册表中删掉。

- 该保护机制的目的是避免网络连接故障，在发生网络故障时，微服务和注册中心之间无法正常通信，但服务本身是健康的，不应该注销该服务，如果eureka因网络故障而把微服务误删了，那即使网络恢复了，该微服务也不会重新注册到eureka server了，因为只有在微服务启动的时候才会发起注册请求，后面只会发送心跳和服务列表请求，这样的话，该实例虽然是运行着，但永远不会被其它服务所感知。所以，eureka server在短时间内丢失过多的客户端心跳时，会进入自我保护模式，该模式下，`eureka会保护注册表中的信息，不在注销任何微服务，当网络故障恢复后，eureka会自动退出保护模式`。自我保护模式可以让集群更加健壮
- 但是我们在开发测试阶段，需要频繁地重启发布，如果触发了保护机制，则旧的服务实例没有被删除，这时请求有可能跑到旧的实例中，而该实例已经关闭了，这就导致请求错误，影响开发测试。所以，在开发测试阶段，我们可以把自我保护模式关闭，只需在eureka server配置文件中加上如下配置即可：`eureka.server.enable-self-preservation=false`【不推荐关闭自我保护机制】

```
什么是自我保护:
	注册中心不会剔除那些挂掉的服务, 注册中心有个服务保护机制,即使服务挂掉,也不会剔除该服务,因为服务有可能因为网络波动问题导致误删,所以说有个保护机制, 会将该服务保存在注册中心, 有个自我保护的阈值, 阈值是0.85,也就是说注册中心存在85个,如果有个服务挂了那么这时数量就是84个,那么没超过阈值,就先保存在服务中心! 说白了自我保护就是为了保证客户端因为网络波动而导致误删的这个机制!
```

> ##### 注册进来的微服务，获取一些消息（团队开发会用到）

主启动类中加入[@EnableDiscoveryClient](https://github.com/EnableDiscoveryClient) 注解:

```java
@SpringBootApplication
// @EnableEurekaClient 开启Eureka客户端注解，在服务启动后自动向注册中心注册服务
@EnableEurekaClient
// @EnableEurekaClient 开启服务发现客户端的注解，可以用来获取一些配置的信息，得到具体的微服务
@EnableDiscoveryClient
public class OrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class);
    }
```



```java
@RestController
public class IndexController {

    /**
     * DiscoveryClient 可以用来获取一些配置的信息，得到具体的微服务！
     */
    @Autowired
    private DiscoveryClient client;
    
    @GetMapping("/getUserById/{id}")
    public Object getUserById(@PathVariable Long id) {
        // 获取微服务列表的清单
        List<String> services = client.getServices();
        System.out.println("discovery=>services:" + services);
        // 得到一个具体的微服务信息,通过具体的微服务id，applicaioinName；
        List<ServiceInstance> instances = client.getInstances("service-user");
        for (ServiceInstance instance : instances) {
            System.out.println(
                    instance.getHost() + "\t" + // 主机名称
                            instance.getPort() + "\t" + // 端口号
                            instance.getUri() + "\t" + // uri
                            instance.getServiceId() // 服务id
            );
        }
        return this.client;
    }
}
```

控制台:

![image-20211117205749979](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117205749979.png)

页面:

![image-20211117205826790](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117205826790.png)

## 4.Ribbon-负载均衡(基于客户端)

> Ribbon是什么？

- Spring Cloud Ribbon 是基于Netflix Ribbon 实现的一套**客户端负载均衡的工具**。
- 简单的说，Ribbon 是 Netflix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将 Netflix 的中间层服务连接在一起。Ribbon 的客户端组件提供一系列完整的配置项，如：连接超时、重试等。简单的说，就是在配置文件中列出 LoadBalancer (简称LB：负载均衡) 后面所有的及其，Ribbon 会自动的帮助你基于某种规则 (如简单轮询，随机连接等等) 去连接这些机器。我们也容易使用 Ribbon 实现自定义的负载均衡算法！

> 为什么要用Ribbon

​	我们知道，为了防止应用出现单节点故障问题，同时为了提高应用的作业能力，我们需要对应用做集群 ，如果我们对user-server(用户服务)做了集群 ，那么这个时候回衍生出一些问题：现在有两个user-server(用户服务)就意味着有两个user-server(用户服务)的通信地址，我的order-server(订单服务)在向user-server(用户服务)发起调用的时候该访问哪个？如何访问？这个时候就需要有一个组件帮我们做`请求的分发`，即：负载均衡器，而Ribbon就是一个客户端负载均衡器

> Ribbon的工作机制

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200321001421198.png)

​	两个User集群,除了服务名字相同而ip不同,注册表注册中心的注册表中形成了一个服务名下面挂载了两个IP地址, 而客户端在每30秒拉取注册中心的清单, 这时我们指定找到服务名字, 然后 Ribbon就会去清单中根据服务名找到两台实例,然后根据某种算法选择其中一个,然后组装http请求路径,然后再发起请求调用

> 实战

(消费者)导入依赖:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```

**服务提供者1**:

修改yaml:

```yaml
#Eureak的配置
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:1010/eureka/ #注册中心的注册地址
  instance:
    instance-id: user:1050 #服务的实例ID(右边的)
    prefer-ip-address: true #使用IP注册到Eureak
server:
  port: 1050
```

访问接口:

```java
@RestController
public class IndexController {

    @Value("${server.port}")
    private Integer port;
    
    @GetMapping("/getUserById/{id}")
    public User getUserById(@PathVariable Integer id) {
        return new User(id, "zs", "我是张三,端口是:"+port);
    }
}
```

**服务提供者2**:

修改yaml:

```yaml
#Eureak的配置
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:1010/eureka/ #注册中心的注册地址
  instance:
    instance-id: user:1051 #服务的实例ID(右边的)
    prefer-ip-address: true #使用IP注册到Eureak
server:
  port: 1051
```

**消费者:**

1. 开启负载均衡:

   ```java
   @SpringBootApplication
   @EnableEurekaClient  //开启服务发现客户端
   public class OrderApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(OrderApplication.class);
       }
   
       @Bean
       @LoadBalanced  //@BloadBalanced载均注解，让 RestTenRlatef有了负载均衡的能力，可以走注册中心做服务发现了
       public RestTemplate restTemplate(){
           return new RestTemplate();  //是 Spring的一个基于 Restful的htr客户端工具，默认没有负载均衡能力
       }
   }
   ```

2. 修改Controller:

   ```java
   @RestController
   public class IndexController {
       @Autowired
       private RestTemplate restTemplate;
       
       @GetMapping("/getUserById/{id}")
       public Object getUserById(@PathVariable Long id) {
           //这里将ip地址端口修改为服务名,然后负载均衡会先去本地缓存中找到注册清单,根据清单找到对应的ip和端口进行请求分发
           String url = "http://SERVICE-USER/getUserById/" + id;
           return restTemplate.getForObject(url, User.class);
       }
   }
   ```

**客户端查看:**

![image-20211117213845521](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117213845521.png)

**我们发送请求到消费者,看看消费者会根据服务名的Ribbon如何负载均衡的:**

第一次请求:

![image-20211117213941060](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117213941060.png)

第二次请求:

![image-20211117213956171](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211117213956171.png)

结论:可以发现端口已经发生了改变,也就意味着消费者根据服务名去本地缓存中找到注册清单根据清单中的服务名找到有另个ip地址,这时根据自己的负载均衡来决定请求给谁!

> 负载均衡算法

Ribbon内置7种负载均衡算法，每种算法对应了一个算法类如下：

| 内置负载均衡规则类        | 规则描述                                                     |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule（默认）    | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。可以通过修改配置loadbalancer..connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。（2）并发数过高的服务器。并发连接数的上线，可以由客户端的..ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| ZoneAvoidanceRule         | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。 |
| BestAvailableRule         | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| Retry                     | 重试机制的选择逻辑                                           |

简单理解下:

RoundRobinRule（默认）:均衡选择

AvailabilityFilteringRule: (1):3次连接失败不会调用  (2):并发数过高的不调用

WeightedResponseTimeRule: 按照响应时间来计算权重值,根据响应时间越快的那么权重就越高,那么就优先调用!

ZoneAvoidanceRule: 以服务器区域来决定,在yaml中默认是defaultZone,那么就修改为zone 来进行选择

BestAvailableRule:选择并发低的服务器

RandomRule:随机

Retry:单机重试:一台服务重试多次,比如重试达到2次,那么就会调用其他服务

> Ribbon能干嘛?

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20201121103107791.png)

- LB，即负载均衡 (LoadBalancer) ，在微服务或分布式集群中经常用的一种应用。
- 负载均衡简单的说就是`将用户的请求平摊的分配到多个服务上`，从而达到系统的HA (高用)。
- 常见的负载均衡软件有 Nginx、Lvs 等等。

- Dubbo、SpringCloud 中均给我们提供了负载均衡，**SpringCloud 的负载均衡算法可以自定义**。
- 负载均衡简单分类：
  1. 集中式LB
     - 即在服务的提供方和消费方之间使用独立的LB设施，如**Nginx(反向代理服务器)**，由该设施负责把访问请求通过某种策略转发至服务的提供方！
  2. 进程式 LB
     - 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
     - **Ribbon 就属于进程内LB**，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！

> 切换自定义规则

新建一个配置类,切换使用不同的规则:

```java
@Configuration
public class ConfigBean {
    //@Configuration -- spring  applicationContext.xml
    /**
     * IRule:  //负载均衡顶级父类
     * RoundRobinRule 轮询策略
     * RandomRule 随机策略
     * AvailabilityFilteringRule ： 会先过滤掉，跳闸，访问故障的服务~，对剩下的进行轮询~
     * RetryRule ： 会先按照轮询获取服务~，如果服务获取失败，则会在指定的时间内进行，重试
     */
    @Bean
    public IRule myRule() {
        return new RandomRule();//使用随机策略
        //return new RoundRobinRule();//使用轮询策略
        //return new AvailabilityFilteringRule();//使用轮询策略
        //return new RetryRule();//使用轮询策略
    }
}
```

我们也可以使用自己自定义的规则:

在myRule包下自定义一个配置类MyRule.java，注意：**该包不要和主启动类所在的包同级，要跟启动类所在包同级**：

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200521131456478.png)

```java
/**
 * @Auther: csp1999
 * @Date: 2020/05/19/11:58
 * @Description: 自定义规则
 */
@Configuration
public class MyRule {
    @Bean
    public IRule myRule(){
        return new MyRandomRule();//默认是轮询RandomRule,现在自定义为自己的
    }
}
```

主启动类开启负载均衡并指定自定义的MyRule配置类:

```java
//Ribbon 和 Eureka 整合以后，客户端可以直接调用，不用关心IP地址和端口号
@SpringBootApplication
@EnableEurekaClient
//在微服务启动的时候就能加载自定义的Ribbon类(自定义的规则会覆盖原有默认的规则),针对某个服务进行单独的负载均衡算法
@RibbonClient(name = "user-server",configuration = MyRule.class)//开启负载均衡,并指定自定义的规则
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class, args);
    }
}
```

自定义的规则(这里我们参考Ribbon中默认的规则代码自己稍微改动)：MyRandomRule.java

```java
public class MyRandomRule extends AbstractLoadBalancerRule {
    /**
     * 每个服务访问5次则换下一个服务(总共3个服务)
     * <p>
     * total=0,默认=0,如果=5,指向下一个服务节点
     * index=0,默认=0,如果total=5,index+1
     */
    private int total = 0;//被调用的次数
    private int currentIndex = 0;//当前是谁在提供服务
    //@edu.umd.cs.findbugs.annotations.SuppressWarnings(value = "RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE")
    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            return null;
        }
        Server server = null;
        while (server == null) {
            if (Thread.interrupted()) {
                return null;
            }
            List<Server> upList = lb.getReachableServers();//获得当前活着的服务
            List<Server> allList = lb.getAllServers();//获取所有的服务
            int serverCount = allList.size();
            if (serverCount == 0) {
                /*
                 * No servers. End regardless of pass, because subsequent passes
                 * only get more restrictive.
                 */
                return null;
            }
            //int index = chooseRandomInt(serverCount);//生成区间随机数
            //server = upList.get(index);//从或活着的服务中,随机获取一个
            //=====================自定义代码=========================
            if (total < 5) {
                server = upList.get(currentIndex);
                total++;
            } else {
                total = 0;
                currentIndex++;
                if (currentIndex > upList.size()) {
                    currentIndex = 0;
                }
                server = upList.get(currentIndex);//从活着的服务中,获取指定的服务来进行操作
            }
            //======================================================
            if (server == null) {
                /*
                 * The only time this should happen is if the server list were
                 * somehow trimmed. This is a transient condition. Retry after
                 * yielding.
                 */
                Thread.yield();
                continue;
            }
            if (server.isAlive()) {
                return (server);
            }
            // Shouldn't actually happen.. but must be transient or a bug.
            server = null;
            Thread.yield();
        }
        return server;
    }
    protected int chooseRandomInt(int serverCount) {
        return ThreadLocalRandom.current().nextInt(serverCount);
    }
    @Override
    public Server choose(Object key) {
        return choose(getLoadBalancer(), key);
    }
    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        // TODO Auto-generated method stub
    }
}
```

> 修改

在上述自定义规则中提出不要与主启动类同包,这是因为主配置类上的`@SpringBootApplication`(这个标签里面有个`@ComponentScan`)所扫描到，不然会不起作用(官方文档上有明确的解释)，这里又两种方法可以不让@SpringBootApplication扫描到，

1. 是调整 RibbonConfig 所在包的位置 (@MapperScan扫描当前包及其子包)， 
2. 是通过@ComponentScan排除注解的方式，所以我们在配置类上贴了一个注解@NoScan ，后面我们需要根据这个注解排除配置不被扫描。

解决办法:

自定义一个注解:	

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface NoScan {
}
```

将注解加在我们需要自定义配置类的上:

```java
@NoScan
@Configuration
public class MyRule {
    @Bean
    public IRule myRule(){
        return new MyRandomRule();//默认是轮询RandomRule,现在自定义为自己的
    }
}
```

主启动类上进行过滤:

```java
@SpringBootApplication
//排除 NoScan标签所在的类
@ComponentScan(excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = NoScan.class))
@EnableEurekaClient
//对哪个服务进行使用自定义规则的负载均衡算法,指定服务的负载均衡配置
@RibbonClient(value = "user-server",configuration = RibbonConfig.class)
public class OrderServerApplication1030
{...}
```

除了 @RibbonClient 注解以外还可以通过@RibbonClients来对多个服务进行策略配置:

```java
@RibbonClients({
	@RibbonClient(value = "服务名",configuration = RibbonConfig.class),
	@RibbonClient(value = "服务名",configuration = RibbonConfig.class)
})
```

当然我们也可以进行全局策略配置:

```yaml
ribbon:
   NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

也可配置某个具体的服务的Ribbon算法:

```yaml
user-server:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

> 调优配置

1. 超时配置

   ```yaml
   ribbon:
     ReadTimeout: 3000					#读取超时时间
     ConnectTimeout: 3000				#链接超时时间
     MaxAutoRetries: 1 				#重试机制：同一台实例最大重试次数
     MaxAutoRetriesNextServer: 1 		#重试负载均衡其他的实例最大重试次数
     OkToRetryOnAllOperations: false  	#是否所有操作都重试，因为针对post请求如果没做幂等处理可能会造成数据多次添加/修改
   # 当然也可以针对具体的服务进行超时配置：如"<服务名>.ribbon…"
   ```

2. 饥饿加载

   我们在启动服务使用Ribbon发起服务调用的时候往往会出现找不到目标服务的情况，这是因为Ribbon在进行客户端负载均衡的时候并不是启动时就创建好的，而是在实际请求的时候才会去创建，所以往往我们在发起第一次调用的时候会出现超时导致服务调用失败，我们可以通过设置Ribbon的饥饿加载来改善此情况，即在服务启动时就把Ribbon相关内容创建好。

   ```yaml
   ribbon:
     eager-load:
       enabled: true #开启饥饿加载
       clients: user-server #针对于哪些服务需要饥饿加载
   ```


## 5.Feign：负载均衡(基于服务端)

> 简介

Feign是声明式Web Service客户端，它让微服务之间的调用变得更简单，类似controller调用service。SpringCloud集成了Ribbon和Eureka，可以使用Feigin提供负载均衡的http客户端

​	Feign是一个声明式的http客户端，使用Feign可以实现声明式REST调用，它的目的就是让Web Service调用更加简单。Feign整合了Ribbon和SpringMvc注解，这让Feign的客户端接口看起来就像一个Controller。`Feign提供了HTTP请求的模板，通过编写简单的接口和插入注解，就可以定义好HTTP请求的参数、格式、地址等信息。而Feign则会完全代理HTTP请求，我们只需要像调用方法一样调用它就可以完成服务请求及相关处理`

我们之前使用Ribbon负载均衡就得这样使用:

​	`String url = "http://SERVICE-USER/getUserById/" + id;`

当我们通过RestTemplate调用其它服务时，所需要的参数须在请求的URL中进行拼接，如果参数少的话或许我们还可以忍受，一旦有多个参数的话，这时**拼接请求字符串就会效率低下，并且显得好傻**

```
1. Feign的出现就是解决了Ribbon拼写参数、拼写URL的复杂性问题
2. Feign整合了Ribbon和SpringMvc注解
```



扩展:Feigin全名叫`nettfix fegin`(是nettfix公司开发的),它的底层依然还是Ribbon->springCloud拿来在这基础上封装了一个`Spring Cloud Open Fegin `

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/2020072410031595.png)

> 实战

消费者导入依赖:

```xml
<!--2.导入Feign的包-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

修改Controller:

```java
@RestController
public class UserController {
    @Autowired
    private UserClient userClient;

    @GetMapping("/getUserById/{id}")
    public User getUserById(@PathVariable Integer id) {
        return userClient.getUserById(id);
    }
}
```

修改UserClient接口:

```java
//用户服务的:远程调用的,Feign的客户端接口 value是要调用的服务名
@FeignClient(value = "SERVICE-USER") //标明这是往哪个服务,Feign根据服务名能够在注册中心找到目标服务的通信地址
public interface UserClient {

 	/**
     * 该方法对应了目标服务的Controller的方法,一模一样,那么这是什么意思呢?
     * 1.根据SERVICE-USER服务名,能去服务注册表中找到对应的集群,也就是说能找到多个服务实例
     * 2.底层使用Ribbon,那么也就是说使用负载均衡的算法选择其中一个实例
     * 3.找到具体的实例后就会组装请求地址和参数,最终就是: http://服务地址:端口/getById/123
     * 4.注意:
     *      1. 服务名和调用的一模一样,如:SERVICE-USER
     *      2. URL必须和调用的一模一样,如:http://服务地址:端口/getById/123
     *      3. 请求参数必须和调用的一模一样,如: id
     *      4. 返回值必须和调用的一模一样, 如: User
     *      5. 方法名可以不用和调用的一模一样
     *
     * @param id id
     * @return {@link User}
     */
    @GetMapping("/getUserById/{id}")
    User getUserById(@PathVariable Integer id);
}
注意:如果调用方如果有在Controller上有@RequestMapping,那么这里也可以在当前接口上加上相同的@RequestMapping即可!
```

主启动类:

```java
@SpringBootApplication
/**
 * 开启Feign支持
 * 默认会扫描当前包的及其子包中的有@FeignClient的接口,进行动态处理
 * basePackages: 可以指定要扫描的路径,如果要指定多个包下路径:basePackages = {"cn.miao.feginclient","cn.miao.xxx"}
 */
@EnableFeignClients(basePackages = "cn.miao.feginclient")
public class PayApplication {
    public static void main(String[] args) {
        SpringApplication.run(PayApplication.class);
    }
}
```

**注意**:

相同:

1. 服务名要一致
2. url路径要一致
3. 参数要一致
4.  返回值类型要一致

不同:

1. 方法名可不同

**建议**:服务名直接去目标服务配置中拷贝，方法直接从目标服务controller中拷贝

测试:

![image-20211118174915625](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118174915625.png)

这里调用`userClient.getById`方法，看起来是像在调用本地方法，其实该接口已经被Feign代理，我们发起调用时其实在像Feign接口配置的目标服务发起调用。

**总结**:

1. Feign 本质上也是实现了 Ribbon，只不过后者是在调用方式上，为了满足一些开发者习惯的接口调用习惯！
2. Feign和Ribbon二者对比，前者显现出面向接口编程特点，代码看起来更清爽，而且Feign调用方式更符合我们之前在做SSM或者SprngBoot项目时，Controller层调用Service层的编程习惯

> Feign的工作原理

​	主启动类上的`@EnableFeignClients`注解开启fegin,然后就会扫描到指定包下的`@FeiginClient`接口, 扫描到了之后就会将该注解所在的接口进行动态代理,然后放进Spring进行管理,所以我们可以通过@Autowrited进行注入,然后在我们发起请求的时候, 会使用jdk的动态代理方式代理接口，生成相应的`RequestTemplate`, 而这个RequestTemplate里面封装好请求的URL一些参数信息,然后RequestTemplate就会交给`HttpClient`去执行请求, 而在这底层又会交给Ribbon的负载均衡, 而Ribbon又会根据接口上的服务名, 然后去本地缓存注册表中找到服务, 然后根据负载均衡算法进行选择,选择后进行发送请求!

![image-20211118203312699](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118203312699.png)

> 其他的一些配置参数

1. 负载均衡配置

   ```yaml
   # Feign已经集成了Ribbon，所以它的负载均衡配置基于Ribbon配置即可，这里使用xml简单配置负载均衡策略如下：
   user-server:
     ribbon:
       NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
   ```

2. 超时配置

   ```yaml
   ribbon:
   	ConnectTimeout: 3000
       ReadTimeout: 6000
   ```

   如果服务调用出现“`com.netflix.hystrix.exception.HystrixRuntimeException：… timed - out and no fallback available`” 错误日志，是因为Hystrix超时，默认Feign集成了Hystrix，但是高版本是关闭了Hystrix，我们可以配置Hystrix超时时间：

   ```yaml
   feign:
      hystrix:
          enabled: true #开启熔断支持
   hystrix:
     command:
         default:
           execution:
             isolation:
               thread:
                 timeoutInMilliseconds: 6000   #hystrix超时时间
   ```

3. 开启日志

   官方文档：https://cloud.spring.io/spring-cloud-openfeign/reference/html/#feign-logging

   创建配置类:

   ```java
   @Configuration
   public class FeignConfiguration {
       @Bean
       Logger.Level feignLoggerLevel() {
           return Logger.Level.FULL;	//打印Feign的所有日志
           
       }
   }
   ```

   Feign支持如下几种日志模式来决定日志记录内容多少：

   - NONE，不记录（DEFAULT）。
   - BASIC，仅记录请求方法和URL以及响应状态代码和执行时间。
   - HEADERS，记录基本信息以及请求和响应标头。
   - FULL，记录请求和响应的标题，正文和元数据

   配置日志打印级别

   ```yaml
   logging:
     level:
       cn.itsource.springboot.feignclient.UserFeignClient: debug
   # 其他日志级别是:error,warn,debug,trace
   # trace:所有的信息都会打印!
   ```

4. 开启GZIP

   ```yaml
   # 就是对数据进行压缩,比较大的可以进行压缩,但是数据大小比较小的话就会增加CPU的开销
   feign:
     compression:
       request:
         enabled: true
         min-request-size: 1024 #最小阈值，小于这个不压缩
         mime-types: text/xml,application/xml,application/json #压缩哪些类型的数据
       response:
         enabled: true
   ```

   

## 6.Hystrix: 服务熔断

一句话总结:Hystrix就是为了解决在请求的调用链中出现的故障扩散的问题!

> 服务雪崩

​	多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”，如果扇出的链路上**某个微服务的调用响应时间过长，或者不可用**，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“**雪崩效应**”。

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20201121144830148.png)

​	对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几十秒内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，**这些都表示需要对故障和延迟进行隔离和管理，以达到单个依赖关系的失败而不影响整个应用程序或系统运行**。

**理解**:在整个服务调用链中, 如果某个服务发生故障,那么就有可能导致整个调用请求崩塌;甚至就会造成整个微服务宕机,这是因为有可能这个A服务并发比较高 然后这个服务挂了, 然后其他请求调用的时候由于是网络调用, 如果调用出现故障那么就会等待2s 出现连接超时,而此时如果并发高有很多请求又在后面进行等待,造成`大量的请求等待`.那么此时就会造成整个微服务崩塌!

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9naXRodWIuY29tL05ldGZsaXgvSHlzdHJpeC93aWtpL2ltYWdlcy9zb2EtMy02NDAucG5n)

> 什么是Hystrix

**Hystrix**是一个应用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，**Hystrix** 能够保证在一个依赖出问题的情况下，不会导致整个体系服务失败，避免级联故障，以提高分布式系统的弹性。

**断路器**本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控 (类似熔断保险丝) ，**向调用方返回一个服务预期的，可处理的备选响应 (FallBack) ，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间，不必要的占用**，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/2020112114554744.png)

Hystrix是处理依赖隔离的框架,将出现故障的服务通过熔断、降级等手段隔离开来，这样不影响整个系统的主业务(比如你得了传染病是不是要把你关起来隔离呢),同时也是可以帮我们做服务的治理和监控。

其设计原则如下：

1. 防止单个服务异常导致整个微服务故障。
2. **快速失败**，如果服务出现故障，服务的请求快速失败，`线程不会等待`;
   - 理解:因为请求一旦等待就会造成大量的请求阻塞
3. **服务降级**，请求故障可以返回设定好的二手方案数据（兜底数据）。
   - 理解:遇到故障异常不能直接把异常返回给界面,所以使用其他的数据进行返回,比如提示信息!
4. **熔断机制**，防止故障的扩散，导致整个服务瘫痪。
   - 理解:比如2次连续请求都调用失败了,那么认为这个服务可能真的故障了,就会将这个服务标记为`熔断状态`,然后其他请求访问这个就会直接返回降级数据!
5. **服务监控**，提供了Hystrix Bashboard仪表盘，实时监控熔断器状态

> Histrix的功能

**资源隔离:**

​	资源隔离包括`线程池隔离`和`信号量隔离`，作用是限制调用分布式服务的资源使用，某一个调用的服务出现问题不会影响其他服务调用 ，这里可以简单的理解为资源隔离就是`限制请求的数量`。

1. 线程池隔离:

   ```
   	使用一个线程池来存储当前请求，线程池对请求作处理，设置任务返回处理超时时间，堆积的请求先入线程池队列
   	每次进来的请求就会新开一个线程去执行,然后当线程池满了之后就会有一个请求队列,后续的请求就会进入这个队列进行排队, 如果两个都满了之后那么后续的请求进行服务降级比如返回一个提示信息!--一个请求也可是一个线程走到线程池就可以新开一个线程去执行, 有2个线程执行
   	
   好处:流量洪峰来临时，处理不完可将数据存储到线程池队里慢慢处理
   缺点:是异步的, 每次都会进行线程的切换, 某些方面性能不是很好
   ```

2. 信号量隔离:

   ```
   	使用一个原子计数器（或信号量）记录当前有多少个线程在运行，请求来先判断计数器的数值，若超过设置的最大线程个数则丢弃该类型的新请求，若不超过则执行计数操作请求来计数器+1，请求返回计数器-1
   	进一个请求就会对信号量+1 ,反正就是进来一个请求+1 走的时候-1  达到最大上限后续的请求就会进行一系列服务降级等等!--- 一个请求在一个线程
   	
   好处:只是记录进来的线程请求,最终一个请求也就是一个线程,不会进行线程切换!不会新开线程
   缺点:严格的控制线程且立即返回模式，无法应对突发流量（流量洪峰来临时，处理的线程超过数量，其他的请求会直接返回，不继续去请求依赖的服务)
   ```

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200724110558712.png)

理解:

1. 线程池隔离:

   ​	Histrix内部提供了一种线程池,线程池里面就有很多的线程,比如设置为最大线程为100;那么此时进来的一个请求,Histrix底层就会为分配一个线程,那么这个请求就会在这个线程池中去执行!注意:这里一定是`新开的一个线程`;当线程池中最大线程数量达到100那么此时就会往线程池队列中去请求排队,而如果两个都达到最大线程数量的时候那么后面的请求就会直接被拒绝,当然这个拒绝也是会走`降级策略`

2. 信号量隔离:

   这个信号量说白了就是一个数字,就是记录进来一个请求就+1,直到计数器达到信号量那么后续的请求就直接被拒绝!



线程池和信号量对比：

| 线程池功能 | 线程池                   | 信号量                      |
| ---------- | ------------------------ | --------------------------- |
| 线程       | 与调用线程非相同线程     | 与调用线程相同（jetty线程） |
| 开销       | 排队、调度、上下文开销等 | 无线程切换，开销低          |
| 异步       | 支持                     | 不支持                      |
| 并发支持   | 支持（最大线程池大小）   |                             |

注意:为什么要说与调用线程非相同线程呢?是因为首先进来的一个请求是一个线程,然后就会进入到线程池中,线程池就会为它分配一个线程去执行;



**服务熔断:**

​	熔断机制是对服务链路的保护机制，如果链路上的某个服务不可访问，调用超时，发生异常等，服务会`触发降级返回托底数据`，然后熔断服务的调用(`失败率达到某个阀值服务标记为短路状态`)，当检查到该节点能正常使用时服务会快速恢复

​	理解:主要是标记为熔断的一个状态,也就是说在调用链上如果出现服务故障就会标记为服务熔断;

**降级机制:**

​	超时降级、资源不足时(线程或信号量)降级，降级后可以配合降级接口返回托底数据。

​	理解:就好比是B计划,比如原本去A服务获取数据那么这是A计划, 如果这个A计划拿不到数据就会走这个B计划来返回数据!

**缓存:**

​	提供了`请求缓存`、请求合并实现 ， 在高并发的场景之下，Hystrix请求缓存可以方便地开启和使用请求缓存来优化系统，达到减轻高并发时请求线程的消耗、降低请求响应时间的效果。

​	理解:对请求进行缓存, 如果一个请求多次调用就会从缓存! 

熔断和降级的区别:

1. 熔断是对服务的熔断，如果服务多次访问失败，服务就会被标记为熔断。
2. 降级指的是方法或者资源不能正常访问就以另外一种方式去执行(降级逻辑),通常是返回一些兜底数据(降级数据)

总结:

1. 限流: `限制请求的数量`，有线程池和信号量2种方式

2. 熔断：服务调用失败达到某种阈值，就会被hystrix`标记为熔断状态`，如果访问一个熔断状态的服务会快速失败，`触发降级`  

3. 降级：如果服务或者某个资源正常访问失败，就会走实现准备好的降级流程`返回降级数据`(兜底数据)  

4. 缓存：对`请求做缓存`，提高性能。

![腾讯课堂图片20211118115837](https://gitee.com/miawei/pic-go-img/raw/master/imgs/%E8%85%BE%E8%AE%AF%E8%AF%BE%E5%A0%82%E5%9B%BE%E7%89%8720211118115837.jpg)

> Hystrix工作机制

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200325082851768.png)

​	正常情况下我们正常访问一个服务的话那么此时断路器是处于一个`关闭状态(Closed)`状态,然后在连续的调用过程中出现异常故障达到请求数量阈值,那么此时这个服务就会进入`服务熔断(Open)状态`也就是说后续的请求就会直接触发快速失败走降级机制,总之不会让请求处于等待的状态,然后后面一段时间内调用都是处于被拒绝的状态,然后在一段时间内保护器会尝试进入`半熔断状态(Half-Open)`,允许少量的请求进来尝试是否请求成功,如果成功就关闭保护器进入一个正常的调用状态,如果请求失败那么还是回到熔断状态!



简短理解:首先A服务是一个健康的服务, 然后当请求多次出现故障,此时这个A服务就会立即进入熔断状态走降级机制,一段时间内每隔5秒就会让少量的请求进入, 如果请求成功那么就给他标记为健康状态!

> 实战

调用者导入依赖:

```xml-dtd
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

配置类开启hystrix

```java
/**
 * 订单的启动类
 */
@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker       //开启Hystrix熔断
public class OrderServerApplication1030
{
    //省略...
}
```

方法熔断:

```java
@RestController
public class IndexController {
    @Autowired
    private RestTemplate restTemplate;

    @HystrixCommand(fallbackMethod = "fallbackMethod")   //方法熔断
    @GetMapping("/getUserById/{id}")
    public User getUserById(@PathVariable Long id) {
        
        String url = "http://SERVICE-USER/getUserById/" + id;
        return restTemplate.getForObject(url, User.class);
    }

    /**
     * 降级方法 ， 参数和返回值必须和被熔断的方法一致 ，方法名要和  fallbackMethod 的值一致
     *
     * @param id id
     * @return {@link User}
     */
    public User fallbackMethod(@PathVariable Long id) {
        //返回托底数据
        return new User(-1 ,"无此用户","用户服务不可用");
    }
}
```

测试:此时我们不启动User服务,那么在调用的过程就会发生异常故障

![image-20211118212249824](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118212249824.png)

总结:

![image-20211118220219643](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118220219643.png)

可以看见这个方法熔断只是针对单个的方法进行熔断降级方法,那么如何针对整个类进行熔断降级呢?因为如果为每一个方法进行熔断降级就会显得代码冗余:

```java
@RestController
@DefaultProperties(defaultFallback ="fallbackMethod")	//统一降级配置
public class OrderController {

	@HystrixCommand   //方法熔断
	@RequestMapping(value = "/order/{id}",method = RequestMethod.GET)
	public User getById(@PathVariable("id")Long id){
        //...省略...
    }

/**
 * 降级方法
 * 注意:公共的托底方法不应该有参数。返回类型必须和熔断的方法的返回类型兼容，比如这里可以返回User或者User的子类。
 *
 */
public User fallbackMethod(){
    //返回友好的提示信息
    return new User(-1L,"无此用户","用户服务不可用");
}
```

> 其他参数配置

1. 熔断参数配置:

   ```yaml
   hystrix: 
     command:
       default:
         circuitBreaker:
           requestVolumeThreshold: 20 #20个请求中
           errorThresholdPercentage: 50 #出错百分比阈值
           sleepWindowInMilliseconds: 50000 #短路5秒钟，尝试恢复
   # 每当20个请求中，有50%失败时，熔断器就会打开，此时再调用此服务，将会直接返回失败，不再调远程服务。直到5s钟之后，重新检测该触发条件，判断是否把熔断器关闭，或者继续打开。
   ```

2. 超时配置:

   ```yaml
   ribbon:
     MaxAutoRetries: 1 #最大重试次数
     MaxAutoRetriesNextServer: 1 #切换实例的重试次数
     OkToRetryOnAllOperations: false # 对所有的操作请求都进行重试，
     ConnectTimeout: 1000 #请求连接的超时时间
     ReadTimeout: 1800 #请求处理的超时时间
   hystrix:
     command:
       default:
         execution:
           isolation:
             thread:
               timeoutInMilliseconds: 3000 #hystrix超时,置thread和semaphore两种隔离策略的超时时间
   ```

   需要注意的是,我们设置了重试机制就需要Hystrix超时时间大于Ribbon超时时间，因为Hystrix超时后会直接熔断就不会再出发重试

   `hystrix超时=(1 + MaxAutoRetries + MaxAutoRetriesNextServer) * ReadTimeout`

   如果要针对于某个API做单独的超时设置也可以通过如下方式单独指定

   ```java
   @HystrixCommand(commandProperties = {
       @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "3000")
   })
   @RequestMapping(value = "/order/{id}",method = RequestMethod.GET)
   public User getById(@PathVariable("id")Long id){
   ...
   }
   ```

3. 资源隔离模式:

   ```yaml
   hystrix:
     command:
       default:
         execution:
           isolation:
             strategy: THREAD #默认是线程池，可以修改为信号量： SEMAPHORE
   ```

   其他方式:

   ```java
    @HystrixCommand(fallbackMethod = "", commandProperties=@HystrixProperty(name="execution.isolation.strategy", value="THREAD") )
    @RequestMapping(value = "/order/{id}",method = RequestMethod.GET)
   public User getById(@PathVariable("id")Long id){
   	...
   }
   ```

4. 最大请求设置

   ```yaml
   hystrix:
     command:
       default:
         execution:
           isolation:
             thread:
               timeoutInMilliseconds: 30000   #hystrix超时
             strategy: SEMAPHORE #默认是线程池，可以修改为信号量： SEMAPHORE
             semaphore:
               maxConcurrentRequests: 10 # SEMAPHORE模式下 , 1秒钟最大请求数量，默认10
   ```

   线程池模式下:

   ```yaml
   hystrix:
     threadpool:
       default:
         coreSize: 10 #最大线程数 , 并发执行的最大线程数，默认10
         maxQueueSize: -1 #最大排队长度。默认-1，使用SynchronousQueue。其他值则使用 LinkedBlockingQueue。设置线程池队列模式
         queueSizeRejectionThreshold: 5 # 设置排队数量
         ...
   ```

5. Fallback设置:

   ```yaml
   
   hystrix:
     command:
       default:
         fallback:
          	 enabled: true #请求失败时是否要走托底
            isolation:
              semaphore:
                maxConcurrentRequests: 10 #并发达到10时,fallback会被被调用
   ```

6. 设置客户端与服务端之间的服务注册:

   ```yaml
   lease-renewal-interval-in-seconds: #客户端每隔多少秒向注册中心续约!
   eureka.instance.lease-expiration-duration-in-seconds: # 注册中心剔除客户端的时间
   #修改续约时间，服务剔除时间 ，以及服务注册表拉取时间改小， 同时设置Ribbin的重试策略
   ```

   

> Fegin使用Hystrix

在使用Fegin的时候其实Fegin本身就已经自带了Hystrix,只不过默认是关闭的;

开启Hystrix:

```yaml
feign:
  hystrix:
    enabled: true #开启熔断支持
```

> 第一种方式:fallback

修改接口:

```java
@FeignClient(value = "SERVICE-USER",fallback = UserClientFallback.class)
public interface UserClient {
    @GetMapping("/getUserById/{id}")
    User getUserById(@PathVariable Integer id);
}
```

降级方法:

```java
//让Spring扫描到该托底类
@Component
public class UserFeignClientFallback implements UserClientFallback {

    //日志打印器
    private Logger log = LoggerFactory.getLogger(UserFeignClientFallback.class);

    @Override
    public User getById(Long id) {
        log.info("用户服务不可用");
        //托底数据
        return new User(-1l,"无此用户","用户服务不可用");
    }
}
```

这种呢就是实现调用接口,然后复写接口中的方法作为托底方法返回拖地数据。当Fiegn调用失败就会以降级方法返回的结果返回给用户

> 第二种方式:fallbackFactory

修改接口:

```java
@FeignClient(value = "SERVICE-USER",fallbackFactory = UserClientFallback.class)
public interface UserClient {
    @GetMapping("/getUserById/{id}")
    User getUserById(@PathVariable Integer id);
}
```

降级方法:

```java
@Component
public class UserClientFallback implements FallbackFactory<UserClient> {

    @Override
    public UserClient create(Throwable throwable) {  //只会创建一次!
        return new UserClient() {
            @Override
            public User getUserById(Integer id) {
                throwable.printStackTrace(); //打印错误
                return new User(-1, "1", "你遇到错误啦!");
            }
        };
    }
}
```

![image-20211118220157014](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118220157014.png)

两者最大的区别就是:fallback不支持在控制台看到触发降级方法的异常信息,而fallbackFactory是可以的!

## 7.zuul: 服务网关

服务网关-微服务的请求入口

> 什么是zuul?

Zull包含了对请求的**路由**(用来跳转的)和**过滤**两个最主要功能：

其中**路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础**，而过**滤器功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础**。Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200325163341376.png)

注意:zuul本身是一个`独立的服务`，默认集成了Ribbon，zuul通过Ribbon将客户端的请求分发到下游的微服务，所以zuul需要`通过Eureka做服务发行`，同时zuul也集成了Hystrix。

zull的底层就是用ribbon做负载均衡的!也就是说它收到请求后也是会根据ribbon的负载均衡算法将请求根据服务名转发到指定的微服务上去!

> 实战:

因为要新建一个服务,所以创建步骤省略直接来核心:

导入依赖:

```xml-dtd
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
```

主启动类上开启zuul:

```java
/**
 * 用户的启动类
 * @EnableEurekaClient： 标记该应用是 Eureka客户端
 * @EnableZuulProxy ： 开启zuul 可以看做是 @EnableZuulServer 的增强版 ，一般用这个
 * @EnableZuulServer : 这个标签也可以开启zuul，但是这个标签开启的Filter更少
 */
@SpringBootApplication
@EnableEurekaClient
@EnableZuulProxy
public class ZuulServerApplication1060
{

    public static void main( String[] args )
    {
        SpringApplication.run(ZuulServerApplication1060.class);
    }
}
```

修改yaml:

```yaml
server:
  port: 1061
zuul:
  prefix: "/servers"  #统一访问前缀
  ignoredServices: "*"  #禁用掉使用浏览器通过服务名的方式访问服务
  routes:
    SERVICE-PAY: "/pay/**"   #指定pay-server这个服务使用 /pay路径来访问  - 别名
    SERVICE-ORDER: "/order/**"   #指定order-server这个服务使用 /order路径来访问
```

测试:

![image-20211118220945888](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211118220945888.png)

格式: http://localhost:zuul端口/前缀/服务别名/资源路径

请求进入到zuul网关以后,根据服务名然后用ribbon从注册表根据服务名找到对应的ip将请求转发到指定的服务上,然后根据访问URL进行请求的访问

> zuul的工作原理

​	zuul的底层是通过各种Filter来实现的，zuul中的filter按照执行顺序分为了“**pre**”前置（”custom”自定义一般是前置），“**routing**”路由，“**post**”后置，以及“**error**”异常Filter组成，

Filter的执行流程图：

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200325165309752.png)

- 正常流程：
  - 请求到达首先会经过pre类型过滤器，而后到达routing类型，进行路由，请求就到达真正的服务提供者，执行请求，返回结果后，会到达post过滤器。而后返回响应。
- 异常流程：
  - 整个过程中，pre或者routing过滤器出现异常，都会直接进入error过滤器，再error处理完毕后，会将请求交给POST过滤器，最后返回给用户。
  - 如果是error过滤器自己出现异常，最终也会进入POST过滤器，而后返回。
  - 如果是POST过滤器出现异常，会跳转到error过滤器，但是与pre和routing不同的时，请求不会再到达POST过滤器了。

`custom filiters`:custorm是我们自己定义的,但是类型是属于前置、后置、路由其中的一种, 当执行到其中某一种的时候就会调用这个custorm!  可以理解为:指定什么类型  就什么时候调

源码解析:

​	当用户的请求进来的时候,会首先进入`ZuulServlet`这个类中在`service`方法中执行,然后就会执行`pre`前置拦截器做一些请求的校验等等,然后就会执行`route`路由,因为zuul本身就是一个单独的服务所以需要注册到Eureka注册中心去,然后zuul既然要分发请求那么其底层也是集成了ribbon,所以我们在zuul配置环境中要指定服务名然后其底层就会从注册服务中拉取注册清单然后根据服务名找到对应的ip地址进行发起请求调用微服务!当执行请求执行完毕就会返回到`post`后置拦截器,然后返回响应给客户端;

```java
public class ZuulServlet extends HttpServlet {
    @Override
    public void service(javax.servlet.ServletRequest servletRequest, javax.servlet.ServletResponse servletResponse) throws ServletException, IOException {
        try {
            init((HttpServletRequest) servletRequest, (HttpServletResponse) servletResponse);

            // Marks this request as having passed through the "Zuul engine", as opposed to servlets
            // explicitly bound in web.xml, for which requests will not have the same data attached
            RequestContext context = RequestContext.getCurrentContext();
            context.setZuulEngineRan();

            try {
                preRoute();   // 1.第一步执行pre前置拦截器
            } catch (ZuulException e) {
                error(e);	  // 1.1 遇到错误执行error拦截器
                postRoute();  // 1.2 error执行完就会走post拦截器
                return;
            }
            try {
                route();
            } catch (ZuulException e) {
                error(e);
                postRoute();
                return;
            }
            try {
                postRoute();
            } catch (ZuulException e) {
                error(e);
                return;
            }

        } catch (Throwable e) {
            error(new ZuulException(e, 500, "UNHANDLED_EXCEPTION_" + e.getClass().getName()));
        } finally {
            RequestContext.getCurrentContext().unset();
        }
    }
}
```

> 自定义filiter

既然要想实现我们自定义的过滤器,那么我们可以来借鉴它已经存在的过滤器来进行借鉴实现,经过发现我们只需覆写这四个方法就可以了:

```java
public abstract class ZuulFilter implements IZuulFilter{
      
    abstract public String filterType();

    abstract public int filterOrder();
   

    //下面两个方法是 IZuulFilter 提供的 
    
    boolean shouldFilter();

    Object run() throws ZuulException;

}
```

自定义的filiter:

```java
package cn.miao.filiter;

import com.netflix.zuul.ZuulFilter;
import com.netflix.zuul.context.RequestContext;
import com.netflix.zuul.exception.ZuulException;
import org.apache.commons.lang.StringUtils;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @program: cloud-parent
 * @description:
 * @author: MiaoWei
 * @create: 2021-11-19 10:28
 **/
@Component
public class CheckLoginFiliter extends ZuulFilter {

    /**
     * 过滤器类型,可通过FilterConstants常量选择
     *
     * @return {@link String}
     */
    @Override
    public String filterType() {
        return "pre";
    }

    /**
     * 过滤器执行顺序-数值越小表示执行顺序就越前,越小越先执行
     *
     * @return int
     */
    @Override
    public int filterOrder() {
        return 0;
    }

    /**
     * 判断请求路径是否符合,比如登录或者注册那么就放行
     *
     * @return boolean 类似于开关,为true表示会执行run()方法,如果为false那么就不会执行run()
     */
    @Override
    public boolean shouldFilter() {
        //拿到请求的路径
        RequestContext requestContext = RequestContext.getCurrentContext();
        String requestURI = requestContext.getRequest().getRequestURI();
        if (requestURI.endsWith("login") || requestURI.endsWith("registered")) {
            //如果是登录或者注册那么就不会往下执行run方法
            return false;
        }
        return true;
    }

    /**
     * 运行
     *
     * @return {@link Object}
     * @throws ZuulException zuul例外
     */
    @Override
    public Object run() throws ZuulException {
        //获取请求对象
        RequestContext requestContext = RequestContext.getCurrentContext();
        HttpServletRequest request = requestContext.getRequest();
        String token = request.getHeader("token");
        if (StringUtils.isEmpty(token)) {
            //token为空则返回错误
            //响应对象
            HttpServletResponse response = requestContext.getResponse();
            //中文编码
            response.setContentType("application/json;charset=utf-8");
            try {
                response.getWriter().write("你发生错误啦");
            } catch (IOException e) {
                e.printStackTrace();
            }
            //阻止filter继续往后执行,直接返回!
            requestContext.setSendZuulResponse(false);
        }

        //这里返回并没有任何意义,也就是说返回任何东西都是无效的!
        return null;
    }
}
```

> 其他配置

https://blog.csdn.net/u014494148/article/details/105098362?spm=1001.2014.3001.5501

其他配置都在这里

## 8.Config: 配置中心

> 概述:

**分布式系统面临的–配置文件问题**

​	微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务，由于每个服务都需要必要的配置信息才能运行，所以一套集中式的，动态的配置管理设施是必不可少的。spring cloud提供了configServer来解决这个问题，我们每一个微服务自己带着一个application.yml，那上百个的配置文件修改起来，令人头疼！

理解:说白了就是说以后微服务很非常多而针对每个服务的配置文件进行修改是非常麻烦的一件事并且还比较繁琐不好统一管理,而这个组件就是为了解决这个问题!

> 配置中心工作流程

![在这里插入图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200328132657430.png)

在spring cloud config 分为了服务端 config server和客户端config client 两个角色。

其工作流程是，微服务通过`ConfigClient`向配置中心`ConfigServer`发起请求获取配置文件，配置中心从Git仓库获取配置，然后再一路返回给微服务。注意当拉取下来之后就会缓存到本地中,而后续使用也都是从本地中读取,但是有个缺点就是云端配置文件如果修改那么本地的服务也要重新启动拉取云端更新的文件,当然这种方法也很麻烦,所以后面还有个组件`Spring Cloud Bus`通过MQ来实现配置自动刷新!

**注意**:

1. ConfigServer也是一个单独的服务所以也需要注册到Eureka服务!
2. EurekaServer的配置不能交给ConfigServer管理，因为必须要先启动EurekaServer才能启动ConfigServer，试问EurekaServer都没配置怎么启动？循环套娃!

> 实战

搭建新的Eureka服务,然后在这基础上导入新的依赖:

```xml-dtd
<dependency>
         <groupId>org.springframework.cloud</groupId>
         <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

主启动类:

```java

/**
 * 配置中心的启动类
 * @EnableConfigServer：开启配置中心
 */
@SpringBootApplication
@EnableEurekaClient
@EnableConfigServer   //开启configServer服务
public class ConfigServerApplication1070{
    public static void main( String[] args )
    {
        SpringApplication.run(ConfigServerApplication1070.class);
    }
}
```

修改配置文件yaml:

```yaml
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
        git:
          #配置远程仓库地址，去仓库中复制
          uri: https://gitee.com/miawei/spring-cloud-config.git
          username: 2439135122@qq.com	#仓库是私有的需要账号
          password: # 密码
          #search-paths: 路径 #如果配置文件不再仓库的根目录，需要配置查找路径

```

注意:如果由于网络问题拉取配置文件失败,那整个微服务的配置文件都可能会拉取失败，针对于这种情况我们可以把配置文件统一管理在本地磁盘中，如下配置：

```yaml
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
      	git:
          native:
            search-locations: classpath:config,file:D:/config   
            #从d:/config 和 classpath:config加载
         #配置远程仓库地址
         #uri: https://gitee.com/little_wolf/springcloud-config-1010.git 
          #username: 1462163787@qq.com	#仓库是私有的需要账号
          #password: 你的密码
          #search-paths: 路径 #如果配置文件不再仓库的根目录，需要配置查找路径    
  profiles:
    active: native  #基于本地的配置
```

> CconfigClient

选择任意一个微服务然后集成;

导入依赖:

```xml-dtd
 <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

新建`bootstrap.yml`配置文件:

注意:这个`bootstrap.yml`的优先级要高于普通的application.yaml,有boostrap.yaml 优先级高 如果这个加载失败,那么就会使用本地的配置!

总之就会优先加载云端的配置, 如果本地有和云端相同配置那么云端的配置优先, 如果云端没有那么就会加载本地的配置,总之两个配置文件相互补缺,以云端为准!

```yaml
#配置中心的地址
spring:
  cloud:
    config:
      uri: http://localhost:1070 #zuul要从configServer配置中心拉取文件
      #你要拉取具体的哪个配置文件
      name: application-zuul  #配置文件名字   dev是环境
      profile: dev #环境  组成完整的文件名：application-zuul-dev.yml
      label: master #主分支
```

这里使用uri的方式进拉取文件,那么我们也可以使用服务名的方式来拉取文件:

```yaml
spring:
  cloud:
    config:
      #uri: http://localhost:1030
      name: application-zuul
      profile: dev
      label: master
      discovery:
        service-id: config-server #配置中心服务名
        enabled: true #使用服务名访问配置中心
eureka:
  client:
    serviceUrl:
      defaultZone: http://peer1:1010/eureka/
      #注册中心地址,需要从注册中心使用服务名发现配置中心
```

