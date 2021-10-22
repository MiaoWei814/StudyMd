#      笔记

## 1.什么是中间件

### 1.1 概述



​	由于业务和市场需求的不断变化,一个企业可能同时运行着多个不同的业务系统,而这些系统基于不同的操作系统、不同的数据库、异构的网络环境，现在的问题是，如何把这些`信息系统结合成一个有机地协同工作的整体`，真正实现企业跨平台、分布式应用。中间件便是解决之道，它用自己的复杂换取了企业应用的简单。

![image-20211017200855816](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211017200855816.png)

理解:其实中间件其实并不是什么高大上的名词,在我们的日常开发中也是随处可遇的!比如MySQL就是一种中间件的技术,当然他也要遵循一些规范与协议,比如TCP/IP协议,它只要遵守这个协议就能把数据写到硬盘上去,所以它具有一个功能就是持久化功能!而我们写的应用程序是Java写的,而我们使用Java也要去遵守这个TCP/IP协议才能与MySQL沟通!

中间件技术要解决的第一个问题就是:互通性;就是一定要可以通讯!

> 中间件（Middleware）是处于操作系统和应用程序之间的软件，也有人认为它应该属于操作系统中的一部分。人们在使用中间件时，往往是一组中间件集成在一起，构成一个平台（包括开发平台和运行平台），但在这组中间件中必须要有一个通信中间件，即中间件=平台+通信，这个定义也限定了只有用于分布式系统中才能称为中间件，同时还可以把它与支撑软件和实用软件区分开来。

理解:中间件就是指平台+通信,也就是说平台就是指Java开发的应用程序,它们遵守某一种协议和规范去和我们底层的操作系统硬件打交道!而这个中间件有个典型的特征,就是垮平台!



我们都知道现在流行的开发方式就是微服务架构或者分布式架构,而分布式架构的话就会把一个系统拆分成若干份!比如拆分成订单系统、支付系统、用户系统等等，那么现在这几个系统之间要实现互联.那么这就是一个问题,当我们把单体系统拆分成了分布式系统那么首先考虑的就是要采用什么中间件技术!并且要有最基本的遵循规范和通讯的功能,并且还要必须具备高可用和持久性!

![image-20211017203653909](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211017203653909.png)

综上所述:所有的中间件都必须要`遵守一定的规范`,而且还要具备我们的`高可用`、`高可扩`和我们的`持久性`这些特征。

好处:中间件可以屏蔽我们的操作系统底层的复杂性,就是说我们不需要去关心这个东西是怎么去存储的去互联互通的，我们只需要使用即可！也就是说我们众多的系统，比如用户系统就是用Java语言写的，订单系统就是用GO语言写的，使用了中间件技术就可以屏蔽我们应用程序之间架构的局限性，也就是说它们可以通过中间件技术把这些架构进行串联起来，来达到一个更加稳健、缩短我们的开发周期也可以减少维护成本的目的，这就是我们的中间件技术！

> 中间件技术与我们日常开发没有脱节，只是使用这些中间件技术它可以去屏蔽我们操作系统底层的复杂性，可以让我们的技术框架与应用程序变得更加的灵活，而不仅仅局限于单一架构的选型，让我们的系统变得更加的稳健和高效，并且这些中间件技术必须要具备这些互通性和沟通性以及持久性还有高可扩性，那么这也是它典型的一些特性，而这些典型的背后必须要遵循一个协议，就是我们所谓的TCP/IP，！

后面的话对于TCP/IP协议就变少了，因为TCP/ip是底层操作系统的协议,它并不能满足我们业务场景所有的需求,所以必须得在它的基础之上构建一个自己的请求信息,比如我们后面的AMQP协议,就会在请求头里增加一些消息内容的一些标签以及我们的交换机的名字等等等!



理解:我自己来总结一波:中间件往大了来说就是`把每个不同的信息系统进行协作串联一起`,变为一个整体进行开发!中间件呢官方一点来说就`是操作系统和应用程序之间的软件`,然后往往是一组中间件构建成一个平台然后还具有通信的功能;中间件必须要具有:遵守协议与规范、跨平台、高可用、高可扩、持久性、互通性、沟通性！其好处就是为了`屏蔽我们操作系统底层的复杂与局限性`！使程序开发人员面对一个简单而统一的开发环境，减少程序设计的复杂性，将注意力集中在自己的业务上，`不必再为程序在不同系统软件上的移植而重复工作`，从而大大减少了技术上的负担。

> 中间件是位于平台(硬件和操作系统)和应用之间的通用服务,这些服务具有标准的程序接口和协议。针对不同的操作系统和硬件平台，它们可以有符合接口和协议规范的多种实现。

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/2018111321555179.jpg)

中间件有个很大的特点，是脱离于具体设计目标，而具备提供普遍独立功能需求的模块。这使得中间件一定是可替换的。如果一个系统设计中，中间件是不可替换的，不是架构、框架设计有问题，那么就是这个中间件，在 别处可能是个中间件，在这个系统内是引擎。

> 中间件一般应用于分布式架构中,用得非常多,只要分家了面临的问题就是沟通与协同,而沟通与协同就会采用一些中间件技术去解决和屏蔽系统之间的复杂性和让我们项目的架构变得更加的灵活,技术框架与我们的选择也不局限于某一种语言->这就是我们选择的原因!

### 1.2 中间件分类



> 分布式消息中间件:

1. ActiveMQ
   - 遵循界面式规范AMQP协议的消息中间件,由Java语言开发是一款老牌的高性能的中间件,但是复杂性要比RabbitMQ高!现在企业使用较少!
2. RabbitMQ
   - 比较流行且企业使用较多,如滴滴美团等等,优点是支持分发的模式、持久化和它的机制是比较完善的，并且最大优点就是跟Spring是同一家公司开发的，所以Spring对它的支持也是很完善的!
3. Kafka
   - 是一款高性能的消息中间件,是基于TCP/IP协议的二级制的传输协议来进行开发的,它的性能是最高的,因为它最贴近与底层,但是缺点就是不支持事务!但是可以支持持久化和一些分发机制,所以说还是比较完善,在一些大数据领域下使用得比较多!
4. RocketMQ
   - 由阿里和滴滴联合开发出来的一款国产消息队列,但是还在开发维护阶段,处于一个不稳定的阶段!

使用场景:

```bash
消息中间件监控数据、异步数据传输场景、削疯填谷场景、任务调度场景、海量数据同步场景、分布式事务场景、日记管理场景、大数据分析场景
```

遵循协议与特性：

```bash
AMQP、MQTT、持久化设计、Kafka协议、消息发布设计、高可用设计、可靠性设计、容错设计
```

注意：TCP协议无法满足我们的定义和需求规范，所以才会在TCP/IP的基础之上再重新去构建了一些内容。所以就命名成了我们的AMQP协议，但是它的底层依然还是TCP/IP协议

> 负载均衡中间件

1. Nginx
2. LVS负载均衡软件
   - 是指如果Nginx发生了故障,我们也会Nginx作集群,这个Lvs进行一个架构与集群
3. KeepAlive
   - 保持心跳来维持我们的高可用的目的
4. CDN
   - 加速,可以使网站的访问速度成倍的提升

> 缓存中间件

1. MemCache
   - 遵守TCP/IP协议,是属于代码级别,会把我们的缓存写入代码中,会占用JVM的内存,只适合小型网站去做缓存!如果分布式架构优先选择Redis
2. Redis
   - 遵守TCP/IP协议,高性能!

> 数据库中间件

1. MyCat
2. ShardingJDBC

> 案例分析

1. 异步数据保存
   - 可以让我们达到一个削峰的目的, 就是它可以异步让我们从串行变为并行,这样的话性能就会变得更高
2. 订单数据的消息分发
3. 分布式事务
4. 消息的容错
5. 分布式锁
6. 分布式会话
7. 分库分表

## 2.什么是消息中间件

### 2.1 架构演变

> 单体架构:

在我们最初的项目架构中,一般采用的是`单体架构`模式:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudybcffe923-72f4-413b-af26-dd91b742311b.png)

就是把所有的业务系统都丢进一个系统中进行架构,用Java语言或者Go语言去开发!

存在的问题:

1. 耦合度太高-所有东西都在一起
2. 运维的成本太高-每改动一个小地方都要重新部署重新打包
3. 不易维护
4. 服务器的成本高
5. 以及升级架构的复杂度也会增大

并且最大的一个问题就是我们把所有的东西都扔进一个项目中,那么我们如果迭代升级一小部分,那么面临的都是重新编译和重新部署项目!

> 分布式架构

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyd40834ed-a15f-4606-bb36-ce475b05a949.png)

分布式架构就是把我们的系统拆分成若干的部分,如果不理解那么就比对两个架构就可以得出:单体架构就是由一个请求由一个系统共同完成,而分布式架构有个典型的特征就是`一个请求由服务器端的多个服务来协同完成`!在分布式架构中就会拆分一个个独立的业务系统,而既然是拆分那么两者之间就要进行沟通,此时我们就要采用中间件技术,比如MQ或者Redis;

和单体架构不同的是，

- 单体架构是一个请求发起jvm调度线程（确切的是tomcat线程池）分配线程Thread来处理请求直到释放，
- 而分布式是系统是：一个请求是由多个系统共同来协同完成，jvm和环境都可能是独立。如果生活中的比喻的话，单体架构就想建设一个小房子很快就能够搞定，如果你要建设一个鸟巢或者大型的建筑，你就必须是各个环节的协同和分布，这样目的也是项目发展都后期的时候要去部署和思考的问题。

存在的问题:

1：学习成本高，技术栈过多
2：运维成本和服务器成本增高
3：人员的成本也会增高
4：项目的负载度也会上升
5：面临的错误和容错性也会成倍增加
6：占用的服务器端口和通讯的选择的成本高
7：安全性的考虑和因素逼迫可能选择RMI/MQ相关的服务器端通讯。

**好处**
1：服务系统的独立，占用的服务器资源减少和占用的硬件成本减少，确切的说是：可以合理的分配服务资源，不造成服务器资源的浪费
2：系统的独立维护和部署，耦合度降低，可插拔性。
3：系统的架构和技术栈的选择可以变的灵活（而不是单纯的选择java）
4：弹性的部署，不会造成平台因部署造成的瘫痪和停服的状态。

### 2.2 消息架构

​	单体架构和分布式架构和选择中间件的使用,从中我们就可以得知使用中间件可以很好的帮我们屏蔽底层操作系统的复杂性,还可以解决我们项目与项目之间的通讯,特别是在我们的分布式架构中我们的系统拆分成若干个子系统,而这些子系统共同来完成我们的请求,那么这个时候系统与系统之间的通讯就变得非常的重要,所以选择中间件必须要具备通讯的功能,让我们的东西变得更加的灵活!并且不仅仅局限于某种技术栈,而且在选择的时候变得更加的丰富与灵活,

所以说把我们的分布式架构系统往后推的话就会有一个新鲜的架构方式,比如说我们的基于消息中间件的分布式系统的架构:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyb888e5f0-2c0f-4576-af88-0176abfa7832.png)

从图中可以知道:

​	我们在选择中间件技术的时候要考虑几个问题:第一个沟通性,这是要一个根本性的问题;比如说我们这边APP采用的是Java语言然后右边会员系统采用的是Go语言,它们就要遵守一个协议和规范,不然它们怎么去沟通。第二个是消息是否具有可靠性，比如说我这个消息我丢给你，那么你是否会把我存起来呢？比如我这个消息断开了，下次是不是可以继续分发呢？...

我们可以这几个方面去考恒消息中间件：

1. 是否具有通讯的能力
2. 是否具有高性能易灵活等等
3. 是否支持跨平台,因为可以屏蔽语言的差异!
4. 具有持久化功能,不能丢失要具有高可靠还具有高可用,
5. 容错性和一些分发的策略
6. ....

> 消息中间件是可以利用可靠的消息传递机制进行系统和系统直接的通讯,并且可以提供消息传递和消息的排队机制,它可以在分布式系统环境下扩展进程间的通讯;

**消息中间件应用的场景:**

1. 跨系统数据传递
2. 高并发的流量削峰
3. 数据的分发和异步处理
4. 大数据分析与传递
5. 分布式事务

比如你有一个数据要进行迁移或者请求并发过多的时候,比如你有10w的并发请求下订单,我们可以在这些订单入库之前,我们可以把订单请求堆积到消息队列中,让它稳健可靠的入库和执行!

这里解释一下异步处理，也就是为什么异步编程能达到流量削峰的目的：

我们一般写的代码都是串行执行，就是一个请求由若干个方法执行的链路关系构成的！有个典型特征就是阻塞，前一个方法没执行完毕后一个方法是无法继续往后执行的；

![image-20211018201550279](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211018201550279.png)



这里是并行执行，也就是不需要一个一个方法执行，可以同时执行，也就是说开了多个线程去执行：

![image-20211018202024877](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211018202024877.png)

这也说明了为什么能高并发的流量削峰，就是取决于并行执行也就是我们的异步编程！我们可以拿到日常生活中去举例子，比如我们在赛跑的时候，接力跑一个接一个，最终跑完就是所有人的时间,这就是串行执行,所有时间的总和加起来就是整个方法执行的结束时间。而并行执行就好比是我们赛跑的时候一起跑，用户都在同一个跑道上，那么这个时候就会有先有后！而不像串行执行，一旦其中某一个方法遇到阻塞，那么整个的耗费时间都是很高的！

![image-20211018202739038](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211018202739038.png)

**常见的消息中间件:**

ActiveMQ、RabbitMQ、Kafka、RocketMQ等等;

**消息中间件的本质及设计**

它是一种接收数据,接收请求,存储数据,发送数据等功能的技术服务。

> MQ消息队列：负责数据的传递接收，存储和传递，所以性能要过于普通服务和技术！

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy7c171d88-687a-4c6c-8a97-2b257467172e.png)

**消息中间件的核心组成部分：**

1. 消息的协议
2. 消息的持久化机制
3. 消息的分发策略
4. 消息的高可用，高可靠
5. 消息的容错机制

### 2.3 消息队列协议

> 什么是协议

其实可以这样去理解：中国人跟美国人之间无法直接去交流，所以说我们必须在这过程中也是中间件需要建立一种规范，我们会把中文翻译成某种格式，然后美国人就把这个给转义出来，他们才能识别！

协议：

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy9e91d500-e775-45f3-92fa-78a6278efc51.png)

​	我们都知道消息中间件负责`数据的传递`,`存储`和`分发消息`三个部分,数据的存储和分发的过程中肯定要遵循某种约定成俗的规范,比如你是采用底层的TCP/IP协议还是UDP协议还是其他的等等,而这些约定成俗的规范就称之为:协议。

所谓的协议是指：

1. 计算机底层操作系统和应用程序通讯时共同遵守的一组约定，只有遵循共同的约定和规范，系统和底层操作系统之间才能互相交流。
2. 和一般的网络应用程序的不同它主要负责数据的接受和传递，所以性能比较高！
3. 协议对数据格式和计算机之间交换数据都必须要严格遵守规范。

> 网络协议的三要素

1. 语法:语法是用户数据与控制信息的结构与格式以及数据出现的顺序
2. 语义:语义是解释控制信息每个部分的意义,它规定了需要发出何种控制信息,以及完成的动作与做出什么样的响应
3. 时序:时序是对事件发生顺序的详细说明

理解:比如我MQ发送一个消息,是以什么数据格式发送到队列中,然后每个部分的含义是什么,发送完毕以后的执行的动作,以及消费者消费消息的动作,消费完毕的响应结果和反馈是什么,然后按照对应的执行顺序进行处理

按照我们对http协议的理解:

1. 语法:http规定了请求报文和响应报文的格式
2. 语义:客户端主动发起请求称之为请求(这是一种定义,同时你发起的是get/post请求)
3. 时序:一个请求对应一个响应.(一定先有请求再有响应,这个就是时序)

```
面试题:为什么消息中间件不直接使用http协议呢？
1.因为Http请求报文头和响应报文头是比较复杂的,包含了cookie、数据的加密解密。状态码、响应码等附加的功能，但是对于一个消息而言，我们并不需要这么复杂，也没有这个必要性，它其实就是负责数据	的传递、存储、分发就行，并且一定要请求的是：高性能、简洁、快速；
2.http大部分情况下都是短连接，在实际交互过程中，一个请求到响应很有可能会中断，中断以后就不会进行持久化，就会造成请求的丢失，这样就不利于消息中间件的业务场景，因为消息中间件可能是一个长	期的获取消息的过程，出现问题和故障要对数据或消息进行持久化等，目的就是为了保证消息和数据的高可靠和稳健的运行！

核心就是http复杂且是短连接
```

而消息中间件采用的并不是http协议，而常见的消息中间件协议有：OpenWire、AMQP、MQTT、Kafka，OpenMessage协议。

> AMQP协议

概述：(全称：Advanced Message Queuing Protocol) 是高级消息队列协议。由摩根大通集团联合其他公司共同设计。是一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。Erlang中的实现有RabbitMQ等。

特性：

1. 分布式事务支持。
2. 消息的持久化支持。
3. 高性能和高可靠的消息处理优势。

> MQTT协议

概述：（Message Queueing Telemetry Transport）消息队列是IBM开放的一个即时通讯协议，物联网系统架构中的重要组成部分。

特点：
1：轻量
2：结构简单
3：传输快，不支持事务
4：没有持久化设计。
应用场景：
1：适用于计算能力有限
2：低带宽
3：网络不稳定的场景。

> OpenMessage协议

概述：是近几年由阿里、雅虎和滴滴出行、Stremalio等公司共同参与创立的分布式消息中间件、流处理等领域的应用开发标准。

特点：

1. 结构简单
2. 解析速度快
3. 支持事务和持久化设计。

> Kafka协议

Kafka协议是基于TCP/IP的二进制协议。消息内部是通过长度来分割，由一些基本数据类型组成。
特点是：
1：结构简单
2：解析速度快
3：无事务支持
4：有持久化设计



> 总结:协议呢就是理解为一种君子契约,是在tcp/ip协议基础之上构建的一种约定成俗的规范和机制、它的主要目的可以让客户端（应用程序 java，go）进行沟通和通讯。并且这种协议下规范必须具有持久性，高可用，高可靠的性能。

### 2.4 消息队列持久化

​	提到持久化我们想到MySQL,那么它天生就是持久化的!简单来说持久化就是将数据存入磁盘。因为电脑无外乎就是CPU、内存、磁盘。存储数据的能力无外乎就是CPU、内存；默认一般都是存于内存中里面，那么存在内存有个典型的特征就是电脑一旦重启或者突发断电那么数据就会丢失！

​	比如我们这里APP或者前端系统把消息丢给了消息中间件，那么此时发生了故障那么也会将数据存入磁盘，然后等待恢复的时候又会将磁盘中的数据读取并且重新开始分发消息！

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyf908e193-4ca3-44b7-87d0-cbb17b55a107.png)

> 常见的持久化方式

|          | ActiveMQ | RabbitMQ | Kafka | RocketMQ |
| -------- | -------- | -------- | ----- | -------- |
| 文件存储 | 支持     | 支持     | 支持  | 支持     |
| 数据库   | 支持     | /        | /     | /        |

### 2.5 消息的分发策略

MQ消息队列有如下几个角色

1. 生产者
2. 存储消息
3. 消费者

```
生产者在生成消息之后MQ进行存储，那么消费者是如何获取消息呢？
	一般获取的数据方式无外乎推（push）或者拉（pull）两种方式，典型的Git就有推拉机制，我们发送的http请求就是一种典型的拉取数据库数据返回的过程，而消息队列MQ就是一种推送的过程，而这些机制会适用到很多的业务场景也有很多对应推机制策略
```

> 场景分析一

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy4acfaeda-fc73-4832-9207-7a9aa45b15a4.png)

比如我在APP上下了一个订单，我们的系统和服务很多，我们如何得知这个消息被那个系统或者那些服务或者系统进行消费，那这个时候就需要一个分发的策略。这就需要消费策略。或者称之为消费的方法论。

> 场景分析二

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy1a150320-9535-423a-99ec-76c5e3e654f7.png)

在发送消息的过程中可能会出现异常，或者网络的抖动，故障等等因为造成消息的无法消费，比如用户在下订单，消费MQ接受，订单系统出现故障，导致用户支付失败，那么这个时候就需要消息中间件就必须支持消息重试机制策略。也就是支持：出现问题和故障的情况下，消息不丢失还可以进行重发。

> 消息的分发策略的机制和对比

|          | ActiveMQ | RabbitMQ | Kafka | RocketMQ |
| -------- | -------- | -------- | ----- | -------- |
| 发布订阅 | 支持     | 支持     | 支持  | 支持     |
| 轮询分发 | 支持     | 支持     | 支持  | /        |
| 公平分发 | /        | 支持     | 支持  | /        |
| 重发     | 支持     | 支持     | /     | 支持     |
| 消息拉取 | /        | 支持     | 支持  | 支持     |

1. 发布订阅
   - 是指生产者生产消息推送到消息队列,然后只要有人订阅我那么大家都可以收到,也就是一种推的机制
2. 轮询分发
   - 将消息投递给消息队列,然后消息队列会按照一定的机制进行推送,这种推送规则就是一种公平的分发,也就是不会因为消费者的延时而造成的不公平性!无论你的服务器性能怎么样,都是公平的!比如说这里有100条消息,这里有三台服务器,那么是不会因为这三台服务器的性能高低而影响,平均每台都是33条,而最后一条随机分发一个服务器进行消费就可以了!
3. 公平分发
   - 这种分发就会造成数据的倾斜;比如这里有100条数据和对应三台服务器消费者,那么可能消费者1性能较慢,那么这里消费消息可能是10个,然后消费者2性能较高那么它这里消费消息就是50个,然后最后一个消息者3性能一般就是40个!这就造成我们数据的倾斜;->能者多劳模式
4. 重发
   - 典型的应用场景:当APP创建一个订单,订单系统出现异常和故障,此时我们消息队列里面有一个应答机制,它没有收到消费成功的反馈,那么就会出现消息的堆积,这个时候就不会出现消息的移除,那么这个时候就会出现消息的转移,如果我们有多个订单系统的话,比如集群,如果第一个订单系统没有收到应答那么它就认为这个消息就没有被消费,那么这个时候就会把消息重新投递到另外一个服务器进行消费!如果还不行就又重复分发!这里就是为了保证消息的可靠性而设计的一种机制!
5. 消息拉取
   - 是一种主动拉送机制,

注意:无论是轮询分发还是公平分发都有一个共同特征:它们的数据一旦被一个消费者消费,那么接下里别的消费者就不可能重复消费这条消息!

### 2.6 消息队列高可用和高可靠

​	我们知道消息中间件可以屏蔽我们操作系统的复杂性让我们应用程序与应用程序之间可以沟通和互联,让我们的技术和架构变得更加的灵活而不需要选择单一的架构方式,而这些要按照一定的规范和约定来进行实现,因为底层的TCP/IP协议并不灵活和支持度不高,所以它们就会在这基础之上重新开发协议进行定义和规范!比如AMQP协议、Kafka协议等等让我们技术变得更加的灵活,传输数据变得更快也变得更加的稳定和可靠, 其中它可以支持持久化,持久化呢就是将消息进行存盘,这样就不会因为写在内存突然遇到宕机或者出现故障而造成数据的丢失,然后呢它由比较好的分发策略,也就是消费者过多的时候就会支持发布与订阅、公平分发、轮询分发和一些重发和消息拉取的机制，当然以上用于评判是否是好的中间件吗？当然还不够！还需要有一个是否是高可用和高可靠，也即是是否支持集群，

> 什么是高可用机制

所谓的高可用:是指产品在规定的条件和规定的时间处于可执行规定功能状态的能力!

当业务量增加时,请求也过大,一台消息中间件服务器的会触及硬件(CPU、内存和硬盘)的极限,一台消息服务器你已经无法满足业务的需求,所以消息中间件必须支持集群部署,来达到高可用的目的!

**理解**：我说白了我不论你这个服务器出现了任何问题你都不能宕机或者不能进行消费的能力，当然这很难，但是我们必须要尽量往这个目标靠拢！

任何的中间件技术都会去考恒当业务发展到一定阶段都会有对应的机制和策略，其中我们的高可用在消息队列中有五种模式：

> 集群模式1·Master-slave主从共享数据的部署方式

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy71c973d1-2a1a-4a82-9d71-85182c38c9f2.png)

**解说**：生产者将消费发送到Master节点，所有的都连接这个消息队列`共享这块数据区域`，Master节点负责写入，一旦Master挂掉，slave节点继续服务。从而形成高可用，

**理解**：我们就会买很多的消息服务器进行集群，然后他们共享的是同一个消息区，其中Master是主机负责数据的写入,Slave不负责数据的写入,然后我们这里客户端把数据写入到Master中,而从节点负责连接到同一个数据空间,这样就能得到数据的共享,但是当Master挂掉了之后,这里问题就会很明显,数据就无法进行写入!所以这种模式在小规模应用场景中可能会用到!

**关键词"共享"**

优点:主机负责写,从机不负责写,从机可以跟主机共享同一个数据区域

缺点:一旦主机挂掉,那么数据无法进行写入

> 集群模式2-Master-slave主从同步部署方式

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy6504af54-f8f7-4d58-aac4-bb547176ae88.png)

**解释**：这种模式写入消息同样在Master主节点上，但是主节点会同步数据到slave节点形成副本，和zookeeper或者redis主从机制很类同。这样可以达到负载均衡的效果，如果消费者有多个这样就可以去不同的节点就行消费，以为消息的拷贝和同步会暂用很大的带宽和网络资源。在后续的rabbtmq中会有使用。

**理解**:生产者生产消息会将消息放在Master中,而Master就负责把数据同步写入到其他从机中,这种就造成消息的副本!但是这种副本的拷贝会需要用到大量的网络带宽和资源,所以推荐在部署集群的时候最好放在同一个机房也就是局域网内,它们的性能才能得到保证,否则就会造成很严重的问题!而这种模式出现的原因就是因为前面共享模式的是共享同一个数据区域,而如果这个区域一旦丢失,那么所有的服务数据都没用了!所以就出现了同步部署,在从机上进行拷贝来防止出现数据丢失造成数据无法修复!

**关键词"拷贝"**

优点:主机把消息拷贝到从机上,达到负载均衡的目的

缺点:需要用到大量的网络带宽,因为用到了副本拷贝!

> 集群模式3·多主集群同步部署模式

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy402ac1b2-2d51-493c-9bc0-37329912dd2b.png)

**解释**：和上面的区别不是特别的大，但是它的写入可以往任意节点去写入。

**理解**:上面的那种模式是单写多读的模式,而这个就是多写多读的模式,一个消息发过来就是一种分发下去,主节点有一个,其他从节点都有一个,都会自动写一份,而消费者就会按照一定的策略来进行读写,也可以从主节点上读取,也可以从从节点上进行读取,这就完成了一个闭环,这样就可以任意节点写入,任意节点读取,多主多从的部署模式

**关键词"多主多从"**

优点:采用多写多读机制,消费者可以任意节点上读取

缺点:因为数据太大而出现问题---演变为下面的模式

> 集群模式4·多主集群转发部署模式

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudycc632a4a-a382-4303-85f9-8f63e93cea7a.png)

解释：如果你插入的数据是broker-1中，元数据信息会存储数据的相关描述和记录存放的位置（队列）。
它会对描述信息也就是元数据信息就行同步，如果消费者在broker-2中进行消费，发现自己几点没有对应的消息，可以从对应的元数据信息中去查询，然后返回对应的消息信息，场景：比如买火车票或者黄牛买演唱会门票，比如第一个黄牛有顾客说要买的演唱会门票，但是没有但是他会去联系其他的黄牛询问，如果有就返回。

**理解**:这是一种元数据的转发机制,这种元数据呢不会把消息主体放在元数据信息里面,它只会把消息的描述信息放在这里面,然后在主机放一份,从机放一份,然后两者之间建立联系之后,消费者去消费消息的时候,如果在我们这个从节点中或者在任意节点中没有找到这个信息,那么它会去这个元数据去匹配,匹配到就返回,没有匹配到他就会把消息的信息携带到我们请求里面然后转发一个请求到别的服务器去询问,直到询问完毕!

**关键词"元数据"**

好处:可以减少我们数据的空间存储和不造成服务器资源的浪费,因为元数据之间都会进行一个同步!



> 集群模式5 Master-slave与 Breoker-cluster组合的方案

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudya02ff996-446e-4d39-b230-dadb183269e5.png)

解释：实现多主多从的热备机制来完成消息的高可用以及数据的热备机制，在生产规模达到一定的阶段的时候，这种使用的频率比较高。

理解:这种就是多主多从的小规模集群的方案!





他们的最终目的都是**为了保证**：消息服务器不会挂掉，出现了故障依然可以抱着消息服务继续使用。

反正终归三句话：
1：要么消息共享，
2：要么消息同步
3：要么元数据共享

> 什么是高可靠机制

所谓**高可用是指**：是指系统可以无故障低持续运行，比如一个系统突然崩溃，报错，异常等等并不影响线上业务的正常运行，出错的几率极低，就称之为：**高可靠**。

在高并发的业务场景中，如果不能保证系统的高可靠，那造成的隐患和损失是非常严重的。

如何保证中间件消息的可靠性呢？可以从两个方面考虑：

1：**消息的传输**：通过协议来保证系统间数据解析的正确性。

2：**消息的存储可靠**：通过持久化来保证消息的可靠性。 

 

## 3.快速入门

### 3.1 安装

由于RabbitMQ是用Erlang编写的，因此需要先安装Erlang环境，

这是下载地址:https://www.erlang.org/downloads

我下载的时候版本是24.1

然后安装好后配置环境变量,这里我贴上我使用的链接:https://www.cnblogs.com/xieshuang/p/10114213.html

成功后的截图:

![image-20211019162043612](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019162043612.png)

然后去RabbitMQ上下载:https://github.com/rabbitmq/rabbitmq-server/releases/

然后这是安装教程:https://www.cnblogs.com/saryli/p/9729591.html

然后我们最终安装完毕并且启动了,此时我们就可以去http://localhost:15672看看:

![image-20211019163836885](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019163836885.png)

默认账号密码是`guest`和`guest`;

> 授权账号和密码

1. 新增用户

   ```erlang
   rabbitmqctl add_user 账号 密码
   ```

2. 设置用户分配操作权限

   ```erlang
   rabbitmqctl set_user_tags 账号 administrator
   ```

这里说一下角色级别:

1. administrator:可以登录控制台、查看所有信息、可以对rabbitmq进行管理
2. montioring:监控者、登录控制台、查看所有信息
3. policymaker:策略制定者、登录控制台、指定策略
4. managment：普通管理员、登录控制台

注:用户级别从上往下,由高到低!

3. 为用户添加资源权限

   ```erlang
   rabbitmqctl set_permissions -p / 账号 ".*" ".*" ".*"
   ```

   这里我如果添加用户级别为`administrator`那么其本身我们设置资源访问权限是可有可无的,因为最高级别本身就可以访问一切!

> 这里总结一下会用到的命令:

```erlang
rabbitmqctl add_user 账号 密码
rabbitmqctl set_user_tags 账号 administrator
rabbitmqctl change_password Username Newpassword 修改密码
rabbitmqctl delete_user Username 删除用户
rabbitmqctl list_users 查看用户清单
rabbitmqctl set_permissions -p / 用户名 ".*" ".*" ".*" 为用户设置administrator角色
rabbitmqctl set_permissions -p / root ".*" ".*" ".*"
```



### 3.2 RabbitMQ的角色分类

1. **none**:

   - 不能访问management plugin,也就是说最起码的登录权限是没有的!

2. **management**:查看自己相关节点信息,相当于就是个人中心,只能看见自己的

   - 列出自己可以通过AMQP登入的虚拟机
   - 查看自己的虚拟机节点 virtual hosts的queues,exchanges和bindings信息
   - 查看和关闭自己的channels和connections
   - 查看有关自己的虚拟机节点virtual hosts的统计信息。包括其他用户在这个节点virtual hosts中的活动信息。

3. **Policymaker**:在management的基础之上我多了一点东西,也就是我可以创建xxx东西

   - 包含management所有权限
   - 查看和创建和删除自己的virtual hosts所属的policies和parameters信息。

4. **Monitoring**:相当于管理员,除了看我自己我还可以看其他人的,但是也只能去看不能删

   - 包含management所有权限
   - 查看其他用户的connections和channels信息
   - 查看节点级别的数据如clustering和memory使用情况
   - 查看所有的virtual hosts的全局统计信息。

5. **Administrator**:最高级别的管理员,我可以查看所有人的东西并且能够删除管理其他人的东西!

   - 最高权限
   - 可以创建和删除virtual hosts
   - 可以查看，创建和删除users
   - 查看创建permisssions
   - 关闭所有用户的connections

6. 具体的操作的界面

   ![image-20211019173809457](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019173809457.png)

我们可以以admin权利登录进去然后创建一个用户,然后赋予权限为`policymaker`,此时,我们在重新登录进去,就是这样:

![image-20211019173920782](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019173920782.png)

因为当前是`policymaker`只能查看当前信息等等,所以此时并没有任何东西!



### 3.3 第一个简单模式和其他消息模式

我们来通过代码的方式来理解一下RabbitMQ之间是如何去通讯的!

RabbitMQ既然是中间件技术,那么它肯定有如下几个东西:

![image-20211019191730371](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019191730371.png)

Producer是消息生成者,消费者是订阅这个消息,我们现在需要干的事情就是将生产者的消息投递到MQ中,消费者就来把它的消息给消费掉;

那么这里我们通过官方网站介绍这6种通讯机制、分发机制:https://www.rabbitmq.com/getstarted.html

![image-20211019192307680](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019192307680.png)

其中:

第一种是简单模式,一个生产者一个消费者中间会有那个队列,

第二种是工作模式,这个就是指我们之前学的消息队列的分发策略中的轮询分发和公平分发两种机制,

第三种是发布与订阅模式,我们可以看见有个蓝色的小块那是交换机,

第四种是Routing模式,它是在发布于订阅模式的基础之上增加了一个路由key,

第五种是主题模式,增加了模糊的路由key,可以模糊匹配,

第六种RPC是一种拉取机制,一般用得比较少,可以不用去理解!

![image-20211019192324454](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019192324454.png)

> 实现步骤:

1. jdk1.8
2. 构建一个maven工程
3. 导入rabbitmq的maven依赖
4. 启动rabbitmq-server服务
5. 定义生产者
6. 定义消费者
7. 观察消息的在rabbitmq-server服务中的过程

### 3.4 使用和观察服务过程

注:rabbitmq和spring同属一个公司开放的产品，所以他们的支持也是非常完善，这也是为什么推荐使用rabbitmq的一个原因。

这里只是展示Simple模式,也就是简单的模式,一个生产者一个消费者,中间一个队列:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/python-one.png)



1. 导入依赖:

   ```xml-dtd
   <!--导入Java原生依赖-->
   <dependencies>
       <dependency>
       <groupId>com.rabbitmq</groupId>
       <artifactId>amqp-client</artifactId>
       <version>5.10.0</version>
       </dependency>
   </dependencies>
   ```

2. 启动MQ服务

   ```bash
   F:\RabbitMQ\rabbitmq_server-3.9.7\sbin\rabbitmq-server.bat
   ```

> 定义生产者

   ```java
   package cn.miao.simple;
   
   import com.rabbitmq.client.Channel;
   import com.rabbitmq.client.Connection;
   import com.rabbitmq.client.ConnectionFactory;
   
   import java.nio.charset.StandardCharsets;
   
   /**
    * @program: QuickStart-RabbitMQ
    * @description: 生产者
    * @author: MiaoWei
    * @create: 2021-10-19 18:28
    **/
   public class Producer {
   
       public static void main(String[] args) {
           //所有的中间件技术都是基于tcp/ip协议基础之上构建新型的协议规范,只不过rabbitMQ遵循的是AMQP协议
           //为什么有AMQP协议:就是因为TCP/IP协议无法去满足它的一些规范,比如我要把这消息的一些标签,通道,都要把它记录起来,这个TCP/IP就无法满足了!
           //所以开发出一个协议在这之上定义一些属于自己的规范,然后命名为AMQP
   
           //1.创建连接工厂
           ConnectionFactory connectionFactory = new ConnectionFactory();
           //设置连接属性
           connectionFactory.setHost("127.0.0.1");
           connectionFactory.setPort(5672);
           connectionFactory.setUsername("admin");
           connectionFactory.setPassword("admin");
           //设置虚拟访问节点,是指把我们的消息发在根节点上
           connectionFactory.setVirtualHost("/");
   
           Connection connection = null;
           Channel channel = null;
           try {
               //2.从连接工厂中获取连接
               connection = connectionFactory.newConnection("生成者");
               //3.从连接中获取通道channel
               channel = connection.createChannel();
               //4.通过通道创建交换机,声明队列,绑定关系,路由key,发送消息和接收消息,申明队列queue存储消息
               /**
                *  如果队列不存在，则会创建,Rabbitmq不允许创建两个相同的队列名称，否则会报错。 
                *
                * @Params1 队列的名称
                * @Params2 是否要持久化durable=false,所谓的持久消息是否要存盘,false: 非持久化,true: 持久化,  思考?非持久化会存盘吗?
                * @Params3 排他性, 是否是一个独占队列,即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
                * @Params4 是否自动删除, 随着最后一个消费者消息完毕消息以后是否把队列自动删除
                * @Params5 携带一些附加参数,可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等。
                */
               String queueName = "queue1";
               //声明队列
               channel.queueDeclare(queueName, false, false, false, null);
               //5.准备发送消息内容
               String message = "Hello MQ,I is Simple";
               //6.发送消息给队列queue,
               /**
                * @Params1 :交换机exchange
                * @Params2 :队列名(路由key),表示往哪个队列里面去发,
                * @Params3 :发送消息过程中是否要对这里面消息进行持久化,一般这里写`MessageProperties.PERSISTENT_TEXT_PLAIN`,
                * @Params4 :发送的内容
                */
               //这里Publish就是我们之前学习redis的命令,是发布者订阅者模式中的发布命令
               channel.basicPublish("", queueName, null, message.getBytes(StandardCharsets.UTF_8));
   			//面试题:可以存在没有交换机的队列吗?不可能!虽然没有指定交换机但是一定存在一个默认的交换机!
               System.out.println("消息发送成功!");
   
           } catch (Exception e) {
               e.printStackTrace();
           } finally {
               //7.关闭通道
               if (channel != null && channel.isOpen()) {
                   try {
                       channel.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
               //8.关闭连接
               if (connection != null && connection.isOpen()) {
                   try {
                       connection.close();
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               }
           }
       }
   }
   ```

   以上就完成了从生产者到队列的这个过程:

   ![image-20211019195727930](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019195727930.png)



​	接下来我们打个断点调试一下,看看发生了哪些变化:

1. 我把断点放在这里:

   ```java
    connection = connectionFactory.newConnection("生成者"); //创建一个连接
   ```

2. 此时我们看页面:

   ![image-20211019200655819](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019200655819.png)

   ​	此时我们就可以看见在导航栏`Connections(连接对象)`这里发现重要的信息,Name是生产者是我们定义连接的名字,UserName是用户名,State是运行的状态!

3. 然后我们继续走断点,走到这一步:

   ```java
   channel = connection.createChannel();//创建一个管道
   ```

4. 然后我们点击导航栏上的`Channels`,然后看:

   ![image-20211019201501038](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019201501038.png)

   ​	此时我们就能发现这里Channel的连接对象是跟前面Connections的连接对象是一致的(因为我重复打断点所以这里对线也是在变化),这是表示创建了一个管道

5. 然后我们继续走:

   ```java
   channel.queueDeclare(queueName, false, false, false, null); //声明队列
   ```

6. 然后此时我们点击导航栏的`Queues`,看看:

   ![image-20211019201837799](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019201837799.png)

   这里队列的名字就是我们刚刚声明的`queue1`这个名字;然后还看到Ready:消息就绪状态,Total表示消息的总数,然后Features是否是持久化,如果我在声明队列的时候表示是持久化的话就是这种带D的这种:

   ![image-20211019202423877](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019202423877.png)

   

7. 执行发送，这个时候可以在web控制台查看到这个队列queue的信息

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy51240e70-79d7-4b3f-9a83-6febb3499a42.png)

8. 我们可以进行对队列的消息进行预览和测试如下：

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyed656121-1e27-4c0e-84e7-70ae48f3b0f1.png)

9. 进行预览和获取消息进行测试

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudydc7c7bf4-bffe-4821-92da-c1c8563631d3.png)



> 定义消费者

```java
package cn.miao.simple;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 生产者
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Producer {

    public static void main(String[] args) {
        //所有的中间件技术都是基于tcp/ip协议基础之上构建新型的协议规范,只不过rabbitMQ遵循的是AMQP协议
        //为什么有AMQP协议:就是因为TCP/IP协议无法去满足它的一些规范,比如我要把这消息的一些标签,通道,都要把它记录起来,这个TCP/IP就无法满足了!
        //所以开发出一个协议在这之上定义一些属于自己的规范,然后命名为AMQP

        //1.创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        //设置虚拟访问节点,是指把我们的消息发在根节点上
        connectionFactory.setVirtualHost("/");

        Connection connection = null;
        Channel channel = null;
        try {
            //2.创建连接Connection
            connection = connectionFactory.newConnection("生成者");
            //3.通过连接获取通道channel
            channel = connection.createChannel();
            //4.通过通道创建交换机,声明队列,绑定关系,路由key,发送消息和接收消息
            /**
             * @Params1 队列的名称
             * @Params2 是否要持久化durable=false,所谓的持久消息是否要存盘,false: 非持久化,true: 持久化,  思考?非持久化会存盘吗?
             * @Params3 排他性, 是否是一个独占队列
             * @Params4 是否自动删除, 随着最后一个消费者消息完毕消息以后是否把队列自动删除
             * @Params5 携带一些附加参数
             */
            String queueName = "queue1";
            //声明队列
            channel.queueDeclare(queueName, false, false, false, null);
            //5.准备发送消息内容
            String message = "Hello MQ,I is Simple";
            //6.发送消息给队列queue,
            /**
             * @Params1 :暂时为空
             * @Params2 :队列名,表示往哪个队列里面去发
             * @Params3 :发送消息过程中是否要对这里面消息进行持久化,一般这里写`MessageProperties.PERSISTENT_TEXT_PLAIN`,
             * @Params4 :发送的内容
             */
            channel.basicPublish("", queueName, null, message.getBytes(StandardCharsets.UTF_8));

            System.out.println("消息发送成功!");

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            //7.关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            //8.关闭连接
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

打印:

![image-20211019203418189](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019203418189.png)

以上就完成了消费者从队列中获取消息的过程:

![image-20211019203324561](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019203324561.png)

接下来我们就来具体的分析分析:

1. 此时我们再看队列中:

![image-20211019203911874](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019203911874.png)

​	此时消息已经被释放了

2. 我们修改代码将声明队列的持久化开启,看看是什么样子:

   ```java
   channel.queueDeclare(queueName, true, false, false, null);
   ```

3. 页面:

   ![image-20211019204918960](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019204918960.png)

​	可以发现这里有个D,这就表示这就是持久化队列,而持久化队列和非持久化队列的区别就是:持久化队列就是无论服务器重启或者故障并不会移除,而不是持久化队列那么在重启的时候就会移除掉!

4. 之前还提到过声明队列的时候可以附加参数,所以我们直接修改看看页面发生的变化:

   ```java
   HashMap<String , Object> map = new HashMap<>();
   map.put("参数1", "参数值1");
   channel.queueDeclare(queueName, true, false, false, map);
   ```

5. 页面:

   ![image-20211019205251198](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019205251198.png)

   发现是已经携带了参数了的!并且我们点击`queue1`可以看到具体的参数:
   ![image-20211019205345935](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211019205345935.png)

6. 这里还有一个问题就是我们在使用生产者的时候说非持久化会存盘吗?答案:会存盘!但是会随着服务器重启而丢失!

### 3.5.AMQP的流转过程

> 概述:AMQP全称:Advanced Message Queuing Protocol(高级消息队列协议)。是应用层协议的一个开发标准，为面向消息的中间件设计。

**AMQP生产者流转过程**:

Broker:就是我们的MQ服务,一个节点的意思

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy7c8a41b8-e3bf-4821-a1f1-a18860277663.png)

理解:这个跟我们在第一个简单模式中写的生产者代码是一样的!

步骤:

1. 首先会建立连接,连接会有一个协议头,就好比我们在代码中会创建一个连接工厂设置参数,然后我们就会将其打包成一些信息并且携带在我们的Protocol协议头里面发送到服务器,进行碰撞,比如比对账户密码是否正确:

   ```java
   ConnectionFactory connectionFactory = new ConnectionFactory();
   connectionFactory.setHost("127.0.0.1");
   ....
   ```

2. 当碰撞成功了就会返回给生产者一个回执,就表示连接已经ok了.然后中间就开始创建我们的通道了

3. 然后我们就可以去发送消息完成整个消息的闭环!



```
面试题:为什么rabbitMQ为什么是基于通道去处理而不是链接呢?
	因为连接是一个短连接,那么就会经历三次握手四次挥手,是一个很慢的!也就是耗时很长又开又关!会造成性能的很大的开销,所以说这里并没有把连接作为处理,而是将这个连接作为一个长连接去处理,这个长连接里面就有一个信道channel,当我们处于一个高并发的场景下,我们这个信道的性能是非常高,它会创建多个通道来处理你的消息!
```

**AMQP消费者流转过程**:

Broker:就是我们的MQ服务,一个节点的意思

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy081077ba-eced-43f9-b148-6f63987f1d2f.png)

​	理解:这个跟生产者的执行流程整体是一样的!也是一样通过连接工厂获取然后设置属性进行打包放在协议头里发送到服务器进行碰撞! 碰撞成功就打开信道,然后就会信道里取一个出来,也就是说未来你会看到连接只有一个可能通道有多个;然后取了一个通道出来了以后在这里进行消费通过Basic.Consume进行消费,消费以后这里就会比上面多了一个点就是ACK应答,应答的话就有两种一种是手动应答一种是自动应答,而我们一般使用的是手动应答,就是代码与我们的NACK进行确认!因为这是一种消费者的可靠的消息消费!而正常应答的话就会从我们服务器中移除!而如果是不应答的话就会消息的不停重试直到消费为止,也就是出现了故障,那么消息队列就会被挂起,一旦被挂起生产者就会屏蔽生产者的消息的接受,系统就会发生故障!

## 4.核心组成部分

### 4.1 组成

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy62a1f9e3-027d-408a-8fb4-a176bd184d23.png)

核心概念:

- **Server**:又称Broke,它是接受客户端的连接,实现AMQP实体服务,也就是我们安装的rabbitmq-server
- **Connection**:连接,应用程序与Broker的网络连接TCP/IP 三次握手和四次挥手,因为要不停的连接造成性能损耗,所以在基础之上开发了一个长连接来通过下面的Chanel信道来维持!
- **Channel**:网络信道,几乎所有的操作我们都是在Channel中进行,Channel是进行消息读写的通道,客户端可以建立多个Channel,但每个Channel代表一个会话任务!
- **Message**:消息,服务与应用程序之间传送的数据,由Properties和body组成;Properties可以对消息进行修饰,比如消息的优先级、延迟等高级特性,Body则是消息体的内容;
- **Virtual Host**:虚拟地址,用于进行逻辑隔离,是最上层的消息路由,一个虚拟机主机路由可以有若干个Exchange和Queue,但是同一个虚拟主机里面不能有相同名字的Exchange
- **Exchange**:交换机,主要用于接收消息,根据路由将发送消息到绑定的队列(`不具备消息存储的能力`)。要记住是交换机来接收投递给队列的,而不是队列来接收消息的!
- **Bindings**:Exchange和Queue之间的虚拟连接,binding中可以保护多个routing key,就会把这个路由key和我们的交换机来绑定这个队列的一个对象!一旦绑定就形成一个闭环,接下来在发消息的过程中只要带上这个路由key,消费者在监听的时候就会通过这个路由key进行匹配!
- **Routing key**:是一个路由规则,虚拟机可以用它来确定如何该路由是一个特定消息,这个路由key就是用于在分发消息的时候是否指定给特定的消费者进行分发!如果不带这个key那么就是全部人都会收到!
- **Queue**:队列,也是消息队列,目的保存消息并将它们转发给消费者!

**理解**:从图中可知一个Producer(生产者)生产我们的消息,会开辟一个连接然后连接里面就会有我们的通道,而通道里面就会有一个概念是交换机,这个交换机我们都看见过就是在生产者在信道中声明队列的第一个参数就是交换机,如:`channel.basicPublish("", queueName, null, message.getBytes(StandardCharsets.UTF_8));`,不过我们没写,那么注意没写不代表没有交换机,没指定交换机但是一定会有一个默认的交换机,这也是我们面试题会问到的:可以存在没有交换机的队列吗?!!!

​	所以说生产者会开启连接开启通道,然后就会把消息通过交换机来传递给队列的!注意`消息一定是通过交换机来传递给队列的!`,虽然我们在声明队列的时候是直接把消息放在了队列中但其实不是这样的,因为MQ的底层语言是Erlang语言写的,而Erlang本身就是专门开发交换机的一个语言;回过头来:交换机就是负责消息的分发和投递,然后呢就会把消息投递到这个队列里面,而在这个投递的过程中就会有一些分发策略,也就是说可以通过路由key来进行过滤筛选给指定的消费者,没有的话直接分发给每一个消费者;

​	这里我们还可以看见broker,代表是服务的意思也就是一个节点,它就是rabbitMQ的服务,后面也就是有很多的broker的集群,在这个集群里为了去隔离和区分,这里也搞了一个虚拟机节点,这个虚拟节点可以理解为我们电脑中的磁盘,比如C盘D盘,目的就是为了隔离!这样子去理解:我们在系统中有订单消息和用户消息等等各种各样的信息全部堆积在这个一个根节点就会庞大难以区分,所以这个虚拟节点就将其分隔开;然后消费者就会来订阅我们的消息,一旦被订阅,那么交换机就会把消息推送给我们的Cousumer(消费者);

**总结**:其实说这么多,总体是这样的:一个生产者生产消息然后创建连接管道,然后在信道里通过交换机来传输消息到MQ服务中,而在MQ中存在多个虚拟节点当然由我们指定,然后在虚拟节点中通过交换机获取到消息然后进行消息分发和投递,这个时候就会有一些分发策略,在这个过程中会通过routingkey来筛选是否确定给指定的消费者分发消息,没有就都发!然后投递到我们的消息队列中,进行保存和转发!其中bindins就是交换机和队列之间的虚拟连接,中间保护了路由key;然后这个消费者也通过相同的步骤,订阅了我们这个消息,那么此时队列就会将消息转发给我们的消费者!



**注意的点:**

- rabbitmq发送消息一定有一个交换机

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudye28575ea-17f4-41a8-ac32-133727fd63ae.png)

​	可以看见Bindings绑定的是默认交换机!

并且在这里也可以看见交换机中有一个自带的默认交换机!

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyd23fdb11-89c8-4883-a027-76d93d257138.png)

> 这是MQ整体的架构图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy23e6e571-d661-4f4b-b4f4-4d4efb766bc3.png)

​	理解:这个跟上面是一样的,一个生产者将消息投递到交换机里面,在交换机里面就会把消息投递到队列里面,而在队列里面就会把消息进行分发,在分发的过程中进行过滤,比如加上路由key来指定消费者,就好比我们SQL中带上了`where routingkey=消费者1`这个意思;如果不过滤那么就不加!

### 4.2 运行流程

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy2704cee9-3595-45de-892d-ee658e848806.png)

**理解**:生产者通过业务数据也就是消息转换成JSON,当然不止JSON也可能是另外的方式,总之序列化转换成字节以后接下来通过交换机或者路由key来添加这个标签头或者这个队列的名字,传递到我们的MQ服务端,如果这个时候有消费者进行订阅了它,那么就可以根据他的策略进行分发,如果是发布者订阅模式那么它就会全部收到,如果你有routingkey的机制的话那么接下来就会根据routingkey来进行匹配;然后消费者在收到消息以后就会有一个反序列化过程,将业务数据反序列化出来比如是一个json,然后我们就能进行相关的业务处理!就是这个逻辑!

## 5.消息模式示例与理解

这是官网地址:https://www.rabbitmq.com/getstarted.html

这里不进行介绍其中的RPC和发布确认模式,只介绍前五种

### 5.1 简单模式

![img](https://www.rabbitmq.com/img/tutorials/python-one.png)

首先我们可以看到:P代表生产者,中间区域代表队列,C代表消费者;

代码示例的话我已经在快速入门/第一个简单模式进行描述了!

这里介绍如何使用MQ的图形化界面去操作这个简单模式:

查看一些基本信息:

1. 创建一个队列:

   ![image-20211022170043529](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022170043529.png)

2. 查看当前队列的信息,

   ![image-20211022170717951](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022170717951.png)

   其中D为持久化的意思,表示随着MQ的服务重启或者宕机,那么重启之后这个队列依然存在消息也会得到持久化;Ready代表是一个就绪的状态,Total代表消息有多少条,Unacked代表消息还有多少条未被确认;后面Message rates就是我们磁盘的一些状况以及我们应答的一些状态;

3. 我们点击这个querue1可以看到Overview:

   ![image-20211022171155626](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022171155626.png)

   这个就是我们当前队列基本运行状况,以及它的一些条目数,...等等

   .... 

4. 点击`publish message`

   ![image-20211022180843073](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022180843073.png)

   这里面就是我们生产消息,其中`Delivery mode`表示是否是持久化的意思,1是不持久化,2是持久化,	

5. 这里就是获取消息的地方,其中`Ack mode`选择的是Nack表示的预览消息,如果是选择的`Automatic ack`那么是应答消息,就会将消息从队列中移除!

   ![image-20211022181206128](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022181206128.png)

> 运行

1. 点击Exchanges交换机发送消息

   ![image-20211022181922744](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022181922744.png)

   

   2. 此时我们看到消息就是:处于一个就绪状态且总共有1条消息;

      ![image-20211022183743995](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022183743995.png)

   3. 然后点queue1然后查看消息:

      ![image-20211022184419003](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022184419003.png)

> 总结:无论是简单工模式还是工作队列模式它们都有一个默认的交换机,发送消息一定是交换机去发送而不是队列!,交换机接收消息推送到队列中,队列以后消费者会自动监听订阅消息,然后进行推送! 还有没有声明交换机那么一定是走默认的交换机

### 5.2 Fanout模式

这个模式也称"**发布与订阅模式**"

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/python-three.png)

#### 5.2.1 web界面

1. 创建一个fanout类型的交换机

   ![image-20211022200712629](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022200712629.png)

   2. 查看我们的交换机

      ![image-20211022200816591](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022200816591.png)

      此时我们的交换机已经创建好了!

   3. 然后我们创建两个队列:

      ![image-20211022201013069](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201013069.png)

   4. 再将两个队列与我们的自定义交换机进行绑定

      ![image-20211022201335325](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201335325.png)

   5. 绑定后就是这种:

      ![image-20211022201508468](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201508468.png)

      6. 接下来我们就往我们的交换机上投递一条信息,我们可以通过web页面俩模拟这个过程:

         ![image-20211022202002123](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022202002123.png)

      7. 然后此时我们就能看到对应的队列中已经存在了消息:

         ![image-20211022202058346](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022202058346.png)

      > 接下来只要有人订阅了我们这个队列,那么接下里都会收到消息;通过交换机与队列进行一对多进行绑定,那么我们只需要往一个交换机进行发送消息,那么消费者只要订阅了队列就会收到消息!就好比是广播,通过一个广播进行扩散!

注意:我们可以看见这里有一个Routing key,那么思考我们可不可以给指定的队列绑定一个key,然后根据key去发送呢?其实这种是毫无意义的!也就是说无论你增加多少key,Fanout模式都会通过交换机将数据给绑定交换机的队列中发送!这里我是做过相关测试的!都会收到!加key是毫无意义的!!!

### 5.3 Direct模式

这个就是我们模式中的`Routing模式`

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy33427b78-879d-4511-9dd7-42fb33108339.png)

这个模式跟上面的Fanout模式是差不多的,只不过这个模式比上面多了一个`路由key`的概念,这个路由key呢就相当于给它指定了一个where条件,比如:where routekey=error,而消费者就必须去绑定这个error,然后我们就会把属于error的这个消费者给查询出来;其实可以想象成是一个过滤的一个条件就可以了!

#### 5.4 web界面

步骤:

1. 创建Direct模式的交换机

   ![image-20211022210832934](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022210832934.png)

2. 然后我们针对一个队列进行绑定一个routing key,这样我们待会就可以不用指定队列去发送:

   ![image-20211022212101541](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212101541.png)

   最终就是这样子的:

   ![image-20211022212145984](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212145984.png)

3. 然后我们现在就往order这个路由key里面发送消息:

   ![image-20211022212416736](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212416736.png)

   > 发现就只有指定的路由key绑定的队列收到了消息,而其他却没有收到!这就是Direct模式
   >
   > 结论:Direct模式其实就是在fanout模式上增加了一个路由key;这个路由key只要命名为相同的key那么就可以归类分组,然后我们就可以根据类型分组去分发消息,它会根据key找到对应的队列然后进行分发,这样就达到了一个按需选择的目的;

比如这种:

![image-20211022213550448](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022213550448.png)

给order发送消息,那么此时对应的两个队列都会收到消息,此时就已经达到了一个按需选择的目的!!!!

