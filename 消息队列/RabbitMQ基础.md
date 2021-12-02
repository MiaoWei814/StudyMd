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

> 将MQ安装到系统服务中,随着电脑启动而启动

```erlang
RabbitMQ Service-install :安装服务 
RabbitMQ Service-remove 删除服务 
RabbitMQ Service-start 启动 
RabbitMQ Service-stop 停止
```

如果没有开始菜单则进入安装目录下sbin目录手动启动:

![image-20211112171022938](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112171022938.png)

```erlang
安装并运行服务 
rabbitmq-service.bat install 安装服务 
rabbitmq-service.bat stop 停止服务 
rabbitmq-service.bat start 启动服务
```

注意踩坑:

1. 如果安装路径有中文，或者系统登录用户是中文账户，启动RabbitMQ会闪退，需要修改MQ的配置
   - 解决方案：https://blog.csdn.net/leoma2012/article/details/97636859

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
               //声明队列-创建队列,
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
               //这里因为是默认交换机所以类型为direct,所以这里的routingkey默认为队列名,并且我们这里走的是默认交换机,而默认交换机和队列进行绑定,所以我们不需要进行绑定交换机和队列之间				的关系
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

> 注意:因为我们没有指定交换机那么一定走的是默认的交换机,而我们在发布消息的时候,routing key却是队列名,这是因为当没有交换机的话那么routingkey一般指定的是我们的队列!  

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

import com.rabbitmq.client.*;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 消费者
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Consumer {
    public static void main(String[] args) {
        //所有的中间件技术都是基于tcp/ip协议基础之上构建新型的协议规范,只不过rabbitMQ遵循的是AMQP协议
        //为什么有AMQP协议:就是因为TCP/IP协议无法去满足它的一些规范,比如我要把这消息的一些标签,通道,都要把它记录起来,这个TCP/IP就无法满足了!

        //1.创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
//        connectionFactory.setVirtualHost("/");  //把消息发在我们的根节点上

        Connection connection = null;
        Channel channel = null;
        try {
            //2.创建连接Connection
            connection = connectionFactory.newConnection("生成者");
            //3.通过连接获取通道channel
            channel = connection.createChannel();
            //4.通过通道创建交换机,声明队列,绑定关系,路由key,发送消息和接收消息
            //这里表示从`queue1`这个队列里面取,-----可以理解为这是在监听队列
            //第二个参数是是否是自动应答
            channel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println("收到消息是 " + new String(delivery.getBody(), StandardCharsets.UTF_8));
                }
                //这里是出现异常的状况,比如接收消息失败!
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                    //出现异常的状况
                    System.out.println("接收消息失败!");
                }
            });

            System.out.println("开始接收消息");
            //这里我不让程序往下执行,进行阻断
            System.in.read();

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

### 4.1 组成(面)

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy62a1f9e3-027d-408a-8fb4-a176bd184d23.png)

核心概念:

- **Server**:又称Broke,它是接受客户端的连接,实现AMQP实体服务,也就是我们安装的rabbitmq-server
- **Connection**:连接,应用程序与Broker的网络连接TCP/IP 三次握手和四次挥手,因为要不停的连接造成性能损耗,所以在基础之上开发了一个长连接来通过下面的Chanel信道来维持!
- **Channel**:网络信道,几乎所有的操作我们都是在Channel中进行,Channel是进行消息读写的通道,客户端可以建立多个Channel,但每个Channel代表一个会话任务!
- **Message**:消息,服务与应用程序之间传送的数据,由Properties和body组成;Properties可以对消息进行修饰,比如消息的优先级、延迟等高级特性,Body则是消息体的内容;
- **Virtual Host**:虚拟地址,用于进行逻辑隔离,是最上层的消息路由,一个虚拟机主机路由可以有若干个Exchange和Queue,但是同一个虚拟主机里面不能有相同名字的Exchange,这个虚拟地址就举个例子:MQ服务就好比是整个数据库服务,而Virture Host就好比是数据库中的单独的一个数据库,而之所以不能出现相同的交换机也可理解不能在单个数据库中出现相同的表名!!!
- **Exchange**:交换机,主要用于接收消息,根据路由将发送消息到绑定的队列(`不具备消息存储的能力`)。要记住是交换机来接收投递给队列的,而不是队列来接收消息的!
- **Bindings**:Exchange和Queue之间的虚拟连接,binding中可以保护多个routing key,就会把这个路由key和我们的交换机来绑定这个队列的一个对象!一旦绑定就形成一个闭环,接下来在发消息的过程中只要带上这个路由key,消费者在监听的时候就会通过这个路由key进行匹配!
- **Routing key**:是一个路由规则,虚拟机可以用它来确定如何该路由是一个特定消息,这个路由key就是用于在分发消息的时候是否指定给特定的消费者进行分发!如果不带这个key那么就是全部人都会收到!
- **Queue**:队列,也是消息队列,目的保存消息并将它们转发给消费者!

**理解**:从图中可知一个Producer(生产者)生产我们的消息,会开辟一个连接然后连接里面就会有我们的通道,而通道里面就会有一个概念是交换机,这个交换机我们都看见过就是在生产者在信道中声明队列的第一个参数就是交换机,如:`channel.basicPublish("", queueName, null, message.getBytes(StandardCharsets.UTF_8));`,不过我们没写,那么注意没写不代表没有交换机,没指定交换机但是一定会有一个默认的交换机,默认交换机的模式是Direct路由模式,这也是我们面试题会问到的:可以存在没有交换机的队列吗?!!!

​	所以说生产者会开启连接开启通道,然后就会把消息通过交换机来传递给队列的!注意`消息一定是通过交换机来传递给队列的!`,虽然我们在声明队列的时候是直接把消息放在了队列中但其实不是这样的,因为MQ的底层语言是Erlang语言写的,而Erlang本身就是专门开发交换机的一个语言;回过头来:交换机就是负责消息的分发和投递,然后呢就会把消息投递到这个队列里面,而在这个投递的过程中就会有一些分发策略,也就是说可以通过路由key来进行过滤筛选给指定的消费者,没有的话直接分发给每一个消费者;

​	这里我们还可以看见broker,代表是服务的意思也就是一个节点,它就是rabbitMQ的服务,后面也就是有很多的broker的集群,在这个集群里为了去隔离和区分,这里也搞了一个虚拟机节点,这个虚拟节点可以理解为我们电脑中的磁盘,比如C盘D盘,目的就是为了隔离!这样子去理解:我们在系统中有订单消息和用户消息等等各种各样的信息全部堆积在这个一个根节点就会庞大难以区分,所以这个虚拟节点就将其分隔开,在web界面在admin那里就可以看见一个Virture Host虚拟节点,默认是"/"根节点!;然后消费者就会来订阅我们的消息,一旦被订阅,那么交换机就会把消息推送给我们的Cousumer(消费者);

**总结**:其实说这么多,总体是这样的:一个生产者生产消息然后创建连接管道,当然可以创建多个连接管道,然后在其中一个连接管道里获取一个信道里通过交换机来传输消息到MQ服务中,而在MQ中存在多个虚拟节点当然由我们指定,这些虚拟节点的目的就是为了隔离与区分,就好比电脑盘符去指定盘符下运行,然后在虚拟节点中通过交换机获取到消息然后进行消息分发和投递,这个时候就会有一些分发策略,如:发布订阅模式,轮询分发等等,在这个过程中会通过routingkey来筛选是否确定给指定的消费者分发消息,没有就都发!然后投递到我们的消息队列中,进行保存和转发!其中bindins就是交换机和队列之间的虚拟连接也就是一个绑定关系,中间保护了路由key,也就是交换机通过路由key进行分发给消息队列;然后这个消费者也通过相同的步骤,订阅了我们这个消息,也就是消费者跟队列进行绑定,生产者生产消息跟交换机绑定,bindings把交换机和队列进行绑定,那么此时队列就会将消息转发给我们的消费者!



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

> 我们只要明白消息模式是通过交换机和队列绑定关系以后,那么消息的发送是通过交换机去进行分发的!

注:消息模式其实就是交换机的类型,指定类型就决定了如何去分发到我们的队列中!当然这消息模式与我们之前的消息的分发策略也是一样的!说白了消息模式就是分发策略的外表,其核心就是分发策略!



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

1. 点击Exchanges交换机发送消息,因为哪怕是简单模式那么都会有一个默认的交换机,所以我们往默认的交换机发送消息!并且我们这里发送消息要指定Routing key,因为是默认的交换机,这里的路由key就相当于指定了队列名,指定队列发送!

   ![image-20211022181922744](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022181922744.png)

   如果我们没指定路由key,那么就会报异常:翻译为:邮件已发布，但未路由。

   ![image-20211023100437256](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023100437256.png)

   2. 此时我们看到消息就是:处于一个就绪状态且总共有1条消息;

      ![image-20211022183743995](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022183743995.png)

   3. 然后点queue1然后查看消息:

      ![image-20211022184419003](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022184419003.png)

> 总结:无论是简单工模式还是工作队列模式它们都有一个默认的交换机,发送消息一定是交换机去发送而不是队列!,交换机接收消息推送到队列中,队列以后消费者会自动监听订阅消息,然后进行推送! 还有没有声明交换机那么一定是走默认的交换机

### 5.2 Fanout模式

这个模式也称"**发布与订阅模式**"

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/python-three.png)

#### 1. web界面

1. 创建一个fanout类型的交换机

   ![image-20211022200712629](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022200712629.png)

   2. 查看我们的交换机

      ![image-20211022200816591](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022200816591.png)

      此时我们的交换机已经创建好了!

   3. 然后我们创建两个队列:

      ![image-20211022201013069](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201013069.png)

   4. 点击queue2和queue3然后将队列与我们的自定义交换机进行绑定

      ![image-20211022201335325](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201335325.png)

   5. 绑定后就是这种:

      ![image-20211022201508468](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022201508468.png)

      6. 接下来我们就往我们的交换机上投递一条信息,我们可以通过web页面俩模拟这个过程:

         ![image-20211022202002123](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022202002123.png)

      7. 然后此时我们就能看到对应的队列中已经存在了消息:

         ![image-20211022202058346](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022202058346.png)

      > 接下来只要有人订阅了我们这个队列,那么接下里都会收到消息;通过交换机与队列进行一对多进行绑定,那么我们只需要往一个交换机进行发送消息,那么消费者只要订阅了队列就会收到消息!就好比是广播,通过一个广播进行扩散!

注意:我们可以看见这里有一个Routing key,那么思考我们可不可以给指定的队列绑定一个key,然后根据key去发送呢?其实这种是毫无意义的!也就是说无论你增加多少key,Fanout模式都会通过交换机将数据给绑定交换机的队列中发送!这里我是做过相关测试的!都会收到!加key是毫无意义的!!!

#### 2. 代码演示

类型是**fanout**模式

特点:Fanout是一种发布与订阅模式,是一种广播机制,它是没有没有路由key的模式!

这是图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy160fd9d8-2b42-4d43-b0d6-b4e47e272e8f.png)

接下来我们就围着这张图去通过代码的方式去描述使用:

> 生产者:

```java
package cn.miao.fanout;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 生产者-fanout模式广播机制,只要跟fanout模式绑定的队列都会收到这个交换机里的消息
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Producer {

    public static void main(String[] args) {

        //1: 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");

        Connection connection = null;
        Channel channel = null;
        try {
            //3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            //4: 从连接中获取信道channel
            channel = connection.createChannel();
            //5: 准备消息发送的内容
            String message = "你好啊!fanout广播模式!";
            String exchangeName = "fanout-exchange";
            //因为Fanout模式不需要路由key,因为毫无意义!
            String routingKey = "";

            /**
             * 6: 发送消息给中间件rabbitmq-server
             * @Params1 :交换机exchange
             * @Params2 :队列名称/routingkey
             * @Params3 :属性配置
             * @Params4 :发送消息的内容
             */
            channel.basicPublish(exchangeName, routingKey, null, message.getBytes(StandardCharsets.UTF_8));

            System.out.println("消息发送成功!");

        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("发送消息出现异常...");
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

整个生产者的过程就是获取连接通过连接创建一个信道,而我们在信道里发送消息给交换机!

投递到Fanout交换机,而交换机与队列进行绑定,那么此时我们往交换机发送消息那么此时绑定对应的队列就会收到一条信息!

> 消费者

```java
package cn.miao.fanout;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 消费者-fanout模式
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Consumer {
    private static final Runnable RUNNABLE = () -> {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        //获取队列的名称
        final String queueName = Thread.currentThread().getName();

        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 5: 申明队列queue存储消息
            /*
             *  如果队列不存在，则会创建
             *  Rabbitmq不允许创建两个相同的队列名称，否则会报错。
             *
             *  @params1： queue 队列的名称
             *  @params2： durable 队列是否持久化
             *  @params3： exclusive 是否排他，即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
             *  @params4： autoDelete 是否自动删除，当最后一个消费者断开连接之后是否自动删除消息。
             *  @params5： arguments 可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等。
             * */
            // 这里如果queue已经被创建过一次了，可以不需要定义
            //channel.queueDeclare("queue1", false, false, false, null);
            // 6： 定义接受消息的回调
            Channel finalChannel = channel;
            //因为我已经在web界面将交换机和队列已经绑定了,所以就可以无需绑定队列!
            finalChannel.basicConsume(queueName, true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println(queueName + "：收到消息是：" + new String(delivery.getBody(), StandardCharsets.UTF_8));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println(queueName + "：开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    };

    public static void main(String[] args) {
        // 启动三个线程去执行充当消费者去订阅消息监听
        new Thread(RUNNABLE, "queue1").start();
        new Thread(RUNNABLE, "queue2").start();
        new Thread(RUNNABLE, "queue3").start();
    }
}
```

首先我们在web界面有三个队列然后与我们的交换机进行绑定,然后这里定义三个线程也就是消费者然后去订阅监听我们的队列,只要生产者生产消息通过fanout交换机转发到队列中,那么消费者就能收到消息!

> 这是控制台打印输出

生产者:

![image-20211023212625710](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023212625710.png)

消费者:

![image-20211023212637663](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023212637663.png)

> 其实无论是代码还是图形化界面都是为了表达:消息是通过交换机去传递给队列,队列之后就会推送给我们消费者,就是这样的一个关系!

### 5.3 Direct模式

这个就是我们模式中的`Routing模式`

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy33427b78-879d-4511-9dd7-42fb33108339.png)

这个模式跟上面的Fanout模式是差不多的,只不过这个模式比上面多了一个`路由key`的概念,这个路由key呢就相当于给它指定了一个where条件,比如:where routekey=error,而消费者就必须去绑定这个error,然后我们就会把属于error的这个消费者给查询出来;其实可以想象成是一个过滤的一个条件就可以了!

#### 1. web界面

步骤:

1. 创建Direct模式的交换机

   ![image-20211022210832934](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022210832934.png)

2. 然后我们针对一个队列进行绑定一个routing key,这样我们待会就可以不用指定队列去发送:

   ![image-20211022212101541](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212101541.png)

   最终就是这样子的:

   ![image-20211022212145984](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212145984.png)

3. 然后我们现在就往order这个路由key里面发送消息:

   ![image-20211022212416736](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022212416736.png)

   注意:在发送消息的时候必须要指定发送的路由key,不然就会失败!
   
   > 发现就只有指定的路由key绑定的队列收到了消息,而其他却没有收到!这就是Direct模式
   >
   > 结论:Direct模式其实就是在fanout模式上增加了一个路由key;这个路由key只要命名为相同的key那么就可以归类分组,然后我们就可以根据类型分组去分发消息,它会根据key找到对应的队列然后进行分发,这样就达到了一个按需选择的目的;

比如这种:

![image-20211022213550448](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211022213550448.png)

给order发送消息,那么此时对应的两个队列都会收到消息,此时就已经达到了一个按需选择的目的!!!!



#### 2. 代码演示

特点:Direct模式是fanout模式上的一种叠加，增加了路由RoutingKey的模式。

图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy33427b78-879d-4511-9dd7-42fb33108339.png)

接下来我们就去围绕这张图去用代码去进行一个代码演示:

> 生产者

```java
package cn.miao.direct;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 生产者-direct模式 指定路由key进行发送
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Producer {

    public static void main(String[] args) {

        //1: 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");

        Connection connection = null;
        Channel channel = null;
        try {
            //3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            //4: 从连接中获取信道channel
            channel = connection.createChannel();
            //5: 准备消息发送的内容
            String message = "你好啊!direct路由模式!";
            String exchangeName = "direct-exchange";
            //指定路由key进行发送,由于我们使用direct类型的交换机和队列绑定,使用routing key,所以我们只需要给路由key,那么
            //对应的队列就会收到消息
            String routingKey1 = "order";
            String routingKey2 = "course";

            /**
             * 6: 发送消息给中间件rabbitmq-server
             * @Params1 :交换机exchange
             * @Params2 :队列名称/routingkey
             * @Params3 :属性配置
             * @Params4 :发送消息的内容
             */
            channel.basicPublish(exchangeName, routingKey1, null, message.getBytes(StandardCharsets.UTF_8));
            channel.basicPublish(exchangeName, routingKey2, null, message.getBytes(StandardCharsets.UTF_8));

            System.out.println("消息发送成功!");

        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("发送消息出现异常...");
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

我们这里就是修改了路由key,然后指定了交换机就可以了!

> 消费者

```java
package cn.miao.direct;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 消费者-direct模式
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Consumer {
    private static final Runnable RUNNABLE = () -> {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        //获取队列的名称
        final String queueName = Thread.currentThread().getName();

        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 5: 申明队列queue存储消息
            /*
             *  如果队列不存在，则会创建
             *  Rabbitmq不允许创建两个相同的队列名称，否则会报错。
             *
             *  @params1： queue 队列的名称
             *  @params2： durable 队列是否持久化
             *  @params3： exclusive 是否排他，即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
             *  @params4： autoDelete 是否自动删除，当最后一个消费者断开连接之后是否自动删除消息。
             *  @params5： arguments 可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等。
             * */
            // 这里如果queue已经被创建过一次了，可以不需要定义
            //channel.queueDeclare("queue1", false, false, false, null);
            // 6： 定义接受消息的回调
            Channel finalChannel = channel;
            //因为我已经在web界面将交换机和队列已经绑定了,所以就可以无需绑定队列!
            finalChannel.basicConsume(queueName, true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println(queueName + "：收到消息是：" + new String(delivery.getBody(), StandardCharsets.UTF_8));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println(queueName + "：开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    };

    public static void main(String[] args) {
        // 启动三个线程去执行充当消费者去订阅消息监听
        new Thread(RUNNABLE, "queue1").start();
        new Thread(RUNNABLE, "queue2").start();
        new Thread(RUNNABLE, "queue3").start();
    }
}
```

> 打印输出:

这是绑定的路由key:

![image-20211024185009575](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024185009575.png)

生产者:

![image-20211024185053347](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024185053347.png)

消费者:

![image-20211024185151316](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024185151316.png)

> 可以发现:这里三个队列都收到了消息,而我们就是指定了路由key和交换机,就可以了!在图形化界面绑定了交换机和队列之间的路由key!

### 5.4 Topic模式

这个是主题模式,这个模式比Direct模式又多了一点内容,也就是可以支持模糊匹配的路由key:
![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy89e09dbc-b6ee-4db6-a6f2-ddbd7b4d95d6.png)

#### 1. web界面

我们来实际操作一下web界面使用主题模式:

步骤:

1. 创建一个Topic模式的交换机:

   ![image-20211023141526044](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023141526044.png)

2. 在交换机里面去绑定对应的队列:

   ![image-20211023141956334](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023141956334.png)

   ```bash
   # 代表的是零个或者一个或者多个也就是多集,多集就是这样:com.xxxx.xxx.xx 无论点多少都无所谓,比如:com、com.xxx.x、com.xxx 都能进行匹配!
   * 代表的是有一个,但是只能有一集 ,比如这样: xx.course.xx,但是像xx.xx.course.xx就不能进行匹配,因为前面存在多集了!!!
   ```

3. 接下里我们我们给queue1和queue2和queue3发送消息,那么此时该如何去发:

   ![image-20211023145204609](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023145204609.png)

   ​	我们随便点开一个queue3查看:

   ​	![image-20211023145848682](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023145848682.png)

> 只会给满足条件的路由key对应的队列发送消息,比如:com.course.order.user.test ,此时每个key都满足,但是除了course,因为它是\*.course.\*,后面存在多集,所以它不满足不会对他进行发送消息!

#### 2.代码演示

特点:Topic模式是direct模式上的一种叠加，增加了模糊路由RoutingKey的模式。

图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy89e09dbc-b6ee-4db6-a6f2-ddbd7b4d95d6.png)

> 生产者

```java
package cn.miao.topics;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 生产者-topic模式 增加了模糊路由key进行发送
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Producer {

    public static void main(String[] args) {

        //1: 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");

        Connection connection = null;
        Channel channel = null;
        try {
            //3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            //4: 从连接中获取信道channel
            channel = connection.createChannel();
            //5: 准备消息发送的内容
            String message = "你好啊!Topic路由模式!";
            String exchangeName = "topic-exchange";
            //这里增加了模糊路由key进行发送!
            String routingKey1 = "com.course.order";

            /**
             * 6: 发送消息给中间件rabbitmq-server
             * @Params1 :交换机exchange
             * @Params2 :队列名称/routingkey
             * @Params3 :属性配置
             * @Params4 :发送消息的内容
             */
            channel.basicPublish(exchangeName, routingKey1, null, message.getBytes(StandardCharsets.UTF_8));

            System.out.println("消息发送成功!");

        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("发送消息出现异常...");
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

> 消费者

```java
package cn.miao.topics;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 消费者-topic模式
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Consumer {
    private static final Runnable RUNNABLE = () -> {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        //获取队列的名称
        final String queueName = Thread.currentThread().getName();

        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 5: 申明队列queue存储消息
            /*
             *  如果队列不存在，则会创建
             *  Rabbitmq不允许创建两个相同的队列名称，否则会报错。
             *
             *  @params1： queue 队列的名称
             *  @params2： durable 队列是否持久化
             *  @params3： exclusive 是否排他，即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
             *  @params4： autoDelete 是否自动删除，当最后一个消费者断开连接之后是否自动删除消息。
             *  @params5： arguments 可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等。
             * */
            // 这里如果queue已经被创建过一次了，可以不需要定义
            //channel.queueDeclare("queue1", false, false, false, null);
            // 6： 定义接受消息的回调
            Channel finalChannel = channel;
            //因为我已经在web界面将交换机和队列已经绑定了,所以就可以无需绑定队列!
            finalChannel.basicConsume(queueName, true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println(queueName + "：收到消息是：" + new String(delivery.getBody(), StandardCharsets.UTF_8));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println(queueName + "：开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    };

    public static void main(String[] args) {
        // 启动三个线程去执行充当消费者去订阅消息监听
        new Thread(RUNNABLE, "queue1").start();
        new Thread(RUNNABLE, "queue2").start();
        new Thread(RUNNABLE, "queue3").start();
    }
}
```



> 打印输出:

这是图形化界面显示的模糊路由key

![image-20211024190319145](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024190319145.png)

生产者:

![image-20211024190355681](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024190355681.png)

消费者:

![image-20211024190605283](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024190605283.png)

> 可以发现,我们使用Topic模式的交换机,建立模糊路由key,那么就会自动去匹配去查找,如果符合要求那么就会给绑定对应的队列去推送消息,而消费者绑定队列,也在监听队列,所以一旦被推送那么就立马收到!当然要注意#代表0个或者多集,而*号代表必须有并且只有一集;

### 5.5 Headers模式

这个是一种参数的模式,也就是说可以根据我们的参数来进行一个过滤!

#### 1. web界面

1. 创建一个Headers模式的交换机

   ![image-20211023190331117](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023190331117.png)

2. 跟队列进行绑定,并且指定了参数:

   ![image-20211023190951809](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023190951809.png)

3. 然后我们发消息:

   ![image-20211023191736630](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023191736630.png)

4. 此时我们去queue1中可以查看到这个消息:

   ![image-20211023191831036](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211023191831036.png)

> 也就说这个模式就是根据我们的条件去达成!通过在发送消息的设置Headers,然后去交换机里找是否满足条件!

代码:

```java
生产者:
String message = "email inform to user"+i; 
Map<String,Object> headers = new Hashtable<String, Object>(); 
headers.put("inform_type", "email");//匹配email通知消费者绑定的header 
AMQP.BasicProperties.Builder properties = new AMQP.BasicProperties.Builder(); 
properties.headers(headers); 
//Email通知 
channel.basicPublish(EXCHANGE_HEADERS_INFORM, "", properties.build(), message.getBytes());
```

消费者:

```java
channel.exchangeDeclare(EXCHANGE_HEADERS_INFORM, BuiltinExchangeType.HEADERS); 
Map<String, Object> headers_email = new Hashtable<String, Object>(); 
headers_email.put("inform_type", "email"); 
//交换机和队列绑定 
channel.queueBind(QUEUE_INFORM_EMAIL,EXCHANGE_HEADERS_INFORM,"",headers_email); 
//指定消费队列 
channel.basicConsume(QUEUE_INFORM_EMAIL, true, consumer); 
```



### 5.6 Work queues模式

当有多个消费者时,我们的消息会被哪个消费者消费呢?我们又该如何均衡消费者信息的多少呢?

主要有两种模式:

1. 轮询模式的分发:一个消费者一条,`按均分配`;
2. 公平分发:根据消费者的消费能力进行公平分发,处理快的处理多,处理慢的处理的少,总之`按劳分配`;

#### 1. 轮询分发(Round-Robin)

图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy6f38311d-bcff-4d63-9c60-4ff4103eb3e0.png)

特点:该模式接收消息时当有多个消费者接入时，消息的分配模式是一个消费者分配一条，直至消息消费完成;

> 生产者

```java
package cn.miao.work.lunxun;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * @author:
 * @description: Producer 简单队列生产者
 * @Date : 2021/3/2
 */
public class Producer {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 6： 准备发送消息的内容
            for (int i = 1; i <= 20; i++) {
                //消息的内容
                String msg = "MiaoDaWei:" + i;
                // 7: 发送消息给中间件rabbitmq-server
                // @params1: 交换机exchange
                // @params2: 队列名称/routingkey
                // @params3: 属性配置
                // @params4: 发送消息的内容
                channel.basicPublish("", "queue1", null, msg.getBytes());
            }
            System.out.println("消息发送成功!");
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```

注意我们这里使用的默认的交换机,所以routingkey是队列名,我们这里循环往队列queue1中推送消息,然后此时我们就看见queue1中就有20条数据,然后我们运行看看消费者是如何获取进行分配的!

![image-20211024201557382](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024201557382.png)

> 消费者1

```java
package cn.miao.work.lunxun;
import com.rabbitmq.client.*;
import java.io.IOException;
/**
 * @author:
 * @description: Consumer
 * @Date : 2021/3/2
 */
public class Work1 {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("消费者-Work1");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();

            // 5： 定义接受消息的回调
            Channel finalChannel = channel;
            //这里第二个参数:自动应答,正常消费消息的话就会剔除,还有轮询模式必须是自动应答
            finalChannel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try{
                        System.out.println("Work1-收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        //模拟一下这个消费者服务器处理消息的速度较慢
                        Thread.sleep(2000);
                    }catch(Exception ex){
                        ex.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println("Work1-开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```

> 消费者2

```java
package cn.miao.work.lunxun;
import com.rabbitmq.client.*;
import java.io.IOException;
/**
 * @author:
 * @description: Consumer
 * @Date : 2021/3/2
 */
public class Work2 {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("消费者-Work2");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();

            // 5： 定义接受消息的回调
            Channel finalChannel = channel;
            //这里第二个参数:自动应答,正常消费消息的话就会剔除,还有轮询模式必须是自动应答
            finalChannel.basicConsume("queue1", true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try{
                        System.out.println("Work2-收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        //模拟一下当前消费者服务器处理消息的速度是较快的!
                        Thread.sleep(200);
                    }catch(Exception ex){
                        ex.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println("Work2-开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```

> 此时我们就来看效果会不会因为其中消费者处理消息速度快慢而影响两个消费者获取消息条数的多少,现在一共发了20条,那么理论上应该每人10条:

这是work1:

![image-20211024203940938](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024203940938.png)

这是work2:

![image-20211024204000282](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024204000282.png)

结论:我们启动了两个消费者然后开始启动生产者投递消息,此时我们就可以看见消费者2因为处理消息速度较快,所以一下就跑完了,而消费者1处理较慢,需要一秒执行一次;但是可以看见两者收到的消息是平均的,哪怕如果出现奇数消息数量,那么最后一个就是随机的!

并且我们需要注意的点就是我们要开启一个自动应答!这样才是轮询分发!

> 总结

轮询模式就是一种均分性,怎么说呢?就好比是现在有一台服务器消费者,它性能比较好处理一条消息只需要100毫秒,而另一台服务器消费者性能比较不好处理消息需要1秒,而是由轮询模式进行分发消息的时候不会因为这其中的服务器消费者处理消息的慢而少消费,而是每台消费者`平衡均分消息数量`!



#### 2. 公平分发(Fair Dispatch)

图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy6f38311d-bcff-4d63-9c60-4ff4103eb3e0.png)

特点:由于消息接收者处理消息的能力不同,存在处理快慢的问题,我们就需要能者多劳模式,处理快的多处理,处理慢的少处理;

> 生产者

```java
package cn.miao.work.fair;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * @author:
 * @description: Producer 简单队列生产者
 * @Date : 2021/3/2
 */
public class Producer {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 6： 准备发送消息的内容
            for (int i = 1; i <= 20; i++) {
                //消息的内容
                String msg = "MiaoDaWei:" + i;
                // 7: 发送消息给中间件rabbitmq-server
                // @params1: 交换机exchange
                // @params2: 队列名称/routingkey
                // @params3: 属性配置
                // @params4: 发送消息的内容
                channel.basicPublish("", "queue1", null, msg.getBytes());
            }
            System.out.println("消息发送成功!");
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```

> 消费者1

```java
package cn.miao.work.fair;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @author:
 * @description: Consumer
 * @Date : 2021/3/2
 */
public class Work1 {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("消费者-Work1");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();

            // 同一时刻，服务器只会推送一条消息给消费者
            // 5： 定义接受消息的回调
            Channel finalChannel = channel;

            finalChannel.basicQos(1);
            //公平分发需要将第二个参数设置为false,表示这是手动应答
            finalChannel.basicConsume("queue1", false, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try{
                        System.out.println("Work1-收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        //模拟一下这个消费者服务器处理消息的速度较慢
                        Thread.sleep(2000);
                        //手动应答,第二个参数表示单条消费
                        //一定要使用我们的手动应答
                        finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
                    }catch(Exception ex){
                        ex.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println("Work1-开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```

> 消费2

```java
package cn.miao.work.fair;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @author:
 * @description: Consumer
 * @Date : 2021/3/2
 */
public class Work2 {
    public static void main(String[] args) {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("消费者-Work2");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();


            // 5： 定义接受消息的回调
            Channel finalChannel = channel;
            //指标定义出来,默认为空表示轮询分发,我们定义出来=1表示一条,表示一次性从队列中取多少条过来的过程
            // 同一时刻，服务器只会推送一条消息给消费者
            finalChannel.basicQos(1);
            //公平分发需要将第二个参数设置为false,表示这是手动应答
            finalChannel.basicConsume("queue1", false, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    try{
                        System.out.println("Work2-收到消息是：" + new String(delivery.getBody(), "UTF-8"));
                        //模拟一下当前消费者服务器处理消息的速度是较快的!
                        Thread.sleep(200);
                        //手动应答,第二个参数表示单条消费
                        //一定要使用我们的手动应答
                        finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
                    }catch(Exception ex){
                        ex.printStackTrace();
                    }
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println("Work2-开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
}
```



> 看输出:

这是work1:

![image-20211024211822590](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024211822590.png)

这是work2:

![image-20211024211838689](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024211838689.png)



> 总结:

可以发现work2的服务器资源消耗的比较少也就是它的处理速度会快,所以它就会进行一种消息的窃取,而work1处理消息的速度比较慢,所以导致被分配的消息数量就少!



#### 3.小结

所以说我们的公平分发讲究的就是一种能者多劳,而轮询分发讲究的是均分机制,这就是两者的区别;

注意点:公平分发一定要使用手动应答,而轮询分发是默认的!

轮询分发:

```java
//轮询分发就是一种公平性,也就是说我们不去配置写与不写,那么都是一种公平机制也就是轮询机制 
// 就跟正常的发送消息即可,重点是在这里:
//消费者, 这里第二个参数为true,表示是自动应答
finalChannel.basicConsume("queue1", true, new DeliverCallback() {
```

公平分发:

```java
//指标定义出来,默认为空表示轮询分发,我们定义出来=1表示一条,表示一次性从队列中取多少条过来的过程
//如果我们把qos=1那么这个时候就有一个时间差,如果这个服务器性能比较快那么接下里就会多处理消息,
//Qos设置多大根据自己的服务器内存状况和磁盘空间决定!
finalChannel.basicQos(1);
//公平分发就一定要开启手动应答
finalChannel.basicConsume("queue1", false, new DeliverCallback() {
    ...
        //这里因为我们设置了手动应答,那么这里就要我们手动的进行应答,还有这里的false表示是否是单条消费
    finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
}
```



我认为公平分发的原理是这样的:公平分发是通过设置`finalChannel.basicQos(1);`qos的多少,让当前消费者在处理完消息后就会立即往队列中一次性获取多少条,这样处理消息比较快的消费者就会不停的往队列里获取消息,而处理较慢的因为大部分的消息都被处理快的抢走了,所以让看起来分配消息却很少!而轮询分发我们不用设置QOS的数量,默认为null,所以每个消费者都是同等数量的消息!



> 这里重新回忆MQ支持消息的模式

```java
1. 简单模式 Simple
    - 简单的一个生产者一个消费者中间一个队列,走的是默认的交换机,简单地将消息投递到交换机中,然后消费者监听收到即可!
2. 工作模式 Work
    - 工作模式分为轮询分发和公平分发,默认是轮询分发因为是自动应答,而公平分发必须要手动应答且设置qos,这个模式可以准确的说是一个分发机制,可以在任意的交换机类型上就可以使用!
    - 轮询分发:同一时间多个消费者监听队列,那么每个消费者收到的消息数量都是均衡的!
    - 公平分发:消费者处理得快的就多获取,处理得慢的就少获取,多劳多得!
3. 发布订阅模式 fanout
    - 这是一种广播机制,它是没有路由key的模式,也就是说只要将消息投递给交换机,而交换机推送消息给队列,而只要订阅了队列那么都可以收到消息
4. 路由模式	direct
    - 这本质上也是一种广播机制,但是它是有路由key的概念,也就是说我们可以根据路由key去推送消息,而路由key可以跟多个队列进行绑定,实现一种类似于广播机制
5. 主题模式 Topic
    - 这个是在direct模式的基础之上增加了模糊路由key的匹配形式,也就是说我们对key进行模糊匹配,一旦匹配对应就会给对应的队列去转发消息
6. 参数模式 headers
    - 这是一种参数匹配的模式,没有路由key,只有设置参数进行匹配!      
```

注意:如果能用发布订阅模式就不要去用路由key模式,因为用路由key一定会有性能损耗,因为还要去过滤才能进行分发,而发布订阅模式就可以直接去分发!主题模式呢一般使用较少,看公司需求决定! 

其实主要是fanout模式、direct模式、Topic模式。这是一层一层叠加的，最开始没有路由key到模糊路由key的概念，而简单模式其实就是fanout模式的简略版，而工作模式准确的说它不是一个模式，它是一种分发策略和机制，它可以在fanout或者direct上使用都是可以的！而参数headers模式用得较少！几乎不可见

消息模式其实就是交换机的分发机制与策略,就是如何将消息分发到指定的队列中!而分发机制我们也可以解释为是一种多线程的分发机制!

### 5.7 完整的声明创建方式

![image-20211024191556418](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024191556418.png)

注意我们是先通过web界面先将交换机和队列进行绑定,然后再通过我们的程序进行发送,这样呢可以让我们的代码变得更加的简洁,不会有声明和绑定的过程;那么接下来我们通过代码的方式脱离这个web界面来绑定交换机和队列之间的关系:

**注意**:如果消费者在监听的时候,如果消费一个不存在的队列那么就会发生异常,会报错的!还有生产者在发送消息的时候如果交换机不存在,那么此时就会发生异常!因为压根找不到就会抛出异常!必须要两种方式去解决:1.web图形化界面创建交换机或队列 2.通过代码程序去创建

> 生产者

```java
package cn.miao.all;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.nio.charset.StandardCharsets;

/**
 * @program: QuickStart-RabbitMQ
 * @description: 生产者-完整的声明创建方式
 * @author: MiaoWei
 * @create: 2021-10-19 18:28
 **/
public class Producer {

    public static void main(String[] args) {

        //1: 创建连接工程
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        connectionFactory.setVirtualHost("/");

        Connection connection = null;
        Channel channel = null;
        try {
            //3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            //4: 从连接中获取信道channel
            channel = connection.createChannel();
            //5: 准备消息发送的内容
            String message = "我是完整的常见声明方式,一切都由我来创建";
            //交换机,
            String exchangeName = "direct-message-exchange";
            // 6: 创建交换机
            //因为交换机不存在,所以在这里会进行创建,这里单词Declare翻译为注册声明的意思
            //第一个true:表示是否持久化,也就是当MQ服务重启后该交换机是否被移除,为true是持久化
            channel.exchangeDeclare(exchangeName, "direct", true);

            //7.创建队列
            /*
             *  如果队列不存在，则会创建
             *  Rabbitmq不允许创建两个相同的队列名称，否则会报错。
             *
             *  @params1： queue 队列的名称
             *  @params2： durable 队列是否持久化
             *  @params3： exclusive 是否排他，即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
             *  @params4： autoDelete 是否自动删除，当最后一个消费者断开连接之后是否自动删除消息。
             *  @params5： arguments 可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等,其实设置参数可以延伸出新的模式:Headers模式
             * */
            channel.queueDeclare("queue5", true, false, false, null);
            channel.queueDeclare("queue6", true, false, false, null);
            channel.queueDeclare("queue7", true, false, false, null);

            //8: 绑定交换机和队列之间的关系
            //第一个参数是队列,第二个参数是交换机名称,第三个参数是路由key,由于我们这里是direct模式所以需要指定一下路由key
            channel.queueBind("queue5", exchangeName, "order");
            channel.queueBind("queue6", exchangeName, "order");
            channel.queueBind("queue7", exchangeName, "course");


            /**
             * 9: 发送消息给中间件rabbitmq-server
             * @Params1 :交换机exchange
             * @Params2 :队列名称/routingkey
             * @Params3 :属性配置
             * @Params4 :发送消息的内容
             */
            channel.basicPublish(exchangeName, "order", null, message.getBytes(StandardCharsets.UTF_8));

            System.out.println("消息发送成功!");

        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("发送消息出现异常...");
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

解释: 上述代码中我定义一个不存在的交换机和队列,就好比什么东西都是我来创建,然后我们来比较一下创建之前和之后的变化:

之前的交换机:

![image-20211024193826946](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024193826946.png)

现在的交换机:

![image-20211024193926051](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024193926051.png)

然后我们点击这个交换机里面看看是否存在绑定了路由key和指定的队列:

![image-20211024194000306](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024194000306.png)

总结:也即是说我们可以通过代码的方式进行手动创建交换机和队列进行绑定这么个之间的关系!

> 消费者

```java
public class Consumer {
    private static final Runnable RUNNABLE = () -> {
        // 1: 创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        // 2: 设置连接属性
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("admin");
        connectionFactory.setPassword("admin");
        //获取队列的名称
        final String queueName = Thread.currentThread().getName();

        Connection connection = null;
        Channel channel = null;
        try {
            // 3: 从连接工厂中获取连接
            connection = connectionFactory.newConnection("生产者");
            // 4: 从连接中获取通道channel
            channel = connection.createChannel();
            // 5: 申明队列queue存储消息
            /*
             *  如果队列不存在，则会创建
             *  Rabbitmq不允许创建两个相同的队列名称，否则会报错。
             *
             *  @params1： queue 队列的名称
             *  @params2： durable 队列是否持久化
             *  @params3： exclusive 是否排他，即是否私有的，如果为true,会对当前队列加锁，其他的通道不能访问，并且连接自动关闭
             *  @params4： autoDelete 是否自动删除，当最后一个消费者断开连接之后是否自动删除消息。
             *  @params5： arguments 可以设置队列附加参数，设置队列的有效期，消息的最大长度，队列的消息生命周期等等。
             * */
            // 这里如果queue已经被创建过一次了，可以不需要定义
            //channel.queueDeclare("queue1", false, false, false, null);
            // 6： 定义接受消息的回调
            Channel finalChannel = channel;
            //因为我已经在web界面将交换机和队列已经绑定了,所以就可以无需绑定队列!
            finalChannel.basicConsume(queueName, true, new DeliverCallback() {
                @Override
                public void handle(String s, Delivery delivery) throws IOException {
                    System.out.println(queueName + "：收到消息是：" + new String(delivery.getBody(), StandardCharsets.UTF_8));
                }
            }, new CancelCallback() {
                @Override
                public void handle(String s) throws IOException {
                }
            });
            System.out.println(queueName + "：开始接受消息");
            System.in.read();
        } catch (Exception ex) {
            ex.printStackTrace();
            System.out.println("发送消息出现异常...");
        } finally {
            // 7: 释放连接关闭通道
            if (channel != null && channel.isOpen()) {
                try {
                    channel.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null && connection.isOpen()) {
                try {
                    connection.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    };

    public static void main(String[] args) {
        // 启动三个线程去执行充当消费者去订阅消息监听
        new Thread(RUNNABLE, "queue5").start();
        new Thread(RUNNABLE, "queue6").start();
        new Thread(RUNNABLE, "queue7").start();
    }
}
```

> 打印看效果:

![image-20211024194214868](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211024194214868.png)

总结:可以发现这里queue5和queue6已经收到消息了,可为什么queue7没收到呢,是因为我们在生产者发送消息的时候指定的路由key是order,也就是queue5和queue6所以说queue7没收到!

> 总结:我们一般可以先通过web界面先将队列和交换机进行绑定,这样我们在代码界面就省略那些队列和交换机的代码,起到一个简洁的作用!还有我们消费者如果在启动过程中队列没有声明没有注册那么就会报异常的!而生产者中交换机如果不存在,那么也是会报出异常的,因为好比就是投递快递到驿站,而驿站不存在你怎么投?



### 5.8 RPC

![image-20211112172903843](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112172903843.png)

RPC即客户端远程调用服务端的方法 ，使用MQ可以实现RPC的异步调用，基于Direct交换机实现，流程如下：

1. 客户端即是生产者就是消费者，向RPC请求队列发送RPC调用消息，同时监听RPC响应队列
2. 服务端监听RPC请求队列的消息，收到消息后执行服务端的方法，得到方法返回的结果
3. 服务端将RPC方法 的结果发送到RPC响应队列
4. 客户端（RPC调用方）监听RPC响应队列，接收到RPC调用结果

## 6.MQ使用场景(面)

在很多面试的场景中我们都会被问到一个点比如问你使用MQ的应用场景,redis的使用场景,如:"小明,你用MQ用在什么场景里面,你是怎么用的,为什么要用"。如果我们直接一上来就直接套概念那么一定会被认为是新手,认为你只是了解过但是并没有是实战过；那么接下来我们使用模板语句来套：

​	面试官你好，其实我是04年xxx公司加入公司的，刚开始的时候我们公司架构采用的是一种单体结构，而单体结构里面就是把所有的业务堆积在一个项目里面，随着我们公司的业务的发展和不断的推进，然后我们公司在第二年的时候,公司项目负责人接下来就把我们的项目进行分裂,也就是说变成一个分布式架构,在这基础之上进行一个拆分,拆分的过程中接下来就会考虑一个问题,比如在拆分的过程中我们做了xxx系统中的xxx模块和另外一个xxx模块,它们之间要进行一个互相沟通和协同,那么这个时候公司就采用了一个消息队列,而在之前选择的时候呢就一直在思考选择用什么消息队列,后面呢就决定了rabbitMQ,而我自己使用rabbitMQ的感受呢?感觉他就是最核心的点就是`异步`的,因为它是一个`多线程`的是一个分发的机制,而且可以让我们的网站呢性能得到一个成倍的提升,因为它的异步可以让我们`处理数据的能力`变得更加的高效和稳健!

因为它的异步所以说它的削峰能力就达到了,还有一个感受就是我们在开发里面我们就会把这些`服务进行分裂`,分裂以后就可以进行解耦,无论是后面增加都是向外扩展不会写入到核心代码中,通过MQ服务进行管理;让其扩展性变强!



```
总结:因为MQ核心是异步的是一种异步的消息队列,其核心是多线程的分发机制,这也导致处理消息所耗时时间是同时是并行的,性能得到了极大的提升!这就是异步!性能得到提升耗时变短那么我们就有更多的时间去处理其他的事情,让一些并发大的请求削掉了流量顶峰,那么这就是削峰!而我们将服务进行分裂,让MQ将核心业务充当生产者而其服务充当消费者,这样就是消息队列,而我们如果增加了服务那么也是向外扩展,因为MQ也是独立的服务,这样我们也不需要去修改核心业务的代码,只需要是以MQ发送指定的消息即可!这样就达到了解耦的目的!!!
```

接下来具体看看什么是解耦、削峰、异步:

记录:

解耦:以前的结构就是服务器A直接向服务器B直接调用,这样就会造成耦合度太高! 而现在服务器C在服务器A和服务器B之间,这样服务器A只需要向服务器C发消息,而服务器B只需要接收服务器C的消息,这样就会进行解耦的作用!让MQ充当中间件技术!而后面如果要扩展服务都不会对整个服务架构有影响!这就是解耦的好处!

### 6.1 解耦、削峰、异步

MQ是一种队列,遵循先进先出的原则；

MQ是一种应用程序之间的通信方法!，不同语言系统之间如何去沟通，那么MQ就在中间充当中间件技术，由它来进行一个转变

好处：解耦、异步、削峰

**异步**：因为MQ是异步的内部是一种多线程的分发机制所以给他发消息是会立即返回的，不需要等待也不需要多大的网络开销啥的；

**削峰**：这是一种流量削峰，也就是并发如果很大的情况下一秒钟请求超过了CPU处理顶峰，比如tomcat一秒钟只能处理200个请求，而如果有300个请求同时请求处理，那么剩下的就只能选择等待，那么这是非常耗时，而现在因为MQ是异步的，所以请求速度特别快不需要一致等待处理成功再返回，那么因为异步所以很快的处理后面的请求做更多的事，所以说流量得到了一个削峰！

**解耦**：因为MQ是中间件技术，也就是处于两个服务的中间，让两个服务不会直接性的调用，让耦合度进行解耦！这样如果要扩展服务也不会对原本的架构产生影响！



> 同步异步的问题(串行)

**串行方式**：将订单信息写入数据库成功后，发送注册邮件，再发送注册短信。以上三个任务全部完成后，返回给客户端

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy3c16e7cd-e504-497e-a9fc-2260f74e5e51.png)

这是对应的Demo代码:

```java
public void makeOrder(){
    // 1 :保存订单 
    orderService.saveOrder();  //从上往下的执行,订单在这里保存的时候,Java虚拟机就会把这里锁起来,进行阻塞
    // 2： 发送短信服务
    messageService.sendSMS("order");//1-2 s
    // 3： 发送email服务
    emailService.sendEmail("order");//1-2 s
    // 4： 发送APP服务
    appService.sendApp("order");    
}
```

解释:比如说我们在网上进行派单,下了个订单花了50毫秒,然后我们在发送短信和服务和短信各花了50毫秒,这种缺点就非常明显就是执行所有的业务总共就要花费所有业务方法的总和也就是200毫秒,如果落实到代码上就是上面这个代码,;

​	可以发现上诉代码就是我们比较常见的代码书写方式;从上至下的代码方式,而这要归功于Java虚拟机编译的过程,不能出现并行,如果要出现并行那么就必须要使用多线程来解决;JVM在调度的过程中会形成一个方法的链路,链路里面就一定会保证一个顺序,这样做为了保证数据的有效传递性,下面的这些方法需要上面方法的参数或者返回值,所以说就必须严格按照的这种阻塞一步一步的去执行,这就叫做**串行执行**;

思考?上面这种方式是非常耗时的,而其中保存订单是主要任务,而其他服务都是附属任务,如果加了事务,那么不可能因为附属任务的失败而取消订单的保存?不能因为不重要的事情去影响重要的事!不然这就是得不偿试了,所以说我们需要使用异步的方式将上诉方式进行剥离出去:

> 并行方式 异步线程池

**并行方式**:将订单信息写入数据库成功后,发送注册邮件的同时,发送注册短信,以上三个任务完成以后,返回给客户端,与串行的差别是:并行的方式可以提高处理的时间

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy29bb193d-94fe-41a5-8d53-8e460316a7ba.png)

这是对应的Demo代码:

```java
public void makeOrder(){
    // 1 :保存订单 
    orderService.saveOrder();
   // 相关发送
   relationMessage();
}
public void relationMessage(){
    // 异步
     theadpool.submit(new Callable<Object>{
         public Object call(){
             // 2： 发送短信服务  
             messageService.sendSMS("order");
         }
     })
    // 异步
     theadpool.submit(new Callable<Object>{
         public Object call(){
              // 3： 发送email服务
            emailService.sendEmail("order");
         }
     })
      // 异步
     theadpool.submit(new Callable<Object>{
         public Object call(){
             // 4： 发送短信服务
             appService.sendApp("order");
         }
     })
      // 异步
         theadpool.submit(new Callable<Object>{
         public Object call(){
             // 4： 发送短信服务
             appService.sendApp("order");
         }
     })
}
```

**解释**:我们需要使用线程池,需要自己去开发,可以看见在上面Demo代码中使用异步new一个线程去执行,这样极大的缩短了时间,但是缺点也很明显:需要自己维护线程池,持久性还有高可用都需要自己去实现,最重要的是耦合在代码中,因为共享的一个JVM空间,本来JVM内存就不多然后这里还要做一些高速缓存的消息中间件来耦合在我们JVM中来瓜分我们的内存,这样也不利于我们的高可用;

**存在的问题**:

1. 耦合度高
2. 需要自己写线程池自己维护成本太高
3. 出现了消息可能会丢失,需要你自己做消息补偿
4. 如何保证消息的可靠性需要自己来写
5. 如果服务器承载不了,你需要自己去写高可用

所以我们侧重于选择这个:

> 异步消息队列的方式:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudydfbeb825-015d-4be0-abba-d2dc7084cb84.png)

**好处**:

1. 完全解耦,用MQ建立桥接
2. 有独立的线程池和运行模式
3. 出现了消息可能会丢失,MQ有持久化功能
4. 如何保证消息的可靠性,死信队列和消息转移的等
5. 如果服务器承载不了,你需要自己去写高可用,HA镜像模型高可用。

按照以上约定，用户的响应时间相当于是订单信息写入数据库的时间，也就是50毫秒，注册邮件，发送短信写入消息队列后，直接返回，因此写入消息队列的速度很快，基本可以忽略，因此用户的响应时间可能是50毫秒，因此架构改变后，系统的吞吐量提高到每秒20 QPS，比串行提高了3倍，比并行提高了两倍！

对应的DEMO代码：

```java
public void makeOrder(){
    // 1 :保存订单 
    orderService.saveOrder();   
    rabbitTemplate.convertSend("ex","2","消息内容");
}
```

**解释:**用MQ性能比较高效,因为它把我们的下单变成了一个生产者,把我们右边的服务变成了消费者,而且这些服务也都可以剥离出我们的项目,而且我们消息队列也是一个服务也是剥离在外的,因此它具备了高可用高可靠的特征,而且并不会发生阻塞的问题,接下来我们所有的消费者就可以监听队列就可以收到消息;

**演变**:从最开始的从串行(从上而下)变成了一个异步执行,异步执行因为我们自己开发多线程就会变得很慢,所以最后到了异步消息队列的模式,让我们的性能得到了成倍的提升,而且服务之间可以分离,用MQ可以建立桥梁;

所以用MQ总耗时=下订单+服务的最长耗时;可能就是50+50这条消息就出去了,最多也就100多一点,但绝不超过200,就把我们的程序执行完了,我们的程序就可以得到极大的提升!那么我就有更多的时间去处理其他事情,那么这就达到一个**削峰**的一个目的;而解耦就体现在下面:

### 6.2 高内聚、低耦合

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudya1f53997-b01d-443f-98cd-86a38223fe19.png)

如果说我临时增加了一个微信服务,那么我们就要进行往外扩展,而且我们可以不用把代码书写在下单服务里面去,而MQ就可理解为将其分隔开,这就达到了一个**解耦**的目的,

还有比如:流量的削峰:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyf2d1ac21-e710-45dc-a166-6b531c95945f.png)

1. 分布式事务的可靠消费和可靠生产

2. 索引、缓存、静态化处理的数据同步



应用场景:

1. 流量监控

2. 日志监控（ELK）

3. 下单、订单分发、抢票





## 7.SpringBoot整合

因为SpringBoot跟MQ都是属于同一个母公司,也就是说天然对MQ的支持是比较完善的!

导入整合MQ的依赖:

```xml-dtd
<!--把MQ的操作繁琐的机制进行封装-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

然后我们可以看见:

![image-20211026090503872](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026090503872.png)

可以看见我们的starter启动器里面就包含了spring对MQ的中间件,这里还第二行就是包括了AMQP本身的客户端的包,发现其本身整合也是用的我们之前Spring的依赖:

这里我进行将Spring依赖也进行记录一下,以后说不定就可以用上:

```xml-dtd
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-amqp</artifactId>
    <version>2.2.5.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-rabbit</artifactId>
    <version>2.2.5.RELEASE</version>
</dependency>
```

> 在yml中进行配置连接MQ

```yaml
# 服务端口
server:
  port: 8080

# 配置rabbitMQ服务
spring:
  rabbitmq:
    username: admin
    password: admin
    virtual-host: /
    host: 127.0.0.1
    port: 5672
```

指定一下配置MQ要清晰明了,因为其本身自动配置的时候就已经有默认值了,不信我们来点看对应的`RabbitProperties`看看:

![image-20211026091800906](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026091800906.png)

> 我们再来看下我们的MQ核心:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyb6899c5b-99c7-401b-9b6c-58bf72fe8734.png)

### 7.1 fanout模式

我们使用fanout模式来完成一个下订单的模拟,我们会绑定交换机和队列的关系,然后建立一个消费者也就是短信服务、邮件服务来监听队列,来完成用户下单来广播我们消息的能力!我们接下来就来体验一下RabbitMQ异步编程的机制以及高内聚、低耦合的特性:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy61c5b87b-d787-4d62-9e7c-85e8f26b02f8.png)

> 首先定义订单的生产者:

首先是模拟下单的过程：

```java
package cn.miao.service;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.UUID;

/**
 * @program: SpringBoot-MQ
 * @description:
 * @author: MiaoWei
 * @create: 2021-10-26 09:25
 **/
@Component
public class OrderService {

    @Autowired
    //这是SpringBoot整合好的模板,我们可以直接拿来使用!
    private RabbitTemplate rabbitTemplate;

    /**
     * 模拟一个用户在我们平台上购买一个商品
     *
     * @param userId    用户id
     * @param productId 产品id
     * @param num       购买的数量
     */
    public void makeOrder(String userId, Long productId, int num) {
        //1. 根据商品ID查询库存是否充足
        //....

        //2. 生成并保存订单
        String orderId = UUID.randomUUID().toString();
        System.out.println("订单生成成功: " + orderId);

        //3. 通过MQ消息队列来完成消息的分发,用它的异步处理机制
        //第一个参数: 交换机; 第二个参数:路由key/queue; 第三个参数: 消息内容
        String exchangeName = "fanout_order_exchange";
        String routingKey = "";
        rabbitTemplate.convertAndSend(exchangeName, routingKey, orderId);
    }
}
```

然后我们配置RabbitMQ的队列和交换机之间的绑定关系:

```java
package cn.miao.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @program: SpringBoot-MQ
 * @description:
 * @author: MiaoWei
 * @create: 2021-10-26 09:45
 **/
@Configuration
public class RabbitmqConfig {
//这里声明和绑定如果已经存在了,那么也不会进行报错!


    //1.声明注册fanout模式的交换机
    @Bean
    public FanoutExchange fanoutExchange() {
        //第一个参数:交换机名字 第二个参数:是否持久化 第三个参数:是否自动删除
        return new FanoutExchange("fanout_order_exchange", true, false);
    }

    //2. 声明队列
    @Bean
    public Queue smsQueue() {
        // durable:是否持久化,默认是false,持久化队列：会被存储在磁盘上，当消息代理重启时仍然存在，暂存队列：当前连接有效
        // exclusive:默认也是false，只能被当前创建的连接使用，而且当连接关闭后队列即被删除。此参考优先级高于durable
        // autoDelete:是否自动删除，当没有生产者或者消费者使用此队列，该队列会自动删除。
        // return new Queue("TestDirectQueue",true,true,false);
        // 一般设置一下队列的持久化就好,其余两个就是默认false


        return new Queue("sms.fanout.queue", true);
    }

    @Bean
    public Queue emailQueue() {
        return new Queue("email.fanout.queue", true);
    }

    @Bean
    public Queue duanxinQueue() {
        return new Queue("duanxin.fanout.queue", true);
    }

    //3. 完成绑定关系(队列与交换机之间的绑定关系)
    @Bean
    public Binding smsBinding() {
        //这里表示将sms队列绑定到我们的交换机上
        return BindingBuilder.bind(smsQueue()).to(fanoutExchange());
    }

    @Bean
    public Binding emailBinding() {
        //因为我们使用的fanout模式的交换机,所以没有路由key,如果要使用路由key,那么在后面 .with(routingKey) 即可
        return BindingBuilder.bind(emailQueue()).to(fanoutExchange());
    }

    @Bean
    public Binding duanxinBinding() {

        return BindingBuilder.bind(duanxinQueue()).to(fanoutExchange());
    }
}
```

此时我们在重新调用一下看看:

```java
package cn.miao;

import cn.miao.service.OrderService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class SpringBootMqApplicationTests {

    @Autowired
    private OrderService orderService;

    @Test
    void contextLoads() {
        orderService.makeOrder("1", 1L, 12);
    }
}
```

运行测试之后我们来看看图形化界面是不是成功了:

![image-20211026102024752](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026102024752.png)

​	此时:我们已经完成了用户模拟下单成功后向MQ服务发起消息的过程,接下来我们来通过服务来监听来收到消息在做相关处理的问题:

> 定义消费者

1. 短信服务

   ```java
   package cn.miao.service.fanout;
   
   import org.springframework.amqp.rabbit.annotation.RabbitHandler;
   import org.springframework.amqp.rabbit.annotation.RabbitListener;
   import org.springframework.stereotype.Component;
   
   /**
    * @program: fanout-order-consumber
    * @description:
    * @author: MiaoWei
    * @create: 2021-10-26 10:34
    **/
   @RabbitListener(queues = {"duanxin.fanout.queue"})
   @Component //得把它注入Spring容器中去,不然监听不了
   public class DuanxinConsumer {
       /**
        * 定义一个方法用于接收消息
        * 首先我们通过@RabbitListener监听绑定队列,然后绑定以后通过@RabbitHandler注入到这个注解的方法上的参数中,用于获取消息服务
        *
        * @param message 消息
        */
       @RabbitHandler //表示这是消息的一个落脚点,@RabbitHandler 代表此方法是一个消息接收的方法
       public void reviceMessage(String message) {
            // 此处省略发短信的逻辑
           System.out.println("duanxin fanout--接收到了订单信息是:-->" + message);
       }
   }
   ```

2. 邮件服务

   ```java
   package cn.miao.service.fanout;
   
   import org.springframework.amqp.rabbit.annotation.RabbitHandler;
   import org.springframework.amqp.rabbit.annotation.RabbitListener;
   import org.springframework.stereotype.Component;
   import org.springframework.stereotype.Service;
   
   /**
    * @program: fanout-order-consumber
    * @description:
    * @author: MiaoWei
    * @create: 2021-10-26 10:34
    **/
   @RabbitListener(queues = {"email.fanout.queue"})
   @Component
   public class EmailConsumer {
       @RabbitHandler
       public void reviceMessage(String message) {
           System.out.println("email fanout--接收到了订单信息是:-->" + message);
       }
   }
   ```

3. sms服务

   ```java
   package cn.miao.service.fanout;
   
   import org.springframework.amqp.rabbit.annotation.RabbitHandler;
   import org.springframework.amqp.rabbit.annotation.RabbitListener;
   import org.springframework.stereotype.Component;
   import org.springframework.stereotype.Service;
   
   /**
    * @program: fanout-order-consumber
    * @description:
    * @author: MiaoWei
    * @create: 2021-10-26 10:34
    **/
   @RabbitListener(queues = {"email.fanout.queue"})
   @Component
   public class EmailConsumer {
       @RabbitHandler
       public void reviceMessage(String message) {
           System.out.println("email fanout--接收到了订单信息是:-->" + message);
       }
   }
   ```

启动测试,我们看看控制台:

![image-20211026105552803](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026105552803.png)

结论:在这个的过程中,使用SpringBoot方式去使用MQ,使我们的代码变得更加的简洁与清爽,而这个跟我们之前使用原生的发送和接收消息本质是一样的;

### 7.2 direct模式(面)

direct模式相较比fanout只是多了路由key,所以在上面的基础上进行修改;

**注意**:在实际开发过程,如果遇到一些场景能使用fanout模式就是用fanout,因为direct要使用路由key这就相当于是一个过滤的条件,势必会增加性能的损耗!

**注意**:消费者在启动过程中如果监听不存在的队列,或者生产者投递不存在的交换机就会发生异常,就会告诉你不存在!那么如何解决呢?我们就可以将配置交换机和队列之间的关系的配置类进行复制,也就是说无论说生产者还是消费者无论谁先启动,那么都不会发生异常!

**面试题**:在开发过程中,我们的绑定关系交换机的声明和队列的声明是绑定在生产者这边好点还是消费者那边好一点?

​	其实这个问题的本身就有一定的歧义,其实无论呢定义在哪一边其实都是可以的当然两边都是OK的,甚至可以在web页面上进行手动的绑定关系,如果说我们用页面去配置那么在程序里配置就意义不大,所以最好我们还是定义在程序里因为后续可能通过一些方式扩展它的里面的一些信息,所以基于代码的方式是比较稳妥的!一般推荐两边都配置,但是最好的方式还是定义在消费者这边去定义最好的,因为在我们启动过程中我们消费者如果你的队列还没有去声明那么接下里肯定就会报错,既然要声明队列和交换机那么最好还是配置在消费者这边会更好一点,因为消费者是直接跟我们队列打交道的地方!还有消费者是最先启动的服务,如果队列不存在那么都会发生启动异常,

定义生产者:

```java
package cn.miao.service;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.UUID;

/**
 * @program: SpringBoot-MQ
 * @description:
 * @author: MiaoWei
 * @create: 2021-10-26 09:25
 **/
@Component
public class OrderService {

    @Autowired
    //这是SpringBoot整合好的模板,我们可以直接拿来使用!
    private RabbitTemplate rabbitTemplate;

    /**
     * 模拟一个用户在我们平台上购买一个商品
     *
     * @param userId    用户id
     * @param productId 产品id
     * @param num       购买的数量
     */
    public void makeOrder(String userId, Long productId, int num) {
        //1. 根据商品ID查询库存是否充足
        //....

        //2. 生成并保存订单
        String orderId = UUID.randomUUID().toString();
        System.out.println("订单生成成功: " + orderId);

        //3. 通过MQ消息队列来完成消息的分发,用它的异步处理机制
        //第一个参数: 交换机; 第二个参数:路由key/queue; 第三个参数: 消息内容
        String exchangeName = "direct_order_exchange";
        String routingKey = "order";
        rabbitTemplate.convertAndSend(exchangeName, routingKey, orderId);
    }
}
```

定义配置队列和交换机:

```java
package cn.miao.config;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @program: SpringBoot-MQ
 * @description:
 * @author: MiaoWei
 * @create: 2021-10-26 09:45
 **/
@Configuration
public class RabbitmqConfig {
//这里声明和绑定如果已经存在了,那么也不会进行报错!


    //1.声明注册direct模式的交换机
    @Bean
    public DirectExchange directExchange() {
        //第一个参数:交换机名字 第二个参数:是否持久化 第三个参数:是否自动删除
        return new DirectExchange("direct_order_exchange", true, false);
    }

    //2. 声明队列
    @Bean
    public Queue smsQueue() {
        // durable:是否持久化,默认是false,持久化队列：会被存储在磁盘上，当消息代理重启时仍然存在，暂存队列：当前连接有效
        // exclusive:默认也是false，只能被当前创建的连接使用，而且当连接关闭后队列即被删除。此参考优先级高于durable
        // autoDelete:是否自动删除，当没有生产者或者消费者使用此队列，该队列会自动删除。
        // return new Queue("TestDirectQueue",true,true,false);
        // 一般设置一下队列的持久化就好,其余两个就是默认false

        return new Queue("sms.direct.queue", true);
    }

    @Bean
    public Queue emailQueue() {
        return new Queue("email.direct.queue", true);
    }

    @Bean
    public Queue duanxinQueue() {
        return new Queue("duanxin.direct.queue", true);
    }

    //3. 完成绑定关系(队列与交换机之间的绑定关系)
    @Bean
    public Binding smsBinding() {
        //这里表示将sms队列绑定到我们的交换机上 这里with参数是Routing Key
        return BindingBuilder.bind(smsQueue()).to(fanoutExchange()).with("order");
    }

    @Bean
    public Binding emailBinding() {
        return BindingBuilder.bind(emailQueue()).to(fanoutExchange()).with("order");
    }

    @Bean
    public Binding duanxinBinding() {
        return BindingBuilder.bind(duanxinQueue()).to(fanoutExchange()).with("course");
    }
}
```

> 定义消费者

消费者呢跟之前也是一样的:

```java
//EmailConsumer:
@RabbitListener(queues = {"email.direct.queue"})
@Component
public class EmailConsumer {
    @RabbitHandler
    public void reviceMessage(String message) {
        System.out.println("email direct--接收到了订单信息是:-->" + message);
    }
}
//SMSConsumer:
@RabbitListener(queues = {"sms.direct.queue"})
@Component
public class SMSConsumer {
    @RabbitHandler
    public void reviceMessage(String message) {
        System.out.println("sms direct--接收到了订单信息是:-->" + message);
    }
}
//DuanxinConsumer
@RabbitListener(queues = {"direct.fanout.queue"})
@Component //得把它注入Spring容器中去,不然监听不了
public class DuanxinConsumer {
    /**
     * 定义一个方法用于接收消息
     * 首先我们通过@RabbitListener监听绑定队列,然后绑定以后通过@RabbitHandler注入到这个注解的方法上的参数中,用于获取消息服务
     *
     * @param message 消息
     */
    @RabbitHandler //表示这是消息的一个落脚点
    public void reviceMessage(String message) {
        System.out.println("duanxin direct--接收到了订单信息是:-->" + message);
    }
}
```

注:在消费者这边也可定义配置队列也是可以的!这样避免先启动消费者找不到队列而报错!

效果呢跟fanout模式一模一样!这里就不展示了,相较于上面多了一个`with(routingKey)和修改了声明交换机的类型DirectExchange`,不过这也是声明交换机和队列的配置里面的!



### 7.3 topic 模式

之前我们都是通过配置类的方式来完成交换机和队列之间的绑定的关系,我们的SpringBoot也提供了一种注解的方式来完成队列和交换机之间的绑定的关系,接下来一起来看看:

> 定义生产者

1. 核心业务投递消息

   ```java
   @Component
   public class OrderService {
       @Autowired
       private RabbitTemplate rabbitTemplate;
       // 1: 定义交换机
       private String exchangeName = "topic_order_exchange";
       // 2: 路由key
       private String routeKey = "";
       public void makeOrder(Long userId, Long productId, int num) {
           // 1： 模拟用户下单
           String orderNumer = UUID.randomUUID().toString();
           // 2: 根据商品id productId 去查询商品的库存
           // int numstore = productSerivce.getProductNum(productId);
           // 3:判断库存是否充足
           // if(num >  numstore ){ return  "商品库存不足..."; }
           // 4: 下单逻辑
           // orderService.saveOrder(order);
           // 5: 下单成功要扣减库存
           // 6: 下单完成以后
           System.out.println("用户 " + userId + ",订单编号是：" + orderNumer);
           // 发送订单信息给RabbitMQ topic
           rabbitTemplate.convertAndSend(exchangeName, routeKey, orderNumer);
       }
   }
   ```

注意:此时我们给topic交换机投递消息,但是此时我们没有topic交换机,需要我们进行配置声明和绑定队列,那么按照之前是使用的是配置类的方式那么接下来使用的是注解的形式,不过将其定义在消费者监听的时候

> 定义消费者

```java
import org.springframework.amqp.core.ExchangeTypes;
import org.springframework.amqp.rabbit.annotation.*;
import org.springframework.stereotype.Component;
// bindings其实就是用来完成队列和交换机绑定关系
@RabbitListener(bindings =@QueueBinding(
        // email.fanout.queue 是队列名字，
        value = @Queue(value = "email.fanout.queue",autoDelete = "false",durable="true"),
        // order.fanout 交换机的名字 必须和生产者保持一致
        exchange = @Exchange(value = "fanout_order_exchange",
                // 这里是确定的rabbitmq模式是：fanout 是以广播模式 、 发布订阅模式
                type = ExchangeTypes.Topic),
    	//设置路由key!
    	key="#.email.#"
))
@Component
public class EmailService {
    // @RabbitHandler 代表此方法是一个消息接收的方法。该不要有返回值
    @RabbitHandler
    public void messagerevice(String message){
        // 此处省略发邮件的逻辑
        System.out.println("email-------------->" + message);
```

....

这里我省略了其他服务,不过既然看到这个应该都是举一反三的那种!

> 不过既然这样那就得先运行消费者,因为交换机不存在生产者启动就会报错!
>
> 无论是用注解还是配置类我们都可以去完成两者之间的绑定关系,两者选择其一即可!推荐使用配置类的方式,后面死信队列等等都是通过配置类的方式来完成,如果后面使用死信队列来跟注解搭配的话就会显得十分鸡肋;

回想我们的sprintBoot的流程:

1. 导入依赖

2. 配置MQ服务,如地址

3. 生产者将消息发送

   ```java
   rabbitTemplate.convertAndSend(exchangeName, routeKey, message);
   ```

4. 生产者可配置声明队列和交换机之间的关系-配置类使用

   ```java
   @Configuration
   public class RabbitmqConfig {
       //1. 声明交换机
       //2. 声明队列
       //3. 交换机和队列进行绑定
   }
   ```

5. 消费者监听队列获取消息

   ```java
   @RabbitListener(queues = {"队列名"}) //监听哪个队列
   @Component
   public class EmailService {
       @RabbitHandler //接收到消息用所在的方法中的参数来接收
       public void messagerevice(String message){
           //接收队列中的消息
       }
   }
   ```

6. 消费者可声明队列与交换机之间的关系

   - 配置类的方式
   - 注解的方式

## 8.上课笔记

### 8.1 消费者监听消息

消费者监听消息的时候可以这样去写:

```java
//测试
//channel:这是信道
//因为回调必须是Cusmer类型,而它又是接口,所以我们new它的子类,由于直接nw的话我们是拿不到消息,所以我们需要对new的子类里的方法进行重写!----ok!
com.rabbitmq.client.Consumer consumer = new DefaultConsumer(channel){
/**
             * 消费者接收消息调用此方法
             * @param consumerTag 消费者的标签，在channel.basicConsume()去指定,
             * @param envelope 消息包的内容，可从中获取消息id，消息routingkey，交换机，消息和重传标志(收到消息失败后是否需要重新发送)
             * @param properties 消息的其他属性
             * @param body	消息主体
*/
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
        System.out.println("consumerTag = " + consumerTag);
        System.out.println("获取交换机 = " + envelope.getExchange());
        System.out.println("获取路由key = " + envelope.getRoutingKey());
        System.out.println("获取消息的id = " + envelope.getDeliveryTag());
        System.out.println("获取其他属性 = " + properties.getHeaders());
        System.out.println("获取消息主体 = " + new String(body));
    }
};
 /**
         * 监听队列String queue, boolean autoAck,Consumer callback
         * 参数明细
         * 1、队列名称
         * 2、是否自动回复，设置为true为表示消息接收到自动向mq回复接收到了，mq接收到回复会删除消息，设置为false则需要手动回复
         * 3、消费消息的方法，消费者接收到消息后调用此方法
*/
channel.basicConsume("queue1", true,consumer); //监听消息队列,然后进行消息回调
```

打印内容:

```c
开始接收消息
consumerTag = amq.ctag-E97ERawq8eqPyn4SenSwgg
获取交换机 = 
获取路由key = queue1
获取消息的id = 1
获取其他属性 = null
获取消息主体 = Hello MQ,I is Simple
```

**理解**:这里消息接收监听是这样子的,`channel.basicConsume`中第三个参数是一个回调类型,也就是接收的消息就会往这里面投递,而这个回调类型是`consumer`,而它是一个接口我们能不能直接实现它可不可以,这是可以的,看源码也看出它的实现类实现的方法也是一个空方法,那么我们可以直接用它的实现类,所以我们直接new一个`DefaultConsumer`默认的实现类,然后我们采用一个匿名内部类重写方法就可以了,而重写的方法中通过`body`就可以拿到消息主体了!

### 8.2 消息确认机制

在上述案例中,如果消费者在监听消息的过程中,如果出现异常或者报错,那么就会导致一个问题:消息没接收到,而在图形化界面中队列中消息已经被自动确认移除了!

那这样就会导致消息出现丢失的情况!!!

因此，RabbitMQ有一个ACK机制。当消费者获取消息后，会向RabbitMQ发送回执ACK，告知消息已经被接收。不过这种回执ACK分两种情况：

- 自动ACK：`消息一旦被接收，消费者自动发送ACK`
- 手动ACK：`消息接收后，不会发送ACK，需要手动调用`

当然这也需要看消息的重要性,并不是什么消息我们都要进行手动ACK:

- 如果消息不太重要，丢失也没有影响，那么自动ACK会比较方便
- 如果消息非常重要，不容丢失。那么最好在消费完成后手动ACK，否则接收消息后就自动ACK;RabbitMQ就会把消息从队列中删除。如果此时消费者宕机，那么消息就丢失了。

这是报错代码:

```java
com.rabbitmq.client.Consumer consumer = new DefaultConsumer(finalChannel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
        System.out.println("consumerTag = " + consumerTag);
        int i = 1 / 0;  //这里发生报错并不会往下执行,消息发生丢失
        System.out.println("获取交换机 = " + envelope.getExchange());
        System.out.println("获取路由key = " + envelope.getRoutingKey());
        System.out.println("获取消息的id = " + envelope.getDeliveryTag());
        System.out.println("获取其他属性 = " + properties.getHeaders());
        System.out.println("获取消息主体 = " + new String(body));
    }
};
channel.basicConsume("queue1",true, consumer); //这里第二个参数是自动应答,只要接收就移除队列中的消息,消息依然被消费
```

这是正确的代码:

```java
//既然是手动应答容易出现消息的丢失,那么我门就进行一个手动应答!
com.rabbitmq.client.Consumer consumer = new DefaultConsumer(finalChannel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
        System.out.println("consumerTag = " + consumerTag);
        int i = 1 / 0;  //这里发生报错,消息发生丢失
        System.out.println("获取交换机 = " + envelope.getExchange());
        System.out.println("获取路由key = " + envelope.getRoutingKey());
        System.out.println("获取消息的id = " + envelope.getDeliveryTag());
        System.out.println("获取其他属性 = " + properties.getHeaders());
        System.out.println("获取消息主体 = " + new String(body));
        //因为我们设置了手动应答,在没有应答之前消息依然在队列中
        //手动应答,第一个参数是消息的id, 第二个参数表示是否批量应答,我们设置false即可,
        finalChannel.basicAck(envelope.getDeliveryTag(), false);
    }
};
channel.basicConsume("queue1",false, consumer); 
```

这是图示:

![image-20211112123637068](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112123637068.png)

这是因为虽然我们设置了手动ACK，但是代码中并没有进行消息确认！所以消息并未被真正消费掉。

当我们关掉这个消费者，消息的状态再次称为Ready

### 8.3 使用场景

开发中消息队列通常有如下应用场景:

- 提高系统响应速度
  - 任务异步处理。 将不需要同步处理的并且耗时长的操作由消息队列通知消息接收方进行异步处理,提高了应用程序的响应时间。
- 提高系统稳定性
  - 系统挂了关系,操作内容放到消息队列--持久化
- 服务调用异步化
  - 服务没有直接的调用关系，而是通过队列进行服务通信--中间件
- 服务解耦
  - 应用程序解耦合 MQ相当于一个中介，生产方通过MQ与消费方交互，它将应用程序进行解耦合
- 排序保证FIFO
  - 遵循队列先进先出的特点
- 消除峰值
  - 异步化提速(发消息),提高系统稳定性(多系统调用),服务解耦(5-10个服务),排序保证,消除峰值

![image-20211112170757263](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112170757263.png)

### 8.4 轮询分发和公平分发

![image-20211112171952548](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112171952548.png)

公平模式与入门程序相比，多了一个消费端，两个消费端共同消费同一个队列中的消息。

**应用场景**：对于任务过重或任务较多情况使用工作队列可以提高任务处理的速度。 

**测试**:

1. 使用入门程序，启动多个消费者
2. 生产者发送多个消息

**结果**:

1. 一条消息只会被一个消费者接收
2. rabbit采用轮询的方式将消息是平均发送给消费者的
3. 消费者在处理完某条消息后，才会收到下一条消息

`轮询分发`:会为每个消费者公平分发消息,这种就会导致一个问题: 处理数据快的消费者提前消费完了,而处理数据慢的消费者的大量消息堵在门口,等待被消费!造成大量的消息堵塞!

```
	工作队列，又称任务队列。主要思想就是避免执行资源密集型任务时，必须等待它执行完成。相反我们稍后完成任务，我们将任务封装为消息并将其发送到队列。 在后台运行的工作进程将获取任务并最终执行作业。当你运行许多工人时，任务将在他们之间共享，但是一个消息只能被一个消费者获取
	这个概念在Web应用程序中特别有用，因为在短的HTTP请求窗口中无法处理复杂的任务
```

--------

`公平分发`:

关键词:`能者多劳`

公平分发:也就是谁处理得快就处理谁!

```
	我们可以使用basicQos方法和prefetchCount = 1设置，简单理解就是一个消费者同时只能处理一个消息。 这告诉RabbitMQ一次不要向工作人员发送多于一条消息。 或者换句话说，不要向工作人员发送新消息，直到它处理并确认了前一个消息。相反，它会将其分派给不是仍然忙碌的下一个工作人员。
```

注意:使用公平分发除了`prefetchCount = 1`拉取消息数量,还有设置一个手动应答!`channel.basicAck(envelope.getDeliveryTag(), false);`并且要开启手动应答模式`channel.basicConsume("queue1", true, consumer);`

### 8.5 持久化-解决数据安全

如何避免消息丢失？

1. 消费者的ACK机制。可以防止消费者丢失消息
2. 但是，如果在消费者消费之前，MQ就宕机了，消息就没了
3. 因为网络问题，消息发送失败
4. 代码BUG，交换机或队列的routingkey找不到

是可以将消息进行持久化呢？要将消息持久化，前提是：**队列**、**Exchange**都持久化

1. 交换机持久化

   ![image-20211112173259096](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112173259096.png)

2. 队列持久化

   ![image-20211112173308791](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112173308791.png)

3. 消息持久化

   ![image-20211112173315261](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112173315261.png)

而所谓的持久化的意思就是MQ服务挂了你依然还存在服务中,等待下一次启动还能继续使用!

**什么情况下会丢失消息?**

1. 没做持久化,MQ挂了
2. 消息自动签收->消息如果消费失败那么消息也就没了

**怎么处理消息丢失?**

1. 做消息持久化
2. 进行手动签收

自动签收的情况下如果消息接收失败,那么消息也就丢失了,因为消息自动签收了,怎么解决?修改为手动签收,就是消息消费成功后进行手动签收消息

### 8.6 场景:

![腾讯课堂图片20211112093101](https://gitee.com/miawei/pic-go-img/raw/master/imgs/%E8%85%BE%E8%AE%AF%E8%AF%BE%E5%A0%82%E5%9B%BE%E7%89%8720211112093101.jpg)

**解读**:

​	我们在平时使用手机天府通进地铁出站的时候,由于经常会遇到上下班的高峰期,而高峰期可能就是1万人或者5万人同时发起请求,那么这个时候服务器的压力肯定是很大的,而我们平时使用较多的Tomcat好比最多就只能处理200个线程,那么这时进来那么1万个请求,那么只有200个请求进来了而剩下的就只能排队等候了,那么这个时候就会出现大量的连接超时等问题,然后这个还会有重试机制,那么如果说你出站失败了那么用户就会发起第二次出站请求,那么重复这样你就发现你的CPU和内存极速上升,然后最终崩了,因为并发量太高了,并且如果说处理数据在当前服务器里处理那还算快可是如果说天府通要进行结算扣费,那么这个时候就会调用第三方支付进口银行进行扣费,那么在服务器内调用第三方服务的接口这个是有网络开销的,就会拖慢我整个的请求速度!

​	而这个时候使用MQ来充当出栈接口和结算系统之间的中间件技术,用户在扫码之后只需要将扣款信息发送到MQ中,那么出站接口这边的工作就做完了然后将发送成功返回给用户说:"出栈成功,稍后结算",然后结算系统获取到MQ中的结算消息,然后调用三方服务进行订单结算!而这两者是分开的,这两边是异步的,以MQ为中心左边只负责发消息而MQ右边接收消息处理,也就是说左边发消息发完成功就直接返回了并不会去等待调用第三方支付造成的网络开销问题;

​	以前的架构是处于一个同步架构,也就是处理一个订单的结算那么是一步一步执行,也可以理解为这是一个面向过程的!而现在使用MQ充当中间件技术,这是一个异步的,可理解为这是一个面向对象的,也就是说MQ左边处理会非常快,那么这就解决了流量并发的一个削峰问题!

![image-20211112202634793](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112202634793.png)

### 8.7 整合springBoot

![image-20211201145805286](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211201145805286.png)

> 消息发送者

1. 导入依赖

   ```xml-dtd
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-amqp</artifactId>
   </dependency>
   ```

2. yaml配置mq

   ```yaml
   spring:
     application:
       name: rabbitmq
     rabbitmq:
       host: 127.0.0.1
       port: 5672
       username: guest
       password: guest
       virtualHost: /
       listener:
         simple:
           acknowledge-mode: manual #手动签收
   ```

3. 添加配置类

   ```java
   import org.springframework.amqp.core.*;
   import org.springframework.beans.factory.annotation.Qualifier;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   //rabbitMQ的配置
   //rabbitMQ的配置
   @Configuration
   public class RabbitMQConfig {
   
   
       //定义交换机
       @Bean
       public Exchange exchangeNameTopic() {
           return ExchangeBuilder.topicExchange(EXCHANGE_NAME_TOPIC).durable(true).build();
       }
   
       //定义邮件的队列Bean
       @Bean
       public Queue emailQueue() {
           return new Queue(QUEUE_NAME_EMAIL, true);
       }
   
   
       //定义短信的队列Bean
       @Bean
       public Queue smsQueue() {
           return new Queue(QUEUE_NAME_SMS, true);
       }
   
       //定义系统消息的队列Bean
       @Bean
       public Queue systemMessageQueue() {
           return new Queue(QUEUE_NAME_SYSTEM_MESSAGE, true);
       }
   
       //定义短信的队列绑定到交换机
       @Bean
       public Binding smsQueueBinding() {
   
           // #.sms.course
           return BindingBuilder.bind(smsQueue()).to(exchangeNameTopic()).with("#.sms").noargs();
   
       }
   
       //定义邮件的队列绑定到交换机
       @Bean
       public Binding emailQueueBinding() {
   
           // #.email.course
           return BindingBuilder.bind(emailQueue()).to(exchangeNameTopic()).with("#.email").noargs();
   
       }
   
       @Bean
       public Binding systemMessageQueueBinding() {
           // #.systemmessage.course
           return BindingBuilder.bind(systemMessageQueue()).to(exchangeNameTopic()).with("#.message").noargs();
   
       }
   
       //定义RabbitTemplate 指定 JSON转换器
       @Bean
       public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
           RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
           rabbitTemplate.setMessageConverter(new Jackson2JsonMessageConverter());
           //1.把要发送的消息内容 ，交换机，rontkey等都保存下来到Mysql
           //2.后续可以用来重写发送消息，甚至可以使用一个定时任务不停的重试，可以甚至一个最大重试次数
           //设置消息回调
           rabbitTemplate.setMandatory(true);
   
           return rabbitTemplate;
       }
   }
   ```

4. 发送消息

   ```java
   ...
   rabbitTemplate.convertAndSend(RabbitMQConfig.EXCHANGE_NAME_TOPIC,"message.email",courseDoc.getName());
   ...
   ```

> 消息接受者

1. 导入依赖(同上)

2. yaml配置mq(同上)

3. 添加配置

   ```java
   import org.springframework.amqp.core.*;
   import org.springframework.amqp.rabbit.config.SimpleRabbitListenerContainerFactory;
   import org.springframework.amqp.rabbit.connection.ConnectionFactory;
   import org.springframework.amqp.rabbit.core.RabbitTemplate;
   import org.springframework.amqp.rabbit.listener.RabbitListenerContainerFactory;
   import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   import java.util.HashMap;
   import java.util.Map;
   
   import static cn.itsource.hrm.constants.MQConstants.*;
   
   @Configuration
   public class MQConfig {
       //定义RabbitTemplate 指定 JSON转换器
       @Bean
       public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
           RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
           rabbitTemplate.setMessageConverter(new Jackson2JsonMessageConverter());
           //1.把要发送的消息内容 ，交换机，rontkey等都保存下来到Mysql
           //2.后续可以用来重写发送消息，甚至可以使用一个定时任务不停的重试，可以甚至一个最大重试次数
           //设置消息回调
           rabbitTemplate.setMandatory(true);
   
           return rabbitTemplate;
       }
   
       //定义 监听器的工厂 ，定义JSON的转换器
       @Bean("rabbitListenerContainerFactory")
       public RabbitListenerContainerFactory<?> rabbitListenerContainerFactory(ConnectionFactory connectionFactory){
           SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
           factory.setConnectionFactory(connectionFactory);
           factory.setMessageConverter(new Jackson2JsonMessageConverter());
           factory.setConcurrentConsumers(3);
           factory.setMaxConcurrentConsumers(10);
           factory.setAcknowledgeMode(AcknowledgeMode.MANUAL);
           factory.setPrefetchCount(1);
           return factory;
       }
   }
   ```

4. 具体的消费者接收

   ```java
   //..
   	/**
   		* 邮件处理程序
       */
       @RabbitListener(queues = QUEUE_NAME_EMAIL, containerFactory = "rabbitListenerContainerFactory")
       public void emailHandler(@Payload EmailTo emailTo, Channel channel, Message message) throws IOException {
           for (String email : emailTo.getEmails()) {
               //1.准备要发送的消息
               String format = CharSequenceUtil.format(emailTemplate, email, emailTo.getId(), emailTo.getCourseName(), DateUtil.formatDate(emailTo.getOnlineTime()));
               //2.发送短信
               log.info(format);
               //消息确认
               channel.basicAck(message.getMessageProperties().getDeliveryTag(), false);
           }
       }
   //...
   ```

   注:

   1. `@RabbitListener`:监听某个队列
   2. `containerFactory`: 获取队列中的消息按照我们定义配置参数进行序列化

   3. 注意: 打了RabbitListener的方法上的参数是由这个注解来决定的! 
   4. `@Payload`: 返回的消息自动封装转为Class 类

