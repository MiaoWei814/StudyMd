# Swagger笔记

## 1. 简介

不分离:

- 我们之前做项目的时候无论前端后端都在一个项目上进行开发,那个时候是处于后端时代,前端只需要提供静态页面HTML,而后端不仅写完数据接口,还要对静态页面进行模板渲染,比如JSP!

分离:

- 后端:后端只需要专心写控制层、服务层、数据访问层【后端团队】
- 前端:前端只需要负责控制层、视图层【前端团队】
  - 而前端团队可以伪造后端数据,就是json,也就是说不需要后端也依然前端工程就能跑起来!
- 前后端如何进行交互?==>通过API文档进行交互
- 前后端相对独立,松耦合,两者之间这是通过调接口获取数据
- 前后端甚至可以部署在不同的服务器上!

那这个时候就会产生一个问题?

- 前后端集成联调,前端人员无法和后端人员无法做到及时协商,导致问题集中爆发!

解决方案:

- 首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险
- 早些年:指定word计划文档
- 现在前后端分离:
  - 前端测试后端接口:postman第三方工具
  - 后端提供接口,需要实时更新最新的消息及改动!

**Swagger**:

1. 号称世界上最流行的Api框架;
2. RestFul Api 文档在线自动生成工具,==>**API文档与Api定义同步更新**

3. 直接运行,可以在线测试APi接口;
4. 支持多种语言:(Java,Php...)

这是官网地址:https://swagger.io/

> 好处就是用swagger在线自动生成、同步更新、测试接口、直接运行；其实Swagger本身来讲就是一个Api文档,供前后端联调查看!不过也流行一些第三方的接口文档,比如Yapi等！

## 2.springBoot集成Swagger

### 2.1 快速使用

1. 导入依赖:

   ```xml-dtd
   <!--开启swagger-->
           <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger2</artifactId>
               <version>2.9.2</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger-ui</artifactId>
               <version>2.9.2</version>
           </dependency>
   ```

2. 我们写一个helloController

   ```java
   @RestController
   public class HelloController {
   
       @GetMapping({"/","/index"})
       public String hello() {
           return "hello Swagger";
       }
   }
   ```

3. 新建配置开启Swagger

   ```java
   @Configuration  //这个注解表示这是一个配置类,点开注解发现有@Component,就是说会将当前类注入到spring容器中!
   @EnableSwagger2 // 开启Swagger2的自动配置
   public class SwaggerConfig {
       //这里暂时为空,也没事,因为有默认值
   }
   ```

   注意这里注解`@EnableSwagger2`表示开启Swagger2的自动配置,但是有个坑要踩一下:就是我导入`springfox-swagger2`的版本得是2.9.2的,往上这个注解就不存在了!\

4. 页面输入"http://localhost:8080/swagger-ui.html":

   ![image-20211003101401716](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003101401716.png)

> 暂时可以说我们可以看见swaager的页面了!

### 2.2 配置swaager

由于Swagger实例Bean是`Docket`,所以我们需要通过配置返回这个实例!

```java
@Bean //配置docket来配置swagger具体参数
public Docket docket() {
    //参数是通过构造方法所需参数所得
    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
}
```

返回apiInfo接口 信息:

```java
public ApiInfo apiInfo() {
        //作者描述信息: 作者名称-访问链接-作者邮箱
        Contact contact = new Contact("MiaoWei", "https://www.baidu.com", "24391351222qq.com");
        return new ApiInfo("" +
                "MiaoDa威的文档",  //标题
                "学习如何配置swagger", //描述
                "v1.0", //版本号
                "https://www.baidu.com",//组织链接
                contact,  // 联系人信息
                "Apache 2.0", //许可证
                "http://www.apache.org/licenses/LICENSE-2.0", //许可链接
                new ArrayList<>());  //扩展
}
```

我们如果不配置,那么在源码底层人家就会默认给出这些信息:

![image-20211003104249455](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003104249455.png)

> 就有一个默认的配置信息

看页面效果:

![image-20211003104317610](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003104317610.png)

### 2.3 配置扫描接口

Swaager的扫描接口是通过`Docket select()`进行链式操作!

```java
@Bean //配置docket来配置swagger具体参数
    public Docket docket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
            	//RequestHandlerSelectors:配置扫描接口的方式
                .apis(RequestHandlerSelectors.basePackage("com.miao.controller"))
            	//过滤掉只有请求包含"/miao/**"下的东西
                .paths(PathSelectors.ant("/miao/**"))
                .build();  //可以知道这是一个工厂模式!
    }
```

我们来具体分析一下:

1. 首先我们在select()进行链式的时候只有两个参数只能用,分别是`apis()`和`paths()`!

   1. apis():

      - 接收参数为`RequestHandler`这是一个接口,这个时候我们选择使用它的实现类`RequestHandlerSelectors`,那这个实现类什么意思呢?**配置扫描接口的方式**!

      - 这是RequestHandlerSelectors扫描接口的方式都有哪些?

        1. any:扫描全部
        2. basePackage:基于某个包去扫描
        3. none:都不扫描
        4. withClassAnnotation:扫描类上的注解(这里参数不是类的class,而是注解的反射对象,比如RequestMapping.class)
        5. witMethodAnnotation:扫描方法上的注解(比如GetMapping.class)

      - 这是对应的源码扫描:

        ```java
        public class RequestHandlerSelectors {
            public static Predicate<RequestHandler> any() {
            	return Predicates.alwaysTrue();
          	}
            public static Predicate<RequestHandler> none() {
            	return Predicates.alwaysFalse();
          	}
            public static Predicate<RequestHandler> withMethodAnnotation(final Class<? extends Annotation> annotation) {
            	return new Predicate<RequestHandler>() {
              		@Override
                    public boolean apply(RequestHandler input) {
                        return input.isAnnotatedWith(annotation);
                    }
            	};
          	}
            public static Predicate<RequestHandler> withClassAnnotation(final Class<? extends Annotation> annotation) {
                return new Predicate<RequestHandler>() {
                  @Override
                  public boolean apply(RequestHandler input) {
                    return declaringClass(input).transform(annotationPresent(annotation)).or(false);
                  }
                };
          	}
            public static Predicate<RequestHandler> basePackage(final String basePackage) {
            	return new Predicate<RequestHandler>() {
              	@Override
              	public boolean apply(RequestHandler input) {
                	return declaringClass(input).transform(handlerPackage(basePackage)).or(true);
             	 }
           	 };
          }
        }
        ```

   2. paths():

      - 过滤什么路径

      - 所需参数是`Predicate<String> selector`,我们查看源码发现这里默认赋值:

        ```java
        private Predicate<String> pathSelector = ApiSelector.DEFAULT.getPathSelector();
        ```

      - 所以我们可以直接写参数为`PathSelectors`

      - 这是PathSelectors所需要的参数都有哪些?

        1. ant:过滤指定路径
        2. any:过滤全部的
        3. none:不过滤全部的
        4. regex:通过正则表达式进行过滤

### 2.4 根据环境切换是否启动

我们要根据当前springBoot的环境进行切换,比如说生产环境下可以查看文档,而发布环境下就不启动swagger

我们查看源码可以发现我们可以通过一个属性就可以控制是否启用:

```java
public class Docket implements DocumentationPlugin {
    ...
   //enable是否启动Swagger,如果为false,则Swagger不能在浏览器中访问
   private boolean enabled = true;
    ...
}
```

如果我们设置为false,那么页面就是这样:

![image-20211003143732258](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003143732258.png)

那么思考?我只希望我的Swagger在生产环境中使用,在发布的时候不使用?

- 判断是不是生产环境,设置标志flag=false
- 注入enable(flag)

我们可以接收一个参数为`Environment`对象,我们就能获取当前环境!

如:

```java
import org.springframework.core.env.Environment;
...
@Bean 
public Docket docket(Environment environment) {
    //environment:获取环境
    String property = environment.getProperty("server.port"); //获取属性值
    String[] activeProfiles = environment.getActiveProfiles(); //获取切换的环境
    System.out.println("activeProfiles = " + Arrays.toString(activeProfiles));
    System.out.println("property = " + property);
}
```

打印台:

![image-20211003144901416](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003144901416.png)

我们获取环境有三个方式:

1. getActiveProfiles:获取选中的环境

2. getDefaultProfiles:获取默认的环境

3. acceptsProfiles:监听当前环境

而我们使用的就是第三种方式:

```java
@Bean //配置docket来配置swagger具体参数
    public Docket docket(Environment environment) {
        //environment:获取环境
        //设置要显示的Swagger环境
        Profiles dev = Profiles.of("dev","test"); //这里就是我们想要的环境
        //通过acceptsProfiles对象环境监听变量是否处于自己设定的环境当中
        boolean flag = environment.acceptsProfiles(dev); //如果是就会返回true,否则就是false

        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .enable(flag)
                .select()
                 //RequestHandlerSelectors:配置扫描接口的方式
                .apis(RequestHandlerSelectors.withMethodAnnotation(RequestMapping.class))
                //过滤掉只有请求包含"/miao/**"下的东西
                .paths(PathSelectors.ant("/miao/**"))
                .build();  //可以知道这是一个工厂模式!
    }
```

现在我们通过*spring.profiles.active=dev*切换dev环境中去了,而dev环境的端口已经改为8082,而我们默认的8080,现在访问看看:

不是dev环境:

![image-20211003152039849](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003152039849.png)

是dev环境:

![image-20211003152056099](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003152056099.png)

> 注意:这个开关关乎着整个swaager的开关!也就是哪怕我们下面的分组,不同的Docket,只要其中一个Docket中开关设置为false,那么整个swagger都会关闭

#### 2.4.1 接口中获取配置文件中属性

我们可以通过`Environment`来获取当前环境中的属性值进行相应的操作!

```java
import org.springframework.core.env.Environment;
@RestController
public class HelloController {

    @Autowired
    Environment env;

    @GetMapping({"/","/index"})
    public String hello() {
        String[] strings = env.getActiveProfiles();
        System.out.println(Arrays.toString(strings));
        return "hello Swagger";
    }
}
```

控制台:

![image-20211003150407853](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003150407853.png)

> 注意:必须是通过注入的方式才能获取,如果是直接放在方法的接收参数中的话,就会报错!如:

![image-20211003150535123](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003150535123.png)



这就是第一种获取我们配置文件的方法,还有一种方法就是通过注解的形式进行获取*@ConfigurationProperties*,这个详细描述在另一篇笔记中,在springBoot的简述和相关原理的注入那一节!



### 2.5 配置API文档的分组

分组很简单,我们在swagger页面上可以发现:

![image-20211003152749347](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003152749347.png)

> 通常一个Docket实例对应一个分组!那么也就意味着我们在实际开发过程中我们每个开发人员为自己写的接口写一个Docket,也就分组,这样就会方便切换我们指定的人写的接口进行测试

如果没有配置分组，默认是default。通过groupName()方法即可配置分组：

我们多加几个Docket测试一下:

```java
//第一个
@Bean
public Docket docket1() {
    return new Docket(DocumentationType.SWAGGER_2).groupName("A");
}
//第二个
@Bean
public Docket docket2() {
    return new Docket(DocumentationType.SWAGGER_2).groupName("B");
}
```

看效果:

![image-20211003153526628](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003153526628.png)

### 2.6 实体配置

![image-20211003155120636](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003155120636.png)

来介绍这个models实体类的配置,也就是我们既然写进接口那么我们交互就是json,而我们后端就是将对象转为json,所以这里我们将展示接口的返回实体类信息:

那Swagger与model如何产生联系呢?

**我们只需要在接口的返回是一个对象,而swagger扫描接口的就会返回加载进去!**

1. 新建接口:

   ```java
   @GetMapping("/hello1")
   public User hello1(){
       return new User();
   }
   ```

2. 新建实体类

   ```java
   public class User {
       private String name;
       private String pssword;
   
       public String getName() {
           return name;
       }
   
       public String getPssword() {
           return pssword;
       }
   }
   ```

   *注意*:Swagger是通过JavaBean的方式进行访问,也就是说这里要通过getxxx方式进行获取到的,否则就会获取不到!

3. 页面:

   ![image-20211003155642091](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003155642091.png)

> 但是这样并不好看,我们得加上注解对这个实体类进行描述一下:

修改后的实体类:

```java
@ApiModel(value="User",description = "用户实体类")
public class User {
    @ApiModelProperty(value = "用户名")
    private String name;
    @ApiModelProperty(value = "密码")
    private String pssword;
    ...
}
```

看效果:

![image-20211003160010020](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003160010020.png)

注:并不是因为*@ApiModel*这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而*@ApiModel*和*@ApiModelProperty*这两个注解只是为实体添加注释的。

- @ApiModel为类添加注释

  ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/397648-20200518152915502-1667156516.png)

- @ApiModelProperty为类属性添加注释

  ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/397648-20200518152933653-887862377.png)



其他注解:

|                    **Swagger注解**                     |                       简单说明                       |
| :----------------------------------------------------: | :--------------------------------------------------: |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
|                @ApiParam("xxx参数说明")                |   作用在参数、方法和字段上，类似@ApiModelProperty    |
|               @Api(tags = "xxx模块说明")               |                    作用在模块类上                    |
|              @ApiOperation("xxx接口说明")              |                   作用在接口方法上                   |
|                @ApiModel("xxxPOJO说明")                |               作用在模型类上：如VO、BO               |

swagger也可以进行测试:

比如说这里并没有参数,直接可以开测:

![image-20211003162125153](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003162125153.png)

然后点击执行测试:

![image-20211003162202420](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003162202420.png)

这就是swagger测试!

### 2.7 换皮肤

1. 我们默认的皮肤：页面访问:http://localhost:8080/swagger-ui.html

   ```xml-dtd
   dependency>
       <groupId>io.springfox</groupId>
       <artifactId>springfox-swagger-ui</artifactId>
       <version>2.9.2</version>
   </dependency>
   ```

   ![image-20211003162732952](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003162732952.png)

2. bootstrap-ui：页面访问:http://localhost:8080/doc.html

   ```xml-dtd
   <!-- 引入swagger-bootstrap-ui包 /doc.html-->
   <dependency>
       <groupId>com.github.xiaoymin</groupId>
       <artifactId>swagger-bootstrap-ui</artifactId>
       <version>1.9.1</version>
   </dependency>
   ```

   ![image-20211003163034490](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003163034490.png)

   > 好看,推荐!

3. Layui-ui ：

   ```xml-dtd
   <!-- 引入swagger-ui-layer包 /docs.html-->
   <dependency>
      <groupId>com.github.caspar-chen</groupId>
      <artifactId>swagger-ui-layer</artifactId>
      <version>1.1.3</version>
   </dependency>
   ```

   效果:

   ![img](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7IExpkhknhzRFQicsic8yibm9ZYA6g5VyspYIqFMokAGg7dbx47P2ibC8Z80saA7XdrByPFhgmrduSHbA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

4. mg-ui：

   ```xml-dtd
   <!-- 引入swagger-ui-layer包 /document.html-->
           <dependency>
               <groupId>com.zyplayer</groupId>
               <artifactId>swagger-mg-ui</artifactId>
               <version>1.0.6</version>
           </dependency>
   ```

   ![image-20211003164005247](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211003164005247.png)

### 2.8 小结

1. 我们可以通过 Swagger给一些比较难理解的属性或者接口，增加注释信息
2. 接口文档实时更新
3. 可以在线测试

Swagger是一个优秀的工具,几乎所有的大公司都会使用它!

【注意点】：在正式发布的时候必须关闭Swagger！因为一旦没关闭用户就能知道你所有的接口然后还能进行测试，并且处于安全考虑，每次都会生成日志在发布环境中就会影响内存！

> 它的诞生就是为了更好的前后端分离！



