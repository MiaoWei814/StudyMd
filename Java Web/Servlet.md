# Servlet详解

## 1.Servlet基础

### 1.1 概念

​	Servlet是SUN公司推出的一套规范(sun公司并没有去实现它),它的实现就是由各大服务器厂商（Tomcat）去实现；主要是规定了如何用JAVA来开发动态网站,也就是说，Java 可以用来开发网站后台，但是要遵循一定的标准。处理请求和发送响应的过程是由一种叫做Servlet的程序来完成的，并且Servlet是为了解决实现动态页面而衍生的东西。理解这个的前提是了解一些http协议的东西，并且知道B/S模式(浏览器/服务器)。

**作用：**

1. 获取前台数据，封装为对象
2. 调用对应的Service业务层[Controller层]
3. 响应前端数据

其实Servlet本身在Tomcat中是"非常被动"的一个角色,处理的事情也很简单（网络请求与响应），其实这并不是他的主要职责，它其实更偏向与业务代码，所谓的Request和Response是Tomcat传给它,用来处理请求和响应的工具,但它本身不处理这些

Servlet带给我们最大的作用就是能够**处理浏览器带来HTTP请求，并返回一个响应给浏览器，从而实现浏览器和服务器的交互**。

### 2.2 Tomcat

> 既然提到Servlet那么势必就会提Tomcat容器

Tomcat概念:	

Tomcat与Servlet的关系:

​	Tomcat是管理所有Servlet对象的创建、运行、销毁的容器。而Servlet对象也只有在Tomcat中才会使用。如果我们写一个Servlet，那么一定要记住把Servlet交给Tomcat去管理。Tomcat作为Servlet容器,负责处理客户请求把请求传送给Servlet,并将Servlet的响应传送回给客户,而Servlet是一种运行在支持JAVA语言的服务器上的组件,Servlet最常见的用途是扩展Java Web服务器功能,提供非常安全的,可移植的,易于使用的CGI替代品.

​	从http协议中的请求和响应可以得知，浏览器发出的请求是一个请求文本，而浏览器接收到的也应该是一个响应文本。但是在上面这个图中，并不知道是如何转变的，只知道浏览器发送过来的请求也就是request，我们响应回去的就用response。忽略了其中的细节，现在就来探究一下。

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/874710-20170214204632894-1786729693.png)

解读:

1. Tomcat将http请求文本接收并解析,然后封装成HttpServletRequest类型的request对象,所有的HTTP头数据读可以通过request对象调用对应的方法查询到
2. Tomcat同时会响应的信息封装为HttpServletResponse类型的response对象,通过设置response属性就可以控制要输出到浏览器的内容,然后将response交给tomcat,Tomcat就会将其变成响应文本的格式发送给浏览器

**注意:**Java Servlet API 是Servlet容器(tomcat)和servlet之间的接口，它定义了serlvet的各种方法，还定义了Servlet容器传送给Servlet的对象类，其中最重要的就是ServletRequest和ServletResponse。所以说我们在编写servlet时，需要实现Servlet接口，按照其规范进行操作。



什么是WEB服务器?

​	比如说我当前在杭州,你能否用自己的电脑访问我桌面上的一张图片?恐怕不行,我们习惯通过URL访问一个网站、下载一部电影了，一个资源如果没有URL映射，那么外界集合很难访问，而WEB服务器说穿了就是将某个主机上的资源映射为一个URL供外界访问。

什么是Servlet容器？

​	Servlet容器顾名思义就是里面存放着Servlet对象。我们为什么能通过WEB服务器映射的URL访问资源？

肯定需要写程序处理请求,主要3个过程:

1. 接收请求
2. 处理请求
3. 响应请求

任何一个应用程序,必然包括这三个步骤。其中接收请求和响应请求时共性功能，且没有差异性。比如我们常说的访问淘宝和京东，都是接收`taobao.com/brandNo=1`,就会响应给浏览器的都是JSON数据,于是,大家就把接收和响应两个步骤抽取成Web服务器:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-3d86f470ec1dc31bbe93d1df2c30fa47_r.jpg)

> 一般描述网络请求啥的,其实都是Tomcat的工作

但是处理请求的逻辑是不同的,所以就把它抽出来做成Servlet,交给程序员自己编写

当然,随着后期互联网发展,出现了三层架构,所以一些逻辑就从Servlet抽取出来,分担到Service和DAO

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-f41587429ebde63225029b0c235960c1_r.jpg)

所以这就迎来了JAVAWeb开发经典三层架构:代码分层、逻辑清晰、还解耦.

但是Servlet并不擅长往浏览器输出HTML页面,所以就出现了JSP,等Spring家族出现后,Servlet开始退居幕后,取而代之的是方便的SpringMVC。SpringMVC的核心组件`DispatcherServlet`其实本质就是一个Servlet,但它已经自立门户,在原来HttpServlet的基础上,又封装了一条逻辑,

**注意:**不要认为SpringMVC就是SpringMVC,跟Servlet没半毛钱关系,本质上就是Servlet

### 2.3 JavaWeb三大组件

引入:在我们之前的学习中启动Java程序我们都要从main方法开始进入,然后调用执行。哪怕后面的Junit本质上也是通过main方法进行执行代码，而我们不知道从什么时候开始，我们已经不再关心，甚至根本不知道到底谁调用了我写的这个程序，反正我写了一个类，甚至从来new过，它就跑起来了。。。我们把模糊的记忆往前推一推，没错，就是在学了Tomcat后！从Tomcat开始我们再也没写过main方法,以前一个main方法启动,程序见的调用井然有序,我们就知道程序所有流转过程.但是到了javaWeb后,Servlet/Filter/Listener一路下来,我们越学越想就晕了,没有main方法启动,没有new,写一个类然后在web.xml中配个标签,启动Tomcat容器,然后就很突兀自己运行起来了

其实这一切的一切,简单的来说就是"注入"和"回调",这样假如Tomcat里有个main方法:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-ce6e39bb02e3c6a2f4eb1e5afaa6e4e6_r.jpg)

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-14c18b69b5fb642f8d56698d2df20171_r.jpg)

![preview](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-d473a8662d758859e75c3f9afce9e982_r.jpg)

可以看出大部分工作容器或者框架都已经帮我们做了,而我们就只要编写实现xxx接口

### 2.4 Servlet开发

其实编写Servlet的话首先得把Tomcat部署起来们这里就演示如何部署tomcat,直接上代码:

1. 创建一个自定义类，**实现Serlvet接口**

   ```java
   package org.javaweb.servletdemo;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   /**
    * @program: YmsdLearn
    * @description: 初始练习servletDemo
    * @author: MiaoWei
    * @create: 2021-08-25 19:50
    */
   public class FirstDemo implements Servlet {
     @Override
     public void init(ServletConfig servletConfig) throws ServletException {
       //      ServletConfig config = new ServletConfig();
       System.out.println("Servlet进行初始化...");
     }
   
     @Override
     public ServletConfig getServletConfig() {
       return null;
     }
   
     @Override
     public void service(ServletRequest servletRequest, ServletResponse servletResponse)
         throws ServletException, IOException {
       servletResponse.getWriter().write("hello Servlet...");
     }
   
     @Override
     public String getServletInfo() {
       return null;
     }
   
     @Override
     public void destroy() {
       System.out.println("Servlet进行销毁结束...");
     }
   }
   
   ```

2. 光写这个还不行运行,得写一个web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <!--注册servlet,让Tomcat知道如何去管理-->
       <servlet>
           <!--这是servlet的名字(随意取)-->
           <servlet-name>firstDemo</servlet-name>
           <servlet-class>org.javaweb.servletdemo.FirstDemo</servlet-class>
           <load-on-startup>2</load-on-startup>
       </servlet>
       
   
       <!--进行映射-->
       <servlet-mapping>
           <servlet-name>firstDemo</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   </web-app>
   ```

3. 运行

   ![image-20210827101005874](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827101005874.png)

   这是浏览器:

   ![image-20210827101025339](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827101025339.png)

其中:我们发现有5个方法需要重写，有init【初始化】，destroy【销毁】,service【服务】,ServletConfig【Servlet配置】,getServletInfo【Serlvet信息】。

关于web.xml中执行顺序是什么?这里我找了一个比较好的一张图可以看出:

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDfcadZjGhMYejhxLrpaCvdZWKSBzAhEHVMibjygzgRibdZuibJJFy2nQjg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

继承HttpServlet编写Servlet程序

​	在上面我们实现Servlet接口,要实现5个方法,这样操作很麻烦!而HttpServlet类已经实现了Servlet接口的所有方法,编写Servlet时,只需要继承HttpServlet,重写你需要的方法即可,并且它在原有Servlet接口上添加了一些与Http协议处理反复噶,它比Servlet接口的功能更为强大:

```java
public class FirstDemo extends HttpServlet {
    @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().write("这是doGet");
  }

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    super.doPost(req, resp);
  }
}
```

结果:

![image-20210827105353165](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827105353165.png)

在我们日常开发中多数用doGet和doPost请求,因为前端请求到后端多数都是get或者post请求



**注意:**也可以不用配置web.xml文件,我们可以直接在Servlet上面直接写注解

@WebServlet("/路径");进行访问



## 2. Servlet生命周期

人的生命周期：出生 -- 取名上户(初始化) --  为人民服务 -- 死亡

来我们来看Servlet的生命周期:

```java
package org.javaweb.servletdemo;

import javax.servlet.*;
import java.io.IOException;

/**
 * @program: YmsdLearn
 * @description: 初始练习servletDemo
 * @author: MiaoWei
 * @create: 2021-08-25 19:50
 */
public class FirstDemo implements Servlet {
  @Override
  public void init(ServletConfig servletConfig) throws ServletException {
    //      ServletConfig config = new ServletConfig();
    System.out.println("Servlet进行初始化...");
  }

  @Override
  public ServletConfig getServletConfig() {
    return null;
  }

  @Override
  public void service(ServletRequest servletRequest, ServletResponse servletResponse)
      throws ServletException, IOException {
    //    servletResponse.getWriter().write("hello Servlet...");
    System.out.println("我被访问啦1");
  }

  @Override
  public String getServletInfo() {
    return null;
  }

  @Override
  public void destroy() {
    System.out.println("Servlet进行销毁结束...");
  }
}
```

当我们第一次启动服务器的时候:

![image-20210827101833741](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827101833741.png)

当我们开始访问的时候:

​	![image-20210827102047051](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827102047051.png)

我们第二次访问:

​	![image-20210827102115097](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827102115097.png)

当我们**关闭Tomcat服务器**的时候:

![image-20210827102303129](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827102303129.png)

所以得出Servlet生命周期可分为5个步骤:

1. **加载Servlet**:当Tomcat第一次访问Servlet的时候,Tomcat会负责创建Servlet的实例(根据web.xml中的路径进行反射创建)
2. **初始化**:当Servlet被实例化后,Tomcat会调用init()方法初始化这个对象
3. **处理服务**:当浏览器访问Servlet的时候,Servlet会调用service()方法处理请求
4. **销毁**:当Tomcat关闭时或者检测到Servlet要从Tomcat删除的时候会自动调用destroy()方法,**让该实例释放掉所占的资源**,一个Servlet如果长时间不被使用的话,也会被Tomcat自动销毁
5. **卸载**:当Servlet调用完destory方法后,等待垃圾回收。如果有需要再次使用这个Servlet，会重新调用init（）方法进行初始化操作

总结：只要访问Servlet，service()就会被调用,init()只有第一次访问Servlet的时候才会被调用,distory()只有在Tomcat关闭的时候才会被调用!



前面我们已经学过了Servlet的生命周期了，我们根据Servlet的生命周期画出Servlet的调用图加深理解:

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDQmr5ygzSLwFeTx4l60Xiahwrz4lp1VuykFVicSZU9OZ4NfPt2myaxZ2A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

解读:比如`http://localhost:8080/ymsdDemo/hello`,当我们在第三步的时候会解析URL的主机也就是`localhost:8080`,然后开始解析想访问的web应用也就是`ymsdDemo`,然后就是web资源`/hello`,就会去加载Servlet

注意:为什么第一次加载init,后续访问都不会加载init,这是因为对于Tomcat来说,Servlet只有一个(因为它是单例的,每次创建的Servlet实例都会缓存起来)

### 2.1 单例Servlet

之前就说过为什么浏览器多次进行Servlet请求,init方法都只会加载一次,这时我们就提过一个概念:**单例**

​	对于浏览器多次对Servlet的请求,一般请求下,服务器只创建一个Servlet对象,也就是说Servlet对象一旦创建,就会驻留在内存中,为后续的请求做服务,直到服务器关闭

![image-20210827110336863](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827110336863.png)

为什么每次访问请求对象和响应对象都是新的

​	对于每次访问请求,Servlet引擎都会创建一个新的HttpServletRequest请求对象和一个新的HttpServletResponse响应对象,然后将这个对象作为参数传递给调用的Servlet的service()方法,service方法再根据请求方式分别调用doxxx方法。

线程安全问题:

​	当多个用户访问Servlet的时候，**服务器会为每个用户创建一个线程**。**当多个用户并发访问Servlet共享资源的时候就会出现线程安全问题**。

原则:

1. 如果一个**变量需要多个用户共享**，则应当在访问该变量的时候，**加同步机制synchronized (对象){}**
2. 如果一个变量**不需要共享**，则**直接在 doGet() 或者 doPost()定义**.这样不会存在线程安全问题

**理解:**当用户在浏览器进行Servlet发起连接的时候,首先在去缓存中查看有对应的Servlet对象实例是否存在,如果没有那么就会创建一个实例然后继续放在缓存中,然后在创建的时候就会执行init()方法,和对应的service(),后续的请求那么都会去缓存中查看是否实例是否还在,还在的话就不会创建直接去调用service()方法,后续关闭服务器的时候,就会自动调用destory()方法



那假如我们不想在第一次访问的时候去加载创建实例,我们想在web服务器在开启的时候就自动跟着一起执行:

> load-on-startup

![image-20210827112941286](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827112941286.png)

这表示将Servlet实例化和初始化的时机提前到项目部署或服务器开启的时候，只需要加上<load-on-startup>标签即可

## 3.ServletConfig对象

通过此对象可以读取web.xml中配置的初始化参数。

常用API：

- String getServletName();   --获得Servlet在web.xml中配置的name值
- String getInitParameter(String name); -- 获得Servet的初始化参数的
- Enumeration getInitParameterNames(); -- 获得所有Servlet的初始化参数的名称

这个对象在创建完servlet对象的时候，接着创建servletConfig对象．而我们如何使用呢?可以直接使用`ServletConfig config = this.getServletConfig();`,

**理解:**这个就是在web.xml设置初始参数,然后在web服务器启动的加载它,而我们只需要使用即可

获取web.xml配置文件的信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--注册servlet,让Tomcat知道如何去管理-->
    <servlet>
        <!--这是servlet的名字(随意取)-->
        <servlet-name>firstDemo</servlet-name>
        <servlet-class>org.javaweb.servletdemo.FirstDemo</servlet-class>
        <init-param>
            <param-name>name</param-name>
            <param-value>MiaoDaWei</param-value>
        </init-param>
        <load-on-startup>2</load-on-startup>
    </servlet>
    

    <!--进行映射-->
    <servlet-mapping>
        <servlet-name>firstDemo</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

代码:

```java
@Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().write("这是doGet");
    ServletConfig config = this.getServletConfig();
    String initParameter = config.getInitParameter("name");
    System.out.println("initParameter = " + initParameter);
    String servletName = config.getServletName();
    System.out.println("servletName = " + servletName);
}
```

结果:

![image-20210827153241833](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827153241833.png)

在web.xml中设置初始参数为name,值为MiaoDaWei,

使用初始参数好处:能够让你的程序更加灵活【更换需求，更改配置文件web.xml即可，程序代码不用改】

**注意**:我们原本可以通过Demo4J进行自己解析的,但是Tomcat容器已经帮我们写好了,所以我们可以直接拿来用则可以,傻瓜式操作

> ServletConfig是针对单个Servlet进行的初始化参数,也就是说其他Servlet是访问不到的!

## 4.ServletContext对象

**概念:**当Tomcat启动的时候,就会创建一个ServletContext对象,它代表着**当前web站点**

作用:

1. ServletContext既然代表着当前web站点,那么所有Servlet都共享着一个ServletContext对象,所以Servlet之间可以通过ServletContext实现**通讯**;
2. ServletConfig获取的是配置的是单个Servlet的参数信息,ServletContext可以获取的是配置整个web站点的参数信息
3. 利用ServletContext读取web站点的资源文件
4. 实现Servlet的转发【用ServletContext转发不多，主要用request转发】

记住这里有个关键字"共享",那么也就是说所有的Servlet都会共享:

**应用场景:**在一个web项目中共享数据，管理web项目资源，为整个web配置公共信息等

常用的API:

```java
1. java.lang.String getContextPath()   --得到当前web应用的路径
2. java.lang.String getInitParameter(java.lang.String name)  --得到web应用的初始化参数
3. java.util.Enumeration getInitParameterNames()  --获取web应用所有的初始化名字
4. void setAttribute(java.lang.String name, java.lang.Object object) --域对象有关的方法
5. java.lang.Object getAttribute(java.lang.String name)  -- 获取参数
6. void removeAttribute(java.lang.String name)  -- 移除参数
7. RequestDispatcher getRequestDispatcher(java.lang.String path)   --转发（类似于重定向）
8. java.lang.String getRealPath(java.lang.String path)     --得到web应用的资源文件
9. java.io.InputStream getResourceAsStream(java.lang.String path)  --获取资源文件
```

### 4.1 获取web站点配置的信息

如果我想要让所有的Servlet都能够获取到连接数据库的信息，不可能在web.xml文件中每个Servlet中都配置一下，这样代码量太大了！并且会显得非常啰嗦冗余。

- web.xml文件支持**对整个站点进行配置参数信息**【所有Servlet都可以取到该参数信息】

  ```xml
  <context-param>
          <param-name>contextName</param-name>
          <param-value>哇塞</param-value>
  </context-param>
  ```

- 第一个Servlet:

  ```java
  @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp){
      ServletContext context = this.getServletContext();
      String initParameter = context.getInitParameter("contextName");
       System.out.println("one = " + initParameter); 
    }
  ```

- 第二个Servlet:

  ```java
  @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp){
      ServletContext context = this.getServletContext();
      String initParameter = context.getInitParameter("contextName");
       System.out.println("two = " + initParameter); 
    }
  ```

  结果:

  ![image-20210827162813060](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827162813060.png)



​	这样就实现了所有的Servlet都实现了"共享"机制

### 4.2 相互通讯

既然说相互通讯那么意思就是说每个Servlet之间能够相互通信,这里又无形之间提到"共享"这个概念,

实现Servlet之间通讯就要**用到ServletContext的setAttribute(String name,Object obj)方法**， 第一个参数是关键字，第二个参数是你要存储的对象

- 这是第一个Servlet:

  ```java
  @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)throws ServletException, IOException {
      ServletContext context = this.getServletContext();
        context.setAttribute("MyName", "你好");
    }
  ```

- 这是第二个Servlet:

  ```java
  @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        System.out.println(context.getAttribute("MyName"));
    }
  ```

- 结果:

  ![image-20210827163405939](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827163405939.png)

**注意:**这里要先执行第一个servlet先把参数set到容器中,然后切换到第二个Servlet里的时候获取的时候就能获取到,不然先执行第二个的话就会返回null

> 理解:这里给我的感觉就更想是将参数和对应的值放到了web站点中,也就是一个公共的地方,大家有需要就去拿

### 4.3 读取资源文件

现在这里在我的目录下有张照片,我现在读取进行相关操作:

![image-20210827180134156](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827180134156.png)

我现在在imgContext这个类里进行读取;

按照以前我们的读取方法一般采用`FileInputStream file= new FileInputStream("1.png");`,因为这跟图片是处于同一目录下所以可以这样读取,但是我错了,因为这里会报错:

![image-20210827180314460](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827180314460.png)

这是为什么呢?

​	我们以前读取文件的时候,如果程序和文件在同一包名,可以直接通过文件名称获取得到的!这之间的原因很简单的,以前我们写的程序都是通过JVM来运行的,而现在,我们是通过Tomcat来运行的,根据web的目录规范,Servlet编译后的class文件时存放在WEB-INF\classes文件夹中的:

![image-20210827180555991](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827180555991.png)

可以发现是没有的,所以这里解决办法是:(采用绝对路径的形式:从盘符开始)

```java
@Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
      FileInputStream file = null;
      try {
      file =
          new FileInputStream(
              "F:\\源码时代\\YmsdLearn\\src\\main\\java\\org\\javaweb\\servletdemo\\contextdemo\\1.png");
      } catch (FileNotFoundException e) {
          e.printStackTrace();
      }
      System.out.println("file = " + file);
  }
```

结果:

![image-20210827180810862](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827180810862.png)

虽然加上绝对路径下是可以找到的,但是如果我读取文件的时候都要写上绝对路径,那么这样也太不灵活了,试想一下,如果我将该**读取文件的模块移到其他的web站点上**，我的代码就又要修改了【因为web站点的名字不一样】。

​	我们可以通过ServletContext读取就可以避免修改代码的情况,因为ServletContext对象是根据当前web站点而生成的:

```java
 @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
      ServletContext context = this.getServletContext();
      InputStream stream = context.getResourceAsStream("/WEB-INF/classes/org/javaweb/servletdemo/contextdemo/1.png");
    System.out.println("stream = " + stream);
  }
```

结果:

![image-20210827181535214](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827181535214.png)

我的目录结构:

![image-20210827181552716](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827181552716.png)

**注意:**这里的1.png在上面目录没有而这里有,是因为我copy上去的,不然也会找不到的!

这里记录一下,我们可以把图片或者资源啥的放在web站点,那么我们就可以直接获取:

![image-20210827191137559](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827191137559.png)

代码:

```java
 // 获取资源路径
      String realPath = this.getServletContext().getRealPath("1.png");
    System.out.println("realPath = " + realPath);
```

结果:

![image-20210827191204512](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827191204512.png)

再记一次:

![image-20210828173935743](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828173935743.png)

![image-20210828173951379](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828173951379.png)

![image-20210828173958259](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828173958259.png)

## 5.Request与Response

> Tomcat收到客户端的http请求,会针对每一次请求,分别创建一个代表请求的request对象和代表响应的response对象

既然request对象代表http请求,那么我们获取浏览器提交过来的数据,找request对象即可,response对象代表http响应,那么我们向浏览器输出数据,找response即可!

### 5.1 HttpServletResponse对象

**概念:**http响应由状态行、实体内容、消息头、一个空行组成。而HttpServletResponse对象就封装了http响应的信息。

#### 5.1.1 getOutputStream

**作用:**使用HttpServletResponse对象的getOutStream()方法向浏览器输出数据

而使用getOutputStream()方法可以用print()也可以使用write(),那具体有什么区别?

不过在测试之前都是建立在获取了OutputStream流的基础上的

```java
 ServletOutputStream outputStream = response.getOutputStream();
```

首先使用`print()`方法

```java
 outputStream.print("aaa");
```

页面:

![image-20210827183852639](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827183852639.png)

没问题,那输入中文呢?

```
 outputStream.print("中国");
```

直接控制台就报错了:

![image-20210827183947211](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827183947211.png)

**为什么会出错呢?**

​	在IO中,outputStream是输出二进制数据的,print()方法接收了一个字符串,print()方法要把"中国"改成二进制数据,Tomca使用IOS 8859-1编码对其进行转换,而我们都知道这个编码对中文都是不支持的,所以才会出现异常!

我们再次使用`write()`方法再试试呢?

```java
 outputStream.write("aaa".getBytes());
```

页面:

![image-20210827184329826](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827184329826.png)



没问题,那么输入中文呢?

```java
 outputStream.write("中国".getBytes());
```

页面:

![image-20210827184457845](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827184457845.png)

发现也没有发现问题;

为什么使用write()方法能够正常向浏览器输出中文呢？

​	因为上面这句代码在转成`byte[]数组的时候默认查的是GB2312编码`,而浏览器是支持GB2312的,所以可以展示出来

但是我们假设一个情况那就是指定`UTF-8`试试呢?

```java
 outputStream.write("中国".getBytes(StandardCharsets.UTF_8));
```

页面:

![image-20210827184910188](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827184910188.png)

所以这也验证了为什么会,乱码,因为浏览器不支持

但是这也存在一个很重要的问题,那就是不可能每次我们都要去做这些操作,能不能来一个一劳永逸的办法:既然HTTP响应有对浏览器说明回送数据是什么类型的消息头，那么HttpservletrespoNse对象就应该有相对应的方法告诉浏览器回送的数据编码格式是什么:

```java
 //设置头信息，告诉浏览器我回送的数据编码是utf-8的
response.setHeader("Content-Type", "text/html;charset=UTF-8");

response.getOutputStream().write("中国".getBytes("UTF-8"));
```

页面:

![image-20210827185635178](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827185635178.png)

说明:这是因为设置消息头的形式在浏览器显示数据的时,自动把页面的编码格式置换成UTF-8,那么这个时候乱码的问题也就解决了!



#### 5.1.2 getWriter

它是Writer的子类,也就说明了只能向浏览器输出字符数据,不能输出二进制数据

在测试之前都是基于下面的代码进行的:

```java
PrintWriter writer = resp.getWriter();
```

直接上代码:

```java
writer.write("你好啊");
```

页面:

![image-20210827190053550](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210827190053550.png)

这里就出现了乱码,这是为什么呢?都说了只能输出字符数据啊!

​	这是因为Tomcat是外国人写的,而Tomcat默认的是ISO-8859-1,所以当我们输出中文数据的时候,Tomcat会依据ISO 8859-1码表给我们数据进行编码,但是由于它不支持中文,所以就出现页面的乱码情况

所以根据上面的情况我们可以设置输出编码:

```java
//原本是ISO 8859-1的编码，我设置成UTF-8
        response.setCharacterEncoding("UTF-8");
```

但是这样设置后,页面还是会乱码,因为我们只是在中文转换的时候把码表设置成UTF-8,但是浏览器默认只认GB2312,所以才会继续乱码

所以我们可以参考上面的解决办法都可以,但是我现在要说的是我要说另一种解决办法:

```java
   //设置浏览器用UTF-8编码显示数据
        response.setContentType("text/html;charset=UTF-8");
```



综上所诉:

既然Servlet有那么多方法解决乱码问题，是不是有一种是最简便的呢？没错！下面这个方法是最简便的，它**不仅设置浏览器用UTF-8显示数据，内部还把中文转码的码表设置成UTF-8了**，也就是说

```java
response.setContentType("text/html;charset=UTF-8");
```

#### 区别:

1. **getWriter()和getOutputStream()两个方法不能同时调用**。如果同时调用就会出现异常
2. Servlet程序向ServletOutputStream或PrintWriter对象中**写入的数据将被Servlet引擎从response里面获取，Servlet引擎将这些数据当作响应消息的正文，然后再与响应状态行和各响应头组合后输出到客户端**
3. Servlet的**serice()方法结束后【也就是doPost()或者doGet()结束后】**，Servlet引擎将检查getWriter或getOutputStream方法返回的输出流对象是否已经调用过close方法，如果没有，Servlet引擎将调用close方法关闭该输出流对象.

#### 5.1.3 文件下载

实现文件下载并难,既然浏览器发送所有的请求都是去找 Servlet的话，那么我就写一个 Servlet，当别人访问我这个 Servlet的时候，它们就可以下载我这个图片了

```java
@Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
       FileInputStream inputStream =
        new FileInputStream(
            "F:\\源码时代\\YmsdLearn\\src\\main\\java\\org\\javaweb\\servletdemo\\contextdemo\\中国.png");
      // 设置消息头,要告诉浏览器我要下载这个文件
    resp.setHeader("Content-Disposition", "attachment;filename="+ URLEncoder.encode("中国.png", StandardCharsets.UTF_8));
      
       // 将读取到的内容送给浏览器
    int len = 0;
    byte[] bytes = new byte[1024];
    ServletOutputStream outputStream = resp.getOutputStream();

    while ((len = inputStream.read(bytes)) != -1) {
      outputStream.write(bytes, 0, len);
    }
      
      //关闭资源
    outputStream.close();
    inputStream.close();
  }
```

这里要注意:

​	我使用的根路径的形式,所以这里关于图片位置的问题暂还未得到彻底解决,在设置消息头的时候,一是要表明给浏览器说我要下载文件,然后还要对下载后的文件名进行编码格式,然后保证下来的文件名为中文

#### 5.1.4 自动刷新

让规定的时间内让页面刷新,更新资源

- 让浏览器实现自动刷新，那肯定又是修改消息头了。

  ```java
   //每3秒自动刷新网页一次
          response.setHeader("Refresh", "3");
  ```

- 为了更好的看效果，我们加入时间值进去

  ```java
    response.getWriter().write("time is :" + System.currentTimeMillis());
  ```

- 每三秒时间值就会发生变化

  ![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2JGfRrpyFnic9icXic42hqhGmlZtjpcx9oiaBia92ibsvCqic8FibKVlovN3bSwWvtSjIwYDC16ofqOFiae0w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  我们可以实现在指定时间内跳转新的URL地址

  ```java
  //三秒后跳转到index.jsp页面去，web应用的映射路径我设置成/，url没有写上应用名
  response.setHeader("Refresh", "3;url='/index.jsp'");
  ```

  

### 5.2 HttpServletRequest

**概念:**HttpServletRequest对象代表客户端的请求,当客户端通过HTTP协议访问服务器时,HTTP请求头中所有信息都封装在这个对象时,开发人员通过这个对象的方法,可以获得客户这些信息.

> 说白了,我们请求的信息都是通过HttpServletRequest来进行获取的

常用方法:

获取浏览器信息:

```java
1. getRequestURL:返回客户端发出请求时的完整URL
2. getRequestURI:返回请求行中的资源名部分
3. getQueryString:方法返回请求行中的叁数部分。
4. getPathInfo :方法返回请求URL中的额外路径信息。额外路径信息是请求URL中的位于 Serve的路径之后和查询参数之前的内容，它以/开头
5. getRemoteAddr :方法返回发出请求的客户机的P地址
6. getRemoteHost :方法返回发出请求的客户机的完整主机名
7. getRemotePort :方法返回客户机所使用的网络端囗号
8. getLocalAddr: 方法返回WEB服务器的IP地址
9. getLocalName: 方法返回WEB服务器的主机名
```

获取请求头:

- getHeader方法
- getHeaders方法
- getHeaderNames方法

获取浏览器提交的数据

- getParameter方法
- getParameterValues（String name）方法
- getParameterNames方法
- getParameterMap方法

#### 5.2.1 防盗链

这里所谓的防盗链就是说我们在浏览器页面上点击需要跳转得到的资源别人直接复制地址就能获取,那么就做出不能让他这样去做:

```java
//获取到网页是从哪里来的
String referer request.getHeader（Referer）
//如果不是从我的首页来或者从地址栏直接访问的
if（referer =s null referer contains（"localhost：8080/zhongfucheng/index.js
//回到首页去
response.sendRedirect（"/zhongfucheng/index.isp"）
//能热行下面的语句，说明是从我的首页点击进来的，那没问题，照常显示
response.setContentType（text/html；charset=UTF-8"）
response.getwriter（），write（"路飞做了 XXXXXXXXXXXXXXXXXXXX");

```

## 6. 跳转

跳转就分为**重定向跳转**和**转发**,两者的功能都是页面跳转

### 6.1 重定向跳转

**概念:**比如说我们点击一个超链接,通知浏览器跳转到另外一个页面就叫重定向跳转

> 注意是通知浏览器去跳转,这很重要

我们来使用以下HttpServletResponse对象的重定向

```java
 //重定向到index.jsp页面
response.sendRedirect("/zhongfucheng/index.jsp");
```

然后我们在浏览器访问Servlet222

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2JGfRrpyFnic9icXic42hqhGmgsficyQhr6UtBicagryu4TGTiatUQzKqlzpPQE8fPImYmEIDiaNvhXWXYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后就会自动跳转到index.jsp页面,此时地址就会发生了变化

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2JGfRrpyFnic9icXic42hqhGmkanP4UsvHPia1hvlog4XRjn6qicRibQn8JibGFia3Bt0qc5RLAnOGjFNqxg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此时我们在开发者工具中可以看出http协议发生了什么?

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2JGfRrpyFnic9icXic42hqhGm7UNj3BLE0eaVYKQBehSHgQibvibibOzJIa2u1KjyZhzmDsjT0UCo7ialcg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

解读:从图中我们可以看出有两个状态码:一个是302,一个是200;其中302状态码在http协议中代表的是临时重定向,举个例子:我找纪律委员说：给我一份请假表，我要回家。纪律委员告诉我：我这里没有请假表，你去找辅导员吧。再看回我访问Sevlet222时：我找Servlet222，Servlet222告诉浏览器：我没有你想要的资源，你要的资源在index.jsp页面中，你自己去找吧。

现在我们知道原来302是代表临时跳转的意思,而重定向是通过302状态码和跳转地址栏实现的,于是我们在Java中**设置http消息头就可以实现重定向跳转**

```java
//设置状态码是302
response.setStatus(302);

//HttpServletResponse把常用的状态码封装成静态常量了，所以我们可以使用SC_MOVED_TEMPORARILY代表着302
response.setStatus(HttpServletResponse.SC_MOVED_TEMPORARILY);

//跳转的地址是index.jsp页面
 response.setHeader("Location", "/zhongfucheng/index.jsp");
```

其实sendRedirect()方法就是对setStatus()和setHeader()进行封装，原理就是setStatus()和setHeader()

### 6.2 转发

转发的功能也是实现页面跳转

代码:

```java
//获取到requestDispatcher对象，跳转到index.jsp
RequestDispatcher dispatcher = req.getRequestDispatcher("/index.jsp");
//调用requestDispatcher对象的forward()实现转发,传入request和response方法
dispatcher.forward(request,response);
```

而我们现在开始访问Servlet111

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDeibDxXkjNBS6icwxVv5fs6KgibSrdmMB1Wm54E8bcMkdFLS3oQ1sCXkcw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在进行重定向的时候通过sendRedirect()重定向可以在资源尾部添加参数提交数据给服务器。那么转发能不能提交数据给服务器呢？

​	可以,在说ServletContext的时候，曾经说过Servlet之间可以通过ServletContext实现通讯，ServletContext也能称之为域对象。而request也可以称之为域对象，只不过ServletContext的域是整个web应用，而request的域仅仅代表一次http请求

下面我们来使用request实现Servlet之间的通讯，Servlet111代码:

```java
//以username为关键字存zhongfucheng值
request.setAttribute( username,"zhongfuche");
//获取到requestDispatcher对象
RequestDispatcher requestDispatcher =request.getRequestDispatcher("/Servlet222");
//调用requestDispatcher对象的forward()实现转发,传入request和response方法
requestDispatcher.forward(request,response)
```

Servlet222代码

```java
//获取到存进request对象的值
String userName =(String) request.getAttribute("username");
//在浏览器输出该值
response.getWriter().write("I am:"+userName);
```

看效果:

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDdvaRUQlSESMiavuIpYaKJ4v6njlLWMcBYEH28xtYCicZeg2FrjiavA72Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

转发的时序图:

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDfWWwXNiaY8J5LppLeedrecBAhFa9B9AvNGY0hS6KCt674wqGBBTBw7g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这里注意的地方:

​	如果在调用 forward方法之前，在 Servlet程序中写入的部分内容已经被真正地传送到了客户端，forward方法将抛出Ⅲ legalState Exception异常。也就是说：不要在转发之前写数据给浏览器

比如说:

```java
Outputstream outputstream response.getoutputstream();
outputstream.write（"------------------------------------".getBytes());
//关闭流，确保让数据到测览器中
outputstream.close（）
//跳转
request.getRequestDispatcher("/Foot"). forward(request, response)
```

- 访问的时候，看到浏览器可以输出数据，Tomcat后台抛出了异常

![图片](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib2FPYiazSBurrVsbt1mkHFfDZtibskx97aCMiaA361ncQFEJxNJM0X1glSZevU1VmIIgBHKZHJZ3b5Pw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

解读:如果在调用 forward方法之前向 Servlet引擎的缓冲区中写入了内容，只要写入到缓冲区中的内容还没有被真正输出到客户端，forward方法就可以被正常执行，原来写入到输出缰冲区中的内容将被清空，但是，已写入到 Http servletrespOnse对象中的响应头宇段信息保持有效

### 6.3 区别:

1. 转发是发生在服务器的
   - 转发是由服务器进行跳转的,在转发的时候浏览器的地址栏里是没有发生变化的,也就是说浏览器是不知道该跳转的动作，转发是对浏览器透明的。也可以这样去理解:实现转发只是一次的http请求，一次转发中request和response对象都是同一个。这也解释了，为什么可以使用request作为域对象进行Servlet之间的通讯。
2. 重定向是发生在浏览器的
   - 重定向是由浏览器进行跳转的，进行重定向跳转的时候，浏览器的地址会发生变化的。曾经介绍过：实现重定向的原理是由response的状态码和Location头组合而实现的。这是由浏览器进行的页面跳转实现重定向会发出两个http请求，request域对象是无效的，因为它不是同一个request对象
3. 转发是服务器跳转只能去往当前web应用的资源
4. 重定向是浏览器跳转，可以去往任何的资源
5. 转发的request对象可以传递各种类型的数据，包括对象
6. 重定向只能传递字符串
7. 转发时：执行到跳转语句时就会立刻跳转
8. 重定向：整个页面执行完之后才执行跳转



转发和重定向如何选择?

​	根据上面说明了转发和重定向的区别也可以很容易概括出来。转发是带着转发前的请求的参数的。重定向是新的请求。

典型的应用场景：

1. 转发: 访问 Servlet 处理业务逻辑，然后 forward 到 jsp 显示处理结果，浏览器里 URL 不变
2. 重定向: 提交表单，处理成功后 redirect 到另一个 jsp，防止表单重复提交，浏览器里 URL 变了

区别:

![image-20210828201056437](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828201056437.png)

## 7.工作机制

https://gitee.com/miawei/pic-go-img/raw/master/imgs/2018120522281643.gif

这里我引入了别人做好的gif图,来表示Servlet的工作机制!

并且我插入了一张Servlet的工作流程:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180513204808318)

注意:在步骤3的时候会首先去缓存区查看是否存在Servlet对象,有的话就直接走第4的步骤

这是执行流程:

![image-20210828102351271](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828102351271.png)

**理解:**Tomcat在启动的时候,就会去加载Tomcat文件夹里加载config文件夹中的`server.xml`和`web.xml`,去找Context标签指定的项目的路径,还有我们项目中的web.xml进行解析,然后解析出其中的一个标签指定到项目路径下解析web.xml中Servlet的配置,然后在浏览器进行访问的时候,首先就会去端口号和主机确定是本地的tomcat服务器,然后根据后面的路径会去Context标签匹配然后跳转指定的项目下,然后解析web.xml,找到关于Servlet的配置,找到映射路径为`<url-pattern>/gp</url-pattern>`,然后根据其`<servlet-name>GetPostServlet</servlet-name>`找到对应的`<servlet-class>`的地址,然后Tomcat会使用反射`Class.forName("").newInstance()`创建实例,不过在这之前就会去缓存区查看是否存在Servlet实例,有的话就直接通过反射调用service方法,没有的话就会去创建实例,而创建实例就会去调用构造方法,而这个构造方法就如同去调用init()方法,在创建Servlet实例的时候还会去检查ServletConfig是否有初始化的参数,有的话就会一并加载!还有在调用service的时候,Tomcat会将请求资源http解析为request对象,并且还会创建一个response对象,然后传进去,经过一系列处理之后,在返回给前端的时候,会将准备写出的东西放在response里的缓冲区,然后service方法处理完后,就将缓冲区中的数据封装为响应给浏览器的响应头响应行之类的给浏览器进行解析

**注意**:我是这样去理解的我们常用的IDEA启动tomcat的时候,就好比自启动了一个tomcat的一个镜像(因为我们可以设置tomcat的端口号),然后我们需要配置项目访问路径就好比在tomcat的webapps里设置访问路径,然后通过浏览器路径path与解析后的web.xml中url进行匹配,然后继续执行后续的操作!

![image-20210828104259787](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828104259787.png)



**注意:**我们在部署项目的时候,可以拷贝项目到Tomcat中的webapps目录中去,在浏览器中访问的话默认是访问ROOT中,所以需要指定文件夹名然后进行访问,还有另一种就是可以直接设置默认项目,

![image-20210828113728402](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828113728402.png)

这个path为我们这个项目设置项目名,表示每次在浏览器访问都要在加上这个path,这个表示上下文路径

比如:

![image-20210828115017641](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210828115017641.png)

> 这个路径不能有特殊符号!且不能有中文

# JSP

> 这里因为本身不是很重要,所以我直接将课堂笔记复制到这里

WEB程序最重要的核心:通过URL地址资源访问

JSP:本质上就是一个Servlet

Servlet的钩子方法:

​	在我们extend HttpServlet的时候,会首先进入父类的Service方法,然后将请求参数的ServletRequest强制转为HttpServletRequest然后调用子类的service()方法,这就是钩子方法!

Servlet的三大职责

1. 接收数据
2. 处理数据[交给Service处理,只拿到结果]
3. 跳转页面

接下来就是关于JSP的笔记了

JSP的引入:

​	我们可以在Servlet的response里写html语法来进行输出,又写前端代码又写后端业务代码,这样会耦合性很强,代码回显得很冗余,分工不明确

所以提出了:
	Servlet【主要负责数据处理】
	JSP：像HTML一样去写页面【实际上也是Servlet，在JSP里写Java代码，所以在编译后成Servlet的Java代码】-》实际上是由Tomcat在将jsp中的标签写到write方法里面去,然后通过response输出在页面上
**结论**:Servet跟JSP页面其实就是Servlet跟Servlet之间共享数据
这样做的结论就是职责分离,各干各的!	

重定向:

​	在向一个Servlet请求的时候,在处理完业务后,准备页面跳转(重定向)到另一个Servlet,这时会首先将response返回这第一个请求,然后又会发起第二个请求到另一个Servlet,这时又要发起新的请求然后处理又返回,注意:在浏览器F12中如果是重定向后那么第一个请求状态码会变成302,表示是临时重定向,可以跨域,因为只是改变了地址栏的地址,两次请求又没有任何关系!

> 理解:重定向就好比超链接一样,在地址栏里跳转到另一个地址栏,会发起两次请求,地址栏会改变



转发:

​	在地址栏向一个Servlet请求的时候,这时准备页面跳转(转发)到另一个Servlet的时候,会将第一个请求request和response一并请求到另一个Servlet去了,这之间的操作就只有一个请求操作,可以共享一个请求对象(可以将请求的request的数据转发到另一个Servlet,这之间共享的都是一个request,而response如果在第一个Servlet写write,在进行转发的时候如果在第二个Servlet也进行了write,那么最终只会输出第二个Servlet里response里的数据),并且转发也是有要求不能是WEB-INF之外的转发路径,不能跨域(比如跳转百度)

> 理解:转发也是页面跳转,只不过这个地址栏是不会发生改变的,转发的话request共享,response转发的最后的Servlet才是往浏览器输出,之前的将会无效也就是说会清空,响应最后一个有效



## JSP的原理:

​	JSP的出现就是为了解决Servlet向浏览器输出HTML的冗余问题,让两者职责分离,

原理:

	JSP是一个程序,是需要服务器进行解析然后通过http协议进行访问,JSP中的标签最终都会以response.write方法进行输出,
本质上是一个Servlet,因为服务器最终会将它进行解析为Servlet对应的class文件,而在class文件里就是用response.write()进行输出的,而之所以说是Servlet还有一种就是继承,在class文件里可以看出继承的父类最终就是Servlet,所以可以证明JSP就是Servlet。
语法:
	<% 中间写java代码 代表局部变量 %>
	<%=  输出到页面上%>
	<%! 声明全局变量%>
指令:
1.page:在jsp头部就是用page指令的,作用就是告诉浏览器的一些编码格式之类的
	关于报错页面导航可以在web.xml中集中设置
2.include:引入的意思,引入其他文件
	<%@ include file="..."%>
	<jsp:include page="..."/> 
九大内置对象: 
  之所以说是内置对象是因为该jsp会被编译为Servlet,而这几个对象早已在内部声明好了,所以可以拿来用
	1.application:ServletContext  上下文对象
	2.config:ServletConfig Servlet的配置对象
	3.out:JspWriter 字符流:页面中的标签输出到浏览器
	4.pageContext:PageContext 页面的上下文对象,代表整个页面
	5.request:HttpServletRequest 请求对象
	6.response:HttpServletResponse 响应对象
	7.Session:HttpSession 	一次会话对象
	8.page:Object 代表当前Servlet
	9.exception:异常,页面发生异常要在page里设置isErrorPage="true"
作用域对象
	表示几个对象在一定范围内可以使用:共享数据
 	1.PageContext :当前jsp页面使用
	2.Request :比如转发,将request转发到其他Servlet都能共享
	3.Session :一次会话对象,浏览器跟服务器之间的会话状态,只要不关闭那么这一次会话就不会失效
	4.application :服务器不关闭那么这个对象都能共享
	作用域从小到大

# EL

EL表达式主要是向我们作用域取值

之前在jsp页面获取值:request.getAttribute("num"),现在就可以用${num}
	这个可以不用在<%%>里写,是可以在HTML的任何地方使用

出现的问题:
	1.如果作用域中有多个名字都是一样的,那么获取的话是按照作用域大小从小到大的顺序读取,
	2.如果非要去同名中指定的一个,那么就需要指定某个作用域中的名字,比如requestScope.xxx,就是加了一个Scope,		 
   	3.如果名字有多个".",那么就是要[]来括起来就可以了,比如${sessionScope["www.baidu.com"]}
	4.清除两边空格问题
${pageContext.request.contextPath} 获取在上下文路径 ,就是tomcat下配置Context标签的path
	5.EL获取到值后还能调方法${string.indexOf(1)}



# JSTL

在原本JSP中对于集合相关我们要用Java代码去处理判断,而此时就提供了JSTL标签库来进行处理相关操作,比如if,for等等操作,目的就是消除Java代码
1.引入 <%@ taglib uri="http:....core" prefix="c"%> 这里的prefix表示JSTL,作用于其他标签之间区分

