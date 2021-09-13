# SpringMVC笔记

## 1.Spring MVC概述

​	SpringMVC是一种基于Java的实现了Web MVC设计模式的请求驱动类性的轻量级Web框架,即使用MVC架构模式的思想将web层进行职责解耦!

**基于请求驱动**:指的就是使用请求-响应类型

**作用:**框架的目的就是帮助我们简化开发,Spring Web MVC也是简化我们日常Web开发的;在之前的java web体系中,通常一个接口对应一个Servlet,会导致我们开发出许多Servlet,而使用SpringMVC可以有效的简化这一步骤!

可以总结出以下几点:

1. SpringMVC是一个基于MVC模式的WEB/表现层框架,它解决了WEB开发中常见的问题:参数接收、文件上传/文件下载、表单验证、国际化等待！
2. Spring是一个轻量级的Java开发框架，为了解决企业应用开发的复杂性而创建的。SpringMVC以Spring框架为核心，为应用程序中的web层（表现层）提出一套优秀的解决方案；

> SpringMVC的功能就是之前的Servlet功能，可以理解为使用SpringMVC代替了Servlet

> SpringMVC是Spring的一个模块,是基于Spring的

### 1.1 MVC设计模式

**概念：**一般指MVC框架，M（model）指数据模型层,V(view)指视图层,C(Controller)指控制层;

**目的:**使用MVC就是将M和V的实现代码分离,使同一个程序可以有不同的表现形式,其中View的定义比较清晰,就是用户界面

比如:

- Model:在请求的过程中,用户的信息被封装在User实体类中,该实体类在Web项目中往往属于`数据模型层`(Model)
- View:在请求显示阶段,跳转的结果网页就属于`视图层`(View);
- Controller:在Web项目开发中,能够及时、正确的响应用户的请求时非常重要的。用户在网页上单击一个URL路径，这对WEB服务器来说，相当于用户发送了一个请求。而获取请求后如何解析用户的输入，并执行相关处理逻辑，最终跳转至正确的页面显示反馈结果，这些写功能往往就是`控制层`（Controller)完成的!

> 我们之前学习Servlet及JSP开发的时候,JavaBean相当于Model,Servlet相当于Controller,JSP相当于View

总结如下:

- 视图层(view):负责格式化数据并把它们呈现给用户,包括数据展示、用户交互、数据验证、界面设计等功能。
- 控制层（Controller）:负责接收并转发请求,对请求进行处理后,指定视图并响应结果发送给客户端.
- 数据模型层(Model):模型对象拥有最多的处理任务,是应用程序的主体部分,它负责数据逻辑(业务规则)的处理和实现数据操作(即在数据库中存取数据).

### 1.2 Web开发模式历程

​	在早期SUN公司推出JSP技术的同时,也推出了两种Web应用程序的开发模式:JSP+JavaBean和Servlet+JSP+JavaBean

1. ## JSP+JavaBean

   这种模式统一把显示层、控制层、数据层的操作全部交给JSP或者JavaBean来进行处理的,我们称之为"Model1"

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/7896890-7b3f9cd59394b017.png)

   **说明**:通过上图可以发现JSP从Http Request(请求)中获得所需的数据.并进行业务逻辑的处理,然后将结果通过Http Response(响应)返回给浏览器,从中可见,JSP+JavaBean模式在一定程度上实现了MVC,也即是JSP将控制层和视图合二为一,JavaBean为模型层!

   > JSP+JavaBean中JSP用于处理用户请求,JavaBean用于封装和处理数据

   **优点**:该模式只有视图和模型,一般把控制器的功能交给视图来实现,适合业务流程比较简单的Web程序!

   **弊端:**

   1. JSP和JavaBean之间严重耦合,Java代码和HTML代码也耦合在了一起

   2. 要求开发者不仅要掌握Java,还要有高超的前端水平

   3. 前端和后端相互依赖,前端需要等待后端完成,后端依赖前端完成,才能进行有效的测试开发

   4. 代码难以复用

总结:JSP+JavaBean模式中JSP身兼数职,既要负责视图层的数据显示,又要负责业务流程的控制,结构较为混乱,并且也不是我们所希望的松耦合架构模式,所以当业务流程复杂的时候并不推荐使用!

   

2. ## Servlet+JSP+JavaBean

由于上面的种种弊端,所以很快这种方式就被Servlet+JSP+JavaBean所替代了,早期的MVC模式,就像下面这图这样,一般我们称之为"Model2"

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/7896890-403a273b08fec826.png)

**解读**:首先用户的请求会到达Servlet,然后根据请求调用相应的Java Bean,并把所有的显示结果交给JSP去完成,这样的模式我们就称之为MVC模式

**各层介绍**:

- M代表 模型(Model):
  - 模型是什么呢?模型就是数据,就是Dao,bean
- V代表视图(View)
  - 视图是什么呢?就是网页,JSP,用来展示模型中的数据
- C代表控制器(Controller)
  - 控制器是什么呢?控制器的作用就是把不同的数据(Model),显示在不同的视图(view)上,Servlet扮演的就是这样的角色

**总结**:Servlet+JSP+JavaBean模式的结构清晰,是一个松耦合架构模式,一般情况下,建议使用这种模式

3. ## MVC的优缺点

   做任何事都有它的有利有弊

   **优点**:

   - 多视图共享一个模型,大大提高了代码的可重用性
   - MVC三个模块相互独立,松耦合架构
   - 控制器提高了应用程序的灵活性和可配置性
   - 有利于软件工程化管理

   > 总之,我们通过MVC设计模式最终可以打造出一个松耦合+高可重用性+高可适用性的完美架构

   **缺点:**

   1. 原理复杂
   2. 增加了系统结构和实现的复杂性
   3. 视图对模型数据的低效率访问

### 1.3 SpringMVC 小结

1. SpringMVC是Spring提供的一个基于MVC设计模式的轻量级Web开发框架,本质上相当于Servlet
2. SpringMVC是结构最清晰的Servlet+JSP+JavaBean的实现,是一个典型的教科书式的MVC框架,不像Struts等其他框架都是变种或者不是完全基于MVC系统的框架
3. SpringMVC角色划分清晰,分工明细,并且和Spring框架无缝结合;
4. SpringMVC框架采用松耦合可插拔的组件结构,具有高度可配置性,比起其他MVC框架具有扩展性和灵活性

> 在SpringMVC框架中,Controller替换Servlet来担负控制器的职责,用于接收请求,调用相应的Model进行处理,处理器完成业务处理后返回处理结果;Controller调用相应的View并对处理结果进行视图渲染,最终客户端得到响应信息;

此外,SpringMVC的注解驱动和对Rest风格的支持,也是它最具特色的功能,无论是在框架设计,还是可扩展性、灵活性等方面都全面超越了 Struts2 等 MVC 框架。并且由于 Spring MVC 本身就是 Spring 框架的一部分，所以可以说与 Spring 框架是无缝集成，性能方面具有先天的优越性，对于开发者来说，开发效率也高于其它的 Web 框架，在企业中的应用越来越广泛，成为主流的 MVC 框架。

SpringMVC的优点:

1. 清晰地角色划分:SprngMVC在Model、View和Controller方面提供了一个非常清晰的角色划分,这3个方面真正是各司其职!
2. 灵活的配置功能:可以把类当做Bean通过XML进行配置
3. 提供了大量的控制器接口和实现类,开发者可以把Spring提供的控制器实现类,也可以自己实现控制器接口
4. 真正做到与View层的实现无关,它不会强制开发者使用JSP,可以根据项目需求使用其他技术
5. 国际化支持
6. 面向接口编程
7. 与Spring框架无缝集成



## 2.第一个程序

直接先贴代码再解释每一步:

![image-20210905192035874](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905192035874.png)

​	Spring MVC是基于Servlet的,`DispatcherServlet`是整个Spring MVC框架的核心,主要负责截获请求并将其分派给相应的处理器处理;所以配置Spring MVC,首先得要定义DispatcherServlet,跟所有Servlet一样,用户必须在web.xml中进行配置

这是必须要修改的web.xml:

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--部署DispatcherServlet-->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--表示容器再启动时会立即加载Servlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <!--处理所有URL-->
        <url-pattern>/</url-pattern>  这里用/来进行拦截,但是对于静态资源文件是访问不了的,所以我们后续会在 spring-mvcxx.xml里设置<mvc:default-servlet-handler/>进行放行
    </servlet-mapping>
</web-app>
```

**注意**:Spring MVC初始化时将在应用程序的WEB-INF目录下查找配置文件,该配置文件的命名规则是"servletName-servlet.xml";

​	当然我们可以将Spring MVC的配置文件存放在应用程序目录中的任何地方,但需要使用servlet的`init-param`元素加载配置文件,通过`contextConfigLocation`参数来指定Spring MVC配置文件的位置,如:

​	![image-20210905193450682](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905193450682.png)

此处使用Spring资源路径的方式进行指定,即`classpath:springmvc-servlet.xml`,并且这里`contextConfigLocation`必须是这个哈!

> 在上面这段配置代码中,首先配置了一个名为"dispatcher"的Servlet,该Servlet是DispatcherServlet类型,而它就是我们Spring MVC的入口,然后并通过`<load-on-startup>1</load-on-startup>`配置标记容器在启动的时候就会解析加载此DispatcherServlet,即自动启动,然后通过servlet-mapping映射到"/",那么也就是说DispatcherServlet需要截获并处理该项目的所有URL请求,并交给由Spring MVC后台控制器来处理!



然后这是dispatcher-servlet.xml:

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="simpleUrlHandlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
        <property name="mappings">
            <props>
                <!--将/hello路径的请求交给id为 helloController的控制处理器-->
                <prop key="/hello">helloController</prop>
            </props>
        </property>
    </bean>
    <bean id="helloController" class="controller.HelloController"/>
</beans>
```

这里通过路径请求交给对应的控制器去处理,其实这里还有一种更简单的写法:

```xml-dtd
<bean id="/wa" class="controller.HelloController"/> // 得是以/开头才能匹配
```

**注意**:这个文件名的开头dispatcher与上面web.xml中的`<servlet-name>`元素配置的dispatcher对应,这是Spring MVC的映射配置文件(xxx-servlet.xml)



这是其中bean对应的class(HelloController):

```java
package controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

/**
 * @program: springmvcdemo
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-05 18:57
 **/
public class HelloController implements Controller {
    @Override
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView view = new ModelAndView("index.jsp");
        view.addObject("message", "这就是Spring MVC");
        return view;
    }
}
```

> Controller是控制器接口,接口中只有一个方法handleRequest,用于处理请求和返回`ModelAndView`,这个参数路径的话会默认从web资源路径下去找

这是视图index.jsp:

```html
<%--
  Created by IntelliJ IDEA.
  User: MiaoDaWei
  Date: 2021/9/5
  Time: 17:56
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>hello</title>
  </head>
  <body>
  ${message}
  123
  </body>
</html>
```

结果:

![image-20210905192502603](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905192502603.png)

以上就是一个很简单的Spring第一个小方法;

## 3.SpringMVC 视图解析器

**访问流程**:当我们对SpringMVC控制的资源发起请求时，这些请求都会被SpringMVC的DispatcherServlet处理，接着Spring会分析看哪一个HandlerMapping定义的所有请求映射中存在对该请求的最合理的映射。然后通过该HandlerMapping取得其对应的Handler，接着再通过相应的HandlerAdapter处理该Handler。HandlerAdapter在对Handler进行处理之后会返回一个ModelAndView对象。在获得了ModelAndView对象之后，Spring就需要把该View渲染给用户，即返回给浏览器。在这个渲染的过程中，发挥作用的就是ViewResolver和View。当Handler返回的ModelAndView中不包含真正的视图，只返回一个逻辑视图名称的时候，ViewResolver就会把该逻辑视图名称解析为真正的视图View对象。View是真正进行视图渲染，把结果返回给浏览器的

**概念**:视图解析器(ViewResolver)是Spring MVC的重要组成部分,负责将逻辑视图名解析为具体的视图对象

**理解**:就是将在Controller返回视图对象中路径进行解析并转换为具体的对应路径名!

介绍几种常用的视图解析器:

### 3.1 URLBasedViewResolver

URLBasedViewResolver是对ViewResolver的一种简单实现,主要提供了一种拼接URL的方式来解析视图

**作用**:主要是通过`prefix`属性指定前缀,`suffix`属性指定后缀,当ModelAndView对象返回具体的View名称时,它会将前缀prefix和后缀suffix与具体的视图名称进行拼接,得到一个视图资源文件的具体加载路径,从而加载真正的视图文件并反馈给用户

**注意**:我们除了使用UrlBasedViewResolver除了要配置前缀和后缀属性质问我,还需要配置"viewClass",因为这是表示解析成哪种视图;

dispatcher-servlet.xml:

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

		<!--配置视图-->
    <bean id="viewResolverr" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/> <!--不能省略-->
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
	...

</beans>
```

而我们的Controller:

```java
@Override
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView view = new ModelAndView("index");
        view.addObject("message", "这就是Spring MVC");
        return view;
    }
```

在浏览器进行访问:

![image-20210905203216498](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905203216498.png)

可以发现是可以正常访问的,这里只是给了`index`表示视图解析器拿到了之后就会拼接URL,最终的路径是`/WEB-INF/jsp/index.jsp`这个路径,而我们智能的IDEA也给了提示:

![image-20210905203428656](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905203428656.png)

所以他就会去对应的文件夹下去找!

### 3.2 InternalResourceViewResolver

**概念**:`InternalResourceViewResolver`称之为"内部资源视图解析器",也是我们日常开发中最常用的视图解析器类型,它是`URLBasedViewResolver`的子类,拥有URLBasedViewResolver的一切特性

**作用**:`InternalResourceViewResolver`能自动将返回的视图名称解析为`InternalResourceView`类型的对象,InternalResourceView会把Controller处理器方法返回的模型属性都存放到对应的request属性中,然后通过RequestDispatcher在服务器端把请求forword重定向到目标URL,那么也可以理解为,使用InternalResourceViewResolver视图解析时,无需再单独指定viewClass属性!

dispatcher-servlet.xml:

```xml-dtd
<bean id="viewResolverr" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
</bean>
```

只需改一个类名就可以了,效果一模一样的!

![image-20210905203216498](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905203216498.png)

## 4.执行流程

我们学习SpringMVC那么也需要了解一下SpringMVC的整个执行流程,了解学习才能我们更好的学习后面原理,并且这也是面试可能会问到的:

![Spring MVC执行流程](https://gitee.com/miawei/pic-go-img/raw/master/imgs/1139441444-0.png)

SpringMVC的执行流程如下:

1. 用户在浏览器通过地址栏发起一个HTTP request请求,该请求会被提交达到`DispatcherServlet`(前端控制器);
2. 由`DispatcherServlet`请求一个或多个`HanderMapping`(处理器映射器),并返回一个执行链(`HandlerExecutionChain`);
3. `DispatcherServlet`将执行链返回的`Handler`信息发送给`HandlerAdapter`(处理器适配器);
4. `HandlerAdapter`根据Handler信息找到并执行相应的handler(常称为`Controller`)
5. `Handler`执行完毕后返回给`HandlerAdapter`一个`MoelAndView`对象(Spring MVC的底层对象,包括Model数据模型和View视图信息)
6. `HandlerAdapter`接收到`ModelAndView`对象后,将其返回给`DispatcherServlet`;
7. `DispatcherServlet`接收到`ModelAndView`对象后,会请求`ViewResolver`(视图解析器)对试图进行解析;
8. `ViewResolver`根据View信息匹配到相应的视图结果,并返回给`DispatcherServlet`;
9. `DispatcherServlet`接收到具体的View视图后,进行视图渲染,将Model中的模型数据填充到View视图中的request域,生成最终的View(视图);
10. 视图负责将结果显示到浏览器(客户端)

**理解**:

1. 首先用户在地址栏发起请求到DispactherServlet,之所以会请到这个前端控制器是因为我们在web.xml中配置了拦截请求然后执行DispatcherServlet!这个是前端控制器用于调用每个组件,起到解耦的作用!

2. 然后`DispatcherServlet`前端控制器将请求地址请求到`HandlerMapping`处理器映射器;

3. 处理器映射器在Spring 容器启动的时候会将所有的请求的路径和所对应的方法放到HashMap中,比如"map.put("/system/article/list",method)",然后将其放在缓存中,而这个`HandlerMapping`处理器映射器就会将请求来的路径(也称handler)去匹配key,匹配成功就会返回一个执行链到`DispatcherServlet`前端控制器中去;

4. `DispatcherServlet前端控制器`将匹配成功的请求去找到`HandlerAdater处理器适配器`进行将请求的参数和对应的方法中的匹配参数进行绑定,进行适配处理

5. 将参数请求绑定后根据Handler中的信息找到对应的`handler处理器`,这个就是我们写业务逻辑代码的Controller

6. 方法执行完毕后返回一个`ModelAndView对象`到处理器适配器去,里面包含了逻辑视图和Model参数

7. 将`ModelAndView对象`返回到处理器适配器

8. 在处理器适配器接收到对象后由将其返回到前端控制器去

9. 前端控制器接收到`ModelAndView对象`后将其请求到`ViewResolver视图解析器`去.将其中的View逻辑视图解析为物理视图

10. `ViewResolver视图解析器`解析完毕后把物理视图返回给前端控制器

11. 前端控制器拿到真正的物理视图(这个物理视图也就是我们在电脑中实际存在的路径,而视图一般是jsp之类的),然后进行视图渲染,将之前返回`ModelAndView`中的Model数据渲染填充到View视图中的Request域中,然后生成最终的View视图

12. 将这个最终的View视图进行HttpResponse进行响应输出到浏览器去

    

    ![image-20210907205615174](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210907205615174.png)

### 4.1 各组件介绍

在介绍执行流程中各个组件优点陌生,下面概述一下:

1. `DispatcherServlet`:前端控制器

   ```
   用户请求到达前端的控制器,它就相当于MVC模式中的C,DispatcherServlet是整个流程控制的中心,相当于是SpringMVC的大脑,由它调用其他组件处理用户的请求,DispatcherServlet的存在降低了组件之间的耦合性;
   ```

2. `HandlerMapping`:处理器映射器

   ```
   HandlerMapping负责根据用户请求找到Handler即处理器(也就是我们所说的Controller),Spring MVC提供了不同的映射器实现不同的映射方式,例如:配置文件方式、实现接口方式，注解方式等，而在我们实际开发中，我们最常用的方式是注解方式
   ```

   比如：@RequestMapping("/xx")或者在配置文件中`<bean id="/xx" class="xxx">`又或者在Controller上实现接口的形式!

3. `Handler`:处理器

   ```
   Handler是继承DispatcherServlet前端控制器的后端控制器,在DispatcherServlet的控制下Handler对具体的用户进行处理,由于Handler涉及到具体的用户业务请求,所以一般情况需要程序员根据业务需求开发Handler;(这里所说的Handler就是指我们的Controller)
   ```

4. `HandlAdapter`:处理器适配器

   ```
   通过HandlerAdapter对处理器进行执行,这是适配器模式的应用,通过扩展适配器可以对更多类型的处理器进行执行
   ```

5. `ViewResolver`:视图解析器

   ```
   ViewResolver负责将处理结果生成View视图,ViewResolver首先根据逻辑视图名解析成物理视图名即具体的页面地址,再生成View视图对象,最后对View进行渲染将处理结果通过页面展示给用户;SpringMVC框架提供了很多的View视图类型,包括:jstIView、freemarkerView、pdfView等,一般情况下需要通过页面标签或页面模板技术将模型数据通过页面展示给用户,需要由程序员根据业务需求开发具体的页面
   ```

### 4.2 流程介绍:

 1. ### 第一站:DispatcherServlet(前端控制器)

    从请求离开浏览器以后,第一站到达的就是DispatcherServlet,看名字这就是一个Servlet,通过Servlet可以拦截并处理HTTP请求,DispatcherServlet会拦截所有的请求,并且将这些请求发送给Spring MVC控制器

    ```xml-dtd
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <!-- 拦截所有的请求 -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    ```

    > DispatcherServlet的任务就是拦截请求发送给Spring MVC控制器

 2. ### 第二站:HandlerMapping(处理器映射)

    **问题**:典型的应用程序中可能会有多个控制器，这些请求到底应该发给哪一个控制器呢？

    ​	所以DispatcherServlet会查询一个或多个处理器映射来确定请求的下一站在哪里,**处理器映射会根据请求所携带的URL信息来进行决策**,例如:上面的例子中,我们通过配置simpleUrlHandlerMapping来将/hello地址交给helloController进行处理:

    ```xml-dtd
    <bean id="simpleUrlHandlerMapping"
          class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
        <property name="mappings">
            <props>
                <!-- /hello 路径的请求交给 id 为 helloController 的控制器处理-->
                <prop key="/hello">helloController</prop>
            </props>
        </property>
    </bean>
    <bean id="helloController" class="controller.HelloController"></bean>
    ```

    

 3. ### 第三站:控制器

    一旦选择了合适的控制器,DispatcherServlet会将请求发送给选中的控制器,到了控制器,请求会卸下其负载(用户提交的请求)等待控制器处理完这些信息:

    ```java
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        // 处理逻辑
        ....
    }
    这个也可以说是我们写的Controller
    ```

 4. ### 第四站:返回DispatcherServlet

    当控制器在完成逻辑处理后,通常会产生一些信息,这些信息就是需要返回给用户并在浏览器上显示的信息,它们被称之为 **模型(model)**。仅仅返回原始的信息时不够的-》这些信息时需要以用户友好的方式进行格式化展示，一般会是HTML，所以，信息需要发送给一个 **视图（view）**,

    前端控制器(DispatcherServlet)所做的最后一件事就是将模型数据打包,并且表示出用于渲染输出的视图名(**逻辑视图名**),**它接下来会将请求连同模型和视图名发送回Dispatcher!**

    ```java
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        // 处理逻辑
        ....
        // 返回给 DispatcherServlet
        return mav;
    }
    ```

 5. ### 第五站:视图解析器

    这样以来，控制器就不会和特定的视图相耦合，传递给 DispatcherServlet 的视图名并不直接表示某个特定的 JSP。（实际上，它甚至不能确定视图就是 JSP）相反，**它传递的仅仅是一个逻辑名称，这个名称将会用来查找产生结果的真正视图**。

    DispatcherServlet 将会使用视图解析器（view resolver）来将逻辑视图名匹配为一个特定的视图实现，它可能是也可能不是 JSP

 6. ### 第六站:视图

    ​	既然 DispatcherServlet 已经知道由哪个视图渲染结果了，那请求的任务基本上也就完成了。

    它的最后一站是视图的实现，在这里它交付模型数据，请求的任务也就完成了。视图使用模型数据渲染出结果，这个输出结果会通过响应对象传递给客户端。

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
             pageEncoding="UTF-8" isELIgnored="false"%>
    
    <h1>${message}</h1>
    ```





































