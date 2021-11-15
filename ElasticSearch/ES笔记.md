# ES笔记

## 1.概念

### 1.1 引入

![image-20211101194331216](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101194331216.png)

ElasticSearch其实本身并不难理解,就比如我们看见我们百度搜索关键词,然后不到一秒中就出现各种以有关键词的标题文档就出来;按照以前我们如果遇到这种场景,我们可能就会使用SQL来查询,就好比如:`...like %%`来进行模糊查询,那么问题来了,如果是大数据的话就好比是上万条的那种那么这个速度就会明显得缓慢,并且这个时候我们还要去考虑一些SQL语句的优化和索引的优化,虽然说相对来说会快一点但是本质上还是达不到一个大数据的一个要求;

ElasticSearch 发音:伊莱克斯sir起

Kibana 发音:卡班纳

那么这个时候解决办法出来了:

> ElasticSearch-分布式全文搜索引擎

可以简单的理解为就两个字"**搜索**"!只要是用搜索那么就可以用它!比如一些应用场景如:淘宝、京东、github、百度等等,只要用到一些搜索都会使用到它!

> 以后只要用到搜索,就可以用到ES(大数据量的情况下使用!)

### 1.2 Doug Cutting

ES跟这个人本身并没有太大关系,但是如果没有它一定没有ES,所以我们可以来看看:

1998年9月4日，Google公司在美国硅谷成立。正如大家所知，它是一家做`搜索引擎`起家的公司。

![image-20211101201153248](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101201153248.png)

无独有偶，一位名叫Doug Cutting的美国工程师，也迷上了搜索引擎。他做了一个用于`文本搜索的函数库`（姑且理解为软件的功能组件），命名为**Lucene**。

> Lucene-读音:撸神死 后面的搜索引擎Solr和ElasticSerach都是基于这个进行封装的!

![image-20211101201415452](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101201415452.png)

Lucene是用JAVA写成的，目标是为各种中小型应用软件加入`全文检索功能`。因为好用而且开源（代码公开），非常受程序员们的欢迎。

早期的时候，这个项目被发布在Doug Cutting的个人网站和SourceForge（一个开源软件网站）。后来，2001年底，Lucene成为Apache软件基金会jakarta项目的一个子项目

![image-20211101201525738](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101201525738.png)

2004年，Doug Cutting再接再励，在Lucene的基础上，和Apache开源伙伴Mike Cafarella合作，开发了一款可以代替当时的主流搜索的开源搜索引擎，命名为Nutch:

![image-20211101201556970](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101201556970.png)

> Nutch-读音:呐起

Nutch是一个建立在Lucene核心之上的`网页搜索应用程序`，可以下载下来直接使用。它在Lucene的基础上加了`网络爬虫`和一些网页相关的功能，目的就是从一个简单的站内检索推广到全球网络的搜索上，就
像Google一样;

Nutch在业界的影响力比Lucene更大。

大批网站采用了Nutch平台，大大降低了技术门槛，使低成本的普通计算机取代高价的Web服务器成为可能。甚至有一段时间，在硅谷有了一股用Nutch低成本创业的潮流。

随着时间的推移，无论是Google还是Nutch，都面临搜索对象“体积”不断增大的问题。(**大数据时代来临,需要新的技术进行革新!**)

尤其是Google，作为互联网搜索引擎，需要存储大量的网页，并不断优化自己的搜索算法，提升搜索效率。

![image-20211101202003793](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101202003793.png)

在这个过程中，Google确实找到了不少好办法，并且无私地分享了出来。

2003年，Google发表了一篇技术学术论文，公开介绍了自己的谷歌文件系统**GFS（Google File System）**。这是Google公司为了`存储海量搜索数据而设计的专用文件系统`。

第二年，也就是2004年，Doug Cutting基于Google的GFS论文，实现了**分布式文件存储系统**，并将它命名为**NDFS（Nutch Distributed File System）**。

![image-20211101202742570](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101202742570.png)

还是2004年，Google又发表了一篇技术学术论文，介绍自己的**MapReduce编程模型**。这个编程模型，用于`大规模数据集（大于1TB）的并行分析运算`。

> 大数据就两个问题:存储+计算!

第二年（2005年），Doug Cutting又基于MapReduce，在Nutch搜索引擎实现了该功能。

![image-20211101202823401](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101202823401.png)

2006年，当时依然很厉害的Yahoo（雅虎）公司，招安了Doug Cutting

![image-20211101202842803](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101202842803.png)

加盟Yahoo之后，Doug Cutting将NDFS和MapReduce进行了升级改造，并重新命名为**Hadoop**（NDFS也改名为HDFS，Hadoop Distributed File System）

> 好家伙,全是靠Google给的灵感,谷歌出一个GFS文件系统它就出一个NDFS分布式文件存储系统,谷歌出一个MapReduce数据分析计算它就模仿了一个!

这个，就是后来大名鼎鼎的大数据框架系统——Hadoop的由来。而Doug Cutting，则被人们称为Hadoop之父

![image-20211101202936536](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101202936536.png)

Hadoop这个名字，实际上是Doug Cutting他儿子的黄色玩具大象的名字。所以，Hadoop的Logo，就是一只奔跑的黄色大象:

![image-20211101203006008](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101203006008.png)

我们继续往下说。

还是2006年，Google又发论文了

这次，它们介绍了自己的**BigTable**。这是一种分布式数据存储系统，一种用来`处理海量数据的非关系型数据库`。

Doug Cutting当然没有放过，在自己的hadoop系统里面，引入了BigTable，并命名为**HBase**

![image-20211101203100394](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101203100394.png)

好吧，反正就是紧跟Google时代步伐，你出什么，我学什么。

所以，Hadoop的核心部分，基本上都有Google的影子。

![image-20211101203226603](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101203226603.png)

2008年1月，Hadoop成功上位，正式成为Apache基金会的顶级项目。

同年2月，Yahoo宣布建成了一个拥有1万个内核的Hadoop集群，并将自己的搜索引擎产品部署在上面。

7月，Hadoop打破世界纪录，成为最快排序1TB数据的系统，用时209秒

> 回到主题,因为ElasticSearch是基于Lucene进行封装的,所以我们来看一下Lucene:

Lucene:是一套信息检索工具包!是用Java语言写的并且开源,所以我们可以直接下载下来,并且这是一个jar包!记住只是一个jar包不包含搜索引擎系统!

它包含了索引结构、读写索引工具、排序、搜索规则...工具类!，因此在使用Lucene时仍需要关注搜索引擎系统，例如数据获取、解析、分词等方面的东西

Lucene和ElasticSearch的关系?

​	ElasticSearch是基于Lucene做了一些封装和增强,



### 1.3 概述

ElasticSearch,简称为ES,`ES是一个开源的高扩展的分布式全文检索引擎`,它可以近乎`实时的存储、检索数据`；本身扩展性很好可以扩展到上百台服务器处理PB(大数据)级别的数据。es也可以使用Java开发并使用Luncene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的**RESTFul API**来隐藏Luncene的复杂性,从而让全文搜索变得简单。

> 据国际权威的数据库产品评测机构DB Engines的统计,在2016年1月,ElasticSearch已超过Solr等,**成为排名第一的搜索引擎类应用**。

```
历史：
	多年前，一个叫做Shay Banon的刚结婚不久的失业开发者,由于妻子要去伦敦学习厨师,他便跟着也去了;在他找工作的过程中,为了给妻子构建一个食谱的搜索引擎,他开始构建一个早起版本的Luncene。
直接基于Luncene工作会比较困难,所以Shay开始抽象Luncene代码以便Java程序员可以在应用中添加搜索功能。他发布了他的第一个开源项目，叫做“Compass”。
	后来Shay找到一份工作，这份工作处在高性能和内存数据网格的分布式环境中，这个时候它公司就迫切的需要高性能的、实时的、分布式的搜索引擎也是理所当然需要的。然后他决定重写Compass库使其成为一个独立的服务叫做ElasticSearch。
	第一个公开版本出现在2010年2月，在那之后ElasticSearch已经成为GitHub上最受欢迎的项目之一,代码贡献者超过300人,一家主营ElasticSearch的公司就此成立,他们一边提供商业支持一边开发新功能,不过ElasticSearch将永远开源且对所有人可用。
	Shay的妻子依旧等待着她的食谱搜索
```

**谁在使用：**

1. 维基百科，类似百度百科，全文检索，高亮，搜索推荐;(`基于权重排序!`)
2. The Guardian（国外新闻网站），类似搜狐新闻，用户行为日志（点击，浏览，收藏，评论）+社交网络数据（对某某新闻的相关看法），数据分析，给到每篇新闻文章的作者，让他知道他的文章的公众反馈（好，坏，热门，垃圾，鄙视，崇拜）
3. Stack Overflow（国外的程序异常讨论论坛），IT问题，程序的报错，提交上去，有人会跟你讨论和回答，全文检索，搜索相关问题和答案，程序报错了，就会将报错信息粘贴到里面去，搜索有没有对应的答案
4. GitHub（开源代码管理），搜索上千亿行代码
5. 电商网站，检索商品,如:淘宝、京东
6. 日志数据分析，logstash采集日志，ES进行复杂的数据分析，kibana做成数据可视化分析;`ELK技术:elasticsearch+logstash+kibana`
7. 商品价格监控网站，用户设定某商品的价格阈值，当低于该阈值的时候，发送通知消息给用户，比如说订阅牙膏的监控，如果高露洁牙膏的家庭套装低于50块钱，就通知我，我就去买(比如秒杀)
8. BI系统，商业智能，Business Intelligence。比如说有个大型商场集团，BI，分析一下某某区域最近3年的用户消费金额的趋势以及用户群体的组成构成，产出相关的数张报表，xxx区最近3年每年消费
   金额呈现100%的增长，而且用户群体85%是高级白领，开一个新商场。`ES执行数据分析和挖掘，Kibana进行数据可视化`
9. 国内：站内搜索（电商，招聘，门户，等等），IT系统搜索（OA，CRM，ERP，等等），数据分析（ES热门的一个使用场景）

> 理解:看过历史的就知道ES是相对谷歌出生的,所以它是一个全文搜索引擎,然后它又是基于Hadoop大数据的所以它具有分布式的特点,而高扩展就已满足我们的"三高"问题,也就是说我们可以用它来做一些集群,做一些高可用是完全没问题的!好处:实时存储和检索,导致查询的效率非常高,本身扩展性好可以搭建几百台服务器作为集群来处理PB(大数据时代)级别的数据;它由全文检索、结构化搜索、数据分析为一体，其自身带有分布式协调管理功能而同一个solr就得使用zookeeper来进行搭建,它核心"实时查询、处理速度快、高扩展"！ES对外提供RESTFul风格的接口而其本身只支持json格式的数据,不过这也够用了,

注意:ES的版本6.x跟7.x发生很大的概念如命令、文档、工具都是有差距的！

### 1.4 ES与Solr的差别

> ElasticSearch的概念:

​	是一个`实时`分布式搜索和分析引擎。它让你前所未有的`速度`处理大数据成为可能。它用于**全文检索**、**结构化搜索**、**分析**以及将这三者混合使用：

1. 维基百科使用ElasticSearch提供全文搜索并高亮关键字,以及输入实时搜索(search-asyou-type)和搜索纠错(did-you-mean)等搜索建议功能;
2. 英国卫报使用ElasticSearch结合全文搜索与地理位置查询,以及more-like-this功能来找到相关的问题和答案。
3. GitHub使用ElasticSearch检索1300亿行的代码

ElasticSearch不仅用于大型企业,它还让像DataDog以及Klout这样的创业公司最初的想法变成可扩展的解决方案。ElasticSearch可以在我们的笔记本上运行,也可以在数以百计的服务器上处理PB级别的数据;

​	ElasticSearch是一个基于Apache Lucene(TM)的开源搜索引擎,无论在开源还是专有领域,Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。但是，`Lucene只是一个库`,想要使用它,你必须使用Java来作为开发语言并将其直接集成到你的应用中,更糟糕的是,Lucene非常复杂,你需要深入了解检索的相关知识来理解它是如何工作的。ElasticSearch也使用Java开发并使用Lucene作为核心来实现所有索引和搜索的功能,但是它的目的`是通过简单的Restful API来隐藏Lucene的复杂性`,从而让全文搜索变得简单。

> Solr概念:

​	Solr是Apache下的顶级开源项目,采用Java开发,它是**基于Lucene的全文搜索服务器**,Solr提供了比Lucene更为丰富的**查询语言**,同时实现了**可配置**、**可扩展**，并对**索引、搜索性能**进行了优化；

Solr可以独立运行，运行在jetty、Tomcat等这些Servlet容器中，Solr索引的实现方法很简单，`用Post方法向solr服务器发送给一个描述FileLd及其内容的XML文档,Solr根据xml文档添加、删除、更新索引`。Solr搜索只需要发送一个Http GET请求，然后对Solr返回XML、json等格式的查询结果进行解析，组织页面布局。Solr不提供构建UI的功能，Solr提供了一个管理页面，通过管理页面可以查询Solr的配置和运行情况。

​	Solr是基于Lucene开发企业级搜索服务器,实际上就是封装了Lucene。

​	**Solr是一个独立的企业级搜索应用服务器**，它`对外提供类似于Web-service的API接口`，用户可以通过HTTP请求，向搜索引擎服务器提交一定格式的文件，生成索引；也可以通过提出查找请求，并通过返回结果；

> Lucene概念:

​	Lucene是Apache软件基金会jakarta项目组的一个子项目,是一个开放源代码的全文检索引擎工具包,但它不是一个完整的全文检索引擎,而是一个全文检索引擎的结构,提供了完整的查询引擎和索引你引擎,部分文本分析引擎(英文和德文两种西方语言)。Lucene的目的就是为软件开发人员提供了一个简单易用的工具包,以方便的在目标系统中实现全文检索的功能,或者是以此为基础建立起完整的全文检索引擎,Lucene是一套用于全文检索和搜寻的开源程式库,由Apache软件基金会支持和提供,lucene提供了一个简单却强大的应用程式接口,能够做全文索引和搜寻。`在Java开发环境里Lucene是一个成熟的免费开源工具,就其本身而言,Lucene是当前以及最近几年最受欢迎的免费Java信息检索程序库`,人们经常提到信息检索程序库虽然与搜索引擎有关,但不应该将信息检索程序库与搜索引擎相混淆。

Lucene是一个全文检索引擎的架构,那为什么是全文检索引擎?

​	全文搜索引擎是名副其实的搜索引擎,国外具有代表的有Google、Fast/AllTheWeb、AltaVista、Inktomi、Teoma、WiseNut等,国内著名的有百度(Baidu);它们都是通过从互联网提取的各个网站的信息(以网页文字为主)而建立的数据库中,检索与用户查询条件匹配的相关记录,然后按一定的排列顺序将结果返回给用户,因此它们是真正的搜索引擎。

从搜索结果来源的角度：全文搜索引擎又可细分两种，一种是拥有自己的检索程序（Indexer），俗称“蜘蛛（Spider）程序或"机器人"(Robot)程序,并自建网页数据库,搜索结果直接从自身的数据库中调用,如上面提到的7家引擎;另一种则是租用其他引擎的数据库,并按自定义的格式排列搜索结果,如Lycos引擎;

> ElasticSearch与Solr比较

![image-20211102143841623](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102143841623.png)

这是对单纯的已有数据进行检索时,Solr会更快;

![image-20211102143914079](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102143914079.png)

因为建立索引好比在mysql里面是一件非常麻烦的过程,所以Solr查询较差!

![image-20211102143921393](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102143921393.png)

数据量增加,尤其是到了大数据时代!



![image-20211102143936631](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102143936631.png)

比如说我们转变基本的搜索引擎以后从Solr转变为ES可以看见效率一般可以提高接近50倍,

> ElasticSearch VS Solr总结

1. ES基本是开箱即用(解压就可以使用!),非常简单。Solr安装略微复杂一丢丢！
2. Solr利用Zookeeper进行分布式管理,而ElasticSearch**自身带有分布式协调管理功能.**
3. Solr支持更多格式的数据,比如JSON、XML、CSV，而ElasticSearch仅支持json文件格式。
4. Solr官方提供的功能非常多但是很多功能用不到，而ElasticSearch本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑
5. Solr查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；
   - ES建立索引快（即查询慢），即实时性查询快，用于facebook新浪等搜索
   - Solr是传统搜索应用的有力解决方案，但ElasticSearch更适合于新兴的**实时**搜索应用
6. Solr比较成熟，有一个更大更成熟的用户、开发和贡献者社区，而ElasticSearch相对开发维护者较少，更新太快，学习使用成本较高！

## 2.下载

声明:

> 1. 因为ElasticSearch是基于开发的,所以安装必须是建立在JDK1.8以上的,这是一个最低要求!
> 2. ES的版本和我们之后对应的Java的核心jar包要进行版本对应!

下载:

官网:https://www.elastic.co/cn/elasticsearch/

![image-20211102211742033](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102211742033.png)

这是下载链接:https://www.elastic.co/cn/downloads/elasticsearch

![image-20211102211957624](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102211957624.png)

### 2.1 安装

点击下载后得到一个压缩包,然后将其解压,然后解压后是这个样子的:

![image-20211102212312139](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211102212312139.png)

目录结构含义:

```bash
bin: 启动文件
config: 配置文件
	log4j2.properties: 日志配置文件
	jvm.options: java虚拟机的配置
	elasticsearch.yml: es的配置文件
data: 索引数据目录
lib: 相关类库Jar包
logs: 日志目录
modules: 功能模块
plugins: 插件,比如我们后面使用的IK分词器
```

**注意**:在jvm.options配置文件默认设置ES启动会给一个G的内存!要注意!!!

> 认识一下elasticsearch.yml

1. 集群配置

```yaml
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
#cluster.name: my-application
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
#node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
```

2. 打印当前日志信息

```yaml
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
#
```

3. 内存信息

```yaml
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
# bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
```

4. 网络配置

```yaml
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
#network.host: 192.168.0.1
#
# Set a custom port for HTTP:
#
#http.port: 9200    这是端口号配置
#
# For more information, consult the network module documentation.
#
```

> 启动

双击ElasticSearch下的bin目录中的elasticsearch.bat启动，控制台显示的日志（等待启动完毕！）：

![image-20211104152123046](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104152123046.png)

记住:默认启动端口为9200,而9300表示是通信地址

> 访问测试

然后我们此时在页面上访问:http://localhost:9200

![image-20211104152311702](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104152311702.png)

出现这几个字样就表示启动成功了!

**解释**:可以看出这是一个json结构,从图中可以看出:

```json
{
  "name" : "DESKTOP-J0R2HIH",   //主机名
  "cluster_name" : "elasticsearch", //es一个人也支持集群,所以默认的集群的名字就是elasticsearch
  "cluster_uuid" : "ex0gxhrlR3-4xotwpJAAvQ", //一个人的集群的uuid
  "version" : {
    "number" : "7.6.1",		//当前的es的版本
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "aa751e09be0a5072e8570670309b1f12348f023b",
    "build_date" : "2020-02-29T00:15:25.529771Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",		//基于Lucene的版本 
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"  //这是一个标准语言,说:你知道的,为了搜索
}
```

### 2.2 安装可视化界面

可视化界面现在用的比较多的是用的是head的插件,而这个插件又依靠Node.js

Head是elasticsearch的集群管理工具，可以用于数据的浏览查询！被托管在github上面

地址:https://github.com/mobz/elasticsearch-head/

**注意**:这是一个标准的前端项目,所以需要安装npm前端需要的依赖之类的!

![image-20211104153557984](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104153557984.png)

在head项目里找到package.json可以看见在scripts脚本里设置的名字是start,所以我们启动的话是`npm run start`

将地址的项目进行克隆到本地以后进行安装依赖启动:

```bash
cnpm install
npm run start
```

> 访问测试

![image-20211104154454532](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104154454532.png)

注意这里虽然启动了head,但是并没有跟我们的ES进行连接,原因为什么呢?

![image-20211104154601445](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104154601445.png)

也就是说这里发生了跨域问题,也就是我们这里从一个网站端口访问另一个端口这也就是两个服务之间的访问,这就已经达成了**跨域问题**!

解决办法:

在ES配置文件中找到elasticSearch.yaml然后打开:

```yaml
# 跨域配置：
 http.cors.enabled: true    # 开启跨域
 http.cors.allow-origin: "*" # 设置任何都访问
```

修改后我们再次访问测试:

![image-20211104161001952](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104161001952.png)

**注意**:修改配置文件记得将ES进行重启!

> 认识一下Head界面

1. 点击索引:

   ![image-20211104163147223](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104163147223.png)

   **我们可以把索引当做一个数据库!**可以建立索引(当做数据库),而在这里面我们要放一些文档(当做是库中的数据!)

   注:其实还有一个表的概念,在ES中是type,但是在7.x版本已经将其淘汰了!而8.x就彻底淘汰了!

   **踩坑**:这个时候可能会出现点击新建没反应,经过一顿排查,原来缺少安装依赖:

   ```bash
   # 安装head插件需要安装两个配置,es5以上就需要安装node和grunt
   # 这里就不介绍node的安装.直接看grount的安装
   npm install -g grunt-cli
   ```

   查看是否安装成功:

   ![image-20211104165248382](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104165248382.png)

   然后回到我们刚刚新建索引后,查看概览:

   ![image-20211104165349426](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104165349426.png)

   可以发现我们的索引是建立成功的,这里的1234是关于集群的分辨信息,后面用到再详细概述

2. 点击数据概览:

   ![image-20211104165547786](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104165547786.png)

3. 点击复合查询:

   ![image-20211104170305786](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104170305786.png)

   记住:这个查询的JSON就是我们未来查询的格式!

> 这个head插件我们就把它当做数据展示工具!

**注意**:我们可以看见这里的JSON查询是没有格式化的!所以我们后面的查询我们用Kibana进行查询!

我们再一次总览一下这个Head插件的所有信息:

![image-20211104172215318](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104172215318.png)

### 2.3 ELK介绍

​	ELK是ElasticSearch、Logstash、Kibana三大开源框架首字母大写简称。市面上也被成为ElasticStack,其中`ElasticSearch是一个基于Lucene、分布式、通过RESTFul方式进行交互的近实时搜索平台框架`。

​	类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用ElasticSearch作为底层支持框架,可见ElasticSearch提供的搜索能力确实强大,市面上很多时候我们简称ElasticSearch为ES,其中`Logstash是ELK的中央数据流引擎`,用于从不同目标(文件/数据存储/MQ)收集的不同格式数据,经过过滤后支持输出到不同目的地(文件/MQ/redis/ElasticSearch/kafka等)。Kibana可以将ElasticSearch的数据通过友好的页面展示出来,提供实时分析的功能。

​	市面上很多开发只要提到ELK能够一致说出它是一个**日志分析架构技术栈**总称，但实际上ELK不仅仅适用于日志分析，它还可以支持其他任何数据分析和收集的场景，日志分析和收集只是更具有代表性，并且唯一性！

![image-20211104174933072](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104174933072.png)

> 理解:ELk其中ES提供搜索能力,Logstash提供从不同地方收集不同格式数据然后过滤输出到不同目的地;而Kibaba将ES的数据进行展示出来并提供分析的功能!

### 2.4 安装kibana

**概念**:Kibana是一个针对ElasticSearch的开源分析及可视化平台,用来`搜索、查看交互存储`在ElasticSearch索引中的数据,使用Kibana可以通过各种`图表进行高级数据分析及展示`,Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板（dshboard）实时显示ElasticSearch查询动态，而设置Kibana非常简单，无需编码或者额外的基础架构几分钟内就可以完成Kibana安装并启动ElasticSearch索引监测。

官网:https://www.elastic.co/cn/kibana/

如:

![image-20211104183350622](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104183350622.png)

这是下载地址:https://www.elastic.co/cn/downloads/kibana

注:下载的时候要注意版本对应关系!也就是ES的版本和这里下载的版本要注意对应!而我们现在ES的版本是7.6.1

下载好以后解压到本地,注意文件夹比较大解压比较慢,因为这是一个标准的工程!

> 解压后的目录结构:

![image-20211104185007419](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104185007419.png)

> 启动测试

点击bin目录下的`kibana.bat`

![image-20211104185454994](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104185454994.png)

当然这里启动也是比较慢的,这是正常的!

然后访问IP:5601，kibana会自动去访问9200，也就是elasticsearch的端口号（当然elasticsearch这个时候必须启动着），然后就可以使用kibana了！

![image-20211104185618975](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104185618975.png)

> 这是都是英文的,所以我们进行汉化修改一下

我们在Kibana文件夹内发现i18n明显这是一个国际化文件,所以我们找到它的国际化配置;

中文包在**kibana\x-pack\plugins\translations\translations\zh-CN.json**

而我们只需要在配置文件`kibana.yml `中加入:

```yaml
i18n.locale: "zh-CN"
```

![image-20211104190104949](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104190104949.png)

![image-20211104190115854](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104190115854.png)

> 重启走一波

![image-20211104190259122](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104190259122.png)

可以发现已经正常的切换过来了!这样我们在使用的过程中也不用受到英文的困扰!



## 3.ES核心概念

在之前的学习过程中,我们已经知道一些概念和安装ES的过程!那么是否知道ES是如何去存储数据?数据结构又是什么?又是如何实现搜索?

这是提几个关键词，先有个印象：**集群、节点、索引、类型、文档、分片、映射**

> ES是面向文档

关系型数据库和elasticSearch客观的对比!

|  Relational DB   | Elasticsearch |
| :--------------: | :-----------: |
| 数据库(database) | 索引(indices) |
|    表(tables)    |     types     |
|     行(rows)     |   documents   |
|   列(columns)    |    fields     |

**理解**：首先我们创建一个数据库那么对应ES的就是叫索引，当然我们还是可以将索引当成数据库一样！然后数据库中创建一个表在es中对应的叫types，不过这个已经慢慢的被弃用了，7.x版本已经过时了8.x版本直接弃用了!行对应MYSQL中的具体的一行数据,对应ES中叫documents文档;当创建好文档以后里面就有具体的字段和规则,比如字段叫age或者name,那么对应ES中的fields

**注意**:在使用ES的过程中一切皆JSON,创建数据库等等都是使用JSON来做!

**流程**:elasticsearch(集群)中可以包含多个索引(数据库),每个索引中可以包含多个类型(表),每个类型下又包含多个文档(行),每个文档中又包含多个字段(列)



**物理设计**:

- es在后台把每个`索引划分成多个分片`,每个分片可以在集群中的不同服务器间迁移

- 一个人也是一个集群!默认的集群名称就是elasticSearch,所以说它不存在单个的,一开启就是被分片了,只是说你搭建了一个而已!什么叫分布式?启动就是集群!

  ![image-20211104193330119](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104193330119.png)

**逻辑设计**:

- 一个索引类型中,包含多个文档,比如说文档1,文档2。当我们索引一篇文章的时候可以通过这样的一个顺序找到它：**索引》类型》文档ID**，通过这个组合我们就能快速索引到某个具体的文档
- 注意：ID不必是整数，而实际上它是个字符串！

### 3.1 各节点介绍(面)

> 文档

我们知道ElasticSearch是面向文档的,那么也就意味着`索引和搜索数据的最小单位是文档`!

而elasticsearch中文档有几个重要属性:

- **自我包含**:一篇文档同时包含字段和对应的值,也就是同时包含 key:value!
- **可以是层次性的**:一个文档中包含自文档,那么复杂的逻辑实体就是这么来的!
- **灵活的结构**:文档不依赖预先定义的模式,我们知道关系型数据库中要提前定义字段才能使用,而在es中对于字段是非常灵活的,有时候我们可以忽略该字段或者动态的添加一个新的字段

尽管我们可以随意的新增或者忽略某个字段,但是每个字段的类型非常重要,比如一个年龄字段类型可以是字符也可以是整型。因为es会保存字段和类型之间的映射及其他的设置。这种映射具体到每个映射的每种类型，这也是为什么在es中类型有时候也称为映射类型！

```
理解:就是我们的一条条数据,就好比是MySQL中的一行一条数据
例如:
	user表
1	zhangsan 18
2	miaowei	 3
说白了以后我们存文档就是存入一条记录进去们,比如1号用户、2号用户等等；
```



> 类型

类型是文档的逻辑容器，就像关系型数据库一样表格是行的容器。类型中对于字段的定义称为映射，比如新增一个字段，那么es是怎么做的呢？es会自动的将新字段加入映射，但是这个字段的不确定它是什么类型，es就开会猜？如果这个值是18，那么es会认为它是整型。但是es也有可能猜不对，所以最安全的方式就是提前定义所需要的映射，这点跟关系型数据库殊途同归了：先定义好字段，然后再使用！

```
理解:这个很少使用了,比如说在Java中name字段类型为String类型而在es中就为text类型;
总之的意思就是说每一个字段都会有一个类型映射,而这个映射就是我们的基本类型,这个时候我们可以不设置它,它会自动去猜,当然我们也可以直接去设置
```

![image-20211104203634555](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104203634555.png)

> 索引

索引是映射类型的容器，es中的索引是一个非常大的`文档集合`。索引存储了映射类型的字段和其他设置。然后它们被存储到了各个分片上了，

```
理解:索引就是数据库,它是根本只有建立索引你才会文档等等这一些东西,
```

**物理设计：节点和分片 如何工作**

​	一个集群至少有一个节点，而一个节点就是一个elasticsearch进程，节点可以有多个索引默认的，如果你创建索引，那么索引将会有5个分片（primary shard,又称主分片）构成的,每一个主分片会有一个副本(replica shard,又称复制分片)

![image-20211104204152173](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104204152173.png)

```
理解:说白了我们存的东西它不止一个地方可以存,我们可以通过分片把他搭在不同的集群上!
```

![image-20211104195908940](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104195908940.png)

上图是一个有3节点的集群,可以看到主分片和对应的复制分片都不会在同一个节点内(p开头的是主分片,r开头的是复制分片),这样有利于某个节点挂掉了,数据也至于丢失。实际上：**一个分片是一个lucene索引,**一个包含倒排索引的文件目录,倒排索引的结构使得es在不扫描全部文档的情况下,就能告诉你哪些文档包含特定的关键字;

> 倒排索引(面)

es使用的是一种称为倒排索引的结构,**采用lucene倒排索引作为底层**。这种结构适用 于快速的全文搜索，一个索引由文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。

例如：现在有两个文档，每个文档包含以下内容：

```yaml
Study every day,good good up to forever # 文档1包含的内容
To forever,study every day, good good up # 文档2包含的内容
```

为了创建倒排索引,我们首先要将每个文档拆分成独立的词(或称之为词条或者tokens),然后创建一个包含所有不重复的词条的排序列表,然后列出每个词条出现在哪个文档:

|  term   | doc_1 | doc_2 |
| :-----: | :---: | :---: |
|  Study  |   √   |   X   |
|   To    |   X   |   X   |
|  every  |   √   |   √   |
| forever |   √   |   √   |
|   day   |   √   |   √   |
|  study  |   X   |   √   |
|  good   |   √   |   √   |
|  every  |   √   |   √   |
|   to    |   √   |   X   |
|   up    |   √   |   √   |

现在，我们试图搜索"to forever"，只需要查看包含每个词条的文档

|  term   | doc_1 | doc_2 |
| :-----: | :---: | :---: |
|   to    |   √   |   X   |
| forever |   √   |   √   |
|  total  |   2   |   1   |

两个文档都匹配,但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在这两个包含关键字的文档都将返回。第一个因为匹配率更高所以权重更高而后面那个权重就较低

再来看一个示例：比如我们通过博客标签来搜索博客文章，那么倒排索引列表就是这样的一个结构：

![image-20211104201358404](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104201358404.png)

如果要搜索含有python标签的文章,那相对于查找所有原始数据而言,查找倒排索引后的数据将会快得多。只需要查看标签这一栏，然后获取相关的文章ID即可！

> es的索引和lucene的索引对比

​	在es中索引(数据库)这个词被频繁使用->这就是术语的使用。在es中索引被分为多个分片，每份分片是一个lucene的索引。`所以一个es的索引(数据库)是由多个Lucene索引组成的`。别问为什么，谁让es使用lucene作为底层呢!如无特指:说起索引都是指es的索引。

接下来的一切操作都在kibana中的Dev Tools下的Console里完成基础操作!

```
理解:所以说倒排索引的效率是非常高的,它的底层是有Lucene倒排索引作为底层构建;它会将文档中所有不重复的词进行放入列表,然后通过输入关键词然后判断哪个列表包含了它.然后快速的计算出包含关键字的文档所在的列表的匹配程度与权重,而如果不存在关键词的文档是不会出现在倒排索引中的,因为完全过滤掉了无关的所有数据来提高效率;

每个索引要被分片,默认是5个分片,而每个分片说白了就是一个个倒排索引,那么这里也就是至少说有5个lucene的索引;
所以说Elasticsearch本身底层就是封装了Lucene,它只是利用了Lucene的功能进行了聚合;	
```

## 4.IK分词器插件

### 4.1 安装

**分词**:即把一段中文或者别的划分成一个个的关键字;

**引入之前的问题**:我们在搜索时候会把自己的信息进行分词,会把数据库中或者索引库中的数据进行分词,然后进行一个匹配操作,**默认的中文分词是将每个字看成一个词**,比如说"我爱祖国"就会被分为"我","爱","祖","国",这显然是不符合要求的,所以我们需要安装**中文分词器ik**来解决这个问题;

**内容**:IK提供了两个分词算法:ik_smart和ik_max_word,其中`ik_smart为最少切分`,`ik_max_word为最细粒度划分`!

> 安装

官网地址:https://github.com/medcl/elasticsearch-analysis-ik/

版本还是要一一对应,我这里es的版本是7.6.1

下载后解压到ElasticSearch根目录下的plugins目录中:

![image-20211104211914797](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104211914797.png)

重新启动 ElasticSearch 服务，在启动过程中，你可以在日志里看到正在加载"analysis-ik"插件的提示信息;

![image-20211104212428507](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104212428507.png)

服务启动后，在bin目录下在命令行运行`elasticsearch-plugin list` 命令来查看加载进来的插件，确认 ik 插件安装成功,注意

![image-20211104212546892](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211104212546892.png)

### 4.2 测试

使用kibana进行测试:

**注**:因为es提供RESTFul风格的接口所以测试用postman也可以,但是不推荐,因为专业的东西就要用专业的工具,各司其职!

1. 找到界面:

   ![image-20211107121815476](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107121815476.png)

2. 输入关于**ik_smart:最少切分**请求信息:

   切最少的

   ```elm
   GET _analyze   // get 请求类型, _analyze分词器
   // 中间就是具体的分词要求,
   {
     "analyzer": "ik_smart",	//选择哪个分词算法
     "text": "中国共产党"		//具体的分词文本
   }
   ```

   看具体的效果:

   ![image-20211107123201033](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107123201033.png)



​	再做点测试:

​	**理解**:将一段文本切分为一个个关键字,进行最少的切分,一旦切分为一个关键字以后就不会参与后面的切分了![image-20211107124208662](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107124208662.png)

2. 输入关于**ik_max_word:最小粒度切分**请求信息:

   穷尽词库的可能!

   ```elm
   GET _analyze
   {
     "analyzer": "ik_max_word",	//选择最小粒度划分
     "text": "中国共产党"
   }
   ```

   看效果:

   ![image-20211107123303022](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107123303022.png)

   最细粒度划分:就会把除了当前的组合拆开外就会把它所有可能的组合都拆开!

   再做点测试:

   **理解**:最小细粒度划分则是将整个文本进行存在可能的组合进行划分,注意:是细粒度的穷尽将整个文本组合进行划分!

   ![image-20211107124441478](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107124441478.png)

### 4.3 自定义关键字

先看一下:

![image-20211107145220577](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107145220577.png)

可以发现分词器认为这是两个词将其拆分开了,但是我认为这是个关键字不允许拆开,那么该如何处理呢?

找到es目录下的plugins中的ik所在的配置文件里,找到配置文件`IKAnalyer.cfg.xml`,我们将其打开:

![image-20211107145407023](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107145407023.png)

打开后就是这样:

![image-20211107145527564](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107145527564.png)

发现这里就是配置ik分词器的词典的位置,同时我们在外面可以发现有很多后缀为`.dic`的文件,我们随便点开看看:

![image-20211107145850543](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107145850543.png)

我们可以得出这就是ik分词器根据这些dic配置文件中的关键词进行分词,当然这点词典肯定是远远不够的;

> 我们配置属于自己的词典

1. 新建配置文件`miaowei.dic`,内容为:

   ```tex
   我爱缪威
   ```

2. 添加到配置文件:

   ![image-20211107150338101](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107150338101.png)

   **解释**:编写我们自己的配置文件然后注入到这里的扩展配置中即可,编写了我们自己的词典那么后续ik分词的时候就会首先查找我们的词典是否包含关键词,包含关键词符合条件那么就不会拆分了;

3. 重启ES,因为需要es重新加载插件!

   ![image-20211107150908952](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107150908952.png)

   可以发现这里已经开始加载我们自己的词典了!

4. 我们再次启动kibana进行测试

   ![image-20211107151156953](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107151156953.png)

   可以发现这里拆分的时候,由于我们设置了自定义词典所以这里就会默认认为`我爱缪威`就是一个关键字!是不允许拆分的!

   > 所以以后我们需要什么关键字就直接添加即可!

## 5.Rest风格说明

首先RESTFul是一种**软件架构风格**,而不是标准,它只是提供了一组**设计原则和约束条件**。它主要用于客户端和服务器交互类的软件，基于这个风格可以更简洁、更有层次、更易于实现缓存等机制。

基本的RESTFul命令说明:

| method |                     url地址                     |          描述          |
| :----: | :---------------------------------------------: | :--------------------: |
|  PUT   |     localhost:9200/索引名称/类型名称/文档id     | 创建文档（指定文档id） |
|  POST  |        localhost:9200/索引名称/类型名称         | 创建文档（随机文档id） |
|  POST  | localhost:9200/索引名称/类型名称/文档id/_update |        修改文档        |
|  POST  |    localhost:9200/索引名称/类型名称/_search     |      查询所有数据      |
| DELETE |     localhost:9200/索引名称/类型名称/文档id     |        删除文档        |
|  GET   |     localhost:9200/索引名称/类型名称/文档id     |   查询文档通过文档id   |

### 5.1 关于索引的基本操作

首先我们同时启动ES、kibana、head

1. 创建一个索引!

   在kibana的console中执行命令：

   ```elm
   // PUT命令  test1:索引名称 type1:类型名称 1:文档id
   PUT test1/type1/1    
   {
     "name":"缪威",  //属性
     "age":3		//属性
   }
   理解:这里相当于创建了数据库为test1,然后在库中创建了表为type1,然后插入了一行数据`name`为"缪威",`age`为"3"然后这一行的id为1  
   {...} 中间部分为请求体
   ```

   返回:

   ```elm
   #! Deprecation: [types removal] Specifying types in document index requests is deprecated, use the typeless endpoints instead (/{index}/_doc/{id}, /{index}/_doc, or /{index}/_create/{id}).
   //翻译:	# !Deprecation:[移除类型]在文档索引请求中指定类型已被弃用，请改为使用无类型端点(/{index}/_doc/{id}， /{index}/_doc，或/{index}/_create/{id})。
   
   
   {
     "_index" : "test1",		//索引名称(数据库)
     "_type" : "type1",		//类型名称(表)
     "_id" : "1",				//文档id(行数据)
     "_version" : 1,			//当前这条数据没有被更新过
     "result" : "created",		//当前处于created创建状态
     "_shards" : {				//分片信息
       "total" : 2,
       "successful" : 1,
       "failed" : 0
     },
     "_seq_no" : 0,
     "_primary_term" : 1
   }
   ```

   我们在head插件中查看索引数据:

   ![image-20211107154910808](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107154910808.png)

   由此可以看见已经完成了自动创建索引!数据也成功的添加了;

2. 那么 name 这个字段用不用指定类型呢。毕竟我们关系型数据库 是需要指定类型的啊 !

   字符串类型
   [text](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html) 、 [keyword](https://www.elastic.co/guide/en/elasticsearch/reference/current/keyword.html)
   数值类型
   [long](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [integer](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [short](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [byte](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [double](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [float](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [half_float](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html), [scaled_float](https://www.elastic.co/guide/en/elasticsearch/reference/current/number.html)
   日期类型
   [date](https://www.elastic.co/guide/en/elasticsearch/reference/current/date.html)
   te布尔值类型
   [boolean](https://www.elastic.co/guide/en/elasticsearch/reference/current/boolean.html)
   二进制类型
   [binary](https://www.elastic.co/guide/en/elasticsearch/reference/current/binary.html)
   等等......

3. 指定字段类型:

   ```elm
   //上面的那个是加数据,这里没有加文档只是创建规则
   // 创建索引test2
   PUT /test2
   {
   	"mappings":{	//所有的规则都是用mappings来设置,mappings表示映射规则
   		"properties":{		//属性设置
   			"name":{		//具体的属性名
   				"type":"text"		//type关键字 "text"基本数据类型-字符串
   			},
   			"age":{			//属性名称
   				"type:"long"		//type关键字 "long"基本数据类型-数值
   			},
   			"birthday":{	//属性名称
   				"type":"date"		//指定类型为日期类型
   			}
   		}
   	}
   }
   ```

4. 执行并查看索引

   ![image-20211107161106602](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107161106602.png)

   ![image-20211107160944530](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107160944530.png)

   我们这是创建了索引及其字段,但是我们并没有加入数据,所以这里是没有的!

> 我们已经了解过Put命令的使用,那么我们来使用Get命令使用方式

1. 获取索引信息

   ![image-20211107161425551](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107161425551.png)

**注意**:GET后面如果指定索引那么就会获取索引的信息,如果指定文档的id那么就会获取文档的信息,就是你指定到谁就是获取谁的信息

2. 查看默认的信息

   ![image-20211107162616946](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107162616946.png)

   注意:在后面就不会指定类型了,默认就是`_doc`类型的

   查看信息(`这里text类型它会默认加上一个关键字类型,也就是不可分割类型的!`):

   ![image-20211107163030814](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107163030814.png)

**如果自己的文档字段没有指定,那么es就会给我们默认配置字段类型!**

对比关系型数据库 ：

`PUT test1/type1/1 `： 索引test1相当于关系型数据库的库，类型type1就相当于表 ，1 代表数据中的主键 id
**这里需要补充的是**:在elastisearch5版本前，一个索引下可以创建多个类型，但是在elastisearch5后，一个索引只能对应一个类型;如果已经指定了类型然而由指定新的类型那么就会报错:

![image-20211107165850651](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107165850651.png)

所以后面慢慢会启用type类型,默认使用_doc,默认使用`_doc`可以指定多个索引

- id相当于关系型数据库的主键id若果不指定就会默认生成一个20位的uuid，属性相当关系型数据库的column(列)。
- 结果中的 result 则是操作类型，现在是 created ，表示第一次创建。如果再次点击执行该命令那么result 则会是 updated ，我们细心则会发现 _version 开始是1，现在你每点击一次就会增加一次。表示第几次更改。

> 扩展:通过命令elasticsearch索引情况:

1. `GET _cat/health`

   效果:

   ```basic
   # 获取数据库的健康值,其中yellow表示健康值为黄色
   1636274388 08:39:48 elasticsearch yellow 1 1 6 6 0 0 3 0 - 66.7%
   ```

   就是这个:

   ![image-20211107164210947](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107164210947.png)

2. `GET _cat/indices?v`

   效果:

   ![image-20211107164601019](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107164601019.png)

   我们可以通过这个命令来查看我们es相关的信息,比如我们可以看见health健康值,index为索引,docs.count为文档的数量

> 总的来说就是可以通过 **GET _cat/**命令来看一些es的默认的配置信息!

其实这个head可视化界面就是在不停的发起请求然后展示在我们页面上,所以可以说它的底层就是一个工具集!

![image-20211107164331445](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107164331445.png)



> 修改索引,提交还是使用PUT即可!然后覆盖!

1. 之前的办法:

   所谓的修改也就是在第一次的基础上进行覆盖!

   ![image-20211107172343594](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107172343594.png)

   但是有个缺点:如果在覆盖的时候如果少了一个字段那么同时在覆盖的时候之前的也会清除掉!

2. 现在的办法:

   通过`POST`进行修改文档,格式为:`localhost:9200/索引名称/类型名称/文档id/_update `

   ![image-20211107173143344](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107173143344.png)

   然后我们再查看是否被修改成功了

   ![image-20211107173227125](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107173227125.png)

   可以发现已经修改了,这样的好处就是效率很高,我们想要修改什么就可以直接修改什么

> 删除索引!

删除操作一律使用`DELETE`请求,格式为`localhost:9200/索引名称/类型名称/文档id `,根据你的请求来判断索引还是删除文档记录!

**注意**:按照格式指定,如果指定到索引那么就是删除索引,如果指定到类型名称那么就是删除类型名称,如果指定文档id,那么就是删除文档

这是之前的索引:

![image-20211107174528568](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107174528568.png)

现在删除索引:

![image-20211107174602289](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107174602289.png)

**为true表示删除成功!**

再来看索引还存不存在:

![image-20211107174625240](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107174625240.png)

### 5.2 基于文档的基本操作(重点)

#### 1.简单操作

> 基本操作

1. 添加数据:

   ![image-20211107210956433](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107210956433.png)

   多添加几条:

   ![image-20211107211559591](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107211559591.png)

2. 获取数据-GET

   ![image-20211107211725733](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107211725733.png)

3. 更新数据-PUT

   ![image-20211107212045587](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107212045587.png)

   缺点:如果put的时候内容不是完整的假设少了一个字段那么修改的时候就会将其原本的数据进行滞空,就好比是删除了一样!put如果不传递值,就会被覆盖!

4. 更新数据-POST

   ![image-20211107213720422](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107213720422.png)

   好处:灵活度更高,只需要修改想要修改的字段,其他字段不会滞空!

> 简单的搜索

1. 通过GET方式来搜索

   ```yaml
   GET miaowei/user/1 # 通过文档id来进行搜索
   ```

> 简单的条件查询,可以根据默认的映射规则,产生基本的查询!

![image-20211107215007835](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107215007835.png)

**注意:** _score 权重

![image-20211107220047229](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107220047229.png)

#### 2.复杂操作

> 复杂操作搜索 select(排序、分页、高亮、模糊查询、精准查询。。。)

1. 构建查询

   ![image-20211107220842901](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107220842901.png)

   增加两条数据再查询一次看看:

   ![image-20211107222308217](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107222308217.png)

   **说明**:relation:eq; 关系:匹配、相等就是equals的意思！ 然后下面查询出来的数据会根据“_score"的匹配度从高到底进行排序!

   > 查询指定的字段

   如果我们查询不需要那么多字段进行查询出来,我们仅仅需要查看name和desc两个属性,其他的不要怎么办呢?

   ![image-20211107223713843](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211107223713843.png)

   其实这个就好比是我们之前使用的是`select *`,而现在加入了就好比是`select name,desc`,就好理解了!

   我们使用Java操作es,所有的方法和对象就是这里面的key!这个key比如就是hits或者query等等等!

   > 排序!

   对查询出来的数据进行排序!

   ![image-20211111110105391](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111110105391.png)

> 分页查询!

![image-20211111110556170](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111110556170.png)

from:从第几个数据开始(数据下标还是从0开始);  size:返回多少条数据  

> 布尔值查询----多条件查询

> and--must

must相当于SQL中的`and`,所有的条件都要符合!

![image-20211111113020088](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111113020088.png)

这是构建查询的错误示例:

```java
GET miaowei/user/_search
{
  "query": {
    "match": {      //不能在一个匹配规则里面加多个字段查询
      "name": "张",
      "age":3
    }
  }
}
//返回
{
  "error" : {
    "root_cause" : [
      {
        "type" : "parsing_exception",
          //翻译为:查询不支持多个字段，找到的字段
        "reason" : "[match] query doesn't support multiple fields, found [name] and [age]",  
        "line" : 5,
        "col" : 13
      }
    ],
    "type" : "parsing_exception",
    "reason" : "[match] query doesn't support multiple fields, found [name] and [age]",
    "line" : 5,
    "col" : 13
  },
  "status" : 400
}
```

```java
GET miaowei/user/_search
{
  "query": {
    "match": {
      "name": "张"
    },
    "match": {     //这里就不能这样去写:因为:“重复键”匹配
      "age": "3"
    }
  }
}
```

**理解**:普通的构建查询只能有一个匹配规则也就是match,而且match里面只能有一个查询字段,这就相当于`select * from xx where 字段 ='查询文本'`

> or--should

上面用must表示and的意思,那么接下来就是表示or或的意思!

![image-20211111114123187](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111114123187.png)

​	等价于**or**的意思,就是两个条件满足其一就可以了!

> not--must_not

![image-20211111114527328](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111114527328.png)

不等于也就是**not**;在SQL中好比是`select * from xx where name!=张三 and age!=1`的结果并返回

not过滤一些条件,就是一种反向操作!

> 过滤器--filter

![image-20211111115345807](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111115345807.png)

- gt:大于>
- lt:小于<
- gte:大于等于 >=
- lte:小于等于 <=

可以进行多个条件进行过滤:

![image-20211111115910480](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111115910480.png)

> 短语搜索--只要含有这个标签满足一个就给我返回这个数据了。

 ![image-20211111120533775](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111120533775.png)

**注意**:

1. 多个条件使用空格隔开,主要满足其中一个结果就可以查出,就可以通过分值来进行基本的判断!

> 精确查询!

term查询是直接通过倒排索引指定的词条进行精确查找的!

![image-20211111120937646](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111120937646.png)

**关于分词**:

- term:是倒排索引之后直接查找精确的值
- match:会使用分词器进行解析!(先分析文档,然后再通过分析的文档进行查询!)

**两个类型**:

- keyword:是不会被分词器进行解析的!
- text:可以被分词器进行解析

比如我这里对两个类型进行测试:

```bash
# 1.新建索引,并设置字段规则,如果不设置会有默认的规则
PUT testdb
{
  "mappings": {
    "properties": {
      "name":{
        "type": "text"
      },
      "desc":{
        "type": "keyword"
      }
    }
  }
}
# 2.新建文档,添加数据进去
PUT testdb/_doc/1
{
  "name":"MiaoDaWei很name",
  "desc":"MiaoDaWei很desc"
}
PUT testdb/_doc/2
{
  "name":"MiaoDaWei很name",
  "desc":"MiaoDaWei很desc2"
}
# 构建查询
GET testdb/_doc/_search
{
  "query":{
    "match":{
      "desc":"MiaoDaWei很desc"
    }
  }
}
# 根据不同类型就会走不同的分词器去实现的，如果类型是keyword那么就会走最下面那个
GET _analyze
{
  "analyzer": "standard",
  "text":"MiaoDaWei很desc"   # 会被拆分成:miaodawei、很、desc
}
# 分词器查询,keyword类型就不会进行解析,而是认为这是一个关键字类型
GET _analyze
{
  "analyzer": "keyword",
  "text":"MiaoDaWei很desc"	# 不会进行拆分，本身就是关键字：MiaoDaWei很desc
}
```

这是构建查询的结果:

![image-20211111160637113](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111160637113.png)

​	![image-20211111161109849](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111161109849.png)

回头过来我们来看看term查询：

1. ![image-20211111161935037](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111161935037.png)

2. ![image-20211111162003439](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111162003439.png)

从结果上看:第一个使用字段`name`等于"很"那么他就会去匹配分词器中被拆分的,只要符合要求就会返回,而字段`desc`等于"MiaoDaWei很desc"不会拆分所以可以精确匹配到

**总结**：

```bash
	首先可以看出match跟term都是用于查询匹配,其中两者主要的区别就是查询的方式不同效率不同,当然我这里数据量太少了看不出而已!
- term:使用倒排索引精确查询
- match:解析文档,然后通过文档来进行查询
	首先我们的字段类型:keyword和text,前者是关键字类型后者是文本类型,在使用分词解析器的时候,分词就会进行一个拆分,当然keyword不会进行拆分而text就会被解析,然后使用term查询就会携带数据往倒排索引查询,倒排索引会将文档中不重复的词放入一个列表中然后通过输入参数快速计算出哪些列表包含了它并且计算相应的匹配分数,而如果文档中不存在该关键字的文档就会自动过滤到不会出现在倒排索引中,这样就会极大的提高了查询效率; 而match查询就会从真个文档进行解析查询,不管是否包含了这个关键字的文档都要去解析匹配,自然两者相比,term的效率自然就比match高!
-------------------------------------------------------------
总之:term就进行精确的解析,它会遇到分词器,而分词器有两种情况一个是会被分词器解析一个是不会被分词器进行解析。
查询资料:
- term:term是代表完全匹配，也就是精确查询，搜索前不会再对搜索词进行分词拆解。是不经过分词的，直接去倒排索引查找精确的值
- match:match进行搜索的时候，会先进行分词拆分，拆完后，再来匹配,match是经过分析(analyer)的，也就是说，文档是先被分析器处理了，根据不同的分析器，分析出的结果也会不同，在会根据分词 结果进行匹配。
拆解后(或者不拆分)再去搜索,然后遇到分词器,对已存在的数据进行分词拆分,然后就会一个个匹配,匹配成功就会返回!
```

> 多个值匹配精确查询-terms

文档:https://www.elastic.co/guide/cn/elasticsearch/guide/current/_finding_multiple_exact_values.html

![image-20211111165315010](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111165315010.png)

其实上述可以缩写修改为:

```bash
# 查询 精确查找多个值
GET testdb/_doc/_search
{
 "query": {
  "terms": {    # 使用terms精确查找多个值
   "t1": ["22", "33"]
 }
 }
}
```



> 高亮查询

![image-20211111171829991](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111171829991.png)

可以发现它会自动加上`<em></em>`标签来展示高亮,那么我们可不可以进行修改呢?就是不要这个高亮标签,我想换成红色高亮标签!

![image-20211111172428218](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111172428218.png)



> 说明:

注意:

​	elasticsearch 在第一个版本的开始 每个文档都储存在一个索引中，并分配一个 映射类型，映射类型用于表示被索引的文档或者实体的类型，这样带来了一些问题, 导致后来在 elasticsearch6.0.0 版本中一个文档只能包含一个映射类型，而在 7.0.0 中，映 射类型则将被弃用，到了 8.0.0 中则将完全被删除。
​	只要记得，一个索引下面只能创建一个类型就行了，其中各字段都具有唯一性，如果在创建映射的时候，如果没有指定文档类型，那么该索引的默认索引类型是 **_doc** ，不指定文档id则会内部帮我们生成一个id字符串

## 6.集成SpringBoot

### 6.1 文档查询

在学习如何集成使用Java来操作ES的时候,我们先从官方文档入手,这样如何去扩展去学习es的使用:

这是中文使用文档:https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

1. 找到es的官网指导文档:https://www.elastic.co/guide/index.html

2. 找到 Elasticsearch Clients（这个就是客户端api文档）

   ![image-20211111175839528](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111175839528.png)

3. 我们使用java rest风格api，大家可以更加自己的版本选择特定的other versions。

   ![image-20211111180149517](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111180149517.png)

4. rest又分为**high level(高级客户端)**和**low level(低级客户端)**，我们直接选择**high level**下面的 **Getting started**

   ![image-20211111180747251](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111180747251.png)

5. 向下阅读找到Maven依赖和基本配置！

   ![image-20211111181045246](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111181045246.png)

6.那么此时我们就找到了maven的配置,那么我们再点击旁边的**Initialzation**初始化设置: 
![image-20211111181438520](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111181438520.png)

可以发现这里可以绑定多个客户端,其实es本身就是一个集群,下面还有一个close表示关闭客户端

> Java REST Client 说明

Java REST Client有两种风格:

**Java Low Level REST Client**:用于Elasticsearch的官方低级客户端。它允许通过http与Elasticsearch集群通信。将请求编排和响应反编排留给用户自己处理。它兼容所有的Elasticsearch版本。（PS：学过WebService的话，对编排与反编排这个概念应该不陌生。可以理解为对请求参数的封装，以及对响应结果的解析）

**Java High Level REST Client **:用于Elasticsearch的官方高级客户端。它是基于低级客户端的，它提供很多API，并负责请求的编排与响应的反编排。（PS：就好比是，一个是传自己拼接好的字符串，并自己解析返回的结果；而另一个是传对象，返回的结果也已经封装好了，直接是对象，更加规范了参数的名称以及格式，更加面对对象一点）

（PS：所谓低级与高级，我觉得一个很形象的比喻是，面向过程编程与面向对象编程）

网上很多教程比较老旧，都是使用TransportClient操作的，在 Elasticsearch 7.0 中不建议使用TransportClient，并且在8.0中会完全删除TransportClient。因此，官方更建议我们用Java High Level REST Client，它执行HTTP请求，而不是序列号的Java请求。既然如此，这里我们就直接用高级了。

### 6.2 基本配置和源码分析

勾选es是在NoSQL去勾选

![image-20211111183359554](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111183359554.png)

> 配置项目基本依赖

```xml-dtd
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

这个要注意一个细节,因为这里没指定版本那么就有SpringBoot默认的版本,那么我们来看一下:

![image-20211111195042429](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111195042429.png)

​	发现这里版本是7.12.1,而我们本地es的版本是7.6.1版本不一致,这种不一致极容易导致有些时候启动或者调用的时候出现错误!

在启动的时候idea控制台也给出了警告信息:

```
2021-11-11 19:40:27.551  WARN 19224 --- [ main] o.s.d.elasticsearch.support.VersionInfo  : Version mismatch in between Elasticsearch Client and Cluster: 7.12.1 - 7.6.1
```

解决办法:

```xml-dtd
<properties>
  <java.version>1.8</java.version>
  <!-- 这里SpringBoot默认配置的版本不匹配，我们需要自己配置版本！ -->
  <elasticsearch.version>7.6.1</elasticsearch.version>
</properties>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

继续阅读文档到Initialization ，我们看到需要构建RestHighLevelClient对象；

```java
RestHighLevelClient client = new RestHighLevelClient(
    RestClient.builder(
        new HttpHost("localhost", 9200, "http"),
        new HttpHost("localhost", 9201, "http"))); // 构建客户端对象
// 操作....
// 高级客户端内部会创建低级客户端用于基于提供的builder执行请求。低级客户端维护一个连接池，并启动一些线程，因此当你用完以后应该关闭高级客户端，并且在内部它将会关闭低级客户端，以释放这
//些资源。关闭客户端可以使用close()方法：
client.close(); // 关闭
```

我们编写一个配置类，提供这个bean来进行操作

```java
@Configuration
//@Configuration注解里面有一个@Compoent,表示这将注入到Spring容器中去
public class ElasticSearchConfig {

    /**
     * "@Bean":就好比是之前学习spring中xml配置文件:<beans id="restHighLevelClient" class="RestHighLevelClient"></beans>
     *
     * @return {@link RestHighLevelClient}
     */
    @Bean
    public RestHighLevelClient restHighLevelClient() {
        //构建客户端对象,如果是集群那么就new多个HttpHost
        return new RestHighLevelClient(RestClient.builder(new HttpHost("127.0.0.1", 9200, "http")));
    }
}
```



> 源码解析:

只要是SpringBoot我们都去看看它的源码,看看它是怎么实现自动配置:

1. 找到`/spring-boot-autoconfigure/2.5.6/spring-boot-autoconfigure-2.5.6.jar!/META-INF/spring.factories:46`这一行,然后点开:

   ```java
   @Configuration(proxyBeanMethods = false)
   @ConditionalOnClass({ ElasticsearchRestTemplate.class })
   @AutoConfigureAfter({ ElasticsearchRestClientAutoConfiguration.class,
   		ReactiveElasticsearchRestClientAutoConfiguration.class })
   @Import({ ElasticsearchDataConfiguration.BaseConfiguration.class,
   		ElasticsearchDataConfiguration.RestClientConfiguration.class,
   		ElasticsearchDataConfiguration.ReactiveRestClientConfiguration.class })
   public class ElasticsearchDataAutoConfiguration {
   
   }
   ```

2. 这是关于ElasticSearch的源码都在这里:

   ![image-20211111212306860](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211111212306860.png)

3. 因为我们使用的Java REST的所以我们需要用下面的那个elasticsearch,然后我们点击下面的那个`ElasticsearchRestClientAutoConfiguration`看看:

   ```java
   @Configuration(proxyBeanMethods = false)
   @ConditionalOnClass(RestHighLevelClient.class)
   @ConditionalOnMissingBean(RestClient.class)
   @EnableConfigurationProperties(ElasticsearchRestClientProperties.class)
   @Import({ RestClientBuilderConfiguration.class, 
             RestHighLevelClientConfiguration.class, //Rest 高级客户端对象
   		  RestClientSnifferConfiguration.class }) 
   public class ElasticsearchRestClientAutoConfiguration {
   
   }
   ```

   这些都是源码里提供的一些对象,我们可以拿来即用

4. 我们点击`RestClientBuilderConfiguration`这个类里面看看:

   ```java
   @Bean
   RestClientBuilder elasticsearchRestClientBuilder(ElasticsearchRestClientProperties properties,
                                                    ObjectProvider<RestClientBuilderCustomizer> builderCustomizers) {
       HttpHost[] hosts = properties.getUris().stream().map(this::createHttpHost).toArray(HttpHost[]::new);
       RestClientBuilder builder = RestClient.builder(hosts); //这里构建一些基本的操作,比如这里端口号
       builder.setHttpClientConfigCallback((httpClientBuilder) -> {
           builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(httpClientBuilder));
           return httpClientBuilder;
       });
       builder.setRequestConfigCallback((requestConfigBuilder) -> {
           builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(requestConfigBuilder));
           return requestConfigBuilder;
       });
       builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(builder));
       return builder;
   }
   ```

   这个方法主要用于可以读取配置文件来构建你的客户端

5. 再往下走:

   ```java
   @Bean
   //我们使用的就是这个高级客户端,如果说你什么都不配置的话那么就默认的就是这个
   RestHighLevelClient elasticsearchRestHighLevelClient(RestClientBuilder restClientBuilder) {
       return new RestHighLevelClient(restClientBuilder);
   }
   ```

6. 我们点击`RestHighLevelClient(restClientBuilder)`默认构造器了哪些东西

   ```java
   public class RestHighLevelClient implements Closeable {
       private final RestClient client;
       private final NamedXContentRegistry registry;
       private final CheckedConsumer<RestClient, IOException> doClose;
       private final IndicesClient indicesClient; 
       private final ClusterClient clusterClient;
       private final IngestClient ingestClient;
       private final SnapshotClient snapshotClient;
       private final TasksClient tasksClient;
       private final XPackClient xPackClient;
       private final WatcherClient watcherClient;
       private final GraphClient graphClient;
       private final LicenseClient licenseClient;
       private final MigrationClient migrationClient;
       private final MachineLearningClient machineLearningClient;
       private final SecurityClient securityClient;
       private final IndexLifecycleClient ilmClient;
       private final RollupClient rollupClient;
       private final CcrClient ccrClient;
       private final TransformClient transformClient;
       private final EnrichClient enrichClient;
   ```

   可以发现这里有一些默认的操作,

7. 回过头来发现虽然我们这里导入了3个类,这三个类都是静态内部类,而核心类就只有一个:`ElasticsearchRestClientConfigurations`

   ```java
   /*
    * Copyright 2012-2021 the original author or authors.
    *
    * Licensed under the Apache License, Version 2.0 (the "License");
    * you may not use this file except in compliance with the License.
    * You may obtain a copy of the License at
    *
    *      https://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */
   
   package org.springframework.boot.autoconfigure.elasticsearch;
   
   import java.net.URI;
   ....
   import xxx
   
   /**
    * Elasticsearch rest client configurations.
    *
    * @author Stephane Nicoll
    */
   class ElasticsearchRestClientConfigurations {
   
   	@Configuration(proxyBeanMethods = false)
   	@ConditionalOnMissingBean(RestClientBuilder.class)
   	static class RestClientBuilderConfiguration {
   
   		@Bean
   		RestClientBuilderCustomizer defaultRestClientBuilderCustomizer(ElasticsearchRestClientProperties properties) {
   			return new DefaultRestClientBuilderCustomizer(properties);
   		}
   		//RestClientBuilder 第一个类
   		@Bean
   		RestClientBuilder elasticsearchRestClientBuilder(ElasticsearchRestClientProperties properties,
   				ObjectProvider<RestClientBuilderCustomizer> builderCustomizers) {
   			HttpHost[] hosts = properties.getUris().stream().map(this::createHttpHost).toArray(HttpHost[]::new);
   			RestClientBuilder builder = RestClient.builder(hosts);
   			builder.setHttpClientConfigCallback((httpClientBuilder) -> {
   				builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(httpClientBuilder));
   				return httpClientBuilder;
   			});
   			builder.setRequestConfigCallback((requestConfigBuilder) -> {
   				builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(requestConfigBuilder));
   				return requestConfigBuilder;
   			});
   			builderCustomizers.orderedStream().forEach((customizer) -> customizer.customize(builder));
   			return builder;
   		}
   
   		....
   
   	}
   
   	@Configuration(proxyBeanMethods = false)
   	@ConditionalOnMissingBean(RestHighLevelClient.class)
   	static class RestHighLevelClientConfiguration {
   
           //RestHighLevelClient 高级客户端,也是我们项目后面会用到的客户端
   		@Bean
   		RestHighLevelClient elasticsearchRestHighLevelClient(RestClientBuilder restClientBuilder) {
   			return new RestHighLevelClient(restClientBuilder);
   		}
   
   	}
   
   	@Configuration(proxyBeanMethods = false)
   	@ConditionalOnClass(Sniffer.class)
   	@ConditionalOnSingleCandidate(RestHighLevelClient.class)
   	static class RestClientSnifferConfiguration {
   
           //第三个类
   		@Bean
   		@ConditionalOnMissingBean
   		Sniffer elasticsearchSniffer(RestHighLevelClient client, ElasticsearchRestClientProperties properties) {
   			SnifferBuilder builder = Sniffer.builder(client.getLowLevelClient());
   			PropertyMapper map = PropertyMapper.get().alwaysApplyingWhenNonNull();
   			map.from(properties.getSniffer().getInterval()).asInt(Duration::toMillis)
   					.to(builder::setSniffIntervalMillis);
   			map.from(properties.getSniffer().getDelayAfterFailure()).asInt(Duration::toMillis)
   					.to(builder::setSniffAfterFailureDelayMillis);
   			return builder.build();
   		}
   
   	}
   	...
   
   }
   
   ```

### 6.3 具体的API测试

#### 1.索引操作

> 1.创建索引

```java
@SpringBootTest
class ApiDemoApplicationTests {
    //按类型进行查找,也就是 RestHighLevelClient这种类型去搜索,
    //类型如果出现多个就会搜索对应的名称,如果需要指定名称的话我们可以用@Qualifer来指定名称
    @Autowired
    private RestHighLevelClient restHighLevelClient;

    //测试索引的创建-----所有的请求都是用Request来创建
    @Test
    void testCreateIndex() throws IOException {
        //1.创建索引请求
        CreateIndexRequest indexRequest = new CreateIndexRequest("miao_index");
        //2.客户端执行创建请求 IndicesClient,   请求后返回响应 ,RequestOptions是请求参数啥的,我们用默认的就行
        CreateIndexResponse response = restHighLevelClient.indices().create(indexRequest, RequestOptions.DEFAULT);
        System.out.println("response = " + response);
    }
}
这是控制台响应输出的:response = org.elasticsearch.client.indices.CreateIndexResponse@c7a77dbc
```

然后我们看看head插件里是否已经创建好了:

![image-20211112084135024](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112084135024.png)

> 2.测试获取索引---索引是一个库,只能判断是否存在

```java
@Autowired
private RestHighLevelClient restHighLevelClient;

 @Test
    void testExistsIndex() throws IOException {
        GetIndexRequest miao_index = new GetIndexRequest("miao_index");
        boolean exists = restHighLevelClient.indices().exists(miao_index, RequestOptions.DEFAULT);
        System.out.println(exists);
    }
```

![image-20211112203619346](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112203619346.png)

> 3.删除索引

```java
@Autowired
private RestHighLevelClient restHighLevelClient;

/**
     * 测试删除索引
     *
     * @throws IOException ioexception
*/
    @Test
    void testDeleteIndex() throws IOException {
        DeleteIndexRequest miao_index = new DeleteIndexRequest("miao_index");
        //删除索引
        AcknowledgedResponse delete = restHighLevelClient.indices().delete(miao_index, RequestOptions.DEFAULT);
        //判断是否删除成功
        System.out.println(delete.isAcknowledged());
    }
```

![image-20211112204137701](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112204137701.png)

这个跟我们在图形化界面删除返回的是一摸一样:

![image-20211112204341773](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112204341773.png)

#### 2.文档操作

> 1.测试添加文档

```java
/**
     * 测试添加文档
     *
     * @throws IOException ioexception
     */
    @Test
    void testAddDocument() throws IOException{
        //创建对象
        User user = new User("缪威", 3);
        //创建索引请求
        IndexRequest indexRequest = new IndexRequest(MIAO_INDEX);

        //规则 put/miao_index/_doc/1
        indexRequest.id("1"); //文档id
        indexRequest.timeout(TimeValue.timeValueSeconds(1));
        indexRequest.timeout("1s");  //添加的最大超时时间

        //将我们的数据放入请求资源 转为json
        indexRequest.source(JSONObject.toJSONString(user), XContentType.JSON);

        //客户端发送请求,获取响应的结果
        IndexResponse index = restHighLevelClient.index(indexRequest, RequestOptions.DEFAULT);

        System.out.println(index.toString());
        System.out.println(index.status());//执行状态 created

    }
```

打印:

![image-20211112211340833](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112211340833.png)

> 2.测试获取文档,判断是否存在

```java
/**
     * 测试获取文档,判断是否存在
     *
     * @throws IOException ioexception
*/
    @Test
    void testIsExistsDocument() throws IOException{
        //命令:GET Index/_doc/1
        GetRequest getRequest = new GetRequest(MIAO_INDEX, "1");
        //不获取返回的 _source的上下文了
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        //排序字段
        getRequest.storedFields("_none_");

        //执行并返回是否存在
        boolean exists = restHighLevelClient.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println("exists = " + exists);

    }
```

打印:

![image-20211112212413211](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112212413211.png)

> 3.获取文档信息

```java
/**
     * 测试获取文档
     *
     * @throws IOException ioexception
*/
    @Test
    void testGetDocument() throws IOException{
        //命令:GET Index/_doc/1
        GetRequest getRequest = new GetRequest(MIAO_INDEX, "1");
        GetResponse documentFields = restHighLevelClient.get(getRequest, RequestOptions.DEFAULT);
        System.out.println(documentFields); //这里返回全部内容和命令是一样的!
        System.out.println(documentFields.getSource());
        System.out.println(documentFields.getVersion());
        System.out.println(documentFields.getType());
}
```

打印:

![image-20211112213314033](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112213314033.png)

> 4.更新文档信息

```java
 /**
     * 测试更新文档
     *
     * @throws IOException ioexception
*/
    @Test
    void testUpdateDocument() throws IOException{
        //创建一个更新请求
        UpdateRequest updateRequest = new UpdateRequest(MIAO_INDEX, "1");
        updateRequest.timeout("1s"); //设置最大超时时间

        User user = new User("缪威很菜", 19);
        //放入到更新请求中,设置为JSON文档类型
        updateRequest.doc(JSONObject.toJSONString(user), XContentType.JSON);

        UpdateResponse update = restHighLevelClient.update(updateRequest, RequestOptions.DEFAULT);
        System.out.println(update);
        System.out.println(update.status());

}
```

打印:

![image-20211112214234507](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112214234507.png)

> 5.删除文档

```java
/**
     * 测试删除请求
     *
     * @throws IOException ioexception
*/
    @Test
    void testDeleRequest() throws IOException{
        //创建一个删除请求
        DeleteRequest deleteRequest = new DeleteRequest(MIAO_INDEX, "1");
        //添加删除超时时间,过了时间就不删除了
        deleteRequest.timeout("1s");

        //发起删除请求
        DeleteResponse delete = restHighLevelClient.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(delete);
        System.out.println(delete.status());

}
```

输出:

![image-20211112215058012](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112215058012.png)

> 6.批量处理

```java
 /**
     * 批量插入数据
     *
     * @throws Exception 异常
     */
    @Test
    public void testBLukRequest() throws Exception{
        //在真实的项目开发中有一种可能就是从数据库中导入,那么就需要批量的插入
        //如果直接用for循环那么也是可以的,但是人家已经为我们提供好了API
        BulkRequest bulkRequest = new BulkRequest();
        bulkRequest.timeout(TimeValue.timeValueMinutes(1));

        ArrayList<User> userList = new ArrayList<>();
        userList.add(new User("缪威1", 1));
        userList.add(new User("缪威2", 2));
        userList.add(new User("缪威3", 3));
        userList.add(new User("缪威4", 4));
        userList.add(new User("缪威4", 4));
        userList.add(new User("缪威5", 5));
        userList.add(new User("缪威6", 6));
        userList.add(new User("缪威7", 7));

        //批处理请求
        for (int i = 0; i < userList.size(); i++) {
            //批量更新和批量删除就在这里修改对应的请求即可,
             bulkRequest.add(new IndexRequest(MIAO_INDEX)
                        .id("" + (i + 1)) //不写id就会为我们随机生成一个id
                        .source(JSONObject.toJSONString(userList.get(i)), XContentType.JSON));
        }

        //执行批量操作
        BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        System.out.println(bulk);
        System.out.println(bulk.hasFailures()); //是否失败,返回false表示成功,就是没有失败就是成功了
        System.out.println(bulk.status());
    }
```

输出:

![image-20211112220953182](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112220953182.png)

图形化界面:

![image-20211112221025474](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112221025474.png)

> 7.查询

```java
/**
     * 查询
     * SearchRequest 搜索请求
     * SearchSourceBuilder 条件构造
     * HighlightBuilder 构建高亮
     * TermQueryBuilder 精确查询
     * MatchAllQueryBuilder
     * 总之xxxQueryBuilder 对应着xxx功能
     *
     * @throws Exception 异常
     */
    @Test
    public void testSearch() throws Exception{
        //构建一个搜索请求
        SearchRequest searchRequest = new SearchRequest(MIAO_INDEX);
        //构建搜索条件,比如排序、高亮等等;所有的资源都是通过这个构建器进行构建的
        SearchSourceBuilder builder = new SearchSourceBuilder();
        //查询--查询条件,我们可以通过QueryBuilders工具类来实现
            //termQuery精确匹配
        TermQueryBuilder termQueryBuilder = QueryBuilders.termQuery("name", "miaowei");
//        MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();  匹配所有
        builder.query(termQueryBuilder);
        //设置最大查询时间
        builder.timeout(TimeValue.timeValueSeconds(60));
        //设置分页,这里我们可以不用设置参数,因为它自带了一些默认的配置
//        builder.from();
//        builder.size();

        //把构建的参数放在搜索请求里面
        searchRequest.source(builder);

        SearchResponse search = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(search.getHits()));
        System.out.println("========================================================");
        //第一个Hits是整个对象,第二个是具体的文档对象,就是里面有具体的参数信息
        for (SearchHit hit : search.getHits().getHits()) {
            System.out.println(hit.getSourceAsMap());
        }
    }
```

打印输出:

![image-20211112223628251](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211112223628251.png)

## 7.实战(京东搜索)

我们来最终模拟一下这个京东页面实现搜索关键字高亮:

![image-20211113080021578](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211113080021578.png)

### 7.1 爬虫

首先现在模拟数据那么肯定要有数据才可以对吧,一般是从数据库获取、消息队列中获取都可以成为数据源,但是这些都是需要我们去造数据,现在直接用一种简单的办法那就是直接去人家官网去拿:爬虫!

以京东为例顺便看到它的地址栏:

https://search.jd.com/Search?keyword=Java&enc=utf-8&pvid=9e638b455bbf47f2a751f5696a9a1546

从中可以得出这京东应该是另外的搜索服务器,然后用keyword关键字进行搜索!

> 爬取数据:(获取请求返回的页面信息,筛选出我们想要的数据就可以了!)---jsoup包

1. 导入相关依赖

   ```xml
   <!--JSON转换-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>fastjson</artifactId>
       <version>1.2.78</version>
   </dependency>
   <!--解析网页-->
   <dependency>
       <groupId>org.jsoup</groupId>
       <artifactId>jsoup</artifactId>
       <version>1.14.3</version>
   </dependency>
   <!--huttol工具-->
   <dependency>
       <groupId>cn.hutool</groupId>
       <artifactId>hutool-all</artifactId>
       <version>5.7.16</version>
   </dependency>
   <!--lombok-->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <optional>true</optional>
   </dependency>
   ```
   
2. 新建一个类用于装载解析后的数据

   ```java
   @Data
   @Accessors(chain = true)
   public class ParseSource {
       private String parseImg;
       private String parseContext;
       private String parsePrice;
   }
   ```

3. 工具类

   ```java
   package com.miao.workjddemo.util;
   
   import cn.hutool.core.text.CharSequenceUtil;
   import com.miao.workjddemo.pojo.ParseSource;
   import io.netty.util.CharsetUtil;
   import org.jsoup.Jsoup;
   import org.jsoup.nodes.Document;
   import org.jsoup.nodes.Element;
   import org.jsoup.select.Elements;
   import org.springframework.stereotype.Component;
   
   import java.io.IOException;
   import java.net.URL;
   import java.util.ArrayList;
   import java.util.List;
   
   /**
    * @program: startDemo
    * @description:
    * @author: MiaoWei
    * @create: 2021-11-15 20:01
    **/
   @Component
   public class HtmlParseUtil {
   
       /**
        * 解析jdbook
        *
        * @return {@link List}<{@link ParseSource}>
        */
       public List<ParseSource> parseJDBook(String keyword) {
           List<ParseSource> result = new ArrayList<>();
           if (CharSequenceUtil.isEmpty(keyword)) {
               throw new RuntimeException("keyword参数不能为空!");
           }
           //因为在获取请求的时候路径URL不允许需要带有空格
           keyword = CharSequenceUtil.cleanBlank(keyword);
   
           try {
               //获取请求
               //注意:需要联网 并且不能获取到AJAX, 这里enc是为了传输中文而不乱码
               String url = CharSequenceUtil.format("https://search.jd.com/Search?keyword={}&enc={}", keyword, CharsetUtil.UTF_8);
               //解析网页(Jsoup返回Document就是浏览器Document对象)
               Document document = Jsoup.parse(new URL(url), 3000);
               //所有能在js中使用的方法,这里都能进行使用
               Element goodsList = document.getElementById("J_goodsList");
               //获取所有的li元素
               Elements liElementsByTag = goodsList.getElementsByTag("li");
               //获取元素中的内容,这里el就是每一个li标签
               ParseSource source;
               for (Element el : liElementsByTag) {
                   source = new ParseSource();
                   //获取标题
                   String context = el.getElementsByClass("p-name").get(0).text();
                   //价格
                   String price = el.getElementsByClass("p-price").get(0).text();
                   //图片
                   String img = el.getElementsByTag("img").eq(0).attr("data-lazy-img");
                   source.setParseContext(context).setParsePrice(price).setParseImg(img);
                   result.add(source);
               }
           } catch (IOException e) {
               return result;
           }
           return result;
       }
   }
   ```

### 7.2 业务编写

