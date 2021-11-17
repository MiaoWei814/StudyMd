# 分布式Dubbo+Zookeeper

## 1. 什么是分布式系统?

在《分布式系统原理与范型》一书中有如下定义:"分布式系统是若干独立计算机的**集合**,这些计算机对于用户来说就像**单个**相关系统";

![image-20211004092459722](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004092459722.png)

> 理解:这里我们用户访问百度或者淘宝,对于用户来说只有一个入口,而淘宝后台有很多服务器,对于用户来说这100个服务器还是一个整体,因为这100个服务器只要串起来就能变成一个,所以用户访问只有一个淘宝或者百度。而回到刚刚那句话：“分布式系统是若干独立计算机的集合”指这若干个服务器都是独立的，对于用户来说就像单个系统；

分布式系统是由一组通过**网络**进行通信、为了完成**共同**的任务而**协调**工作的计算机节点组成的系统,分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是`利用更多的机器,处理更多的数据`!

> 理解：比如这100个服务器都是独立的它们都要给淘宝提供服务，那么它们内部都要建立起联系（要连起来不然怎么算一个整体），而建立联系的途径有两种：Http网络、[RPC](就是可以调用另外一台电脑上的东西)远程调用；这些服务器都存在一个共同的目标就是给用户提供[服务](为什么呢?因为如果不是为了共同目标那么我们放在这里将毫无意义可言)，

分布式系统是建立在网络之上的软件系统!

> 理解:没有网络是不行的!

首先要明确的是,只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（如加内存、加磁盘、使用更多的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才考虑分布式系统。因为:分布式系统要解决的问题本身就是和单机系统一样的,而由于分布式系统多节点、通过网络通信的拓扑结构,会引起很多单机系统没有的问题,为了解决这些问题又会引入更多的机制、协议,带来更多的问题...

> 理解:分布式不要一开始就考虑进去,因为一台服务器如果已经足够用了,如果此时我们搭建分布式,我们去阿里云整集群,那么就有点大材小用了!就是说单机能干的我们就没必要去搞分布式。使用分布式还会引发问题,比如中间的服务器崩了怎么办...

假设我们现在这个网站有100个人同时访问,这时我们发现在我电脑上跑不起来了,怎么办?这时我们就可以考虑把单机复制几个,让别人可以去访问其他的,那这期间就可以实现一个操作就是"Nginx(负载均衡)",就是说让用户看起来还是一个电脑,但是它访问的却是不同的东西了:

![image-20211004094948180](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004094948180.png)

这里Nginx就是一代理服务器,准确叫反向代理服务器,而后面的单机就叫横向扩展,就是不停的在扩展单机,如果说加载到一定的时候需求不够的情况下,才会考虑分布式! 

![image-20211004095750887](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004095750887.png)

如果我们是在下单的流程中,我们就会把下订单这个模块放在第一个[单机上](这个电脑可以处理一万个订单),物流和支付模块比较缓慢我们就放多个单机上,进行拆开,那拆开怎么联系呢?通过HttpRestful风格;而我们只需要将其放在不同的电脑上进行调用就好了,比如说第一个电脑处理完订单后就会给第二个电脑发一个请求让这个电脑去处理,这样就极大提高了效率



理解:分布式系统就是多个电脑的集合也就是多个服务器,多个服务器一起完成共同的目标而进行协调处理,它们之间通过RPC或者HTTP协议进行联系起来,将一台服务器的压力变成多个人一起来做,就好比是人多力量大干事情就快了很多,那么时候用呢?我们对一台电脑服务器无法处理数据或者因为其他问题无法满足任务的时候,我们就会先考虑复制单机采用Nginx负载均衡的方式进行实现,如果还不行才会去考虑分布式系统,如果说一台服务器能干的事情就不需要第二台服务器,我们如果一台计算机能处理我们就不考虑分布式,就三个字"没必要"!因为使用分布式虽然好处很明显但是缺点也是有的,第一维护成本、资金的消耗、机制还有协议等等,其实最重要的点就是分布式系统和单击实现的目标都是一样的!

## 2. Dubbo介绍

> 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。
>
> Dubbo就是为了解决分布式出现四大问题的一种解决方案!

springCloud也是一样的,但也不一样,这是一个生态是要解决一系列问题

随着互联网的发展,网站应用的规模不断扩大,常规的[垂直应用架构](就是我们上面画图的垂直架构流程)已无法应对,分布式服务架构以及流动计算架构势在必行,急需一个治理系统确保架构有条不絮的演进.

Dubbo有这张图:

![img](https://dubbo.apache.org/imgs/user/dubbo-architecture-roadmap.jpg)

分析:首先我们系统从一个个框架和用户变成一个ORM,就是说1~10个人访问这个All in One是够用的;那到10~1000个人的时候All in one不够用了就要拆分,那么MVC的架构就出来了,后面1000~10000个人的时候MVC架构已经不够用了,我们就要实现RPC远程控制调用,就是说将这个系统放在不同的电脑上,进行分布式服务;最后就是我们把所有的服务放在云端,那这个云端什么意思呢:把所有东西放在一个特别大的机器上去跑;

1. 单一应用架构

   当网站流量很小的时候,只需一个应用将所有功能部署在一起,用于介绍部署节点和成本,此时,用于简化增删改查工作量的数据访问框架(ORM)是关键;

   ![image-20211004103550085](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004103550085.png)

   适用于小型网站,比如小型管理系统,将所有功能部署到一个功能里,简单易用

   缺点:

   1. 性能扩展比较难
   2. 协同开发问题
   3. 不利于升级维护(如果要升级就要把这个系统关掉,让用户等待)

2. 垂直应用架构

   当访问量逐渐增大,单一应用增加机器带来的加速度越来越小,将应用拆成互不相干的几个应用,以提升效率,此时.用于加速前端页面的开发的Web框架(MVC)是关键

   ![image-20211004104235717](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004104235717.png)

   通过切分业务来实现各个模块独立部署,降低了维护和部署的难度,团队各司其职更易管理,性能扩展也更方便,更有针对性!

   缺点:

   - 公用模块无法重复利用,开发性的浪费

3. 分布式服务架构

   当垂直应用越来越多.应用之间交互不可避免,将核心业务**抽取**出来,作为独立的业务,逐渐形成稳定的服务,使前端应用能更快速的响应多变的市场需求,此时,用于提高业务复用及整合的**分布式服务框架(RPC)**是关键!

   ![image-20211004105033879](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004105033879.png)

4. 流动计算架构

   当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。

   ![image-20211004105140677](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004105140677.png)

   理解:这个比如我们写完订单这个模块,我们就把它进行注册起来,而这个注册中心放在云端,注册完用户要用那么直接去注册中心拿

### 2.1 基本概念

这是dubbo运行基本原理:

![image-20211004135942904](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004135942904.png)

init:初始要做的事情,async:异步,sync:同步

- Provider(服务提供者):暴露服务的服务提供方,向注册中心注册自己提供的服务
- Consumer(服务消费者):调用远程服务的服务消费方,服务消费者在启动时,向注册中心订阅自己所需的服务,服务消费者,从提供者地址列表中,基于软负载均衡算法,选一台提供者进行调用,如果调用失败,再选一台调用;
- Registry(注册中心):注册中心返回服务提供者地址列表给消费者,如果有变更,注册中心将基于长连接推送变更数据给消费者;
- Monitior(监控中心):服务消费者和提供者,在内存中累计调用次数和调用时间,定时每分钟发送一次统计数据到监控中心

**执行流程关系说明**:

1. 服务容器负责启动,加载,运行服务提供者
2. 服务提供者在启动时,向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

**前台 中台 后台**，这个中台就是支撑前后台的中间商，所有东西都要经过它

> 理解：服务器一旦启动成功会将一些服务放在注册中心以供别人去使用和加载，哪怕没有服务是临时加载的那么也都会放在注册中心也就是无论我们加什么都会放进去；然后消费者就去注册中心消费，这之前都是init初始化干的事情；接下来是异步干的事情：消费者如果要去注册中心消费，就会被notify(通知)你可以去使用,那么消费者就可以直接去inoke执行,这个执行是同步;然后Monitor是监控的意思,也就是说监控中心去统计这之间的信息!

### 2.2 安装zookeeper

在dubbo官方文档中推荐我们安装zookeeper注册中心

我们直接去zookeeper官方安装即可,

注意:这里可能会有人遇到这个错误【org.apache.zookeeper.server.quorum.QuorumPeerMain】，这个错误的原因是你使用了大于3.5.5的版本，3.5.5开始，带有bin名称的包才是我们想要的下载可以直接使用的里面有编译后的二进制的包，而之前的普通的tar.gz的包里面是只是源码的包无法直接使用。所以如果下载3.5.5以后的版本的Zookeeper安装包，我们乖乖的下载带有bin标识的包就不会有问题了。

步骤:

1. 运行/bin/zkServer.cmd,发现会闪退:

   - 解决方案:将zkServer.cmd以文本文件打开,然后我们添加`pause`,这样就不会发生闪退了!

     ![image-20211004152119093](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004152119093.png)

2. 再次运行依然报错了,并且给出提示说没有zoo.cfg这个文件:

   - 解决方案:将conf/zoo_sample.cfg文件复制一份,然后将其重命名为zoo.cfg即可

     ![image-20211004152312942](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004152312942.png)	

   - 并且我们可以查看这个cfg里面都写了什么?

     ```properties
     ...
     # 临时数据存储的目录
     dataDir=/tmp/zookeeper
     ...
     # zookeeper的端口号
     clientPort=2181
     ```

3. 好接下来都成功了,那么我们依次运行`zkServer.cmd`,然后是`zkCli.cmd`

4. 我们在`zkCi.cmd`中输测试:
   ls / :列出zookeeper根下保存的所有节点

   ```bash
   [zk: localhost:2181(CONNECTED) 0] ls /
   [zookeeper]
   ```

   reate –e /miao 123：创建一个miao节点，值为123

   ```bash
   [zk: localhost:2181(CONNECTED) 1] create -e/miao 123
   org.apache.commons.cli.UnrecognizedOptionException: Unrecognized option: -e/miao   //演示一个错误示范,这里/左边要有空格
   [zk: localhost:2181(CONNECTED) 2] create -e /miao 123
   Created /miao
   ```

   get /miao：获取/miao节点的值

   ```bash
   [zk: localhost:2181(CONNECTED) 4] get /miao
   123
   ```

   我们再重新查询一下节点:

   ![image-20211004153105829](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004153105829.png)

   

### 2.3 安装dubbo-admin

dubbo本身并不是一个服务软件,它其实就是一个jar包,能够帮你的java程序连接到zookeeper,并利用zookeeper消费,提供服务,

dubbo-admin:是一个监控管理后台,主要查看我们注册了哪些服务,哪些服务被消费了

但是为了让用户更好的管理监控众多的dubbo服务,官方提供了一个可视化的监控程序dubbo-admin,这个不安装也不影响使用

因为admin是被托管到github上的,所以我们需要从github上进行下载:

地址 ：https://github.com/apache/dubbo-admin/tree/master

> 我这里尝试了无数种方法,最终确定在master-0.2.0这个分支上才可以正常的安装,其他的貌似对于我来讲都不行,就暂时这样吧,搞了我一下午!

步骤:

1. 解压完毕后我们打开`F:\dubbo-admin-develop\dubbo-admin-server\src\main\resources`下的`application.properties`:

2. 我们指定当前安装的zookeeper地址:

   ```properties
   admin.registry.address=zookeeper://127.0.0.1:2181
   # 如果没有端口,那么就是默认为8080,如果遇到端口冲突,可以修改
   server.port=7001
   ```

3. 在项目根目录下进行打包:

   ```properties
   # 利用maven命令打包,跳过测试直接打包
   mvn clean package -Dmaven.test.skip=true 
   ```

   ![image-20211004181720864](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004181720864.png)

   

   出现以下情况就是成功了:

   ![image-20211004181750945](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004181750945.png)

4. 执行 target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

   ```properties
   # 运行jar包,必须在jar所在的文件夹执行cmd命令
   java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
   ```

   **注意**:在运行期间会去自动查找zookeeper,所以一定要启动!

   ![image-20211004181813193](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004181813193.png)

5. 启动完毕我们打开看看:http://localhost:7001

   ![image-20211004181833968](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004181833968.png)

   默认是用户名为root,密码也是root:

   ![image-20211004182049701](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004182049701.png)

### 2.4 SpringBoot+Dubbo+zookeeper

我们实例试一试模拟dubbo的基本原理:

#### 1.框架搭建:

   创建一个空项目,然后创建一个模块名为"provider-server":

   接下来我们创建一个卖票的服务~**提供者**

   编写接口:

   ```java
   public interface TicketService {
       String getTicket();
   }
   ```

   编写实现类:

   ```java
   public class TicketServiceImpl implements TicketService {
       @Override
       public String getTicket() {
           return "good";
       }
   }
   ```

   然后我们创建另一个模块名为"consumer-server":

   创建一个用户的服务:-**消费者**

   编写service:

   ```java
   public interface UserService {
       //想拿到provider-server提供的票
   }
   ```

#### 2.服务提供者:

   我们需要将服务提供者注册到注册中心,所以我们需要整合Dubbo和Zookeeper:

   1. 导入依赖:

      ```xml-dtd
      <!-- Dubbo Spring Boot Starter -->
      <dependency>
          <groupId>org.apache.dubbo</groupId>
          <artifactId>dubbo-spring-boot-starter</artifactId>
          <version>2.7.3</version>
      </dependency>
      <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
      <dependency>
          <groupId>com.github.sgroschupf</groupId>
          <artifactId>zkclient</artifactId>
          <version>0.1</version>
      </dependency>
      ```

      【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；

      ```xml-dtd
      <!-- 引入zookeeper -->
      <dependency>
         <groupId>org.apache.curator</groupId>
         <artifactId>curator-framework</artifactId>
         <version>2.12.0</version>
      </dependency>
      <dependency>
         <groupId>org.apache.curator</groupId>
         <artifactId>curator-recipes</artifactId>
         <version>2.12.0</version>
      </dependency>
      <dependency>
         <groupId>org.apache.zookeeper</groupId>
         <artifactId>zookeeper</artifactId>
         <version>3.4.14</version>
         <!--排除这个slf4j-log4j12-->
         <exclusions>
             <exclusion>
                 <groupId>org.slf4j</groupId>
                 <artifactId>slf4j-log4j12</artifactId>
             </exclusion>
         </exclusions>
      </dependency>
      ```

   2. 编写配置文件:

      ```properties
      # 服务应用名字(服务要有名字,别人才找得到)
      dubbo.application.name=provider-server
      # 注册中心地址
      dubbo.registry.address=zookeeper://127.0.0.1:2181
      # 哪些服务要被注册(扫描哪些包下的注册进去)
      dubbo.scan.base-packages=com.miao.server
      ```

   3. 在service的实现类中配置服务注解,进行发布注册服务:

      ```java
      import org.apache.dubbo.config.annotation.Service;
      import org.springframework.stereotype.Component;
      //zookeeper:服务注册与发现
      
      //这里使用了Dubbo以后尽量不要使用Service注解,因为使用@Service注解有两个包,一个是spring的一个是dubbo,为了区分,我们选择使用万能注解@Compont
      @Component //放在容器中
      //可以被扫描到,在项目启动时就自动注册到注册中心-zookeeper
      @Service //将服务发布出去
      public class TicketServiceImpl implements TicketService {
          @Override
          public String getTicket() {
              return "good";
          }
      }
      ```

      **理解**:我们之前不是看Dubbo的基本原理的时候在容器启动的时候,服务提供方就会将服务自动注册到注册中心去,而这里就是在应用启动起来的时候,dubbo就扫描指定的包下带有@Component注解的服务,然后通过@Serrvice发布到指定的注册中心去!

      **注意**:项目启动的时候,要将zookeeper启动起来,然后再启动项目,否则要报错!

   4. 我们在dubbo-admin中观察效果:

      ![image-20211004203750429](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004203750429.png)

      我们点进去看看:

      ![image-20211004203918912](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004203918912.png)

​		

​		这里服务名是以我们实现类的接口为名称,然后我们看见我们的主机名和我们的应用名!可以说我们已经注册到zookper中去了!

#### 3.服务消费者

1. 导入依赖:

   ```xml-dtd
   <!--dubbo-->
   <!-- Dubbo Spring Boot Starter -->
   <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-spring-boot-starter</artifactId>
      <version>2.7.3</version>
   </dependency>
   <!--zookeeper-->
   <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
   <dependency>
      <groupId>com.github.sgroschupf</groupId>
      <artifactId>zkclient</artifactId>
      <version>0.1</version>
   </dependency>
   <!-- 引入zookeeper -->
   <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-framework</artifactId>
      <version>2.12.0</version>
   </dependency>
   <dependency>
      <groupId>org.apache.curator</groupId>
      <artifactId>curator-recipes</artifactId>
      <version>2.12.0</version>
   </dependency>
   <dependency>
      <groupId>org.apache.zookeeper</groupId>
      <artifactId>zookeeper</artifactId>
      <version>3.4.14</version>
      <!--排除这个slf4j-log4j12-->
      <exclusions>
          <exclusion>
              <groupId>org.slf4j</groupId>
              <artifactId>slf4j-log4j12</artifactId>
          </exclusion>
      </exclusions>
   </dependency>
   ```

2. 编写配置文件:

   ```properties
   # 服务提供者已经将服务提交给注册中心了,那么消费者应该从哪里拿服务,肯定是从注册中心拿
   # 注册中心地址
   dubbo.registry.address=zookeeper://127.0.0.1:2181
   # 消费者去注册中心拿服务需要暴露自己的姓名
   dubbo.application.name=consumer-server
   ```

3. 修改消费者的服务类:

   ```java
   import org.apache.dubbo.config.annotation.Reference;
   import org.springframework.stereotype.Service;
   
   @Service  //这里使用spring的注解,因为只要引入我们才能使用注入
   //这里得是class类,如果是接口那么接口调接口,是不行的!
   public class UserService {
       //想拿到provider-server提供的票,去注册中心拿
       //引用服务提供者提供的服务,有两种方法:1.正常开发的话引入pom坐标;2.一般情况下可以定义路径相同的接口名
       @Reference
       TicketService TICKET_SERVICE;  //注意这里是用于引用于注册中心的服务,并不是注入xxx实现类
   
       public void buyTicket(){
           String ticket = TICKET_SERVICE.getTicket();
           System.out.println("在注册中心拿==>"+ticket);
       }
   }
   ```

   这里由于要去服务中心拿,一般我们使用POM坐标的形式获取,但是这里为了简单我们直接使用服务的全路径的形式获取,所以:

   ![image-20211004210607735](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004210607735.png)

   将之前的接口拷过来!只是为了用路径相同的接口名去注册中心获取而已!它是根据这个接口名去找的!

   就是dubbo-admin中的这个:

   ![image-20211004211308276](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004211308276.png)

4. 编写测试类:

   ```java
   @SpringBootTest
   class ConsumerServerApplicationTests {
       @Autowired
       UserService userService;
       @Test
       void contextLoads() {
           userService.buyTicket();
       }
   }
   ```

5. 看控制台:

   ![image-20211004210737414](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004210737414.png)



#### 4.小结

步骤:

前提:zooker服务必须是已开启!

1. 首先是提供者提供服务
   1. 导入依赖
   2. 配置注册中心的地址以及服务发现名和扫描的包
   3. 在想要的注册的服务上添加注解@Service-是dubbo的注解
2. 消费者如何去消费
   1. 导入依赖
   2. 直接注册中心的地址和配置自己服务名
   3. 从远程注入服务@Refence,注意这里的注解@Service是spring的注解

> dubbo就是两个不同的应用进行连接起来!Sping Boot+ dubbo+ zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想

## 3.RPC

之前说服务器之间联系通过HTTP或RPC进行联系起来;

HTTP协议是:无状态协议,是通信协议,基于网络的!而RPC也是一种协议,用于网络通信,其本质上没有区别,只是功能优点不一样!

![image-20211004105734444](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004105734444.png)

### 3.1 简介

RPC[Remote Procedure Call]:

​	是指**远程过程调用**,是一种进程间**通信方式**,它是一种技术的思想,而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器）的过程或函数，而不用程序员显示编码这个远程调用的细节，即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。



也就是说两台服务器A,B,一个应用部署在A服务器上,想要调用B服务器上应用提供的函数/方法,由于不在一个内存空间,不能直接调用,需要通过网络来表达调用的语义和传达调用的数据,为什么要用RPC呢?就是无法在一个进程内,甚至一个计算机内通过本地调用的方式完成的需求,比如不同的系统间的通讯,甚至不同的组织间的通讯,由于计算能力需要横向扩展,需要在多台机器组成的集群上部署应用,RPC就是要像调用本地的函数一样去调远程函数;

![image-20211004111027068](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004111027068.png)

之前本地调用就是在自己的电脑里就可以直接调用,而RPC呢?就是在A电脑里去通过B电脑里的数据,这就叫远程调用,也许说A电脑在中国,而B电脑在美国,而这之间就需要远程调用了,并且要注意这之间的通信也是极不可靠的,这也说明分布式的缺点!

还是不懂:就阅读这篇文章：https://www.jianshu.com/p/2accc2840a1b

### 3.2 基本原理

![image-20211004111503428](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004111503428.png)

解释:首先客户端的函数会去调用客户端的一些东西,然后通过网络sockets通信方式去通信到了另一台电脑上,另一台电脑上就会去解析我们这边的代码,解析后比如说认为你要调用我们这边的服务数据,那么调用完毕后就会告诉服务器端,然后再通过sockets通信再返回回去

### 3.3 步骤解析

![image-20211004111805301](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211004111805301.png)

解释:由客户端调用,然后将其序列化,因为在网络中无法直接传输需要将其序列化为文本然后对面再反序列化得出这个结果,然后序列化将其发送给服务器端,然后服务器端反序列化后调用本地的服务,服务器处理完后就会将结果返回出去,然后返回给客户端本地的调用结果!

RPC有两个重要模块:通信,序列化

1. 通信:通信是为了传输,而我们也可以使用Http传输
2. 序列化:为了方便我们数据传输,数据传输需要转换(为什么需要序列化,是因为我们在使用网络传输的时候不可能直接传输一个User对象,而是将其序列化改为二进制文件)

## 4.回顾

回顾之前的:

```java
架构:
之前的单一应用程序到三层架构
三层架构 + MVC
	目的:解耦,分层 
开发框架:
	spring:
        IOC AOP 两大核心
            IOC:控制反转
                原来我们都是一步步操作,现在交给容器了!我们需要什么就去拿就可以了!
            AOP:切面(本质就是动态代理)
				为了解决什么?不影响业务本来的情况下,实现动态增加功能,大量应用在日志、事务等等方面    
        简述:spring是一个轻量级的Java开源框架,也是一个容器
        目的:解决企业开发的复杂性问题
        缺点:spring是春天同时也因为它十分复杂,比如写大量的配置文件!
	SpringBoot:
		诞生:简化spring的开发,是新一代JavaEE的开发标准
        注意:它并不是新东西,它是spring的升级版,将spring与springMVC进行整合,将之前的大量的配置文件进行自动配置封装!    
        好处:开箱即用!->它自动帮我们配置了非常多的东西,我们拿过来就可以用	
        特性：约定大于配置！
-------------------------------------------------------------------------------------------------------------------
随着公司体系的越来越大，用户越来越多，以上的架构就已经不够用了！
微服务架构：--》新架构
       什么是微服务:
			模块化功能化。
       最初现状：
            比如我们电商系统就有用户模块、支付、娱乐...,按照之前架构思想就全放在不同的Controller,然后我们乱跳转,而一旦人数过多,一台服务器就明显跟不上了。
       横向解决：
           一台服务器解决不了,那就多增加几台服务器,这就是横向的解决问题!那什么是横向呢?就是不够就直接拿来用,造成一直在铺面!这种也会引发一个问题:假设A服务器占用98%的资源,而B服务器只			 占用了10%的资源,造成资源不公平。
       负载均衡：
           而如果说A服务器突然崩了,那么这98%的资源将直接崩盘了。那如何解决呢?我们要让每台服务器都要资源平衡,这就是负载均衡,把每台服务器的压力均衡的分给每个服务器。
       分配服务器：
		   如果说用户模块和签到模块，其中用户模块人数很多而签到非常少。用户占大量的资源，签到占用非常少，可以这样给用户多一点服务器，给签到少一点服务器。这个跟上面的是有区别的，上面所			有功能都集中在一块，不管你人多不多反正一台服务器解决不了那就横向扩展，再不行我再负载均衡也能解决，而这个方式是将原来的整体项目分成模块化，比如用户就是单独的项目，签到也是单			 独的项目，两者之间进行通信，这个就是将原来的属于用户的模块的服务器将其拆分成一个个服务器，让其模块化
微服务架构问题？
       分布式架构会遇到的四个核心问题：
               1.这么多服务，客户端如何去访问？
               2.这么多服务，服务之间如何进行通信？
               3.这么多服务，如何治理呢？
               4.服务挂了，怎么办？
       解决方案：
               SpringCloud:
					简述:是一套生态,就是用于以上分布式架构四个问题,
					缺点:使用springCloud必须要掌握springBoot,因为SpringCloud是基于springBoot的;
			   1.SpringCloud NetFlix:
					简述:早些年出来了一套解决方案!一站式解决方案
                    如何解决(一一对应):
						1. 采用API网关,使用zuul组件
                        2. Feign --》基于HttpClient的通信方式,同步并阻塞
                        3. 服务注册与发现--》Eureka
                        4. 熔断机制--》Hystrix
                    缺点:2018年年底,NetFlix宣布无限期停止维护,生态不再维护,就会慢慢跟社会脱节
               2.Apache Dubbo zookeeper:
					简述:之前是阿里巴巴的,这是第二套解决系统
                    如何解决(一一对应):
						1. API网关没有,要么找第三方要么自己实现
                        2. Dubbo是一个高性能的基于Java实现的RPC通信框架,RPC:就是解决通信的一个问题
                        3. 服务与注册--》zookeeper:动物园管理者(Hadoop,Hive),这里生态圈里图标都是动物
                        4. 没有!--》借助了NetFlix的熔断机制
                    缺点:不完善!
               3.SpringCloudAlibaba:
					简述:一站式解决方案!
               4.Server Mesh:
					简述:服务网格,一种微服务标准
                    解决方案:istio
     这么多方案都是万变不离其宗,那四个问题:
			1.API网关,解决服务器路由问题;
            2.HTTP、RPC框架,异步调用;
			3.服务注册与发现,进行高可用高效率的管理;
            4.熔断机制,服务崩了->服务降级;
	为什么要解决这个问题?
        本质:网络不可靠的!
               
```



