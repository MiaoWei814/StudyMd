# 笔记

## 1.简介

在web开发中,安全一直都是非常重要的一个方面!而我们之前做安全比如登录的时候考虑用户是否存在登录,那么我们就使用拦截器和过滤器进行实现!安全是非功能性需求,但是重要程度不言而喻!

如果说安全有问题的话,那么就可能存在漏洞,那么用户隐私泄漏~,

我们去看一下SpringSecurity官方解释:

```java
Spring Security is a powerful and highly customizable authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.

Spring Security is a framework that focuses on providing both authentication and authorization to Java applications. Like all Spring projects, the real power of Spring Security is found in how easily it can be extended to meet custom requirements
    
翻译:
Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架,它实际上是保护基于spring的应用程序的标准

spring Security是一个框架,侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样,Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求
```

​	从官网的介绍中我们可以得出这是一个权限框架,想我们之前做项目是没有使用框架是怎么控制权限的?使用Spring的AOP横切技术和拦截器实现的,而对于权限一般为细分为**功能权限**、**访问权限**和**菜单权限**,如果我们还依然采用原始的那种权限控制那么就会造成我们代码会写得非常的繁琐、冗余!



那么如何解决以上的问题呢?

​	一些主流框架就应运而生而Spring Scecurity就是其中一种。Spring是一个非常流行且成功的Java应用开发框架,Spring Security基于Spring框架,提供了一套web应用安全性的`完整解决方案`,一般来说,Web应用的安全性包括**用户认证**(Authentication)和**用户授权**(Authorization)两个部分，

> 用户认证：指验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统，而用户认证一般要求用户提供用户名和密码，然后系统通过校验用户名和密码来完成认证过程！
>
> 用户授权：指验证某个用户是否有权限执行某个操作，在一个系统中，不同用户具有的权限是不同的，一般系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限

而Spring Security框架对其都对其进行很好的支持,比如:

1. 用户认证:包括HTTP基本认证、HTTP表单认证、HTTP摘要认证、OpenID和LDAP等;
2. 用户授权:基于角色的访问控制和访问控制列表(Access Control List,ACL),对应用中的领域对象进行细粒度的控制!



> spring Security的出现就是为了简化大量原生代码的实现,比如拦截器,过滤器!核心就是提供了认证访问



### 1.1 小结

SpringSecurity是针对Spring项目的安全框架,也是SpringBoot底层安全模块默认的技术选型,他可以实现强大的Web安全控制,对于安全控制,我们仅需要引入spring-boot-starter-security模块,进行少量的配置,即可实现强大的安全管理!

记住这几个类:

- WebSecurityConfigurerAdapter:自定义Security策略
- AuthenticationManagerBuilder:自定义认证策略
- @EnableWebSecurity:开启WebSecurity模式

Spring Security的两个主要目标是"认证"和"授权"(访问控制)

- "认证"(Authentication)
- "授权"(Authorization)

## 2. 认证和授权

来吧,我们快速的使用看看效果:

导入依赖:

```xml-dtd
<!--spring Security-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 2.1 授权

1. 编写Spring Security配置类:

   ```java
   //理解AOP的好处,横切进去,不改变原有代码就能实现
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
       //可以链式编程
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           //首页所有人都可以访问,但功能页只能有对应权限的人才能访问
           //增加认证请求
           http.authorizeRequests()
                   //添加一个地址,所有人都可以访问
                   .antMatchers("/").permitAll()
                   .antMatchers("/level1/**").hasRole("vip1")
                   .antMatchers("/level2/**").hasRole("vip2")
                   .antMatchers("/level3/**").hasRole("vip3");
       }
   }
   ```

   此时我们一个简单的认证demo就做好了,我们进页面试试,看看会不会被拦截下来

2. 首页访问:

   ![image-20211001204802821](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211001204802821.png)

   > 可以看见已经被拦截下来了,此时报了异常为403,表示无权限!

   那么我们不想跳这种异常页面,那么此时我们就可以这样:

3. 在configure()方法中加入以下配置，开启自动配置的登录功能!

   ```java
   // 开启自动配置的登录功能
   // /login 请求来到登录页
   // /login?error 重定向到这里表示登录失败
   http.formLogin();//也可以进行链式编程,跟下面默认页面的链式是一样的
   ```

4. 此时页面:

   ![image-20211001210002432](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211001210002432.png)

   但是可以发现,这个页面并不是我们手写的,而是自动跳转的,这是spring Security默认的

5. 为什么会自动跳转?

   我们查看formLogin的注释信息可以得出:

   ![image-20211001210140566](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211001210140566.png)

   没指定就会是默认页面,那么我们可以指定页面的,如:

   ```java
    ....antMatchers("/level3/**").hasRole("vip3").and().formLogin().loginPage("/toLogin"); //没权限跳转指定页面!
   ```

   

### 2.2 认证

1. 编写Spring Security配置类:

   ```java
   //理解AOP的好处,横切进去,不改变原有代码就能实现
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
       @Override
       //认证
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           //可以从内存拿可以从jdbc中去拿
           //auth.jdbcAuthentication() jdbc中拿
   //        auth.inMemoryAuthentication() 内存中拿
           //从内存中获取-从哪个用户-密码-获取权限(本身是数组,可设置多个)
           auth.inMemoryAuthentication().withUser("miao").password("123456").roles("vip2", "vip3")
                   //多个用户以and中间拼接
                   .and()
                   .withUser("root").password("123456").roles("vip1", "vip2", "vip3")
                   .and()
                   .withUser("guest").password("123456").roles("vip1");
       }
   }
   ```

2. 页面访问试试:

   我们还没登录进首页然后随便点一个level1然后突然后端报错了!

   ![image-20211001212134934](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211001212134934.png)

   根据报错信息说"这个密码没对其加密"

   > 也即是说Spring Security认为这密码没加密是不安全的,因为会被反编译class文件是可以看见的!叫我们去硬编码这个密码

3. 解决办法(我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码):

   ```java
    @Override
       //认证
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           //可以从内存拿可以从jdbc中去拿
           //auth.jdbcAuthentication() jdbc中拿
   		//auth.inMemoryAuthentication() 内存中拿
           //从内存中获取-从哪个用户-密码-获取权限(本身是数组,可设置多个)
           //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
      		//要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
      		//spring security 官方推荐的是使用bcrypt加密方式。
           auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder()) //这个也是较为推荐的
                   .withUser("miao").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                   //多个用户以and中间拼接
                   .and()
                   .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                   .and()
                   .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
       }
   ```

4. 测试OK了!

## 3. 权限控制和注销

### 3.1 注销

1. 首页我们在前端静态页面中写个按钮表示是注销:

   ```html
   <!--注销-->
   <a class="item" th:href="@{/logout}"> 
       <i class="sign-out icon"></i> 注销
   </a>
   ```

   这里跳转路径不是乱写的,而是根据注销的功能里人家写好了的:

   ![image-20211002090338267](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002090338267.png)

2. 在自动配置类中开启注销的功能:

   ```java
   @Override
       protected void configure(HttpSecurity http) throws Exception {
           ...
           //开启了注销的功能
           http.logout();
       }
   ```

   这样我们注销功能就已经做好了,但是这里注销后跳转到的是人家写的页面,那我们也想跳转我们的页面,那么

3. 对注销后能跳转到指定页面:

   ```java
    //注销,开启了注销功能,注销成功跳转到首页
   http.logout().logoutSuccessUrl("/");  //跳转到首页
   ```

### 3.2 权限控制

现在就是说我们登录一个页面,有些东西需要的权限很高,而我登录的权限很低,那么这个时候我们需要将其隐藏,只显示我们当前权限只有的功能!

那么我们可以跟`thymeleaf`一起搭配使用!

1. 导入thymeleaf跟springSecurity整合的依赖:

   ```xml-dtd
   <!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
   <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-springsecurity5</artifactId>
      <version>3.0.4.RELEASE</version>
   </dependency>
   ```

2. 然后在前端页面添加约束命名空间

   ```xml-dtd
   xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
   ```

   注意:使用上面的依赖,貌似在idea没有代码提示,我查找了一下,换成这个就可以了:

   ```xml-dtd
   xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
   ```

3. 页面上使用:

   ```html
   				<!--判断当前用户是否已经登录过认证过-->
                   <div sec:authorize="!isAuthenticated()">
                       <!--未登录-->
                       <a class="item" th:href="@{/toLogin}">
                           <i class="address card icon"></i> 登录
                       </a>
                   </div>
                   <!--当前是认证过-->
                   <div sec:authorize="isAuthenticated()">
                       <a class="item">
                           <i class="address card icon"></i>
                           用户名: <span sec:authentication="name"></span>&nbsp;
                           角色: <span sec:authentication="principal.authorities"></span>
                       </a>
                   </div>
                   <div sec:authorize="isAuthenticated()">
                       <!--注销-->
                       <a class="item" th:href="@{/logout}">
                           <i class="sign-out icon"></i> 注销
                       </a>
                   </div> 
   ```

4. 此时我们看页面效果:

   ![image-20211002095143019](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002095143019.png)

   > 可以说这里我们就已经能看到我们想要的效果了!

5. 如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

   ```java
   http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
   http.logout().logoutSuccessUrl("/");
   ```

6. 既然我们能控制未登录和注销这两个按钮的显示隐藏,那么我们也能对某个权限才有的功能进行显示:

   ```html
    <!--有vip1的权限的才会解开-->
               <div class="column" sec:authorize="hasRole('vip1')">
                   <div class="ui raised segment">
                       <div class="ui">
                           <div class="content">
                               <h5 class="content">Level 1</h5>
                               <hr>
                               <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                               <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                               <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
                           </div>
                       </div>
                   </div>
               </div>
   ```

7. 看效果:

   ![image-20211002101055526](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002101055526.png)

## 4.记住我

有些网站在开始登录的时候有个小选框表示记住我,这样每次进来都会重新去登录,那么我们这里该如何去实现呢?

1. 用Security实现记住我:

   ```java
   @Override
       protected void configure(HttpSecurity http) throws Exception {
           //记住我,默认存储为14天
           http.rememberMe();
       }
   ```

2. 开启了这个功能,我们在登录的状态下,哪怕关闭浏览器然后重新进入依然保持登录的状态!

3. 思考?这个是什么机制如何实现的呢?

   我们在登录后在F12控制台查看:

   ![image-20211002102615030](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002102615030.png)

   可以发现这里有个我们登录的cookie,然后看它的最大时间计算后得出14天,也就是说这个cookie会默认存储14天,

4. 我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

   ![image-20211002102853828](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002102853828.png)

5. 结论：

   > 登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，

## 5.定制登录页

我们之前的登录都是要通过spring Secrity默认的,但是在实际开发中不可能用它默认的页面,那么如何去自定义我们的页面呢?

1. 在刚才的登录页配置后面指定 `loginpage`

   ```java
   http.formLogin().loginPage("/toLogin"); //将原来默认的login改为toLogin,表示没有权限就会默认访问这个地址,现在去访问login就会是404
   ```

2. 指明登录的页面:

   ```html
   <!--未登录-->
   <a class="item" th:href="@{/toLogin}">
       <i class="address card icon"></i> 登录
   </a>
   ```

3. 前端登录页将请求地址改为:

   ```html
    <form th:action="@{/toLogin}" method="post"> //这里请求方式必须是post,否则不生效
        ...
   </form>
   ```

   这是人家源码文档中告诉:

   ![image-20211002105321913](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002105321913.png)

4. 好的目前来说就可以了!就可以实现自定义登录页!

### 5.1 自定义登录跳转

思考?那如果说我在登录的时候不想跳转`/toLogin`,就想自己定义一个行不行?答案是行!

```html
<form th:action="@{/login}" method="post"></form>
```

Security配置类里也要更改一下:

```java
http.formLogin().loginPage("/toLogin").loginProcessingUrl("/login");// 登陆表单提交请求
```

这样就可以了!

### 5.2 登录参数请求

我们在前端form表单登录请求的时候,有一个坑就是上传的参数名必须为`username`,`password`,而我们前端就想传一个name和pwd这两个字段怎么办呢?

这是源码文档中描述:

![image-20211002110201930](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211002110201930.png)

​	意思就是说请求到这里默认参数名为`username`和`password`这两个!

​	那如何修改呢?官网也已经给了提示了,我们只需要添加`usernameParameter`和`passwordParameter`就可以了!

```java
http.formLogin()
                .loginPage("/toLogin")
                .loginProcessingUrl("/login")// 登陆表单提交请求
                .usernameParameter("name")
                .passwordParameter("pwd");
```

这是前端代码:

```html
<form th:action="@{/login}" method="post">
    <div class="field">
        <label>Username</label>
        <div class="ui left icon input">
            <input type="text" placeholder="Username" name="name"> //修改了用户名字段
            <i class="user icon"></i>
        </div>
    </div>
    <div class="field">
        <label>Password</label>
        <div class="ui left icon input">
            <input type="password" name="pwd">  //修改密码字段
            <i class="lock icon"></i>
        </div>
    </div>
    <input type="submit" class="ui blue submit button"/>
</form>
```



### 5.3 记住我

我们还没有实现记住我的功能,我们在前端写一个记住我:

```html
<div class="field">
    <input type="checkbox" name="remember">记住我
</div>
```

后台接收:

```java
//定制记住我的参数！
http.rememberMe().rememberMeParameter("remember");
```

完美!测试也没问题!
