# ES笔记

## 1.概念

### 1.1 引入

![image-20211101194331216](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211101194331216.png)

ElasticSearch其实本身并不难理解,就比如我们看见我们百度搜索关键词,然后不到一秒中就出现各种以有关键词的标题文档就出来;按照以前我们如果遇到这种场景,我们可能就会使用SQL来查询,就好比如:`...like %%`来进行模糊查询,那么问题来了,如果是大数据的话就好比是上万条的那种那么这个速度就会明显得缓慢,并且这个时候我们还要去考虑一些SQL语句的优化和索引的优化,虽然说相对来说会快一点但是本质上还是达不到一个大数据的一个要求;

发音:伊莱克斯sir起

那么这个时候解决办法出来了:

> ElasticSearch-分布式全文搜索引擎

可以简单的理解为就两个字"**搜索**"!只要是用搜索那么就可以用它!比如一些应用场景如:淘宝、京东、github、百度等等,只要用到一些搜索都会使用到它!

> 以后只要用到搜索,就可以用到ES(打数据量的情况下使用!)

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
	后来Shay找到一份工作，这份工作处在高性能和内存数据网格的分布式环境中，因此高性能的、实时的、分布式的搜索引擎也是理所当然需要的。然后他决定重写Compass库使其成为一个独立的服务叫做ElasticSearch。
	第一个公开版本出现在2010年2月，在那之后ElasticSearch已经成为GitHub上最受欢迎的项目之一,代码贡献者超过300人,一家主营ElasticSearch的公司就此成立,他们一边提供商业支持一边开发新功能,不过ElasticSearch将永远开源且对所有人可用。
	Shay的妻子依旧等待着她的食谱搜索
```

**谁在使用：**

1. 维基百科，类似百度百科，全文检索，高亮，搜索推荐/2
2. The Guardian（国外新闻网站），类似搜狐新闻，用户行为日志（点击，浏览，收藏，评论）+社交网络数据（对某某新闻的相关看法），数据分析，给到每篇新闻文章的作者，让他知道他的文章的公众反馈（好，坏，热门，垃圾，鄙视，崇拜）
3. Stack Overflow（国外的程序异常讨论论坛），IT问题，程序的报错，提交上去，有人会跟你讨论和回答，全文检索，搜索相关问题和答案，程序报错了，就会将报错信息粘贴到里面去，搜索有没有对应的答案
4. GitHub（开源代码管理），搜索上千亿行代码
5. 电商网站，检索商品
6. 日志数据分析，logstash采集日志，ES进行复杂的数据分析，ELK技术，elasticsearch+logstash+kibana
7. 商品价格监控网站，用户设定某商品的价格阈值，当低于该阈值的时候，发送通知消息给用户，比如说订阅牙膏的监控，如果高露洁牙膏的家庭套装低于50块钱，就通知我，我就去买
8. BI系统，商业智能，Business Intelligence。比如说有个大型商场集团，BI，分析一下某某区域最近3年的用户消费金额的趋势以及用户群体的组成构成，产出相关的数张报表，xxx区最近3年每年消费
   金额呈现100%的增长，而且用户群体85%是高级白领，开一个新商场。ES执行数据分析和挖掘，Kibana进行数据可视化
9. 国内：站内搜索（电商，招聘，门户，等等），IT系统搜索（OA，CRM，ERP，等等），数据分析（ES热门的一个使用场景）

> 理解:看过历史的就知道ES是相对谷歌出生的,所以它是一个全文搜索引擎,然后它又是基于Hadoop大数据的所以它具有分布式的特点,而高扩展就已满足我们的"三高"问题,也就是说我们可以用它来做一些集群,做一些高可用是完全没问题的!好处:实时存储和检索,导致查询的效率非常高,本身扩展性好可以搭建几百台服务器作为集群来处理PB(大数据时代)级别的数据,

注意:ES的版本6.x跟7.x发生很大的概念如命令、文档、工具都是有差距的！