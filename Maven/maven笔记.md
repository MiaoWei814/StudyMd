# Maven笔记

## 1.概念:

**引入**:之前我们在构建一个项目或者框架的时候都会用到很多第三方的类库,一个项目JAR包的数量之多往往让我们瞠目结舌,并且JAR包之间的关系错综复杂,一个JAR包往往又会引用其他的JAR包,缺少任何一个JAR包都会导致项目编译失败,而我们在开发项目的时候往往需要更多的精力去记忆和存储JAR包,这是一件很麻烦很费精力的一件事,有的时候因为少一个JAR包都会报一些让人摸不着头脑的异常!而最重要的一件事就是JAR包的版本对项目也有影响,JAR包的版本过高过低也会造成某些数据不兼容的异常,这就很恼火!

**概念**:Maven是一个项目管理工具,它包含一个项目对象模型(Project Object Model),反映在配置中就是一个pom.xml文件;Maven是一组标准集合、一个项目的生命周期、一个依赖管理系统,另外还包括定义在项目生命周期阶段的插件(plugin)以及目标(goal);

当我们使用Maven的时候,通过一个自定义的项目对象模型,pom.xml来详细描述我们自己的项目

> 理解:Maven就是一款帮助程序员构建项目的工具,我们只需要告诉Maven需要哪些JAR包,它会帮助我们下载所有的JAR,极大提升开发效率

Maven中的有两大核心:

- **依赖管理**:对jar包的统一管理(Maven 提供了一个Maven的[中央仓库](https://mvnrepository.com/),当我们在项目中添加完依赖之后,Maven会自动去中央仓库下载相关的依赖,并且解决依赖的依赖问题)
- **项目构建**:对项目进行编译、测试、打包、部署、上传到[私服](这里私服指的是公司搭建的仓库)等！

**含义**:是Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。

**优势:**自动构建,jar包依赖管理,契约编程

画图:

![image-20210929183753736](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210929183753736.png)

## 2.常用命令

这里列举几个常用的命令:

|              常用命令              |   中文含义    |                       说明                       |
| :--------------------------------: | :-----------: | :----------------------------------------------: |
|             mvn clean              |     清理      |       这个命令可以用来清理已经编译好的文件       |
|            mvn compile             |     编译      |            将Java代码编译成Class文件             |
|              mvn test              |     测试      |        项目测试(执行test目录下的测试用例)        |
|            mvn package             |     打包      |    根据用户的设置,将项目打包成jar包或者war包     |
|            mvn install             |     安装      |          将生成的jar包放在本地仓库中去           |
|             mvn deploy             |     上传      |                将jar包上传到私服                 |
|               mvn -v               | 查询maven版本 | 本命令用于检查maven是否安装成功(cmd命令行查看)。 |
|           mvn source:jar           |   打包源码    |                只会将源码进行打包                |
|           mvn idea:idea            |   生成项目    |                   生成idea项目                   |
| mvn package -Dmaven.test.skip=true |   打包项目    |          maven打包不执行测试用例的命令           |

当然以上的命令必须进入到pom.xml的目录中进行执行:

比如:

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-dd0dbc65256b2f2d43fc8f2f774ac975_r.jpg" alt="img" style="zoom: 67%;" />

如果在IDEA的工具中则可以点击下就可以了:

![image-20210911204910153](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210911204910153.png)

画图:

![image-20210929185739437](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210929185739437.png)

## 3.仓库和坐标

### 3.1 仓库

**概念**:Maven仓库用来存放Maven管理的所有jar包,分为**本地仓库**和**远程仓库**;

1. 本地仓库:Maven本地的Jar包仓库;
2. 远程仓库:
   - 私服:搭建在局域网中,一般公司都会有私服,私服一般使用nexus来搭建!
   - 中央仓库:Maven官方提供的远程仓库!

**注意**:当项目编译时,Maven首先从本地仓库中寻找项目所需的Jar包,若本地仓库没有,再到Maven的中央仓库下载所需的Jar包

### 3.2 坐标

**概念**:在Maven中,坐标是Jar包的唯一标识,Maven通过坐标在仓库中找到项目所需的Jar包

比如:

```xml-dtd
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>4.3.4.RELEASE</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

**说明**:在以上代码中`groupId`和`artifactId`和`verson`构成了一个Jar包在仓库中的坐标

接下来解读每一个标签的含义:

|     标签     |                             说明                             |
| :----------: | :----------------------------------------------------------: |
| dependencies |   在dependencies标签中,添加项目需要的jar所对应的maven坐标    |
|  dependency  |                一个dependency标签表示一个坐标                |
|   groupld    | 团体、公司、组织结构等等的唯一标识，团体标识的约定是它以创建这个项目的组织名称和逆向域名（例如：org.javaboy）开头! |
|  artifactld  |         artifactld相当于在一个组织中项目的唯一标识符         |
|   version    |                一个项目的版本,指当前jar的版本                |
|    scope     |                         表示依赖范围                         |

这里举出依赖范围比较图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HdnRER0tLNHVZbTB2aWFoZWxEaWJPUXp0a0Qwd2FpYzd4ejF3UzAxeUk2UUd6eEZHSFpvUzZ3VW1nbk1VTndpYlpBbXlTMkJDdEprT2JMaWFCRWliSlFWaWNpYnBBLzY0MA)

比如下面这样呢:

![preview](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-acdadd4b177e6c587d41afe78f16527c_r.jpg)

## 4.依赖

**概念**:依赖就是我们在pom.xml中的坐标,而坐标就是我们本地仓库中jar包的位置,而通常一个Jar包会引用其他Jar包,这之间的引用就是依赖

**分类**:

1. 传递依赖:如果我们的项目引用了一个Jar包,而该Jar包又引用了其他Jar包,那么在默认情况下项目编译时,Maven会把直接引用和简洁引用的Jar包都下载到本地

2. 排除依赖:如果我们只想下载直接引用的Jar包,那么需要在pom.xml中做如下配置:(将需要排除的Jar包的坐标写在中):

   ```xml-dtd
   <exclusions>
      <exclusion>
         <groupId>cn.missbe.web.search</groupId>
         <artifactId>resource-search</artifactId>
         <packaging>pom</packaging>
         <version>1.0-SNAPSHOT</version>
      </exclusion>
   </exclusions>
   ```

### 4.1 依赖冲突

**概念**:若项目中多个Jar同时引用了相同的Jar时，会产生依赖冲突，但Maven采用了两种避免冲突的策略，因此在Maven中是不存在依赖冲突的!

**依赖产生冲突的原因**:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HdnRER0tLNHVZbTB2aWFoZWxEaWJPUXp0a0Qwd2FpYzd4ejgyY0VLMGljd2YwRDcwbmdaaWJnOUwzc1JpYXlHdTFtR01pY09Pb3VudXpqZFF6dEgxclhrZE9hRVEvNjQw)

**说明**:在图中,`a.jar`依赖`b.jar`;同时`a.jar`依赖`d.jar`,而这个时候a和b,d的关系是直接依赖的关系,a跟c的关系时间接依赖的关系!

**解决冲突**:

1. 短路优先:

   ```java
   本项目——>A.jar——>B.jar——>X.jar
   本项目——>C.jar——>X.jar
   // 若本项目引用了A.jar,A.jar又引用了X.jar,并且C.jar也引用了X.jar
   // 那么结论是:Maven只会引用依赖路径最短的Jar
   ```

2. 声明优先:

   ```java
   若引用路径长度相同时，在pom.xml中谁先被声明，就使用谁
   ```

## 5.聚合工程

**概念**:所谓的聚合工程，实际上也就是多模块项目。在一个比较大的互联网项目中，项目需要拆分成多个模块进行开发，比如订单模块、VIP 模块、支付模块、内容管理模块、CMS、CRM 等等。这种拆分方式，实际上更接近于微服务的思想。在一个模块中，还可以继续进行拆分，例如分成 dao、service、controller 等!

这个时候不是有一个"**分包**"不就可以吗?

​	小项目当然可以分包，大项目就没法分包了。比如，在一个大的电商系统中，有一个子模块叫做用户管理、还有一个子模块叫做订单管理，这两个子模块都涉及到用户，像这种情况，我们就需要将用户类单独提取出来，做成单独的模块，供其他模块调用。

> 理解:多个项目同时运行就称之为聚合

多模块项目展示:

```
|--javaboy-parent
      |-- javaboy-cms
      |-- javaboy-crm
      |-- javaboy-manger
           |-- javaboy-manager-model
           |-- javaboy-manager-dao
           |-- javaboy-manager-service
           |-- javaboy-manager-web
```

说明:以 javaboy-manger 为例，javaboy-manager 本身并不提供功能，它只负责管理他自己的子模块，而他的子模块每一个都无法独立运行，需要四个结合在一起，才可以运行。项目打包时，model、dao、service 都将打包成 jar，然后会自动将打包好的 jar复制到web中，再自动将 web 打包成 war 包

### 5.1 创建聚合工程

其实创建聚合工程就是在一个maven工程上创建另一个maven项目:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HdnRER0tLNHVZbTB2aWFoZWxEaWJPUXp0a0Qwd2FpYzd4ejgydDJWMUlRQ0g3T09zVlBpYWw2MkY5a2FVcDZOYTUwQ2Z5eW55MGlic2h2ZHhkcjNCbTlYMFBBLzY0MA)

点击之后我们就只需要填入当前Module的artifactId就可:(因为这里就默认会继承第一个项目)

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HdnRER0tLNHVZbTB2aWFoZWxEaWJPUXp0a0Qwd2FpYzd4emliQTdodGN2Nk9WWnc3TkpsTGVQQ2tWclZWZzY4WENFSVRvSDUxcGRDSjNyaWJRdXJCVlhEOXhnLzY0MA" alt="img" style="zoom: 67%;" />

avaboy-manager 创建完成后，此时，观察 javaboy-parent 的 pom.xml 文件，发现它自动加上了 packing 属性：

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9HdnRER0tLNHVZbTB2aWFoZWxEaWJPUXp0a0Qwd2FpYzd4ekxlT1lZbkhMWG51a25NWDRXeDZtNUdFT3ZhaWJnNE9pYWRldGNRUEJxVU11TzVIOUVpYU90RUxady82NDA)

**注意**:它的packaging属性值为`pom`,这表示它是一个聚合工程,同时,它还多了`modules`节点,指明了它自己的子模块!

而切换到子模块"javaboy-manager"的pom.xml中,可以发现其自身多了一个`parent`节点,这个parent节点描述了它的父模块的属性值:

```xml-dtd
<parent>
    <artifactId>javaboy-parent</artifactId>
    <groupId>org.javaboy</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
```

**注意**:这个parent不仅仅是一个简单的父子关系描述,它存在继承关系,一般我们可以在parent中统一定义依赖或者插件的版本号

### 5.2 继承

**概念**:在聚合多个项目时，如果这些被聚合的项目中需要引入相同的Jar，那么可以将这些Jar写入父pom中，各个子项目继承该pom即可。

**如何实现继承**:

父pom配置:将需要继承的Jar包的坐标放入标签即可

```xml
<dependencyManagement>
    <dependencies>
          <dependency>
            <groupId>cn.missbe.web.search</groupId>
            <artifactId>resource-search</artifactId>
            <packaging>pom</packaging>
            <version>1.0-SNAPSHOT</version>
          </dependency> 
    </dependencies>
</dependencyManagement>
```

子pom配置:

```xml
<dependencyManagement>
    <dependencies>
          <dependency>
            <groupId>cn.missbe.web.search</groupId>
            <artifactId>resource-search</artifactId>
            <packaging>pom</packaging>
            <version>1.0-SNAPSHOT</version>
          </dependency> 
    </dependencies>
</dependencyManagement>
```



## 6.注意点

我们在安装maven的时候,会默认会去`C:\Users\MiaoDaWei\.m2\`寻找settings.xml这个配置文件,所以

1. 我们不需要在安装maven的地方`F:\Maven\apache-maven-3.6.3`有settings.xml,因为会默认取m2下去找

2. 我们再配置文件中需要配置以下三个东西:

   ```xml-dtd
   <!--配置本地仓库位置-->
   <localRepository>F:\Maven\MavenRepository</localRepository>
   
   <!--配置maven的远程仓库地址-->
   <mirror>
   　　	<id>alimaven</id>
   　　	<mirrorOf>central</mirrorOf>
   　　	<name>aliyun maven</name>
   　　	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
   </mirror>
   
   <!--设置这个每次创建maven工程后的默认jdk为1.8-->
   <profile>  
           <id>jdk18</id>  
           <activation>  
               <activeByDefault>true</activeByDefault>  
               <jdk>1.8</jdk>  
           </activation>  
           <properties>  
               <maven.compiler.source>1.8</maven.compiler.source>  
               <maven.compiler.target>1.8</maven.compiler.target>  
               <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  
           </properties>   
   </profile> 
   ```

   

## 7.pom.xml详解

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!--详解-->
    <!--这是父项目的坐标,如果项目中没有规定某个元素的值,那么父项目中的对应值即为项目的默认值,也就是没写那么默认取父项目的对应值
      坐标包括groupID、artifactID、version-->
    <parent>
        <!--被继承的父项目的构件标识符-->
        <artifactId>com.example</artifactId>
        <!--被继承的父项目的全球唯一标识符-->
        <groupId>emp</groupId>
        <!--被继承的父项目的版本-->
        <version>1.0-SNAPSHOT</version>
    </parent>

    <!--分割线-->
    <!--这是声明项目描述符遵循哪一个POM模型版本,模型本身的版本很少改变,虽然如此但它仍然是必不可少的,这是为了当Maven引入了新的特性或者其他模型变更的时候,确保稳定性-->
    <modelVersion>4.0.0</modelVersion>

    <!--分割线-->
    <!--项目的全球唯一标识符,通常使用全限定的包名区分该项目和其他项目,并且构建时生成的路径也是由此生成,如cn.hutool生成的相对路径为:cn/hutool-->
    <groupId>cn.hutool</groupId>
    <!--构件的标识符,它和groupID一起搭配来进行表示唯一标识的一个构件,换句话说:你不能有两个不同的项目拥有同样的artifactID和groupID;在某个特定的
       groupID下,artifactID也是必须是唯一的;构件是项目产生的或使用的一个东西,Maven为项目产生的构件包括:JARs,源码,二进制发布和WARs等等-->
    <artifactId>hutool-all</artifactId>
    <!--项目当前版本,格式为:主版本.次版本.增量版本-限定版本号  SNAPSHOT: -->
    <version>5.7.11-SNAPSHOT</version>
    <!--项目产生的构件类型,例如:jar、war、ear、pom。插件可以创建他们自己的构件类型,所以前面列的不是全部构件类型,可理解为打包类型-->
    <packaging>war</packaging>
    <!--项目的名称,Maven产生的文档用 -->
    <name>javaBoy</name>
    <!--项目主页的URL,Maven产生的文档用-->
    <url>http://www.baidu.com</url>
    <!--项目的详细描述,Maven产生的文档用。当这个元素能够用HTML格式描述的时（例如：CDATA中的文本会被解析器忽略，就可以包含HTML标签）；这里
        不鼓励使用纯文本描述，如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档-->
    <description>A maven project to study maven.</description>

    <!--分割线-->
    <!--描述了这个项目构建环境中的前提条件-->
    <prerequisites>
        <!--构建该项目或使用该插件所需要的Maven的最低版本-->
        <maven>4.0</maven>
    </prerequisites>

    <!--分割线-->
    <!--构建项目需要的信息-->
    <build>
        <!--该元素设置了项目源码目录,当构建项目的时候,构建系统会编译目录里的源码,该路径是相对于pom.xml的相对路径-->
        <sourceDirectory>src</sourceDirectory>
        <!--该元素设置了项目脚本源码目录,该目录和源码目录不同:绝大多数情况下该目录下的内容会被拷贝到输出目录(因为脚本是被解释的,而不是被编译的)-->
        <scriptSourceDirectory>src</scriptSourceDirectory>
        <!--该元素设置了项目单元测试使用的源码目录,当测试项目的时候,构建系统会编译目录里的源码,该路径是相对于pom.xml的相对路径-->
        <testSourceDirectory>src</testSourceDirectory>
        <!--被编译过的应用程序class文件存放的目录-->
        <outputDirectory>src</outputDirectory>
        <!--被编译过的测试class文件存放的目录-->
        <testOutputDirectory>src</testOutputDirectory>
        <!--使用来自该项目的一系列构建扩展-->
        <extensions>
            <!--描述使用到的构建扩展-->
            <extension>
                <!--构建扩展的groupID-->
                <groupId>com.example</groupId>
                <!--构建扩展的artifactID-->
                <artifactId>emp</artifactId>
                <!--构建扩展的-->
                <version>1.0-SNAPSHOT</version>
            </extension>
        </extensions>
        <!--这个元素描述了该项目相关的所有资源路径列表,例如和项目有关的属性文件,这些资源被包含在最终的打包文件里-->
        <resources>
            <!--这个元素描述了项目有关或测试相关的所有资源路径-->
            <resource>
                <!--描述了资源的目标路径,该路径相对target/classes目录,举个例 子，如果你想资源在特定的包里(org.apache.maven.messages)，
                    你就必须该元素设置为org/apache/maven /messages。然而，如果你只是想把资源放到源码目录结构里，就不需要该配置-->
                <targetPath>src</targetPath>
                <!--是否使用参数值代替参数名,参数值取自properties元素或者文件里配置的属性-->
                <filtering>false</filtering>
                <!--描述存在资源的目录,该路径相对于POM路径-->
                <directory>src</directory>
                <!--包含的模式列表,例如**/*.xml-->
                <includes/>
                <!--排除的模式列表,例如**/*.xml-->
                <excludes/>
            </resource>
        </resources>
        <!--这个元素描述了单元测试相关的所有资源路径,例如和单元测试相关的属性文件-->
        <testResources>
            <!--这个元素描述了测试相关的所有资源路径-->
            <testResource>
                <targetPath>src</targetPath>
                <filtering>false</filtering>
                <directory>src</directory>
                <includes/>
                <excludes/>
            </testResource>
        </testResources>
        <!--构建产生的所有文件存放的目录-->
        <directory>src</directory>
        <!--产生的构件的文件名,默认值是${artifactId}-${version}-->
        <finalName>s</finalName>
        <!--当filtering开关打开时,使用到的过滤器属性文件列表-->
        <filters/>
        <!--子项目可以引用的默认插件信息,该插件配置项直到被引用时才会解析或绑定到生命周期,给定插件的任何本地配置都会覆盖这里的配置-->
        <pluginManagement>
            <!--使用的插件列表-->
            <plugins>
                <!--plugin元素包含描述插件所需要的信息-->
                <plugin>
                    <!--插件在厂库里的groupId-->
                    <groupId>com.example</groupId>
                    <!--插件在仓库里的artifactId-->
                    <artifactId>emp</artifactId>
                    <!--被使用的插件的版本(或版本范围)-->
                    <version>1.0-SNAPSHOT</version>
                    <!--是否从该插件下载maven扩展(例如打包和类型处理器),由于性能原因,只有在真需要下载时,该元素才被设置成enabled-->
                    <extensions>false</extensions>
                    <!--在构建生命周期中执行一组目标的信息,每个目标可能有不同的配置-->
                    <executions>
                        <!--execution元素包含了插件执行需要的信息-->
                        <execution>
                            <!--执行目标的标识符,用于表示构建过程中的目标,或者匹配继承过程中需要合并的执行目标-->
                            <id>w</id>
                            <!--绑定了目标的构建生命周期阶段,如果省略,目标就会被绑定到源数据里配置的默认阶段-->
                            <phase>a</phase>
                            <!--配置的执行目标-->
                            <goals/>
                            <!--配置是否被传播到子POM-->
                            <inherited>w</inherited>
                            <!--作为DOM对象的配置-->
                            <configuration/>
                        </execution>
                    </executions>
                    <!--项目引入插件所需的额外依赖-->
                    <dependencies>
                        <dependency>
                            ...
                        </dependency>
                    </dependencies>
                    <!--任何配置是否被传播到子项目-->
                    <inherited>false</inherited>
                    <!--作为DOM对象的配置-->
                    <configuration/>
                </plugin>
            </plugins>
        </pluginManagement>
        <!--使用的插件列表-->
        <plugins>
            <!--单独的插件-->
            <plugin>
                <groupId>com.example</groupId>
                <artifactId>emp</artifactId>
                <version>1.0-SNAPSHOT</version>
                <extensions>false</extensions>
                <executions>
                    <execution>
                        <id>a</id>
                        <phase>package</phase>
                        <goals/>
                        <inherited>a</inherited>
                        <configuration/>
                    </execution>
                </executions>
                <!--额外依赖-->
                <dependencies>
                    <dependency>
                        ...
                    </dependency>
                </dependencies>
                <goals/>
                <inherited>w</inherited>
                <configuration/>
            </plugin>
        </plugins>
    </build>

    <!--分割线-->
    <!--模块(有时称作子项目)被构建成项目的一部分,列出每个模块元素是指向该模块的目录的相对路径-->
    <modules/>
    <!--发现依赖和扩展的远程仓库列表-->
    <repositories>
        <!--包含需要连接到远程仓库的信息-->
        <repository>
            <!--如何处理远程仓库里发布版本的下载-->
            <releases>
                <!--true或false表示该仓库是否下载某种类型构建(发布版,快照版)开启-->
                <enabled>false</enabled>
                <!--该元素指定更新发生的频率,Maven会比较本地POM和远程POM的时间戳,那么这里的选项是:always(一直),daily(默认,每日),interval:X(这里X是以分钟为单位的时间间隔),
                    或者never(从不)-->
                <updatePolicy>always</updatePolicy>
                <!--当Maven验证构件校验文件失败时该怎么做:ignore(忽略),fail(失败)或者warn(警告)-->
                <checksumPolicy>fail</checksumPolicy>
            </releases>
            <!--如何处理远程仓库里快照版本的下载,有了releases和snapshots这两组配置,POM就可以在每个单独的仓库中,为每种类型的构件采取不同的策略;
                例如:可能有人会决定只为开发目的开启对快照版本下载的支持-->
            <snapshots>
                <enabled>false</enabled>
                <updatePolicy>always</updatePolicy>
                <checksumPolicy>fail</checksumPolicy>
            </snapshots>
            <!--远程仓库的唯一标识符,可以用来匹配在settings.xml文件里配置的远程仓库-->
            <id>jboss-community</id>
            <!--远程仓库的名称-->
            <name>b</name>
            <!--远程仓库URL-->
            <url>https://repository.jboss.org/nexus/content/repositories/public/</url>
            <!--用于定位和排序构件的仓库布局类型,可以是default(默认)或者Legacy(遗留),Maven 2为其仓库提供了一个默认的布局；然 而，Maven 1.x有一种不同的布局。
                我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。-->
            <layout>default</layout>
        </repository>
    </repositories>
    <!--发现插件的远程仓库列表,这些插件用于构建和报表-->
    <pluginRepositories>
        <!--包含需要连接到远程插件仓库的信息,参见repositories/repository元素-->
        <pluginRepository></pluginRepository>
    </pluginRepositories>

    <!--分隔符-->
    <!--该元素描述了项目相关的所有依赖,这些依赖组成了项目构建过程中的一个个环节,他们会自动从项目定义的仓库中下载-->
    <dependencies>
        <dependency>
            <!--依赖的groupID-->
            <groupId>com.example</groupId>
            <!--依赖的artifactID-->
            <artifactId>emp</artifactId>
            <!--依赖的版本号,在Maven2.0可以配置成版本号的范围-->
            <version>1.0-SNAPSHOT</version>
            <!--依赖类型,默认类型是Jar,它通常表示依赖的文件的扩展名,但也有例外,一个类型可以被映射成另外一个扩展名或分类器,类型经常和使用的打包方式对应,尽管这也有例外,一些类型的例子:
                jar,war,ejb-client和test-jar,如果设置extensious为true,就可以在plugin里定义新的类型,所以前面的类型的例子不完整-->
            <type>jar</type>
            <!--依赖的分类器,分类器可以区分属于同一个POM,但不同构建方式的构件,分类器名被附加到文件名的版本号后面,例如:如果你想要构建两个单独的构件成JAR,
                一个使用Java 1.4编译器,另一个使用Java 6编译器,你就可以使用分类器来生成两个单独的JAR构件-->
            <classifier/>
            <!--依赖范围,在项目发布过程中,帮助决定那些构件被包括进来
                - compile:默认范围,用于编译
                - provided:类似于编译,但支持你期待的jdk或者容器提供,类似于classpath
                - runtime:在执行时需要
                - test:用于test任务时使用
                - system:需要外在提供相应的元素,通过systemPath来取得
                - systemPath:仅用于范围为system ,提供相应的路径
                - optional:当项目自身被依赖时,标注依赖是否被传递,用于连续依赖时使用-->
            <scope>test</scope>
            <!--仅供system范围使用,注意:不鼓励使用这种元素,并且在新的版本中该元素可能会被覆盖掉,该元素为依赖规定了文件系统上的路径,需要绝对路径而不是相对路径,
                推荐使用属性匹配绝对路径,例如${java.home}-->
            <systemPath>studyMarkDown</systemPath>
            <!--当计算传递依赖时,从依赖构建列表里,列出被排除的依赖构建集,即告诉maven你只依赖指定的项目,不依赖项目的依赖,此元素主要用于解决版本冲突的问题-->
            <exclusions>
                <exclusion>
                    <groupId>se.javasoft.development</groupId>
                    <artifactId>modelparent</artifactId>
                </exclusion>
            </exclusions>
            <!--可选依赖,如果你在项目B中把C依赖声明为可选,你就需要在依赖于B的项目(例如项目A)中显式的引用对C的依赖,可选依赖阻断依赖的传递性-->
            <optional>true</optional>
        </dependency>
    </dependencies>
    <!--继承自该项目的所有子项目的默认依赖信息,这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖(必须描述groupID和artifact ID信息),
        如果group ID和artifact ID以外的一些信息没有描述,则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId></groupId>
                <artifactId></artifactId>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--项目分发信息，在执行mvn deploy后表示要发布的位置,有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。-->
    <distributionManagement>
        <!--部署项目产生的构件到远程仓库需要的信息-->
        <repository>
            <!--是分配给快照一个唯一的版本号（由时间戳和构建流水号）？还是每次都使用相同的版本号？参见repositories/repository元素-->
            <uniqueVersion>false</uniqueVersion>
            <id>central</id>
            <name>a</name>
            <url>https://repo1.maven.org/maven2/</url>
            <layout>default</layout>
        </repository>
        <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库-->
        <snapshotRepository>
            <uniqueVersion>false</uniqueVersion>
            <id>central</id>
            <name>a</name>
            <url>https://repo1.maven.org/maven2/</url>
            <layout>default</layout>
        </snapshotRepository>
        <!--部署项目的网站需要的信息-->
        <site>
            <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置-->
            <id>a</id>
            <!--部署位置的名称-->
            <name>a</name>
            <!--部署位置的URL，按protocol://hostname/path形式-->
            <url>scp://svn.baidu.com/banseon:/var/www/localhost/banseon-web</url>
        </site>
        <!--项目下载页面的URL。如果没有该元素，用户应该参考主页,使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）-->
        <downloadUrl>a</downloadUrl>
        <!--给出该构件在远程仓库的状态。不得在本地项目中设置该元素,因为这是工具自动更新的。有效的值有：none（默认），
            converted（仓库管理员从 Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确的和最终的）-->
        <status>partner</status>
    </distributionManagement>
    <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里activation元素的说明）。格式是<name>value</name>-->
    <properties/>
</project>
```

