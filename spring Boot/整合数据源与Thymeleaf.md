# SpringBoot笔记

## 1.Thymeleaf

### 1.1 概述

**引入**:前端交给我们的页面是HTML页面;按照我们之前学习web开发的时候,我们需要把他们转成jsp页面,jsp好处就是当我们查出一些数据转发到JSP页面以后,我们可以用jsp轻松实现数据的显示,以及交互等等,

jsp支持非常强大的功能,不仅能写Java代码还能写HTML页面,按照之前的开发可以用JSP,但是现在学习SpringBoot这种情况,这个项目是以jar的方式,不是war,然后我们用的还是嵌入式的Tomcat,所以:**他现在默认是不支持jsp的**,

那么直接用纯静态页面那么会给开发造成非常大的麻烦,而SpringBoot自然有解决办法:推荐使用Thymeleaf模板引擎;

> 模板引擎很多但常用的是:jsp、freemarker、Thymeleaf

模板引擎核心思想图:

![image-20210921194501055](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210921194501055.png)

模板引擎的作用就在于就是我们写一个HTML页面模板,然后呢,页面中有些值是动态的,比如商品价格等等,然后我们写一个表达式,而这些动态值是从我们后台数据库获取然后组织然后将数据交给模板引擎,而模板引擎就会去页面模板对表达式进行解析然后填充数据,最终这个页面由我们想要的内容进行展示出来,这就是我们这个模板引擎,不管是哪一个模板引擎都是这个核心思想;

### 1.2 快速使用

引入maven:

```xml-dtd
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-thymeleaf</artifactId> <!--在springBoot做任何事都是starter启动对应场景即可-->
</dependency>
```

如何使用:

springBoot核心东西就是自动装配,所以我们使用之前看springBoot为我们做了些什么?

1. 在spring-factories找到`ThymeleafAutoConfiguration`;

2. 然后点进去找到ThymeleafProperties这个配置类

   ```java
   @EnableConfigurationProperties(ThymeleafProperties.class)
   ```

3. 然后我们点击`ThymeleafProperties.class`进去看看:

   ![image-20210921200009853](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210921200009853.png)

   从图中可以看出有一个路径:`classpath:/templates/`,然后还有一个后缀;

   **结论**:我们只需要把我们的HTML页面放在类路径下的templates下,那么thymeleaf就可以自动帮我们渲染了;

测试:

1. 编写Controller:

   ```java
   @Controller
   public class HelloController {
   
       @RequestMapping("/demo")
       public String sayHello() {
           return "test";
       }
   }
   ```

2. 编写html:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>thymeleaf</title>
   </head>
   <body>
   这是跳转到thymeleaf页面
   </body>
   </html>
   ```

3. 地址栏访问:

   ![image-20210921200837364](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210921200837364.png)

> 是成功通过thymeleaf解析跳转过来了!

**结论**:我们在Controller层返回view的时候,会被thymeleaf解析然后去resource根目录下的templates文件夹下对其view加上后缀:.html,拼接而成去寻找,找到就返回这个页面

> 我们之前做首页的时候,我们可以通过Controller去跳转页面,那么就可以通过thymeleaf去实现!





**注意**:这是Thymeleaf的视图解析器:`ThymeleafViewResolver`这个类,这个也就是我们在Controller返回字符串的时候会由视图解析器去进行解析视图;而上面的那些是视图解析器解析过后如何去找对应的资源,

```java
public class ThymeleafViewResolver extends AbstractCachingViewResolver implements Ordered {
    //这个方法的意思就是会判断返回的逻辑视图名前是否带有指定的前缀,如:redirect、forward等等不会直接处理，
    protected View createView(String viewName, Locale locale) throws Exception {
        if (!this.alwaysProcessRedirectAndForward && !this.canHandle(viewName, locale)) {
            //翻译:ThymeleafViewResolver 无法处理视图 \"{}\"。传递给链中的下一个解析器。
            vrlogger.trace("[THYMELEAF] View \"{}\" cannot be handled by ThymeleafViewResolver. Passing on to the next resolver in the chain.", viewName);
            return null;
        } else {
            String forwardUrl;
            if (viewName.startsWith("redirect:")) {
                //翻译:[THYMELEAF] View \"{}\" 是一个重定向，不会被 ThymeleafViewResolver 直接处理。
                vrlogger.trace("[THYMELEAF] View \"{}\" is a redirect, and will not be handled directly by ThymeleafViewResolver.", viewName);
                forwardUrl = viewName.substring("redirect:".length(), viewName.length());
                RedirectView view = new RedirectView(forwardUrl, this.isRedirectContextRelative(), this.isRedirectHttp10Compatible());
                return (View)this.getApplicationContext().getAutowireCapableBeanFactory().initializeBean(view, "redirect:");
            } else if (viewName.startsWith("forward:")) {
                //[THYMELEAF] View \"{}\" 是一个转发，不会被 ThymeleafViewResolver 直接处理。
                vrlogger.trace("[THYMELEAF] View \"{}\" is a forward, and will not be handled directly by ThymeleafViewResolver.", viewName);
                forwardUrl = viewName.substring("forward:".length(), viewName.length());
                return new InternalResourceView(forwardUrl);
            } else if (this.alwaysProcessRedirectAndForward && !this.canHandle(viewName, locale)) {
                //[THYMELEAF] ThymeleafViewResolver 无法处理视图 \"{}\"。传递给链中的下一个解析器。
                vrlogger.trace("[THYMELEAF] View \"{}\" cannot be handled by ThymeleafViewResolver. Passing on to the next resolver in the chain.", viewName);
                return null;
            } else {
                //[THYMELEAF] 视图 {} 将由 ThymeleafViewResolver 处理并为其创建一个 {} 实例",
                vrlogger.trace("[THYMELEAF] View {} will be handled by ThymeleafViewResolver and a {} instance will be created for it", viewName, this.getViewClass().getSimpleName());
                return this.loadView(viewName, locale);
            }
        }
    }
}
```

> 也就是说我们在返回的时候如果加上重定向或者转发的方式那么就不会走Thymeleaf视图解析器!

### 1.3 语法

语法的话以最新官方文档为准,这里是Thymeleaf 官网：https://www.thymeleaf.org/ ,

简单测试一下:

1. 修改Controller:

   ```java
   @Controller
   public class HelloController {
   
       @RequestMapping("/demo")
       public String sayHello(Model model) {
           model.addAttribute("msg", "shy哥牛逼");
           return "test";
       }
   }
   ```

2. 添加命名约束空间

   ```xml-dtd
   xmlns:th="http://www.thymeleaf.org"
   ```

3. 修改html:

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>thymeleaf</title>
   </head>
   <body>
   <div th:text="${msg}"></div>
   </body>
   </html>
   ```

4. 启动测试:

   ![image-20210921204215593](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210921204215593.png)

   这是常用的一些Thymeleaf语法:

   1. 我们可以使用任意的th:attr来替换Html中原生属性的值!

      ![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Idia351qHgmH2vbzurk1Pp6fGYIwv043icVDYuybRJDCGTSNTMEibFzzMdlKS4t07TQoicQJKQAe0slQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   2. 我们能写哪些表达式呢?

      ```java
      Simple expressions:（表达式语法）
      Variable Expressions: ${...}：获取变量值；OGNL；
          1）、获取对象的属性、调用方法
          2）、使用内置的基本对象：#18
               #ctx : the context object.
               #vars: the context variables.
               #locale : the context locale.
               #request : (only in Web Contexts) the HttpServletRequest object.
               #response : (only in Web Contexts) the HttpServletResponse object.
               #session : (only in Web Contexts) the HttpSession object.
               #servletContext : (only in Web Contexts) the ServletContext object.
      
          3）、内置的一些工具对象：
      　　　　　　#execInfo : information about the template being processed.
      　　　　　　#uris : methods for escaping parts of URLs/URIs
      　　　　　　#conversions : methods for executing the configured conversion service (if any).
      　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
      　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
      　　　　　　#numbers : methods for formatting numeric objects.
      　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
      　　　　　　#objects : methods for objects in general.
      　　　　　　#bools : methods for boolean evaluation.
      　　　　　　#arrays : methods for arrays.
      　　　　　　#lists : methods for lists.
      　　　　　　#sets : methods for sets.
      　　　　　　#maps : methods for maps.
      　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
      ==================================================================================
      
        Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
        Message Expressions: #{...}：获取国际化内容
        Link URL Expressions: @{...}：定义URL；
        Fragment Expressions: ~{...}：片段引用表达式
      
      Literals（字面量）
            Text literals: 'one text' , 'Another one!' ,…
            Number literals: 0 , 34 , 3.0 , 12.3 ,…
            Boolean literals: true , false
            Null literal: null
            Literal tokens: one , sometext , main ,…
            
      Text operations:（文本操作）
          String concatenation: +
          Literal substitutions: |The name is ${name}|
          
      Arithmetic operations:（数学运算）
          Binary operators: + , - , * , / , %
          Minus sign (unary operator): -
          
      Boolean operations:（布尔运算）
          Binary operators: and , or
          Boolean negation (unary operator): ! , not
          
      Comparisons and equality:（比较运算）
          Comparators: > , < , >= , <= ( gt , lt , ge , le )
          Equality operators: == , != ( eq , ne )
          
      Conditional operators:条件运算（三元运算符）
          If-then: (if) ? (then)
          If-then-else: (if) ? (then) : (else)
          Default: (value) ?: (defaultvalue)
          
      Special tokens:
          No-Operation: _
      ```

      案例:

      1. controller:

         ```java
         @Controller
         public class HelloController {
         
             @RequestMapping("/demo")
             public String sayHello(Model model) {
                 model.addAttribute("msg", "<h1>shy哥牛逼</h1>");
                 model.addAttribute("lists", Arrays.asList("MiaoWei", "哇塞", "真棒"));
                 return "test";
             }
         }
         ```

      2. html:

         ```html
         <!DOCTYPE html>
         <html lang="en" xmlns:th="http://www.thymeleaf.org">
         <head>
             <meta charset="UTF-8">
             <title>thymeleaf</title>
         </head>
         <body>
         <!--原样输出-->
         <div th:text="${msg}"></div>
         <!--转义-->
         <div th:utext="${msg}"></div>
         
         <!--循环-->
         <h4 th:each="user:${lists}" th:text="${user}"></h4>
         <hr>
         <!--行内写法-->
         <h4 th:each="user:${lists}">[[${user}]]</h4>
         </body>
         </html>
         ```

      3. 测试:

         ![image-20210921205338899](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210921205338899.png)

> 使用Thymeleaf模板语法:
>
> 两种方式: 举个例子:
>
> 1.<label class=sr-only th:text="${login}"></label>
>
> 2.<label class=sr-only>[[${login}]]</label>

### 1.4 其他笔记:

1. 关闭模板引擎的缓存

   ```yaml
   # 关闭模板引擎的缓存
   spring:
     thymeleaf:
       cache: false
   ```

2. 我们在html中对路径的引用:

   ```html
   <!--这里进行thymeleaf语法, 这里/表示会从根路径下去找,而我们处于static静态文件夹下,所以可以直接去static下的asserts-->
   <link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
   ```

   > 这里无论项目怎么改变,比如增加Content虚拟路径,这里的路径都不会发生改变,都是会从classPath下去找资源,是万能匹配的!如果我们增加了,那么这里`@`就会默认给你添加前缀!

3. 国际化消息的获取:

   ```html
   <h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
   ```

4. 对数据的判断和数据的获取:

   ```html
   <!--这里不显示是针对这行标签不显示,如果不加if判断,那么只是这里msg没有而已!-->
   <p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p> //not 取反, strings内置对象
   ```

5. 封装成组件,然后另一个地方引用:

   ```html
   <!--封装成组件-->
   <nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar"></nav> <!--给这个nav封装成一个组件!-->
   
   <!--引用-->
   <!--引用组件,这里~表示引用的意思! dashboard表示文件的名字,也可以全称如:,dashboard.html; sidebar表示在这个文件里面的组件的名字,一般由th:fragment表示-->
   <div th:insert="~{dashboard::sidebar}"></div>
   注意:这里dashboard是指当前文件与引用的文件是处于同一级别的,如果是含有其他层级,则需要设置相应的文件夹来获取,如:<div th:replace="~{commons/commons.html::topbar}"></div>
   
   
   扩展:
   insert:插入
   replace:替换
   区别:插入会嵌套一层div 而replace不会,直接替换
   ```

6. 传参:

   ```html
   <!--以前我们传参是用?形式拼接,但是这个使用thymeleaf语法可以直接用()来进行使用,这种无论是请求给后端还是说请求给另一个index.html,那么都是可以获取到的-->
   <!--这里如果是请求给后端的,那么是会默认为GET请求的方式-->
   <div th:replace="~{commons/commons.html::sidebar(avtive='main.html')}"></div>
   
   <!--这是另一个页面-->
   <!--这里使用三元表达式-->
   <a th:class="${avtive=='main.html'} ? 'nav-link active' :'nav-link'" th:href="@{/index.html}">
       
   <!--上面使用()来传参默认使用GET方法,但是我们想用路径参数的方式传参,就是这种 http:localhost:8080/miao/emp/3 ,那么就可以用下面这种方式拼接就可以了-->
   <a class="btn btn-sm btn-primary" th:href="@{/emp/}+${emp.getId()}">编辑</a>
   ```

7. 循环:

   ```html
   <!--这里首先获取emps数据然后赋值给emp元素-->
   <tr th:each="emp:${emps}">
       <!--通过emp元素进行getbean使用然后通过thymeleaf绑定文本输出-->
       <td th:text="${emp.getId()}"></td>
       <!--另一种写法-->
       <td>[[${emp.getLastName()}]]</td>
       <td th:text="${emp.getEmail()}"></td>
       <!--三元表达式-->
       <td th:text="${emp.getGender()==0} ? '女':'男'"></td>
       <!--这里直接get获取对象,只不过这里会调用对象的toString方法-->
       <td th:text="${emp.getDepartment()}"></td>
       <!--这里会调用对象中的属性方法-->
       <td th:text="${emp.department.getDepartmentName()}"></td>
   	<!--这里用到thymeleaf内置对象的dates对象,将获取的date数据转换为我们想要的格式-->
       <td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
   </tr>
   ```

   
   
   

## 2.MVC自动配置原理

虽然SprinBoot为我们的web做了大量的自动化配置,但是我们得清楚SpringBoot对我们的SpringMVC做了**哪些配置**、**如何扩展**、**如何定制**！

那么我们从两个途径去了解去搞清楚:

### 2.1 官方文档

地址:https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

```java
Spring MVC Auto-configuration
// Spring Boot为Spring MVC提供了自动配置，它可以很好地与大多数应用程序一起工作。
Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
// 自动配置在Spring默认设置的基础上添加了以下功能：
The auto-configuration adds the following features on top of Spring’s defaults:
// 包含视图解析器
1.Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
// 支持静态资源文件夹的路径，以及webjars
2.Support for serving static resources, including support for WebJars 
// 自动注册了Converter：
// 转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为int类型,又或者前台传一个user中的字段,后台接收的时候会自动封装到user中去
// Formatter：【格式化器，比如页面给我们了一个2019-8-10，它会给我们自动格式化为Date对象】
3.Automatic registration of Converter, GenericConverter, and Formatter beans.
// HttpMessageConverters:消息转换
// SpringMVC用来转换Http请求和响应的的，比如我们要把一个User对象转换为JSON字符串，可以去看官网文档解释；
4.Support for HttpMessageConverters (covered later in this document).
// 定义错误代码生成规则的
5.Automatic registration of MessageCodesResolver (covered later in this document).
// 首页定制
6.Static index.html support.
// 图标定制
7.Custom Favicon support (covered later in this document).
// 初始化数据绑定器：帮我们把请求数据绑定到JavaBean中！
8.Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).

/*
如果您希望保留Spring Boot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
的@configuration类，类型为webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供
RequestMappingHandlerMapping、RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义
实例，则可以声明WebMVCregistrationAdapter实例来提供此类组件。
*/
If you want to keep Spring Boot MVC features and you want to add additional MVC configuration 
(interceptors, formatters, view controllers, and other features), you can add your own 
@Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide 
custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or 
ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

// 如果您想完全控制Spring MVC，可以添加自己的@Configuration，并用@EnableWebMvc进行注释。
If you want to take complete control of Spring MVC, you can add your own @Configuration annotated with @EnableWebMvc.
```

### 2.2 源码解析

从官方文档中可以得出SpringBoot为SpringMVC做了大量的自动配置,那么我们接下来仔细看看都有些什么?

1. ### 视图解析器

   首先从官方文档中得出能配置的第一项:

   ```java
   Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans. //包含视图解析器
   ```

   从文字上挑选出`ContentNegotiatingViewResolver`表示内容协商视图解析器;

   不难看出这就是我们之前学习springMVC中的视图解析器,之前我们都是手动进行配置,而现在看看springBppt怎么配置的:

   > 视图解析器:根据方法的返回值取得视图对象(view),然后由视图对象决定如何渲染(转发、重定向);

   接下来我们去看看源码:

   首先我们还是得从`WebMvcAutoConfiguration(因为这里是springMVC所有的配置都在这个配置类)`开始找:然后搜索`ContentNegotiatingViewResolver`找到如下方法:

   ```java
   @Bean
   @ConditionalOnBean(ViewResolver.class)
   @ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
   public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
   			ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
   			resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiationManager.class));
   			// ContentNegotiatingViewResolver使用所有其他视图解析器来定位视图，因此它应该具有较高的优先级
   			resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
   			return resolver;
   }
   ```

   然后我们点击`ContentNegotiatingViewResolver`看看:

   ```java
   public class ContentNegotiatingViewResolver extends WebApplicationObjectSupport
   		implements ViewResolver, Ordered, InitializingBean {..}
   ```

   可以发现是实现了视图解析器接口`ViewResolver`的类,我们就可以把它看做视图解析器;

   - 那思考一下既然实现视图解析器的接口,那该如何接管这个视图解析器呢?

   答:既然要接管那么我们就得按照这个规则来:我们去看这个`ViewResolver`接口:

   ```java
   public interface ViewResolver {
   	@Nullable
   	View resolveViewName(String viewName, Locale locale) throws Exception;//解析视图
   }
   ```

   那么我们在回过头再看看是怎么实现这个的方法的:

   ```java
   @Override  //默认的视图解析器
   	@Nullable //@Nullable 即参数可为null
   	public View resolveViewName(String viewName, Locale locale) throws Exception {
   		RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
   		Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
   		List<MediaType> requestedMediaTypes = getMediaTypes(((ServletRequestAttributes) attrs).getRequest());
   		if (requestedMediaTypes != null) {
               //获取候选的视图
   			List<View> candidateViews = getCandidateViews(viewName, locale, requestedMediaTypes);
               //获取最好的视图
   			View bestView = getBestView(candidateViews, requestedMediaTypes, attrs);
   			if (bestView != null) {
   				return bestView;
   			}
   		}
   
   		....
   	}
   ```

   既然获取候选的视图,那它怎么获得候选的视图的呢?

   我们点击`getCandidateViews()`看看:

   ```java
   private List<View> getCandidateViews(String viewName, Locale locale, List<MediaType> requestedMediaTypes)
   			throws Exception {
   
   		List<View> candidateViews = new ArrayList<>();
   		if (this.viewResolvers != null) {
   			Assert.state(this.contentNegotiationManager != null, "No ContentNegotiationManager set");
               //这里就是遍历所有的视图解析器,从它们的工具类中去获取
   			for (ViewResolver viewResolver : this.viewResolvers) {
                   //把它封装成一个对象
   				View view = viewResolver.resolveViewName(viewName, locale);
   				if (view != null) {
                       //添加进去
   					candidateViews.add(view);
   				}
   				...
   		}
   		if (!CollectionUtils.isEmpty(this.defaultViews)) {
   			candidateViews.addAll(this.defaultViews);
   		}
   		return candidateViews;
   	}
   ```

   可以得出`getCandidateViews`中就是把所有的视图解析器进行遍历解析,所以得出一个结论:

   > ContentNegotiatingViewResolver这个视图解析器就是用来组合所有的视图解析器的

   而我们再研究它这里有个属性`viewResolvers`,看看它是在哪里进行赋值的?

   ```java
   protected void initServletContext(ServletContext servletContext) {
       // 这里它是从beanFactory工具中获取容器中的所有视图解析器
       // ViewRescolver.class 把所有的视图解析器来组合的
       Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext(), ViewResolver.class).values();
       ViewResolver viewResolver;
       if (this.viewResolvers == null) {
           this.viewResolvers = new ArrayList(matchingBeans.size());
       }
       // ...............
   }
   ```

   这里表示它是在容器中去找视图解析器,那么思考一下我们也可以写一个视图解析器然后放进容器中,然后被加载解析进行组合起来:

   **自定义视图解析器**:

   1. 实现视图解析器接口,并覆写其解析视图方法

      ```java
      @Configuration
      public class MyMvcConfig implements WebMvcConfigurer {
      
          @Bean //加载到Spring容器中
          public ViewResolver myViewResolver() {
              return new MyViewResolver();
          }
      
          /**
           * 我的视图解析器-自定义一个视图解析器
           *
           * @author MiaoDaWei
           * @date 2021/09/24
           */
          public static class MyViewResolver implements ViewResolver {
              @Override
              public View resolveViewName(String viewName, Locale locale) throws Exception {
                  return null;
              }
          }
      }
      ```

2. 由于在SpringMVC执行流程中都会执行到`dispatcherServlet`,所以我们全局搜索`dispatcherServlet`这个类,并找到`doDispatch`这个方法:

   ![image-20210924195722308](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924195722308.png)

   我们在doDispatcher这里打断点可以发现这里有很多方法,我们找到viewResolvers视图解析器,展开发现默认的是`ContentNegotiatingViewResolver`,然后我们找到自己自定义写的视图解析器,

   得出一个结论:

   > 如果我们想自定义一些定制化的功能,只要写这个组件,然后将它交给springBoot,而springBoot就会帮我们自动转配!

   也即是说springBoot自动配置的mvc中,会从Bean容器中获取候选的所有的视图解析器,然后进行解析封装选择一个最好的视图解析器然后运行;举个例子:我们在之前写Thymeleaf的时候返回视图,就是由ThymeLeaf视图解析器进行解析然后去templates文件夹去匹配!

2. ### 日期转换器

   我们从`WebMvcAutoConfiguration`这个类点进去搜索`Formatter`找到`mvcConversionService`这个方法:

   ```java
   @Bean
   		@Override
   		public FormattingConversionService mvcConversionService() {
               //这里从配置文件中获取格式化规则
   			Format format = this.mvcProperties.getFormat();
   			WebConversionService conversionService = new WebConversionService(new DateTimeFormatters()
   					.dateFormat(format.getDate()).timeFormat(format.getTime()).dateTimeFormat(format.getDateTime()));
   			addFormatters(conversionService);
   			return conversionService;
   }
   ```

   我们点击这个`this.mvcProperties.getFormat();`看看配置文件:

   ```java
   private final Format format = new Format();
   //然后我们点击这个Format格式类对象
   
   public static class Format {
   
   		/**
   		 * Date format to use, for example `dd/MM/yyyy`.
   		 */
   		private String date;
   
   		/**
   		 * Time format to use, for example `HH:mm:ss`.
   		 */
   		private String time;
   
   		/**
   		 * Date-time format to use, for example `yyyy-MM-dd HH:mm:ss`.
   		 */
   		private String dateTime;
   }
   ```

   以上就是默认的格式转换,

   > 可以发现:转换器就是自动把前台传的日期通过配置文件进行获取格式化规则进行格式化转换给我们后台

   那么试想既然是通过配置文件进行获取的,那么我们可以在yaml中进行修改成我们想要的格式:

   ![image-20210924202442450](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924202442450.png)

   自定义格式后,如果前台传的不是这个格式就会报错;

   这里进行演示:

   yaml:

   ```yaml
   spring:
     mvc:
       format:
         date: yyyy-MM-dd
   ```

   controller:

   ```java
   @Controller
   public class DateController {
       @RequestMapping("/test")
       public void test(Date date){
           System.out.println("date = " + date.getTime());
       }
   }
   ```

   1. 这是输入正确的数据:

      ![image-20210924204853089](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924204853089.png)

      控制台:

      ![image-20210924204917432](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924204917432.png)

   2. 这是输入错误的数据:

      ![image-20210924205011614](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924205011614.png)

      控制台也会打印错误日志!

      ![image-20210924205041189](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924205041189.png)

### 2.3 修改springBoot默认设置

通过以上的源码解析可以看出原理都是一样的,springBoot在自动配置很多组件的时候,会先去看容器中是否存在用户自己配置的(如果用户自己配置@bean),如果有就用用户配置的,如果没有就用自动配置的;

就打个比方:比如我们的视图解析器,我们放进容器中,springBoot就会将用户配置的和自己默认定义的组合起来进行组合成候选配置,然后选出一个最好的!

那我们如何在已有的MVC的配置上扩展自己想要的自定义配置呢?

这是官方文档:

```java
/*
如果您希望保留Spring Boot MVC功能，并且希望添加其他MVC配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
的@configuration类，类型为webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供
RequestMappingHandlerMapping、RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver的自定义
实例，则可以声明WebMVCregistrationAdapter实例来提供此类组件。
*/
If you want to keep Spring Boot MVC features and you want to add additional MVC configuration 
(interceptors, formatters, view controllers, and other features), you can add your own 
@Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide 
custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or 
ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.
```

表示:我们要做的就是编写一个`@Configuration`注解类,并且实现接口`WebMvcConfigurer`,但是要注意不能标注@EnableWebMVC注解;

案例:

**引入**:我们之前学习SSM的时候,我们要跳转一个登录界面的时候,由于对应的JSP放在web-inf下面,不能直接通过地址栏访问,所以我们通过访问Controller,然后通过地址去跳转!

Controller:

```java
//如果我们要扩展springMVC,官方建议我们这样去做!因为这样既能保留所有的自动配置,又能使用我们扩展的配置
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //视图跳转
        //浏览器发送/miao,那么就会test页面
        registry.addViewController("/miao").setViewName("test");
    }
}
```

地址栏:

![image-20210924211142062](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210924211142062.png)

这里是跳转成功了,只不过这里没传值而已,但是已经跳转过来了!

#### 2.3.1 why不加@EnableWebMVC注解的原理

在官方文档中说到我们在扩展SpringMVC的时候是不能加入`@EnableWebMvc`这个注解,那么为什么呢?

1. 我们点击这个注解查看:

   ```java
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.TYPE)
   @Documented
   @Import(DelegatingWebMvcConfiguration.class)
   public @interface EnableWebMvc {
   }
   ```

   可以得出加上这个注解就是导入了这个一个类的意思

2. 我们在`WebMvcAutoConfiguration`自动配置类里找到`WebMvcAutoConfigurationAdapter`这个方法:

   ```java
   @Configuration(proxyBeanMethods = false)
   	@Import(EnableWebMvcConfiguration.class)
   	@EnableConfigurationProperties({ WebMvcProperties.class,
   			org.springframework.boot.autoconfigure.web.ResourceProperties.class, WebProperties.class })
   	@Order(0)
   	public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {
           ...
       }
   ```

   我们在其注解中找到`@Import(EnableWebMvcConfiguration.class)`,表示在自动配置时会导入这个类

3. 我们点击这个EnableWebMvcConfiguration这个类查看:

   ```java
   @Configuration(proxyBeanMethods = false)
   	@EnableConfigurationProperties(WebProperties.class)
   	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
           ...
       }
   ```

   这里可以看出默认继承了`DelegatingWebMvcConfiguration`这个类,跟我们使用注解`@EnableWebMvc`导入的类一模一样

4. 我们点击DelegatingWebMvcConfiguration这个类查看这个方法:

   ```java
   @Autowired(required = false) //自动注入
   	//setConfigurers:设置配置
   	//这里获取所有实现了WebMvcConfigurer接口的东西
   	public void setConfigurers(List<WebMvcConfigurer> configurers) {
           //如果这个集合不为空
   		if (!CollectionUtils.isEmpty(configurers)) {
               //就把这个添加进去
   			this.configurers.addWebMvcConfigurers(configurers);
   		}
   	}
   ```

   这段代码的意思就是会获取容器中的所有实现`WebMvcConfigurer`接口的意思!

5. 我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

   ```java
   protected void addViewControllers(ViewControllerRegistry registry) {
       this.configurers.addViewControllers(registry);
   }
   ```

6. 我们点进去看一下

   ```java
   
   public void addViewControllers(ViewControllerRegistry registry) {
       Iterator var2 = this.delegates.iterator();
   
       while(var2.hasNext()) {
           // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
           WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
           delegate.addViewControllers(registry);
       }
   }
   ```

   所以得出**结论**：所有的WebMvcConfiguration都会被作用，不止Spring自己的配置类，我们自己的配置类当然也会被调用；

#### 2.3.2 全面接管springMVC

官方文档:

```java
If you want to take complete control of Spring MVC
you can add your own @Configuration annotated with @EnableWebMvc.
//翻译:如果你想要全面接管spsringMVC,那么你要添加@Configuration注解和@EnableWebMvc注解
```

> 全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

当前,我们工作中肯定不推荐全面接管springMVC;不过我们来分析一下为什么说加了这个注解就会全面失效:

1. 我们点击`@EnableWebMvc`这个注解里面:发现它这里导了一个类

   ```java
   @Import({DelegatingWebMvcConfiguration.class})
   public @interface EnableWebMvc {
   }
   ```

2. 我们点击它`DelegatingWebMvcConfiguration`,发现继承了一个父类 WebMvcConfigurationSupport

   ```java
   public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
     // ......
   }
   ```

3. 不慌,我们暂时先回到`WebMvcAutoConfiguration`的自动配置类里面:

   ```java
   @ConditionalOnMissingBean(WebMvcConfigurationSupport.class) //注解意思:当这个容器中没有这个组件的时候,那么这个配置类才会生效
   ...
   public class WebMvcAutoConfiguration {...}
   ```

   而这个组件又跟我们@EnableWebMvc注解中导入的类的父类一模一样,那么也可以得出结论:

   > 加上@EnableWebMvc就会使我们MVC自动配置类全盘失效!

   注意:我们在底层代码中看到许多带有`xxx Configuration`这个配置类,帮助我们进行扩展配置,只要看见了这个东西,我们就要注意了!

## 3.国际化

**引入**:我们在浏览许多网站的时候都会遇到涉及中英文切换甚至更多语言的切换,这时我们就需要学习国际化了!

### 3.1 快速使用

1. 第一先设置IDEA的poperties的编码,不然每个页面就会乱码,这样就会导致我们白写了:Ctrl+alt+S

   ![image-20210927140933943](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927140933943.png)

   2. 编写配置文件:

      1. 在resources资源目录下新建一个文件夹`i18n`,然后在里面新建一个文件:`login.properties`,然后又新建一个`login_zh_CN.properties`

         ![image-20210927141817047](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927141817047.png)

         注意:这里login_en_US.properties是我后面加的!

      2. 然后此时我们右键`Resource Bundle login`:

         ![image-20210927142004401](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927142004401.png)

      3. 这里就会自动识别国际化配置文件:然后输入:`en_US`,这样就会变成上面的那样,就会变得很快捷

 ### 3.2 配置文件原理

我们去看一下SpringBoot是如何去对国际化的自动配置!这里我们全局搜索`MessageSourceAutoConfiguration`这个类

然后就可以看见:

```java
@Bean
@ConfigurationProperties(prefix = "spring.messages") //前缀,也就是我们配置文件中修改默认值的前缀!
public MessageSourceProperties messageSourceProperties() {
	return new MessageSourceProperties(); //配置文件,可在yaml中对其修改
}
```

1. 搜索方法`messageSource`这个方法,这里发现SpringBoot已经自动配置好了管理我们国际化资源文件的组件 `ResourceBundleMessageSource`:

   ```java
   @Bean
   									//获取properties传递过来的值进行判断
   	public MessageSource messageSource(MessageSourceProperties properties) {
   		ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
   		if (StringUtils.hasText(properties.getBasename())) {
               //设置国际化文件的基础名（去掉语言国家代码的）
   			messageSource.setBasenames(StringUtils
                                          												//从配置文件中获取基本的名字
   					.commaDelimitedListToStringArray(StringUtils.trimAllWhitespace(properties.getBasename())));
   		}
   		if (properties.getEncoding() != null) {
   			messageSource.setDefaultEncoding(properties.getEncoding().name());
   		}
   		messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
   		Duration cacheDuration = properties.getCacheDuration();
   		if (cacheDuration != null) {
   			messageSource.setCacheMillis(cacheDuration.toMillis());
   		}
   		messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
   		messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
   		return messageSource;
   	}
   ```

2. 从上面从配置文件获取`baseName`,那么我们也去点一下看一下这个是什么:

   ```java
   public class MessageSourceProperties {
       private String basename = "messages";
       private Charset encoding = StandardCharsets.UTF_8;
   }
   ```

   结论:由此可见它是会从配置文件中获取messages路径下的国际化,所以我们需要在springBoot配置文件中去改变我们的真实的项目路径:

   ```yaml
   spring:
     messages:
       basename: i18.login
   ```

   这里关于路径的写法:其本身是从classPath下去寻找,然后这里合并文件夹为login

   ![image-20210927152702241](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927152702241.png)

> 这里路径一定要对,不然页面使用就会发生乱码的问题

### 3.3 页面使用

我们使用thymeleaf的语法使用`#{xxx}`来表示:

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
		<meta name="description" content="">
		<meta name="author" content="">
		<title>Signin Template for Bootstrap</title>
		<!-- Bootstrap core CSS -->
		<!--这里进行thymeleaf语法, 这里/表示会从根路径下去找,而我们处于static静态文件夹下,所以可以直接去static下的asserts-->
		<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet">
		<!-- Custom styles for this template -->
		<link th:href="@{/css/signin.css}" rel="stylesheet">
	</head>

	<body class="text-center">
		<form class="form-signin" action="dashboard.html">
			<img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
			<input type="text" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
			<input type="password" class="form-control" th:placeholder="#{login.password}" required="">
			<div class="checkbox mb-3">
				<label>
					<!--第一种写法:-->
<!--          <input type="checkbox" value="remember-me" th:text="#{login.remember}"> Remember me-->
					<!--第二种写法:-->
          <input type="checkbox" value="remember-me">[[#{login.remember}]]
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm">中文</a>
			<a class="btn btn-sm">English</a>
		</form>
	</body>
</html>
```

![image-20210927161011188](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927161011188.png)

### 3.4 自定义配置解析

我们光配置了国际化后也是不够的,因为这是会根据我们当前浏览器进行判断的,那么我们想切换英文又可以切换中文那怎么实现呢?

1. 我们在web的自动配置类里面找到`localeResolver`这个方法:

   ```java
   @Override
   @Bean
   @ConditionalOnMissingBean(name = DispatcherServlet.LOCALE_RESOLVER_BEAN_NAME)
   @SuppressWarnings("deprecation")
   		//localResolver翻译为区域解析器
   public LocaleResolver localeResolver() {
       //这里判断如果我们手动配置了本地区域化的信息,也就是会去容器去找是否用户自定义配置了
       if (this.webProperties.getLocaleResolver() == WebProperties.LocaleResolver.FIXED) {
           //那么就会返回用户配置的
           return new FixedLocaleResolver(this.webProperties.getLocale());
       }
       if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
           return new FixedLocaleResolver(this.mvcProperties.getLocale());
       }
       //这里就会走系统默认的
       AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
       Locale locale = (this.webProperties.getLocale() != null) ? this.webProperties.getLocale()
           : this.mvcProperties.getLocale();
       localeResolver.setDefaultLocale(locale);
       return localeResolver;
   }
   ```

2. 我们点击`AcceptHeaderLocaleResolver`这个类:国际化解析器

   ```java
   public class AcceptHeaderLocaleResolver implements LocaleResolver {...} //可以发现这里实现了区域解析器,可理解为实现了这个类就是自定义国际化器
   ```

3. 我们找下他实现的方法:

   ```java
   @Override
   public Locale resolveLocale(HttpServletRequest request) {
       //获取一个默认的消息
      Locale defaultLocale = getDefaultLocale();
       //消息不为空并且获取浏览器请求头里的语言等于空
      if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
          //返回默认消息
         return defaultLocale;
      }
      Locale requestLocale = request.getLocale();
      List<Locale> supportedLocales = getSupportedLocales();
      if (supportedLocales.isEmpty() || supportedLocales.contains(requestLocale)) {
         return requestLocale;
      }
      Locale supportedLocale = findSupportedLocale(request, supportedLocales);
      if (supportedLocale != null) {
         return supportedLocale;
      }
      return (defaultLocale != null ? defaultLocale : requestLocale);
   }
   ```

   那假如我们现在想点击链接让我们的国际化资源生效，就需要让我们自己的Locale生效！

   我们去自己写一个自己的LocaleResolver，可以在链接上携带区域信息！

4. 修改前端请求参数:

   ```html
   <a class="btn btn-sm" th:href="@{/index.html(i18nMessage='zh_CN')}">中文</a>
   <a class="btn btn-sm" th:href="@{/index.html(i18nMessage='en_US')}">English</a>
   ```

   > 这里请求参数的话不用像之前?的形式可以直接用()来表示

5. 设置自己的国际化组件:

   ```java
   package com.example.springbootproject.config;
   import cn.hutool.core.text.CharSequenceUtil;
   import org.springframework.web.servlet.LocaleResolver;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.util.Locale;
   
   /**
    * @program: springbootProject
    * @description: 自定义国际化资源配置
    * @author: MiaoWei
    * @create: 2021-09-27 16:52
    **/
   public class MyLocaleResolver implements LocaleResolver {
       /**
        * 解决语言环境
        *
        * @param request 请求
        * @return {@link Locale}
        */
       @Override
       public Locale resolveLocale(HttpServletRequest request) {
           //获取请求中的语言参数
           String i18nMessage = request.getParameter("i18nMessage");
           //获取默认的地区
           Locale locale = Locale.getDefault();
   
           if (CharSequenceUtil.isNotEmpty(i18nMessage)) {
               //拆分语言参数: zh_CN en_US
               String[] split = i18nMessage.split("_");
               //国家,地区
               locale = new Locale(split[0], split[1]);
           }
           return locale;
       }
   
       @Override
       public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
   		//这里不会返回任何东西,我们需要的是返回
       }
   }
   ```

6. 光配置完还不行,还要把它放心我门的spring容器中,这样springBopo才会识别加载我们的:

   ```java
   @Bean
   public LocaleResolver localeResolver(){
       return new MyLocaleResolver();
   }
   ```

7. 看效果:

   ![image-20210927171749205](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927171749205.png)

   这时我们点击English:

   ![image-20210927171820328](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210927171820328.png)

### 3.5 总结

前端:使用Thymeleaf进行配置:`#{}`,或者使用`[[#{}]]`来获取国际化消息,路径的话用`@{}`

后端:

1. 在resouces资源目录下添加`i18n`资源文件夹,然后进行在配置文件中配置路径
2. 添加国际化`key=value`
3. 实现`LocaleResolver`接口进行重写
4. 将重写的类进行添加到Spring容器里去!

## 4.整合JDBC

在整合之前先介绍一下SpringData这个东西,因为对于数据访问层,无论是SQL(关系型数据库)还是NOSQL(非关系型数据库),**SpringBoot底层都是采用Spring Data的方式进行统一处理**.

**SpringData**:是Spring的一个子项目,用于简化数据库访问,支持NoSQL和关系数据库存储,其主要目标是使数据库的访问变得方便快捷;

> SpringBoot底层都是采用Spring Data的方式进行统一处理各种数据库,Spring Data也是Spring中与Spring Boot、Spring Cloud等齐名的知名项目!

**Spring Data 官网**:https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

### 4.1 开始使用

1. 首先我们新建项目,然后我们需要在创建SpringBoot项目的时候勾选指定的启动器:

   ![image-20210928145703327](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210928145703327.png)

2. 创建完毕后我们查看pom.xml依赖:

   ```xml-dtd
   <dependencies>
           <!--整合JDBC-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jdbc</artifactId>
           </dependency>
           <!--导入web项目所需要的依赖-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <!--导入MySQL驱动包-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <scope>runtime</scope>
           </dependency>
           <!--lombok依赖-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <!--测试-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   ```

   可以看出springBoot可以帮我们导入了MySQl的驱动并且也导入JDBC相关的依赖!

3. 驱动都有了我们现在去配置文件中配置数据库的信息

   ```yaml
   spring:
     datasource:
       username: root
       password: root
       # userUnicode:保证不乱码  characterEncoding:设置字符集 serverTimezone:设置时区
       url: jdbc:mysql://localhost:3306/test?userUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
       driver-class-name: com.mysql.cj.jdbc.Driver
   ```

   **注意**:这里连接数据库的时候要把时区加上,不然就会报以下异常:

   ![image-20210928153247591](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210928153247591.png)

   配置完了我们就可以开始开干了,但是该怎么写呢?

   由于SpringBoot自动装配的特性,我们在导入了JDBC和MySQL驱动的依赖和配置了数据库那么就会自动去产生一些对象;

4. 我们可以看对应的properties文件看看我们能配哪些参数:

   ```java
   @ConfigurationProperties(prefix = "spring.datasource") //这是前缀
   public class DataSourceProperties implements BeanClassLoaderAware, InitializingBean {
       //类加载器
   	private ClassLoader classLoader;
   	//是否生成随机数据源名称。
   	private boolean generateUniqueName = true;
   	//名称
   	private String name;
   	//数据库的类型
   	private Class<? extends DataSource> type;
   	//驱动
   	private String driverClassName;
   	//数据库连接地址
   	private String url;
   	//用户名
   	private String username;
   	//密码
   	private String password;
   	//数据源的 JNDI 位置。设置时会忽略类、网址、用户名和密码。
   	private String jndiName;
   	@Deprecated
   	private DataSourceInitializationMode initializationMode = DataSourceInitializationMode.EMBEDDED;
   	//权限
   	@Deprecated
   	private String platform = "all";
   
   	//在启动的时候会帮你执行一个建库和一些操作的一个脚本
   	private List<String> schema;
   	@Deprecated
   	private String schemaUsername;
   	@Deprecated
   	private String schemaPassword;
   	@Deprecated
   	private List<String> data;
   
   	//账号密码
   	@Deprecated
   	private String dataUsername;
   	@Deprecated
   	private String dataPassword;
   	//其他我就不知道了
   	@Deprecated
   	private boolean continueOnError = false;
   	@Deprecated
   	private String separator = ";";
   	@Deprecated
   	private Charset sqlScriptEncoding;
   	private EmbeddedDatabaseConnection embeddedDatabaseConnection;
   	private Xa xa = new Xa();
   	private String uniqueName;
   ```

5. 我们打开一个测试类进行测试一下:

   ```java
   package com.example;
   
   import org.junit.jupiter.api.Test;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.context.SpringBootTest;
   
   import javax.sql.DataSource;
   import java.sql.Connection;
   import java.sql.SQLException;
   
   @SpringBootTest
   class SpringBootDataApplicationTests {
       //这里就是自动帮我们创建好的对象,我们注入后就可以直接使用了!
       @Autowired
       private DataSource dataSource;
       
       @Test
       void contextLoads() throws SQLException {
           //class com.zaxxer.hikari.HikariDataSource:这个数据源相较于P3C0,速度是最快的!也可以理解是SpringBoot的内置数据源
           System.out.println("数据源==>" + dataSource.getClass());
   
           Connection connection = dataSource.getConnection();
           System.out.println("数据库连接==>"+connection);
           connection.close();
       }
   }
   ```

   控制台:

   ![image-20210928153415058](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210928153415058.png)

   ​	结果:我们可以看到它默认给我们配置的数据源为:`class com.zaxxer.hikari.HikariDataSource`,而这个数据源我们并没有手动配置!而获取的连接信息也可以看出是用JDBC的连接实现!

6. 我们既然看到了配置文件对应的properties,那么我们也去看对应的自动配置类,所以全局搜索`DataSourceAutoConfiguration`来看一下:

   ```java
   	@Configuration(proxyBeanMethods = false)
   	@Conditional(PooledDataSourceCondition.class)
   	@ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
   	@Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
   			DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
   			DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
   	protected static class PooledDataSourceConfiguration {
   
   	}
   ```

   可以看出这里数据连接池配置方法这里导入了不同的数据源,不过SpringBoot默认的使用的是第一个`HikariDataSource`作为数据源

   > HikariDataSource号称JAVA WEB当前速度最快的数据源,相较于传统的C3P0、DBCP、Tomcatjdbc等连接池更加优秀；

   既然springBoot默认使用那么我们也可以使用`spring.datasource.type`来指定自定义的数据源类型,值为要使用的连接池实现的完全限定名

### 4.2 JDBCTemplate

在SpringBoot里面有很多`xxxTemplate`,这是一个模板,也就是说springBoot给我们写好的Bean,我们可以直接拿来即用!

1. 首先我们得找到对应JDBC所在目录:

   ![image-20210928161410050](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210928161410050.png)

   2. 然后找到`JdbcTemplateConfiguration`这个类然后点进去:

      ```java
      @Configuration(proxyBeanMethods = false)
      //这里做了一个判断,如果这个Bean不存在,那么这个类就不生效
      @ConditionalOnMissingBean(JdbcOperations.class)
      class JdbcTemplateConfiguration {
      
      	@Bean
      	@Primary
          //这里所需参数是数据源和配置就好了,而数据源springBoot已经帮我们做好了,而配置我们也自定义写好了,所以我们可以直接拿来用
      	JdbcTemplate jdbcTemplate(DataSource dataSource, JdbcProperties properties) {
      		JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
      		JdbcProperties.Template template = properties.getTemplate();
      		jdbcTemplate.setFetchSize(template.getFetchSize());
      		jdbcTemplate.setMaxRows(template.getMaxRows());
      		if (template.getQueryTimeout() != null) {
      			jdbcTemplate.setQueryTimeout((int) template.getQueryTimeout().getSeconds());
      		}
      		return jdbcTemplate;
      	}
      }
      ```

   3. 编写Controller我们来测试一下:

      ```java
      @RestController
      public class JDBCController {
          @Autowired
          private JdbcTemplate jdbcTemplate;
      
          @RequestMapping("/query")
          public List<Map<String,Object>> queryList(){
              String sql = "select * from user";
              //查询返回一个list
              List<Map<String, Object>> query = jdbcTemplate.queryForList(sql);
              return query;
          }
      
          @RequestMapping("/addUser")
          public String addUser(){
              String sql = "insert into user(id,name,pwd) values (2,'小威','2222')";
              jdbcTemplate.update(sql); //这里可以执行SQL,如果我们使用?占位符,那么我们可以在这里多传一个参数为数组,而我们就对应给数组赋值占位符的数据就可以!
              return "ok";
          }
      }
      ```

 ### 4.3 总结:

      1. 有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；
      2. 即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。
      3. 数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。
      4. Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用
      5. JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

 JdbcTemplate主要提供以下几类方法：

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

## 5. 整合Druid

### 5.1 概述

Java程序很大一部分要操作数据库,为了提高性能操作数据库的时候,又不得不使用数据库连接池来管理;

**来源**:Druid是阿里巴巴开源平台上一个数据库连接池实现,它结合了C3P0、DBCP等DB池的优点，同时加入了**日志监控**！

**优点**：Druid可以很好的监控DB池连接和SQL的执行情况,天生就是针对监控而生的DB连接池!

Druid的Github地址:https://github.com/alibaba/druid/

Druid读音为"德鲁伊"

> springBoot 2.0以上默认使用Hikari数据源,可以说Hikari与Druid都是当前JavaWeb上最优秀的数据源!

### 5.2 数据参数

**com.alibaba.druid.pool.DruidDataSource的基本参数:**

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeupdhDcaDPRLHgnULFbaJB5kPtC8n5QVLaUbbTRfa4ZyqficzZYrd2llA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

​	![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeubiciawTdz0tg1EKDjZ1xaIgjRW9CZ4Apr4hvNz3iaQVQIKS3sXy629Lgg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LYLicOHVGnwu7ibGvbwXibYeuaVD6mK3LJrtZ4B6fRKCLDgYicAVGzTUTzWdCNB5lF4tLpbcCT0uq1EA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



### 5.3 配置

1. 我们导入依赖:

   ```xml-dtd
   <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
           <!--阿里巴巴数据库连接池Druid-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.1.21</version>
           </dependency>
   ```

   我们可以来看一下其中的大致源码:

   我们点击`druid-1.1.21.jar/com.alibaba.druid/pool/`可以找到`DruidDataSource`:

   ```java
   public class DruidDataSource extends DruidAbstractDataSource implements DruidDataSourceMBean, ManagedDataSource, Referenceable, Closeable, Cloneable, ConnectionPoolDataSource, MBeanRegistration {
       private static final Log LOG = LogFactory.getLog(DruidDataSource.class);
       private static final long serialVersionUID = 1L;
       private volatile long recycleErrorCount;
       private long connectCount;
       private long closeCount;
       private volatile long connectErrorCount;
       private long recycleCount;
       private long removeAbandonedCount;
       ....
   }
   ```

   可以发现这里面就是我们上面数据参数,而且这里面的还比上面更多,并且这里还设置了初始化参数!

2. 指定连接池(默认是Hikari连接池)

   ```yaml
   spring:
     datasource:
     # 指定连接池
       type: com.alibaba.druid.pool.DruidDataSource
   ```
   
   此时为了验证我们是否指定连接池成功,可以采用自动注入`DruidDataSource`就可以看到:
   
   ```c
   指定Druid数据源==>class com.alibaba.druid.pool.DruidDataSource
   ```
   
3. 然后我们在配置文件中指定Druid的初始参数:

   ```yaml
   spring:
     datasource:
       username: root
       password: root
       # userUnicode:保证不乱码  characterEncoding:设置字符集 serverTimezone:设置时区
       url: jdbc:mysql://localhost:3306/test?userUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
       driver-class-name: com.mysql.cj.jdbc.Driver
       # 指定连接池
       type: com.alibaba.druid.pool.DruidDataSource
   
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       # 初始化大小
       initialSize: 5
       # 最小值
       minIdle: 5
       # 最大值
       maxActive: 20
       # 最大等待时间,配置获取连接等待超时,时间单位是毫秒数ms
       maxWait: 60000
       # 配置间隔多久才进行一次检测,检测需要关闭的空闲连接
       timeBetweenEvictionRunsMillis: 60000
       # 配置一个连接在连接池中最小生存的时间
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
       #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
       filters: stat,wall,log4j
       # 最大PSCache连接
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

   可以看到Druid比其他数据源强大之处是有一个日志监控功能,也就是说从日志监控,那么此时没有log4j我们就无法进行监控功能,所以哦我们需要导入包:

4. 导入Log4J

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

5. 然后我们现在自定义配置给我们DruidDataSource绑定全局配置文件中的参数,然后添加到容器中:

   ```java
   /**
    * @program: springBoot-data
    * @description: Druid连接池全局配置文件自定义
    * @author: MiaoWei
    * @create: 2021-09-29 22:30
    **/
   @Configuration //这个就是对应我们之前的一个个的Bean元素-ApplicationContext.xml中
   public class DruidConfig {
   
       /*
          将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
          绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
          @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
          前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
        */
       //我们需要将yaml配置文件写的参数跟当前文件进行绑定起来
       @ConfigurationProperties(prefix = "spring.datasource")  //绑定配置文件,这里需要指明前缀
       @Bean //放进容器中
       public DataSource druidDataSource() {
           return new DruidDataSource(); 
   }
   ```

   具体这是什么意思呢?其实就是我们把yaml配置文件中对Druid的配置注入到我们同名参数`DruidDataSource`中,然后我们就可以使用Druid配置参数,我们来做一个测试

6. 看测试:

   1. 注释Controller

   ```java
       @ConfigurationProperties(prefix = "spring.datasource") 
      // @Bean 我们只需要注释掉这里,不让注入到Bean容器中去
       public DataSource druidDataSource() {
           return new DruidDataSource();
       }
   ```

   2. 看一眼配置文件:

   ```yaml
   # 最大值
   maxActive: 21
   ```

   3. 执行测试:

   ```java
   @SpringBootTest
   class SpringBootDataApplicationTests {
       @Autowired
       private DataSource dataSource;
       @Autowired
       private DruidDataSource druidDataSource;
       
       @Test
       void contextLoads() throws SQLException {
           System.out.println("默认数据源==>" + dataSource.getClass());
           System.out.println("指定Druid数据源==>"+druidDataSource.getClass());
           int maxActive = druidDataSource.getMaxActive();
           System.out.println("Druid最大连接数==>" + maxActive);
           DruidDataSource source = (DruidDataSource) dataSource;
           System.out.println("DataSource转DruidDataSource最大连接数==>"+source.getMaxActive());
   }
   ```

   4. 打印结果:

   ![image-20210929231343814](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210929231343814.png)

   > 可以发现我们明明设置了最大值可是这里依然还是Druid的默认值,所以我们需要手动将关于Druid的配置注入到DruidDataSource

   然后我们修改Controller:

   ```java
    @ConfigurationProperties(prefix = "spring.datasource") 
       @Bean 
       public DataSource druidDataSource() {
           return new DruidDataSource();
       }
   ```

   再执行看效果:
   ![image-20210930082357555](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930082357555.png)

#### 5.3.1 Druid默认配置源码

   当我没对Druid的配置文件进行绑定的时候,可以看到其默认最大连接数为8,众所周知我们都知道其默认是8,但是我们也只是看博客等文章才知道,今天我们来追寻其源码:

由于Druid是自定义数据源所以我们springBoot并没有对其自动配置,所以我们需要从maven依赖处找到:

![image-20210930084638423](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930084638423.png)

1. 然后找到`com.alibaba.druid.pool`,然后选中`DruidDataSource`这个类,这个类是Druid数据源的配置信息,然后我们全局搜索`maxActive`:

   ```java
   {
       String property = properties.getProperty("druid.maxActive");  //从配置文件中获取属性为druid.maxActive
       if (property != null && property.length() > 0) {
           try {
               int value = Integer.parseInt(property);
               this.setMaxActive(value);  //这里进行set赋值
           } catch (NumberFormatException e) {
               LOG.error("illegal property 'druid.maxActive'", e);
           }
       }
   }
   ```

2. 我们点击`setMaxActive`点进去发现这是一个设值的的一个过程:

   ```java
   public void setMaxActive(int maxActive) {
           if (this.maxActive == maxActive) {
               return;
           }
   
           if (maxActive == 0) { //配置的最大连接数不能为0,否则就会抛出异常
               throw new IllegalArgumentException("maxActive can't not set zero");
           }
   
           if (!inited) {
               this.maxActive = maxActive;
               return;
           }
   
           if (maxActive < this.minIdle) {
               throw new IllegalArgumentException("maxActive less than minIdle, " + maxActive + " < " + this.minIdle);
           }
   
           if (LOG.isInfoEnabled()) {
               LOG.info("maxActive changed : " + this.maxActive + " -> " + maxActive);
           }
   
         ...
       }
   ```

3. 然后我们点击其中的`this.maxActive`看看默认的maxActive参数:

   ```java
   //DruidAbstractDataSource:管理属性的默认赋值
   public abstract class DruidAbstractDataSource extends WrapperAdapter implements DruidAbstractDataSourceMBean, DataSource, DataSourceProxy, Serializable {
           public final static int  DEFAULT_MAX_ACTIVE_SIZE = 8;
           protected volatile int maxActive  = DEFAULT_MAX_ACTIVE_SIZE;//可以发现这里对其属性值进行赋值,那么这里就是默认值,我们点击进去就是8
   }
   
   
   ```



### 5.4 配置Druid数据源监控

Druid提供了一种监控的功能,并提供了一种web页面供我们使用查看!

配置Controller:

```java
@Configuration //这个就是对应我们之前的一个个的Bean元素-ApplicationContext.xml中
public class DruidConfig {
    //实现后台监控功能,web.xml
    //由于这里Servlet中没有内置容器web.xml,所以我们通过这种方式进行注册
    // 1. ServletRegistrationBean:注册一个Servlet的Bean
    @Bean //写完记得要放在Bean容器中,才能被加载进去!
    public ServletRegistrationBean statViewServlet(){
        //配置一个访问路径,也就是我们配置后我们就能进入我们后台监控页面
        ServletRegistrationBean<StatViewServlet> registrationBean = new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");
        //配置后台监控登录账号密码
        HashMap<String, String> map = new HashMap<>();
        //这里loginUsername与loginPassword都是Druid底层写好了的,我们不能改!
        map.put("loginUsername", "admin");
        map.put("loginPassword", "123456");

        //允许谁能访问
        //allow:这是一个固定参数,value值为空表示这里是任何一个人都能访问,如果想指定本机才能访问,那么就可以写"localhost"
        map.put("allow", "");

        //禁止谁能访问
//        map.put("MiaoDaWei", "IP地址");

        registrationBean.setInitParameters(map);//设置初始化参数
        return registrationBean;
    }
}
//这个后台监控相当于就是web.xml,因为spirngBoot内置了servlet容器,所以没有web.xml,而替代方法就是:ServletRegistrationBean
```

好了,我们配置完毕后就可以重新启动了!

浏览器访问:http://localhost:8080/druid

结果:

![image-20210930095811409](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930095811409.png)

在输入对应的账号密码就可以了!里面有很多监控等功能:

![image-20210930095858795](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930095858795.png)

然后我们需要配置Druid web监控filter过滤器

```java

//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
//由于springBoot内置了容器,没有web.xml,所以过滤器的话也是通过xxxRegistration
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    //设置过滤器,WebStatFilter:是阿里巴巴的过滤器
    bean.setFilter(new WebStatFilter());

    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);

    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

> 其实配置这些最大的作用就是为了方便我们查看SQL的执行

### 5.5 解析Druid注入配置文件源码

看过DruidDataSource知道其获取配置参数都是以:

![image-20210930100717663](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930100717663.png)

来进行获取,但是我们在配置文件的时候,并没有找到以druid相关的,也就是我们springBoot并没有对其进行自动配置类,这样无法获取Druid的专属配置,所以我们需要将自定义的Druid数据源添加到容器中:

![image-20210930101904636](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930101904636.png)

这里就是将`spring.datasource`前缀下所有配置将其注入到`DruidDataSorce`里面去,那么我们怎么知道如何进行注入的呢?

来我们看源码:

1. 我们在`DruidDataSource`中找到有关对`maxActive`有关的东西,然后我们通过断点来看看:

   ![image-20210930110534756](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930110534756.png)

   这里意思首先会从配置文件中获取属性`druid.maxActive`的属性,但是没有,获取为null,所以这里是获取失败的!也就进不去里面的方法!

2. 我们点击`this.setMaxActive()`然后打断点试试:

   ![image-20210930110740502](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930110740502.png)

   咦,看到没有,这里传的数字是21,明显对应了我们之前在配置文件中写的,说明我们把配置文件中的参数进行注入到我们`DruidDataSource`中去了,并且对我们Druid中的数据参数进行重新赋值!

3. 思考:这里的`setMaxActive`全局搜索一下只有上面获取配置文件时才调用了一次,那么这里为什么能传参呢?我们在`setMaxActive`方法里执行完毕执行return,看看:

   ![image-20210930114242797](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210930114242797.png)

   哦哦哦,我看到`invoke`我就知道这是由反射进行创建的,当前类是:`JavaBeanBinder`

4. 由此可见,我们就能大致能得出一个结论就是我们的注入参数其实就是通过反射进行创建进行配置的!

## 6.整合mybatis

我们之前使用SSM进行整合的时候也是使用mybatis的整合包,那么我们今天使用springBoot进行整合mybatis的时候,也是有一个整合包!

整合依赖:

```xml-dtd
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

这是对应的官方文档:http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址:https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1

思考:可以发现这里`mybatis-spring-boot-starter`这个启动器跟我们之前的启动器是不一样,之前都是`spring-boot-starter-xxx`!

**解答**:因为`mybatis-spring-boot-starter`这是mybatis官方的,是mybatis自己研发出来的!,而以`spring-boot-starter-xxx`这个格式的都是springBoot官方自己的!

### 6.1 快速使用

1. 我们创建对应的实体类:

   ```java
   @Data
   public class User {
       private Integer id;
       private String name;
       private String pwd;
   }
   ```

2. 创建mapper

   ```java
   @Repository //用@Compont也是可以,但是还是要分层思想
   @Mapper  //这个注解表示了这是一个mybatis的mapper类
   //也可以在启动类上加上注解 @MapperScan("mapper所在的包路径"),就会去扫描指定包下的mapper都会被加载进去
   public interface UserMapper {
       List<User> queryList();
   }
   ```

   这里两个注解都不能少,不加`@Repository`表示无法注入到Spring容器中,不加`@Mapper`就会失效,无法进行映射管理器,这两个是核心注解!

   其中@Mapper相当于我们之前SSM整合mybatis的时候在applicationContext.xml中的:

   ```xml-dtd
   <!--4.扫描mapper接口:spring就会创建mapper接口的对象-->
   <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <property name="basePackage" value="org.learn.mapper"/>
   </bean>
   ```

3. 创建mapper.xml

   我们在resource资源目录下创建:

   ```xml-dtd
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.example.mapper.UserMapper">
   
       <select id="queryList" resultType="com.example.pojo.User">
           select * from user
       </select>
   
   </mapper>
   ```

4. 创建Controller:

   ```java
   @RestController
   public class UserController {
       @Autowired
       private UserMapper userMapper;
   
       @GetMapping("/test")
       public List<User> queryList(){
           List<User> users = userMapper.queryList();
           return users;
       }
   }
   ```

5. yaml配置文件:

   ```yaml
   # 整合mybatis
   mybatis:
     # 别名
     type-aliases-package: com.example.pojo
     # mapper地址  这里是以/分隔 表示这是文件夹分隔
     mapper-locations: classpath:mybatis/mapper/*.xml
   ```

   注意这里就等同于我们之前SSM整合mybatis的是一致的:

   ```xml-dtd
   <!--3.配置SqlSessionFactoryBean 进行整合mybatis-->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <!--3.1注入连接池-->
           <property name="dataSource" ref="dataSource"/>
           <!--3.2注入:找到对应的mapper.xml加入到映射文件中-->
           <property name="mapperLocations" value="classpath:org.learn.mapper/*Mapper.xml"/>
           <!--3.3注入:将指定包下的类进行批量别名,首字母小写-->
           <property name="typeAliasesPackage" value="org.learn.domain"/>
       </bean>
   ```

6. 执行效果:

   ![image-20211001144105590](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211001144105590.png)

   > 可以发现并不难,就是核心注解和对应的配置文件,就可以快速的整合mybatis了!

