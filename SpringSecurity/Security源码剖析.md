# 笔记

## 1. 入门案例

1. 导入依赖:

   ```xml-dtd
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.0.5.RELEASE</version>
     </parent>
   
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
     </properties>
   
     <dependencies>
   <!--这里引入security-->
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-security</artifactId>
       </dependency>
   
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
       </dependency>
   
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
     </dependencies>
   ```

2. 主配置类:

   ```java
   @SpringBootApplication
   public class ApplicationConfig {
       public static void main(String[] args) {
           SpringApplication.run(ApplicationConfig.class);
       }
   }
   ```

3. web控制器(当用户认证成功之后会重定向到该方法，返回“登录成功”给用户)

   ```java
   @Controller
   public class AuthController {
   
   	//登录成功后重定向地址
       @RequestMapping("/loginSuccess")
       @ResponseBody
       public String loginSuccess(){
           return "登录成功";
   	} 
   }
   ```

4. 配置security

   ```java
   @Configuration
   @EnableWebSecurity
   //SpringSecurity提供了一个配置类WebSecurityConfigurerAdapter用来提供给程序员对SpringSecurity做自定义配置
   public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
   
        //提供用户信息，这里没有从数据库查询用户信息，在内存中模拟,
       @Bean
       public UserDetailsService userDetailsService(){
           InMemoryUserDetailsManager inMemoryUserDetailsManager = 
           new InMemoryUserDetailsManager();
           inMemoryUserDetailsManager.createUser(User.withUsername("zs").password("123").authorities("admin").build());
           return inMemoryUserDetailsManager;
       }
     
   
       //密码编码器：不加密,通过该编码器对密码进行加密匹配
       @Bean
       public PasswordEncoder passwordEncoder(){
           return NoOpPasswordEncoder.getInstance();
       }
       
       //授权规则配置,哪些资源需要什么权限..
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http.authorizeRequests()                                //授权配置
                   .antMatchers("/login").permitAll()  			//登录路径放行
                   .anyRequest().authenticated()                   //其他路径都要认证之后才能访问
                   .and().formLogin()                              //允许表单登录
                   .successForwardUrl("/loginSuccess")             // 设置登陆成功页
                   .and().logout().permitAll()                    //登出路径放行 /logout
                   .and().csrf().disable();                        //关闭跨域伪造检查
       }
   }
   ```

5. 测试:

   ![image-20211204141056170](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204141056170.png)

   1. 输入刚刚基于内存的用户信息:zs,123

      ![image-20211204141222544](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204141222544.png)

   2. 随便输入账号和密码,那么再看效果:

      ![image-20211204141251024](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204141251024.png)

### 1.1 小结思路

这个要认识一个 新单词:`AuthenticationManager` 读音:哦声t ke性麻了杰

这个案例实现思路:

![image-20211204141625442](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204141625442.png)

大概步骤是:

1. SpringSecurity根据我们在WebSecurityConfig中的配置会对除了“/login”之外的资源进行拦截做认证检查
2. 如果没有认证会跳转到默认的认证页面“/login”
3. 输入用户名和密码后点击登录，SpringSecurity会带着用户名调用 `UserDetailsService.loadUserByUsername`获取用户的认证信息(用户名，密码，权限等)
4. 然后执行认证工作：表单密码和loadUserByUsername加载的数据库的密码进行匹配(PasswordEncoder)
5. 认证成功跳转成功地址

## 2.认证与授权概念

SpringSecurity是基于Filter实现认证和授权，底层通过`FilterChainProxy`代理去调用各种Filter(Filter链)，

1. Filter通过调用`AuthenticationManager`完成认证 ，

2. 通过调用`AccessDecisionManager`完成授权

大概图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/wps2DE2.tmp.jpg)

我们知道，SpringSecurity是通过很多的过滤器链共同协作完成认证，授权的流程，SpringSecurity中核心的过滤器链如下:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/wps9AD1.tmp.jpg)

//每个过滤器链都是什么意思?
1. SecurityContextPersistenceFilter

   ```
   这个filter是整个filter链的入口和出口，请求开始会从SecurityContextRepository中	获取SecurityContext对象并设置给SecurityContextHolder。在请求完成后将SecurityContextHolder持有的SecurityContext再保存到配置好的SecurityContextRepository中，同时清除SecurityContextHolder中的SecurityContext
   ```

2. UsernamePasswordAuthenticationFilter

   ```
   默认拦截“/login”登录请求，处理表单提交的登录认证，将请求中的认证信息包括username,password等封装成UsernamePasswordAuthenticationToken，然后调用AuthenticationManager的认证方法进行认证
   ```

3. BasicAuthenticationFilter

   ```
   基本认证，httpBasic登录，弹出登录框登录
   ```

4. RememberAuthenticationFilter

   ```
   记住我
   ```

5. AnonymousAuthenticationFilter

   ```
   匿名Filter，用来处理匿名访问的资源，如果SecurityContext中没有Authentication，就会创建匿名的Token(AnonymousAuthenticationToken),然后通过SecurityContextHodler设置到SecurityContext中
   ```

6. ExceptionTranslationFilter

   ```
   用来捕获FilterChain所有的异常，进行处理，但是只会处理AuthenticationException和AccessDeniedException，异常，其他的异常	会继续抛出
   ```

7. FilterSecurityInterceptor

   ```
   用来做授权的Filter,通过父类(AbstractSecurityInterceptor.beforeInvocation)调用AccessDecisionManager.decide方法对用户进行授权
   ```

## 3. 认证原理

![image-20211206210815409](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211206210815409.png)

接下来就会围绕这个认证的流程进行不断的找源码一步一步走:

1. 首先我们知道在整个security的过程中都是通过filiter过滤器链进行调用的,核心就是`FilterChainProxy类`的`doFilterInternal方法`中的`chain.doFilter(fwRequest, fwResponse);`去进行调用!

2. 调用执行到`SecurityContextPersistenceFilter`类的`doFilter()`方法

   ![image-20211204144828902](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204144828902.png)

   ```
   这里面就会首先从SecurityContextRepository去加载SecurityContext然后放进SecurityContextHolder中!
   ```

3. 此时在`chain.doFilter(holder.getRequest(), holder.getResponse());`又会回到最初的`FilterChainProxy`中的静态内部类`VirtualFilterChain`的`nextFilter.doFilter(request, response, this);`

4. 跳转到`UsernamePasswordAuthenticationFilter`这个类中:

   ![image-20211204145901176](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204145901176.png)

   这里则是将请求账号和密码封装成Token,注意这个token在后面的使用中都是用这个token进行调用执行!

   这里`this.getAuthenticationManager().authenticate(authRequest)`会首先获取认证管理器对象,然后调用`authenticate(token)`执行认证请求,注意这里是将封装好的token作为参数传递进去!
   
   然后我们通过authenticate的debug进去:

5. 此时就会调用执行到`ProviderManager`中`authenticate`方法:

   ```java
   public Authentication authenticate(Authentication authentication) throws AuthenticationException {
     	private AuthenticationManager parent;
       Authentication result = null;
       for (AuthenticationProvider provider : getProviders()) {
           ...
           //这里才是真正执行认证的地方,而这个authenticate是一个接口,就去找到对应的实现类:AbstractUserDetailsAuthenticationProvider这个抽象类    
           result = provider.authenticate(authentication);
           ...
       }
       if (result == null && parent != null) {
           //首先会执行到这里,通过认证管理器去调用认证,不过这里看似认证其实并不是真正执行的地方
           result = parent.authenticate(authentication);
       }
       
   }
   ```

   注意:在之前传参的时候我们明明看到传递的是`UsernamePasswordAuthenticationToken类型`但是传递进来的却是`Authentication`这个类型? 实际上这是因为token中是实现了该Authentication接口

   ![image-20211204151843682](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204151843682.png)

   

6. 通过`provider.authenticate(authentication)`就找到对应的实现类`AbstractUserDetailsAuthenticationProvider`,而这个却是抽象类所以继续往下找到`DaoAuthenticationProvider`这个子类

   进入这个类也就意味着进入到查询用户的阶段了,就好比是进入了项目的DAO层了,并且这里方法名`retrieveUser`翻译为"检索用户"

   ![image-20211204152711350](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204152711350.png)

   注意:这里传递进来的参数是前端的username和封装的token,虽然这里我觉得完全是可以从token中拿,但是毕竟是人家设计的,不好说

   这里`this.getUserDetailsService()`获取到`userDetailsService`这个接口的代理对象,通过这个代理对象去调用`loadUserByUsername`去加载用户通过前端传递的用户名

   那么就思考了?它去哪里加载?用户名是什么我们知道吗?

   1. 我们点击`UserDetailsService`可以发现:

      ![image-20211204153455256](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204153455256.png)

      可以发现:这里通过缓存或者内存或者jdbc都可以获取

      而我们在自定义的配置类,我们已经实现了该接口,自定义从哪里获取:

   2. 查看我们自定义配置类:

      ![image-20211204153854334](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204153854334.png)

      new一个内存用户管理器,然后创建用户,账号为zss,密码为123,权限为admin,这里的创建用户其实就是往Map中去put!

   3. 那么这里就会走进`InMemoryUserDetailsManager`中的`loadUserByUsername`

      ![image-20211204154213906](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204154213906.png)

      可以发现这里会将前端用户去map中获取,然后返回一个UserDetails类型,然后就会判断说这个用户名是否存在啊,如果不存在就会抛出异常,如果存在就new一个User返回,而这个Uer实现了UserTails,注意这里比如`isEnabled`表示是否启用啊,`isAccountNonExpired`账户是否过期啊等等之类的

   4. 到现在一个从上往下查找的过程就结束了,这就是一个用户登录认证的完整流程,当然具体是走内存还是JDBC都是由我们自己决定的,如果我们自定义了那么就会我们想要的那个类里面去!

   -----

找到之后就会返回,一路返回到`AbstractUserDetailsAuthenticationProvider`这个类中的`authenticate`这个方法中

1. 执行`preAuthenticationChecks.check(user)`进行检查用户的账户是否有效或者启用之类的一些状态:

   ![image-20211204155423681](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204155423681.png)

2. 检查完毕就会走`additionalAuthenticationChecks(user,(UsernamePasswordAuthenticationToken) authentication);`注意,这里就会进行校验密码了:

   走到`DaoAuthenticationProvider`中:

   ![image-20211204160338354](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204160338354.png)

   3. 由于我们自定义的配置类中定义返回`PasswordEncoder`注入到IOC容器中,所以优先使用我们这个自定义的配置:

      ```java
       //密码编码器：不加密
      @Bean
      public PasswordEncoder passwordEncoder(){
          return NoOpPasswordEncoder.getInstance();  //没有密码
      }
      ```

   4. 此时就会从`passwordEncoder.matches`就会默认走进`NoOpPasswordEncoder`中:

      ![image-20211204160625928](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204160625928.png)

      可以发现这里是用的equals进行比较的!

   5. 此时比较成功之后就会一路返回到`AuthenticationManager`接口的实现类`ProviderManager`中的

      ![image-20211204160947146](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204160947146.png)

      这里也是我们最开始执行认证的地方,然后执行下面的copyDetails,说白了这里就是将前端封装的token的details(就包括请求ip与sessionId)进行复制到从Security查询返回的UserDetails中去!

   6. 可以发现这里就有sessionId了,我们来看前端:

      ![image-20211204161425272](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204161425272.png)

      再来看后台:

      ![image-20211204161517914](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204161517914.png)

      7. 最后就一路返回到最初的起点:`SecurityContextPersistenceFilter`

         ![image-20211204161820211](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204161820211.png)

         就会从holder中获取SecurityContext,然后清除holder中的Context,最后再将从holder获取到context保存到`SecurityContextRepository`中!

### 3.1 调用链

![image-20211204162330838](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204162330838.png)

1. 请求过来会被过滤器链中的`UsernamePasswordAuthenticationFilter`拦截到，请求中的用户名和密码被封装成`UsernamePasswordAuthenticationToken`(Authentication的实现类)
2. 过滤器将UsernamePasswordAuthenticationToken提交给认证管理器(`AuthenticationManager`)进行认证
3. AuthenticationManager委托`AuthenticationProvider`(DaoAuthenticationProvider)进行认证，AuthenticationProvider通过调用`UserDetailsService`获取到数据库中存储的用户信息(UserDetails)，然后调用`passwordEncoder`密码编码器对UsernamePasswordAuthenticationToken中的密码和UserDetails中的密码进行比较
4. AuthenticationProvider认证成功后封装Authentication并设置好用户的信息(用户名，密码，权限等)返回,将用户信息new一个UsernamePasswordAuthenticationToken类型进去!
5. Authentication被返回到UsernamePasswordAuthenticationFilter,通过调用`SecurityContextHolder`工具把`Authentication封装成SecurityContext中存储起来`。然后UsernamePasswordAuthenticationFilter调用`AuthenticationSuccessHandler.onAuthenticationSuccess`做认证成功后续处理操作
6. 最后SecurityContextPersistenceFilter通过`SecurityContextHolder.getContext()`获取到SecurityContext对象然后调用SecurityContextRepository将SecurityContext存储起来，然后调用`SecurityContextHolder.clearContext`方法清理SecurityContext

注意：SecurityContext是一个和当前线程绑定的工具，在代码的任何地方都可以通过SecurityContextHolder.getContext()获取到登陆信息

> 在整个过程中我发现还有session相关的具体操作!如:查询成功后就会将前端请求的token中的details复制到查询返回的Usertails中的details中!

总结一波:

![image-20211204163924800](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204163924800.png)

### 3.2 定义密码编码器

在我们的案例中，密码一值是明文的,我们指定的密码编码器是 NoOpPasswordEncoder ，这个是不加密的，但是在生产环境中我们数据库中的密码肯定是密文，所以我们需要指定密码的编码器，那么SpringSecurity在认证时会调用我们指定的密码编码器进行认证

BCryptPasswordEncoder是SpringSecurity内部提供的编码器，他的好处在于多次对相	同的明文加密出来的密文是不一致的，但是多次加密出来的不同密文确有能检查通过，这种方式增加了密码的安全性，测试代码如下:

```java
public class PasswordTest {
    @Test
    public void testPassword(){
        BCryptPasswordEncoder bCryptPasswordEncoder = new BCryptPasswordEncoder();
        String enPass = bCryptPasswordEncoder.encode("123");
        System.out.println(enPass);
        System.out.println(bCryptPasswordEncoder.matches("123", enPass));
    }
}
```

在配置类中定义编码器如下:

```java
@Bean
public PasswordEncoder passwordEncoder(){
    //return NoOpPasswordEncoder.getInstance();
    return new BCryptPasswordEncoder();
}
```



## 4.授权流程原理

​	授权一定是在认证通过之后，授权流程是通过`FilterSecurityInterceptor`拦截器来完成，`FilterSecurityInterceptor`通过调用`SecurityMetadataSource`来获取当前访问的资源所需要的权限，然后通过调用`AccessDecisionManager`投票决定当前用户是否有权限访问当前资源。授权流程如下:

RBAC传统授权流程:

![image-20211204175640008](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204175640008.png)

Security授权流程：

![image-20211204175723933](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204175723933.png)





![image-20211204175956117](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211204175956117.png)

1. 在FilterSecurityInterceptor中会调用其父类AbstractSecurityInterceptor的beforeInvocation方法做授权之前的准备工作
2. 该方法中通过SecurityMetadataSource..getAttributes(object);获得资源所需要的访问权限 ，通过SecurityContextHolder.getContext().getAuthentication()获取当前认证用户的认证信息，即Authentication对象
3. 然后通过调用AccessDecisionManager.decide(authenticated, object, attributes);进行授权，该方法使用了投票机制来决定用户是否有资源访问权限
   - AccessDecisionManager接口有三个实现类，他们通过通过AccessDecisionVoter投票器完成投票，三种投票策略如下:
     - AffirmativeBased:只需有一个投票赞成即可通过
     - ConsensusBased:需要大多数投票赞成即可通过，平票可以配置
     - UnanimousBased:需要所有的投票赞成才能通过
4. 而投票器也有很多，如RoleVoter通过角色投票，如果ConfigAttribute是以“ROLE_”开头的，则将使用RoleVoter进行投票，AuthenticatedVoter 是用来区分匿名用户、通过Remember-Me认证的用户和完全认证的用户(登录后的)
5. 投票通过，放心请求响应的资源

### 4.1 web授权

**API说明:**


在Security配置类中，可以通过HttpSecurity.authorizeRequests()给资源指定访问的权限，其API如下：
1. anyRequest():任何请求
2. antMatchers（“/path”） ：匹配某个资源路径
3. authenticationed() : 保护URL需要登录访问anyRequest().authenticationed()
4. permitAll():指定url无需保护(放行)一般用户静态资源antMatchers（“/path”）.permitAll()
5. hasRole(String role)：某个资源需要用户拥有什么样的role才能访问
6. antMatchers（“/path”）.hasRole(“admin”)
7. hasAuthority(String authority):某个资源需要用户拥有什么样的权限才能访问
`antMatchers（“/path”）.hasAuthority(“admin”)`
8. hasAnyRole(String ...roles):某个资源拥有指定角色中的一个就能访问
9. hasAnyAuthority(String ... authorities):某个资源拥有指定权限中的一个就能访问
10. access(String attribute):该方法使用SPEL表达式,可以创建复杂的限制
11. hasIpAddress(String ip):拥有什么样的ip或子网可以访问该资源

### 4.2 方法授权

SpringSecurity提供了一些授权的注解让我们可以在service,controller等的方法上贴注解进行授权，即在方法上指定方法方法需要什么样的权限才能访问

**@PreAuthorize**

PreAuthorize适合进入方法前的权限验证，拥有和Secured同样的功能，甚至更强大，该注解需要在配置类开启：@EanbleGlobalMethodSecurity(prePostEnabled=true) 方法授权支持，然后在Controller贴注解如下：

1. @PreAuthorize(“isAnonymous()”) : 方法匿名访问


2. @PreAuthorize(“hasAnyAuthority(‘p_user_list’,‘p_dept_list’)”) :拥有p_user_listr或者p_dept_list的权限能访问 

3. @PreAuthorize(“hasAuthority(‘p_transfer’) and hasAuthority(‘p_read_accout’)”) : 拥有p_transfer权限和p_read_accout权限才能访问.

该标签不需要有固定的前缀。

开启@PreAuthorize授权支持

```java
@Configuration
@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled= true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
```

使用@PreAuthorize进行方法授权

```java
@PreAuthorize("hasAnyAuthority('employee:add','employee:update')")
@RequestMapping("/employee/add")
public String add(){	
    return "employee.add";
}
```

指明了方法必须要有 employee:add 或者 employee:update的权限才能访问 ， 该注解不需要有固定的前缀。

注意格式“@PreAuthorize("hasAuthority('employee:add')")” ，hasAuthority不能省略，括号中是单引号。



总结:

![image-20211207161059378](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211207161059378.png)

