# Spring笔记

## 1.概念

Spring框架准确来讲应该是`spring framework`!所以以后提到spring framework第一眼以为很陌生其实它就是spring,很多框架比如spring boot也都是基于这个框架为基础而进行开发的!

> spring是我们目前主流的JavaWeb开发框架

**注意：**Spring底层原理：xml+dom4j+工厂设计模式+反射

spring的出现就是为了解决处理对象的创建的、以及对象的依赖关系！和对JavaEE各层提供了解决方案

### 1.1 什么是spring

​	Spring是一个开源框架,目的就是为了解决企业程序开发复杂性而创建的。框架的主要优势之一就是分层架构，分层架构允许您使用哪一个组件，同时为J2EE应用程序开发提供集成的框架了；同时又是一个轻量级IOC和AOP容器框架，为Java应用程序提供基础性服务，用于简化开发，使开发者只关注业务需求，为构建企业级应用，提供轻量级一站式解决方案！

理解：从上文我提炼出几个关键字 **开源**、**解决**、**轻量级**、**简化开发**、**一站式**，提炼出来后是不是理解更清楚了呢？

### 1.2 Spring 特点:

Spring是一个开源的**轻量级**控制反转(**IOC**)和面向切面编程(**AOP**)的容器框架;

1. 轻量级:相对于重量级(框架设计比较繁琐，配置较多，例如EJB（tomcat不支持），现在基本不用了）而言，开发使用都比较简单，功能强大
2. IOC(控制反转):将创建对象的权利和依赖关系维护(字段赋值)交给Spring容器(不再使用以前的new关键字创建对象)。对于某个具体的对象而言，以前是它控制其他对象，而现在所有对象都被spring控制，所以这叫控制反转
3. AOP（面向切面编程）：将相同的逻辑抽取出来,即将业务逻辑从应用服务中分离出来。然后以拦截的方式作用在一个方法的不同位置。例如：日志，事务的处理；

### 1.3 Spring 优点:

1. 控制反转:Spring通过控制反转实现了松散耦合,对象们给出它们的依赖,而不是创建或查找依赖的对象们,方便解耦降低维护难度,提高了开发效率
2. 面向切面的编程(AOP):Spring支持面向切面的编程,可以很方便的**实现对程序进行权限拦截和运行监控等功能**,可以将一些通用任务和安全,事务,日志等进行集中式管理,从而提供了更好的复用!
3. MVC框架:spring的web框架是一个精心设计的框架,是web框架的一个很好的替代品
4. 低侵入式设计:代码污染极低,独立于各种应用服务器,基于Spring框架的应用,可以真正实现Write Once,Run Anywhere的承诺
5. **集成能力强**:Spring 致力于 Java EE 应用各层的解决方案，对每一层都提供了技术支持。在表现层提供了与 Spring MVC、Struts2 框架的整合，在业务逻辑层可以管理事务和记录日志等，在持久层可以整合 MyBatis、Hibernate 和 JdbcTemplate 等技术。这就充分体现出 Spring 是一个全面的解决方案，对于已经有较好解决方案的领域，Spring 绝不做重复的事情。
6. 异常处理:Spring提供方便的API把具体技术相关的异常(比如由JDBC,Hibernate抛出的)转化为一致的unchecked异常
7. 容器:Spring包含并管理应用中对象的生命周期和配置
8. 轻量级:Spring是轻量的,基本的版本大约也就2MB
9. Spring 支持 JUnit4，可以通过注解方便地测试 Spring 程序。
10. 只需要通过配置就可以完成对事务的管理，而无须手动编程。

> Spring的DI（依赖注入）机制降低了业务对象替换的复杂性，提高了组件之间的解耦

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmWUCpCfC5jcxPJXwoMhcjMF203QicHAFo667MzoMW75e9jW8wwufT7Fb97OhfcaoUxfrwnnicSibpmg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmWUCpCfC5jcxPJXwoMhcjMXTQ0tWBz1RLWaK9RicHcdSLYqYD6t4zffdHVaasI74AbGtrt52wVMug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如上图所示，本来 ABCD 是互相关联在一起的，当加入第三方容器的管理之后，每个对象都和第三方法的 IoC 容器关联，彼此之间不再直接联系在一起了，没有了耦合关系，全部对象都交由容器来控制，降低了这些对象的亲密度，就叫“解藕”

## 2.IOC

IOC也称控制反转,IOC是一种设计思想,在开发中,将你设计好的对象交给容器控制,而不是由程序员显示地用代码进行对象的创建,而在Spring框架中将这种思想具体实现化:

把创建和查找**依赖**对象的控制权交给IOC容器,由IOC容器进行**注入**、组合对象。这样对象与对象之间是松耦合、便于测试、功能可复用（减少对象的创建和内存消耗），使得程序的整个体系结构变得可维护性、灵活性、扩展性变高.

> 理解:控制反转也就是控制权的转移,将我们创建对象的方式进行反转了,以前对象的创建都是由我们开发人员自己new,包括依赖关系也是自己注入(比如:IStudentService service=new StudentImpl()),而使用了spring之后,对象的创建以及依赖关系可以由spring完成创建以及注入; 说白了控制反转就是反转了对象的创建方式,从我们自己创建反转给程序创建(spring)

对于IOC来说,最重要的就是**容器**:容器负责创建、配置和管理bean，也就是说它管理着bean的生命，控制着bean的依赖注入

通俗点讲，因为项目中每次创建对象是很麻烦的，所以我们使用Spring IOC容器来管理这些对象,需要的时候你就直接用,而不用管他怎么来的,什么时候要销毁,对于我们只管用就可以了

画图理解：

![image-20211008160644540](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008160644540.png)

### 2.1 DI

**概念:**提到IOC就不得不提DI,DI全称(Dependency Injection)依赖注入,是IOC容器装配、注入对象的一种方式

**作用：**通过依赖注入机制，简单的配置即可注入需要的资源，完成自身的业务逻辑，不需要关心资源的出处和具体实现

> 理解：可以理解为spring这个容器中，替你管理着一系列的类，前提是你需要将这些类交给spring容器进行管理,然后在你需要的时候,不需要自己去定义,而是可以直接向spring容器索取,当sprinhg容器知道你的需求之后.就会去它管理的组件中进行查找,然后直接给你需要的组件

实现IOC思想就需要用DI做支持

其中注入就有两种:

1. 注入方式:
   - set方式注入
   - 构造方法注入
   - 字段注入
2. 注入类型:
   - 值类型注入
   - 引用类型注入

### 2.2 IOC容器

Spring 提供 2 种不同类型的 IoC 容器，即 BeanFactory 和 ApplicationContext 容器。

#### 2.2.1 BeanFactory 容器

BeanFactory是最简单的容器,由`org.springframework.beans.factory.BeanFactory`接口定义,采用懒加载方式,所以容器启动比较快,BeanFactory提供了容器最基本的功能

简单来说:BeanFactory就是一个管理bean的工厂,它主要负责初始化各种Bean,并调用他们的生命周期方法。

BeanFactory接口有多个实现类,最常见的就是`XmlBeanFactory`,所以使用BeanFactory需要创建XmlBeanFactory类的实例,然后通过XmlBeanFactory类的构造函数来传递Resource对象:

```java
	    ClassPathResource resource = new ClassPathResource("applicationContext.xml");
        XmlBeanFactory factory = new XmlBeanFactory(resource);
        MessagePrinter bean = factory.getBean(MessagePrinter.class);
        bean.printMessage();
```

**注意:**目前来讲,这种方式已经被弃用了!

#### 2.2.2 ApplicationContext 容器

`ApplicationContext`继承了`BeanFactory`接口,所以Application不仅拥有了BeanFactory容器的最基本功能还增加了很多企业级功能,比如:AOP、国际化（i18n）、事件支持等;

这个容器由`org.springframework.context.ApplicationContext`接口定义,对象在启动时加载,所以相较于BeanFactory容器的懒加载相比,这个容器启动的话就会就会很慢;

ApplicationContext容器提供了两个常用的实现类:

1. ClassPathXmlApplication实现类
 ```java
   ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
 ```

 该类主要是从类路径ClassPath中寻找指定的XML配置文件,并完成ApplicationContext的实例化工作

2. FileSystemXmlApplicationContext实现类

```java
ApplicationContext application=new FileSystemXmlApplicationContext("F:\源码时代\springProject\src\main\resources\applicationContext.xml")
```

 该类从指定的文件系统路径中寻找指定的XML配置文件,并完成ApplicationContext的实例化工作

两者区别:

- 读取位置不同:

​	在读取Spring配置文件时两者读取的位置方式不同:`FileSystemXmlApplicationContext`不会从类路径中读取配置文件,而是通过参数指定配置文件的位置,也可以理解为可以读取本地硬盘上的资源(如以上代码),而`ClassPathXmlApplicationContext`只会获取类路径中的配置文件,不然就会报IO异常

- 项目不同:

  在Java项目中,会采用`ClassPathXmlApplicationContext`类实例化ApplicationContext容器的方式;

  在web项目中,Application容器的实例化工作会交由Web服务器完成,Web服务器实例化ApplicationContext容器通常使用基于ContextLoaderListener实现的方式,并且在web.xml添加以下参数:

  ```xml
  <!--指定Spring配置文件的位置，有多个配置文件时，以逗号分隔-->
  <context-param>
      <param-name>contextConfigLocation</param-name>
      <!--spring将加载spring目录下的applicationContext.xml文件-->
      <param-value>
          classpath:spring/applicationContext.xml
      </param-value>
  </context-param>
  <!--指定以ContextLoaderListener方式启动Spring容器-->
  <listener>
      <listener-class>
          org.springframework.web.context.ContextLoaderListener
      </listener-class>
  </listener>
  ```

#### 2.2.3 applicationContext & BeanFactory区别

共同点:BeanFactory和ApplicationContext都是通过XML配置文件加载Bean的

不同点:

1. BeanFactory是spring的原始接口,而ApplicationContext是继承了前者,所以前者的实现类功能都比较单一,而后者提供了更多的功能
2. 创建对象时机不同:
   - BeanFactory实现懒加载方式,容器启动较快,在每次获得对象时才会创建对象.并且如果Bean的某一个属性没有注入那么加载容器后第一次调用getBean()方法才会抛出异常,也就是容器启动不会检查,只有第一次获取getBean才会检查
   - ApplicationContext实现饿加载方式,容器启动较慢.在每次启动的时候就会检查Bean属性有没有注入,没有就会抛出异常这样有利于所依赖的属性是否被注入,并且每次容器启动时就会创建容器中配置的所有对象并检查
3. 加载位置不同:
   - BeanFactory只能从类路径下加载配置文件
   - ApplicationContext不仅可以从类路径下加载配置文件还可以从硬盘的绝对路径下加载配置文件

选择:通常在实际的开发项目中,都选择使用`ApplicationContext`,只有在系统资源比较少时才会考虑使用`BeanFactory`



### 2.3 Spring配置文件详解

**简述:**由Spring IOC容器管理的对象称为[Bean](也就是我们Java的类),而Bean根据Spring配置文件中的属性而进行创建

SpringIOC容器看作是一个大工厂,Bean相当于工厂的产品,如果希望这个大工厂生成和管理Bean.那么只需要告诉容器需要哪些Bean,以及需要哪种方式装配Bean;

**理解**:所谓的大工厂就是配置文件,在配置文件里通过一些标签属性进行管理这些对象类的使用方式

Spring配置文件支持两种格式,即XML文件和Properties文件格式

- Properties配置文件主要以key-value键值对的形式存在,只能进行读取配置文件进行赋值.不能进行其他操作,适用于简单属性配置
- XML配置文件时树形结构,比Properties更加灵活,并且还很清晰,但是配置比较繁琐,适用于大型复杂的项目

我们使用最多的就是XML文件格式,XML配置文件的根元素是<beans>,该元素包含多个子元素<bean>,而每一个<bean>元素都定义一个Bean,并描述了如何进行装配到容器中使用

#### 2.3.1 元素属性

- Bean元素:使用该元素描述需要spring容器管理对象
- name属性:给被管理的对象起一个名字,也就是我们在使用`getBean("name值")`
- class属性:被管理对象的完整类名-完全限定类名
- id属性:与name属性一模一样,名称不可重复,不能使用特殊字符

助理Bean元素就是所谓的<Bean>标签,其他name和id都是Bean标签里的属性,要记住!

**注意:**

name和id的区别:

1. 配置两个相同的id或者name都不能通过(可以理解为bean的名称不可重复,不管是id或者name只要其中定义一个名字那么其他都是不能重复的),比如:

   ```xml-dtd
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="service" class="hello.MessagesService">
   
       </bean>
       <bean name="service" class="hello.MessagePrinter">
           <property name="service" ref="service"/>
       </bean>
   </beans>
   ```

   报错信息:可以看出在<Beans>中的节点中Bean的名称已经使用过了

   ![image-20210902192107834](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210902192107834.png)

2. 如果同时配置了id又配置了name,则两个都生效,如果id和name都没指定那么默认就会以类全名作为name,如:<bean class="com.xxxx.BeanController";比如:

   ```java
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="service" class="hello.MessagesService">
   
       </bean>
       <bean id="printer" name="printerr" class="hello.MessagePrinter">
           <property name="service" ref="service"/>
       </bean>
   </beans>
   ```

   测试:

   ```java
   ....
   Object bean = context.getBean("printerr");
   Object bean1 = context.getBean("printer");
   System.out.println(bean==bean1);
   ```

   结果:

   ![image-20210902192622355](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210902192622355.png)

   如果默认不写id和name,那么就以class的参数指定:

   ```java
   Object bean = context.getBean("hello.MessagePrinter");
   System.out.println("bean = " + bean);
   ```

   结果:

   ![image-20210902192815362](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210902192815362.png)


#### 2.3.2 Bean元素进阶

这里进阶就分为"Bean的作用域"和"Bean的生命周期"

1. ### Bean作用域

**作用域:**这个也很好理解就是bean的创建的作用域,一般以`socpe属性`来表示

Spring 容器在初始化一个 Bean 实例时，同时会指定该实例的作用域。Spring 5 支持以下 6 种作用域。

- singleton

  ```
  默认值,表示单例模式;表示Spring容器中只有一个Bean实例,Bean以单例的方式存在,该Bean实例将存储在告诉缓存中.每一次请求Bean的时候都会去缓存区里去拿实例,可以说这是共享实例
  ```

  > 会针对单个<bean>进行创建实例并放在缓存中,如果多个<bean>的class都是一样的,那么会为每个bean创建实例,每个bean之间的地址都是不一致的,这种意义上来讲似乎这种方式并不是单例的!

- prototype

  ```
  原型模式,表示每次通过Spring容器获取Bean时,容器都会创建一个Bean实例;说白了就是每次创建都是一个新的对象
  ```

- request

  ```
  在web环境下,每次Http请求,Spring容器会为该请求创建一个Bean实例,但是只能在该请求作用域中有效,也就是跟http的生命周期一致
  ```

- session

  ```
  在web环境下,同一个Http Session共享一个Bean实例,不同的Session使用不同的Bean实例,该作用域仅在当前Http Session有效
  ```

- application

  ```
  同一个web应用共享一个Bean实例,该作用域在当前ServletContext内有效
  ```

  ...

  其他的不常用,用到再说,这几个已经够用了,不论是web环境下还是当前Java项目下都已经是常用的了

  **注意:**除了`singleton`和`prototype`以外的属性都只能在web环境下使用,如果在非web环境下使用ClassPathXmlApplicationContext 加载这些作用域中的任意一个的 Bean,就会抛出异常:

  ![image-20210902195611858](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210902195611858.png)

这里本来想演示单例模式的作用域,但是我无形之中在之前的元素属性那节已经演示过了,两次获取都是同一个对象,对象地址都是一样的!

以下演示prototype多例模式的过程:

xml配置文件:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="service" class="hello.MessagesService">

    </bean>
    <bean class="hello.MessagePrinter" scope="prototype">
        <property name="service" ref="service"/>
    </bean>
</beans>
```

测试:

```java
Object bean = context.getBean("hello.MessagePrinter");
Object bean1 = context.getBean("hello.MessagePrinter");
System.out.println("bean = " + bean);
System.out.println("bean1 = " + bean1);
System.out.println(bean==bean1);//可以证明两次获取都是不同的对象,地址都是不一样的
```

结果:

![image-20210902200341947](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210902200341947.png)

2. ### 生命周期

   Spring容器在确保一个Bean能够使用之前,会进行许多工作,Spring容器中bean的生命周期如下:

   ![Bean的生命周期](https://gitee.com/miawei/pic-go-img/raw/master/imgs/10551T325-0.png)

   解读步骤:
   **注意:**是采用Spring容器中的ApplicationContext容器方式:

   1. Spring容器启动,加载所有的bean并查找被Spring管理的Bean,并通过反射实例化

   2. 利用依赖注入完成Bean中所有属性值的配置注入

   3. 如果该Bean实现了`BeanNameAware`接口,则Spring就会调用该bean的`setBeanName()`方法传入当前bean在配置文件中的[id值](有id传id,没id传name,如果都没有那就传class值)

      ```java
      public class MessagePrinter implements BeanNameAware {
          @Override
        public void setBeanName(String name) {
      	...
        }
      }
      ```

   4. 如果该Bean实现了`BeanFactoryAware`接口,则Spring调用`setBeanFactory()`方法传入当前工厂实例的引用

      ```java
      public class MessagePrinter implements BeanFactoryAware {
          @Override
        public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
          ...
        }
      }
      ```

   5. 如果该Bean实现了`ApplicationContextAware`接口,则Spring调用`setApplicationContext()`方法传入当前ApplicationContext实例的引用

      ```java
      public class MessagePrinter implements ApplicationContextAware {
          @Override
        public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
      	...
        }
      }
      ```

   6. 如果该Bean实现了`BeanPostProcessor`接口,则Spring调用该接口的预初始化方法`postProcessBeforeInitialization()`方法对Bean进行加工操作,此处很重要,因为[AOP](面向切面编程,相当于切开然后塞点东西进去)就是利用它实现的

      ```java
      public class MessagePrinter implements BeanPostProcessor {
          @Override
        public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
          return null;
        }
      }
      ```

   7. 如果该bean实现了`InitializingBean`接口,则Spring将调用`afterPropertiesSet()`方法

      ```java
      public class MessagePrinter implements InitializingBean {
          @Override
        public void afterPropertiesSet() throws Exception {
      	...
        }
      }
      ```

   8. 如果在配置文件中通过`init-method`属性指定了初始化方法,则调用该初始化方法

      ```xml-dtd
      <bean id="printer" name="printerr" class="hello.MessagePrinter" scope="prototype" init-method="afterPropertiesSet">
      	<property name="service" ref="service"/>
      </bean>
      这个 init-method指定方法必须是该 bean中的无参方法
      ```

   9. 如果`BeanPostProcessor`和Bean关联,则Spring将调用该接口的初始化方法`postProcessAfterInitialization()`方法,此时Bean已经可以被应用系统使用了

      ```java
      public class MessagePrinter implements BeanPostProcessor {
          @Override
        public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
          return null;
        }
      }
      ```

   10. 如果在<bean>元素中指定该Bean的作用域为`singleton`,则将该Bean放入Spring IOC的缓存池中,触发了Spring对该Bean的生命周期管理;如果在<bean>中指定该Bean的作用域为`prototype`,则将该Bean交给调用者,调用者管理该bean的生命周期,Spring不再管理该Bean。

   11. 如果Bean实现了`DisposableBean`接口,则Spring会调用`destroy`方法销毁Bean;如果在配置文件中通过`destory-method`属性指定了Bean的销毁方法,则Spring将调用该方法对Bean进行销毁

       ```java
       public class MessagePrinter implements DisposableBean {
       	@Override
         public void destroy() throws Exception {
       	...
         }
       }
       ```

       ```xml-dtd
       <bean id="printer" name="printerr" class="hello.MessagePrinter" scope="prototype" destroy-method="destroy">
       	<property name="service" ref="service"/>
       </bean>
       这个 destroy-method指定方法必须是该 bean中的无参方法
       ```

   **注意:**在使用过程中尽量使用配置文件进行设置,因为如果在bean中实现方法过多那么势必也会造成代码的耦合性过高

   Spring官方提供了三种方法进行初始化和销毁回调:

   1. 实现 InitializingBean 和 DisposableBean 接口；
   2. 在 XML 中配置 init-method 和 destory-method；
   3. 使用 @PostConstruct 和 @PreDestory 注解(前者初始化后者销毁)。

   如果想在多个bean都调用共同的销毁或者初始化方法那么就在根元素<beans>中添加` default-init-method=""`或者`default-destroy-method=""`

3. ### 模块化配置(导入其他配置文件)

   ```xml-dtd
   <beans>
       <import resource = “spring配置文件的全路径名” />
   </beans>
   ```

#### 2.3.3 spring创建对象

spring创建对象总共有三种方式:

1. 通过构造函数创建对象
   - 利用无参构造函数+setter方法注入值
   - 利用有参构造函数直接注入
2. 通过静态方法创建对象
3. 通过工厂方法创建对象



**注意:**我们默认的创建对象方式就是无参构造函数进行创建对象,我们在使用Spring容器中在启动的时候会执行所有的<bean>元素所在的bean对象;

来看代码:

xml:

```xml-dtd
	<bean id="test" class="hello.Person">
        <property name="name" value="试试"></property> //这里 property针对属性进行赋值
    </bean>

    <bean id="test1" class="hello.Person"></bean>
```

> 这里进行字段赋值的时候,是通过JavaBean的方式,比如字段属性name,而正常getter方法是getName方法,而在进行prototype赋值的时候输入name,这个name不是字段属性的值,而是将getName拆分,将get拆开然后将首字母小写,进行匹配的

测试:这里只是获取其中一个bean的实例

```java
	@Test
    public void test() throws Exception{
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Object test = context.getBean("test");
    System.out.println("test = " + test);
    }
```

Bean:

```java
package hello;

/**
 * @program: springProject
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-02 21:23
 */
public class Person {
  private String name;
  private Integer id;
  public Person() {
    System.out.println("我是无参构造");
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }

  @Override
  public String toString() {
    return "Person [name=" + name + ", id=" + id + "]";
  }
}
```

执行:

![image-20210903091554916](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903091554916.png)

**总结**:可以发现我们在使用`ApplicationContext`容器的时候,一旦容器启动的时候会自动创建所有<bean>的实例,而创建方式就是默认的无参构造函数,所以会调用无参的构造方法!



1. ## 构造方法创建对象

   - 利用无参构造函数+setter方法注入值

     **注意:**这里详细代码在上面进行展示了,这里象征性贴一下,所以一个bean必须有一个无参构造方法,否则就会报错

     ```xml-dtd
      	<bean id="test" class="hello.Person">
             <property name="name" value="试试"></property>
         </bean>
     ```

   - 利用有参构造函数直接注入

     直接上代码:

     xml:

     ```xml-dtd
     	<bean id="test" class="hello.Person">
             <constructor-arg name="name" value="有参"/>
             <constructor-arg name="id" value="1"/>
         </bean>
     ```

     bean:

     ```java
     public class Person {
       private String name;
       private Integer id;
       public Person(String name,Integer id) {
           this.id=id;
           this.name=name;
         System.out.println("我是有参构造");
       }
     
       public String getName() {
         return name;
       }
     
       public void setName(String name) {
         this.name = name;
       }
     
       public Integer getId() {
         return id;
       }
     
       public void setId(Integer id) {
         this.id = id;
       }
     
       @Override
       public String toString() {
         return "Person [name=" + name + ", id=" + id + "]";
       }
     }
     ```

     测试:

     ```java
      	@Test
         public void test() throws Exception{
             ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
             Object test = context.getBean("test");
             System.out.println("test = " + test);
         }
     ```

     结果:

     ![image-20210903095318976](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903095318976.png)

2. ## 通过静态方法创建对象

   先看代码:

   xml:

   ```xml-dtd
    	<bean id="test" class="hello.PersonFactory" factory-method="createPerson">
           <constructor-arg name="name" value="有参"/>
           <constructor-arg name="id" value="1"/>
       </bean>
   ```

   Person:

   ```java
   public class Person {
     private String name;
     private Integer id;
   
     public Person(String name, Integer id) {
       this.id = id;
       this.name = name;
       System.out.println("我是有参构造");
     }
   
     public Person() {}
   
     public String getName() {
       return name;
     }
   
     public void setName(String name) {
       this.name = name;
     }
   
     public Integer getId() {
       return id;
     }
   
     public void setId(Integer id) {
       this.id = id;
     }
   
     @Override
     public String toString() {
       return "Person [name=" + name + ", id=" + id + "]";
     }
   }
   ```

   PersonFactory:

   ```java
   public class PersonFactory {
     public PersonFactory() {
       System.out.println("这是工厂无参构造方法");
     }
   
     public static Person createPerson() {
       return new Person();
     }
   
     public static Person createPerson(String name, Integer id) {
       return new Person(name, id);
     }
   }
   ```

   执行:

   ```java
    	@Test
       public void test() throws Exception{
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Object test = context.getBean("test");
           System.out.println("test = " + test);
       }
   ```

   结果:

   ![image-20210903101733968](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903101733968.png)

   > 这里就是利用了另一个类去创建实例,其本身不会创建实例!静态的工厂方法核心就是class+factory-method

   **注意:**必须是`static`方法,否则就会报异常:

   ![image-20210903102131396](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903102131396.png)

3. ## 通过工厂方法创建对象

   这个方法就是解决上面在工厂里不加`static`而进行报异常的解决办法:

   xml:

   ```xml-dtd
   	<bean id="beanFactory" class="hello.PersonFactory"/>
       <bean id="test" factory-bean="beanFactory" factory-method="createPerson">
           <constructor-arg name="name" value="大"/>
           <constructor-arg name="id" value="1"/>
       </bean>
   ```

   执行结果:

   ![image-20210903104035835](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903104035835.png)

   **结论:**这个与静态方法的区别就是静态方法必须是static修饰并且在xml配置文件中是针对单个bean进行工厂方法调用,而现在的工厂方法进行引用然后执行工厂方法创建对象

   > 实例工程方法需要先创建工厂实例,然后在创建所需对象的时候,将其赋值为factory-bean



### 2.4 依赖注入

Spring依赖注入(**DI**)和控制反转含义相同,使用依赖注入可以更轻松的管理和测试应用程序

**控制反转**:当某个Java实例需要另一个Java实例时,传统的方法是由调用者创建被调用者的实例(例如,使用new关键字获取被调用者的实例),而使用Spring框架后,被调用者的实例不再由调用者创建,而是由Spring容器创建,这就是将对象的创建权利交给Spring容器去创建,这就是控制反转!

**依赖注入**:Spring容器在创建被调用者的实例时,会自动将调用者需要的对象实例注入给调用者,调用者通过Spring容器获得被调用者的实例,这就称为依赖注入!

**理解:**控制反转我就不说了就是把我们传统的new对象的方式改为Spring容器给我们创建,而我们调用即可!而依赖注入可以理解为A类中某一个属性类型是另B类,而我们实例化A那么我们首先就要把B类实例化然后通过赋值的形式赋值给A类中属性,这样可以理解了吗?

在Spring容器中依赖注入有两种方式:**构造函数注入**、**setter设值方式传入**！

#### 2.4.1 构造函数注入

指 IoC 容器使用构造函数注入被依赖的实例。可以通过调用带参数的构造函数实现依赖注入，每个参数代表一个依赖。

Student:

```java
public class Student {
  private String studentName;
  private Integer studentAge;
  private Teacher teacher;

  public Student(String studentName, Integer studentAge, Teacher teacher) {
    this.studentName = studentName;
    this.studentAge = studentAge;
    this.teacher = teacher;
  }

  @Override
  public String toString() {
    return "Student{"
        + "studentName='"
        + studentName
        + '\''
        + ", studentAge="
        + studentAge
        + ", teacher="
        + teacher
        + '}';
  }
}
```

Teacher:

```java
public class Teacher {
    private String teacherName;
    private Integer teacherAge;

    public String getTeacherName() {
        return teacherName;
    }

    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }

    public Integer getTeacherAge() {
        return teacherAge;
    }

    public void setTeacherAge(Integer teacherAge) {
        this.teacherAge = teacherAge;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "teacherName='" + teacherName + '\'' +
                ", teacherAge=" + teacherAge +
                '}';
    }
}
```

xml:

```xml-dtd
		<bean id="demo" class="hello.demo.constructorSetter.Student">
                <constructor-arg name="studentName" value="构造方法学生名称"/>
                <constructor-arg name="studentAge" value="1"/>
                <constructor-arg name="teacher" ref="demo1"/>
        </bean>
        <bean id="demo1" class="hello.demo.constructorSetter.Teacher">
                <property name="teacherName" value="构造方法老师名称"/>
                <property name="teacherAge" value="123"/>
        </bean>
<!--
	参数解释:
		1.name:字段名
		2.value:值
		3.ref:引用类型值
		4.index:构造方法参数对应的顺序,从1开始,如果不写那么默认从左往右
		5.type:指字段的类型
-->
```

> 注意:这里使用constructor-arg的时候如果不指定index那么就会按照构造方法的参数顺序进行依次赋值,而index是指定的话是从0开始

结果:

![image-20210903153027393](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903153027393.png)

#### 2.4.2 setter方式注入

指 IoC 容器使用 setter 方法注入被依赖的实例。通过调用无参构造器或无参 static 工厂方法实例化 Bean 后，调用该 Bean 的 setter 方法，即可实现基于 setter 的 DI

Student:

```java
public class Student {
  private String studentName;
  private Integer studentAge;
  private Teacher teacher;

  public String getStudentName() {
    return studentName;
  }

  public void setStudentName(String studentName) {
    this.studentName = studentName;
  }

  public Integer getStudentAge() {
    return studentAge;
  }

  public void setStudentAge(Integer studentAge) {
    this.studentAge = studentAge;
  }

  public Teacher getTeacher() {
    return teacher;
  }

  public void setTeacher(Teacher teacher) {
    this.teacher = teacher;
  }

  @Override
  public String toString() {
    return "Student{" +
            "studentName='" + studentName + '\'' +
            ", studentAge=" + studentAge +
            ", teacher=" + teacher +
            '}';
  }
}
```

Teacher:

```java
public class Teacher {
    private String teacherName;
    private Integer teacherAge;

    public String getTeacherName() {
        return teacherName;
    }

    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }

    public Integer getTeacherAge() {
        return teacherAge;
    }

    public void setTeacherAge(Integer teacherAge) {
        this.teacherAge = teacherAge;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "teacherName='" + teacherName + '\'' +
                ", teacherAge=" + teacherAge +
                '}';
    }
}
```

xml:

```xml-dtd
		<bean id="demo" class="hello.demo.Student">
				<property name="studentName" value="学生名称"/>  <!-- name指JavaBean中setter方法的名字,这里只不过去除了前面的set并且首字母小写-->
				<property name="studentAge" value="1"/>			<!-- value指参数值-->
                <property name="teacher" ref="demo1"/>			<!-- ref指注入的bean,也可以理解为这是一个链接,链接指向一个类一个实例-->
        </bean>
        <bean id="demo1" class="hello.demo.Teacher">
                <property name="teacherName" value="老师名称"/>
                <property name="teacherAge" value="123"/>
        </bean>
```

测试:

```java
 @Test
    public void test() throws Exception{
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Object demo = context.getBean("demo");
        System.out.println("demo = " + demo);
    }
```

结果:

![image-20210903151406794](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903151406794.png)

> 总结:这里Spring容器就会将两个类实例化,然后Student类中进行属性赋值的时候,其中一个Teacher属性需要赋值Teacher实例,然后Spring通过ref标签属性进行引用类型注入,值类型用value注入

注意:

> 使用 setter 注入时，在 Spring 配置文件中，需要使用 <bean> 元素的子元素 <property> 为每个属性注入值。而使用构造注入时，在配置文件中，主要使用 <constructor-arg> 标签定义构造方法的参数，使用其 value 属性（或子元素）设置该参数的值

#### 2.4.3 函数注入

函数注入分为**p名称空间注入**和**spel注入**两种,说白了就是换了一种方式,一种简化而已

1. ### p名称空间注入

   之所以是说是P名称空间注入是因为在配置文件加入了一种约束文件然后以p来命名引用而已

   ```java
   xmlns:p="http://www.springframework.org/schema/p"
   ```

   书写格式: 值类型注入-p:属性名="值" 引用类型注入-p:属性名-ref="引用的<bean>name属性"

   解读:把Student类中的`studentName`属性值设置为haha,`studentAge`设置为20, 引用属性`teacher`引用了<bean name="run" class="..."/>

   ```java
   <bean name="run2" class="hello.demo.Student" p:studentName="haha" p:studentAge="20" p:teacher-ref="run"/>
   ```

   > 实际上就是set注入,spring特有,只是这里简化了<property>写法而已

2. ### Spel注入:

   ```xml-dtd
   <bean name="runSpel" class="cn.itcats.thread.Run">
   <!--取bean标签中name为"user"中property为"name"中的value值--!>
       <property name="name" value="#{user.name}"></property>
   </bean>
   ```

   这里是的引用其他bean的属性值来进行赋值注入

   SpEL特性：(1)、使用Bean的ID来引用Bean；(2)、调用方法和访问对象的属性；(3)、对值进行算术、关系和逻辑运算；(4)、正则表达式匹配；(5)、集合操作



这里我把两者合并使用了一下:

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd"
       xmlns:p="http://www.springframework.org/schema/p" >

// 开始使用
	<bean name="run" class="hello.demo.Teacher">
                <property name="teacherName" value="#{run2.studentAge}"/>
        </bean>
        <bean name="run2" class="hello.demo.Student" p:studentName="haha" p:studentAge="20" p:teacher-ref="run"/>
</beans>
```

执行结果:

![image-20210903184214702](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903184214702.png)

**注意:**这里有个注意的地方就是使用Spel注入中获取其他bena的属性的时候必须是位于自己bean的下面才能获取到,不然永远是null

#### 2.4.4 复杂注入

我们在Bean中难免会遇到数组或者List还有map啊这些参数类型,那么Spring也是有办法的!

1. ### Array数组的注入

   数组的类型既有基本数据类型也有引用数据类型,那么这里都展示出来:

   Person:

   ```java
   public class Person {
     private String name;
   
     public String getName() {
       return name;
     }
   
     public void setName(String name) {
       this.name = name;
     }
   
     @Override
     public String toString() {
       return "Person{" + "name='" + name + '\'' + '}';
     }
   }
   ```

   ArrayBean:

   ```java
   public class ArrayBean {
     private Person[] refPerson;
     private int[] arrayInt;
   
       public Person[] getRefPerson() {
           return refPerson;
       }
   
       public void setRefPerson(Person[] refPerson) {
           this.refPerson = refPerson;
       }
   
       public int[] getArrayInt() {
           return arrayInt;
       }
   
       public void setArrayInt(int[] arrayInt) {
           this.arrayInt = arrayInt;
       }
   
       @Override
       public String toString() {
           return "ArrayBean{" +
                   "refPerson=" + Arrays.toString(refPerson) +
                   ", arrayInt=" + Arrays.toString(arrayInt) +
                   '}';
       }
   }
   ```

   注意看最重要的配置文件:这里用array表示多元素注入

   ```xml-dtd
   <bean id="arr" class="hello.demo.arrayBean.ArrayBean">
   				<!--还有一种简单写法,缺点:是按逗号分隔开的数组,而一旦遇到字符串本身就是逗号的字符串,那么就无法处理了!-->
   				<!-- <property name="arrayInt" value="2,3,4"/>-->
                   <property name="arrayInt">
                       <array>
                           <value>2</value>
                           <value>3</value>
                       </array>
                   </property>
                   <property name="refPerson">
                       <array>
                           <ref bean="person"/>
                           <ref bean="person1"/>
                           <ref bean="person1"/>
                       </array>
                   </property>
               </bean>
       <bean id="person" class="hello.demo.arrayBean.Person" p:name="哇塞"/>
       <bean id="person1" class="hello.demo.arrayBean.Person" p:name="大哇塞"/>
   ```

   **解读**:如果属性为基本数据类型那么就用`array`包裹然后使用`value`来进行参数注入可如果是引用类型那么由原来的value改为`ref`进行引用

   **注意:**这里还有一种简单写法:如果只准备注入一个值(对象)那么可以直接使用value|ref即可

   ```xml-dtd
   <property name="refPerson" ref="person"/>
   ```

   来吧直接看结果:

   ![image-20210903191000855](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903191000855.png)

   如果只注入一个的话那么也是可以的:

   ![image-20210903191031475](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903191031475.png)

2. ### List集合类型注入和Set

   这里的话跟上面没什么区别只是由原来的标签`<array>`改成了`<lsit>`而已

   直接上代码,其他的我直接省略吧:

   ```xml-dtd
   	<bean id="arr" class="hello.demo.arrayBean.ArrayBean">
                   <property name="personList">
                       <list>
   						<!--外部bean可以重复使用,一个对象,这两个引用都是一个地址,因为引用都是已经创建好了的-->
                           <ref bean="person"/>
                           <ref bean="person"/>
   						<!-- 内部bean,是新创建的,两次地址都不一样,是因为每次创建<bean>都是新创建的-->
   						<bean id="person" class="hello.demo.arrayBean.Person" p:name="哇塞"/>
   						<bean id="person" class="hello.demo.arrayBean.Person" p:name="哇塞"/>
                       </list>
                   </property>
                   <property name="integerList">
                       <list>
                           <value>1</value>
                           <value>2</value>
                       </list>
                   </property>
               </bean>
       <bean id="person" class="hello.demo.arrayBean.Person" p:name="哇塞"/>
       <bean id="person1" class="hello.demo.arrayBean.Person" p:name="大哇塞"/>
   <!--使用Set类型注入-->
   <!--跟以上使用方式一模一样,我们只需要将<List>替换为<Set>即可-->
   ```

   多个元素注入就用list,如果只注入一个就直接:

   `<property name="integerList" value="1"/>`或者ref就可以了

3. ### Map集合类型注入

   bean:

   ```java
   @Data
   public class ArrayBean {
     private Map<String,String> stringMap;
     private Map<String,Person> personMap;
     private Map<Person,Person> personPersonMap;
   }
   ```

   xml:

   ```xml-dtd
    <bean id="arr" class="hello.demo.arrayBean.ArrayBean">
                   <property name="stringMap">
                       <map>
                           <entry key="map1" value="awda"/>
                       </map>
                   </property>
                   <property name="personMap">
                       <map>
                           <entry key="map2" value-ref="person"/>
                       </map>
                   </property>
                   <property name="personPersonMap">
                       <map>
                           <entry key-ref="person" value-ref="person1"/>
                       </map>
                   </property>
               </bean>
       <bean id="person" class="hello.demo.arrayBean.Person" p:name="哇塞"/>
       <bean id="person1" class="hello.demo.arrayBean.Person" p:name="大哇塞"/>
   ```

   结果:

   ![image-20210903193411110](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903193411110.png)

   > map的话哪怕只注入一个也要写map,其实我这里只列举出来三个典型的,如果出现map里面有数组啊list集合啊之类的那么这里就需要重重嵌套

4. ### Properties的注入

   第一种方式:这是支持中文的:
   
   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807153755364)
   
   第二种方式:这是不支持中文的:
   
   ```xml-dtd
   <property name="pros1">
       <value>
           name=tom
           jpa.driveClassName=com.mysql.jdbc.Driver
   		<!--
   			还有种写法:
   			name tom
   		-->
       </value>
   </property>
   ```
   
   

#### 2.4.5 Bean自动装配

本来这一节我不打算写笔记,但是在后面使用注解`@Autowired`和`@Resource`的时候两者都是基于注解形式的自动装配,所以在这里学习使用XML形式的进行自动装配是什么样子的?

**引入:**Bean的装配可以理解为依赖关系注入,而Bean的装配方式也就是Bean的依赖注入方式。而我们学习Spring容器的XML的装配有两种实现方式：**setter注入**和**构造注入**

**概念：**自动装配就是指Spring容器在不使用`<constructor-arg>`和`<property>`标签的情况下,可以使用自动装配(autowirte)相互协作的Bean之间的关联关系,将一个Bean注入其他Bean的Property中.

使用自动装配需要配置 <bean> 元素的 autowire 属性。autowire 属性有五个值,如:

|    名称     |                             说明                             |
| :---------: | :----------------------------------------------------------: |
|     no      |  默认值,表示不是有自动装配,那么Bean依赖必须通过ref属性定义   |
|   byName    | 根据Property的name自动转配,如果一个Bean的name的Property的name相同,则自动装配这个Bean到Property |
|   byType    | 根据Property的数据类型(Type)自动装配,如果一个Bean的数据类型兼容另一个Bean中Property的数据类型,则自动装配 |
| constructor | 类似于byType,根据构造方法参数的数据类型,进行byType模式的自动装配 |

1. ## no

   **理解**:"no"就是默认值,就是我们平常使用构造方法或者setter方法进行注入的方式,

   来看:

   Man:

   ```java
   public class Man {
     private String name;
     private int age;
   
     public Man() {
       System.out.println("在man的构造函数内");
     }
   
     public Man(String name, int age) {
       System.out.println("在man的有参构造函数内");
       this.name = name;
       this.age = age;
     }
   
     public void show() {
       System.out.println("名称：" + name + "\n年龄：" + age);
     }
   
     public String getName() {
       return name;
     }
   
     public void setName(String name) {
       this.name = name;
     }
   
     public int getAge() {
       return age;
     }
   
     public void setAge(int age) {
       this.age = age;
     }
   }
   
   ```

   Person:

   ```java
   public class Person {
     private Man man;
   
     public Person(Man man) {
       System.out.println("在Person的构造函数内");
       this.man = man;
     }
   
     public void man() {
       man.show();
     }
     public Person(){}
   }
   ```

   xml:

   ```xml-dtd
   <!--不使用自动装配-->
       <bean id="man" class="hello.demo.autowire.Man">
           <constructor-arg name="name" value="哇塞"/>
           <constructor-arg name="age" value="1"/>
       </bean>
   
       <bean id="person" class="hello.demo.autowire.Person" autowire="no">
           <constructor-arg name="man" ref="man"/>
       </bean>
   ```

   结果正常为:

   ![image-20210904093834548](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904093834548.png)

2. ## byName

   好重点来了,"byName"顾名思义就是根据name去匹配,总的意思就是说:

   理解:一个Bean中的属性需要注入其他Bean的类型,那么这个时候跟上面"no"的用法是一样的,在构造方法中指定name属性和对其引用类型的赋值,但是这个byName是一个升级版本,可以不用写`<constructor-arg name="man" ref="man"/>`这种代码,直接使用`autowire="byName"`,就可以完成一模一样的功能,但是这个也是有条件的必须Bean的属性名和对应Bean的name要一一对应,并且在调用方的Bean内部要有无参构造方法!

   代码:由于之前的代码都是一样的,所以只改配置文件就可以了

   ```xml-dtd
   <bean id="person" class="hello.demo.autowire.Person" autowire="byName"/> 
   // 等同于:
   	<bean id="person" class="hello.demo.autowire.Person">
           <property name="man" ref="man"/>  
       </bean>
   ```

   测试:

   ```java
   @Test
       public void test() throws Exception{
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Person demo = (Person)context.getBean("person");
           demo.man();
       }
   ```

   执行:

   ![image-20210904103808612](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904103808612.png)

   > 可以发现与上面少了一个Person的无参构造方法打印语句,所以可以结论出自动装配不会走有参构造会走无参构造并且会走字段赋值的形式也就是setter方法

   **注意**:使用这种方式那么在对应的Bean内需要setter方法

   **结论**:可以发现使用以上两者都是没区别的,只是使用上面那个方式是一个升级版本吧,那么从下面方式可以看出在bean内部进行自动装配是通过bean的setter方法进行赋值的,所以setter方法必须要有

3. ## byType

   "byType"按书面话来讲:XML 文件中 Bean 的 id 与类中的属性名称可以不同，但必须只有一个类型的 Bean

   **理解**:也就是说在自动装配中,Bean的属性名称可以不用其他Bean的id相同,但是必须要类型相同,也就是通过类型去匹配,这具体是什么意思呢?我们先不慌,先来看代码执行:

   ```xml-dtd
   <bean id="person" class="hello.demo.autowire.Person" autowire="byType"/>
   // 等同于
   	<bean id="person" class="hello.demo.autowire.Person">
           <property name="man" ref="man"/>
       </bean>
   ```

   执行效果:

   ![image-20210904103808612](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904103808612.png)

   这时你会发现这个byType跟byName两者可能跟上面没什么区别,但是我接下来我把属性名改一下:

   Person:(我这里把原本`Man`类型的名字由原来的"man"换成"Miao")

   ```java
   public class Person {
     private Man Miao;
   
     public Person(Man miao) {
       System.out.println("在Person的构造函数内");
       this.Miao = miao;
     }
   
     public Person() {
     }
   
     public void man() {
       Miao.show();
     }
   
     public void setMiao(Man miao) {
       this.Miao = miao;
     }
   }
   ```

   xml:

   ```xml-dtd
   <bean id="person" class="hello.demo.autowire.Person" autowire="byType"/>
   // idea提示优化成: ->两者都是一样的可以等同于
   	<bean id="person" class="hello.demo.autowire.Person">
           <property name="miao" ref="man"/>
       </bean>
   ```

   好接下里我们详细解释一下这个说"类型"匹配是是具体是什么意思?

   提到"类型"匹配,我们首先想到的应该就是`priavte Man Miao`它的类型应该就是`Man`,这就是它的引用类型,那么接下来就应该去配置文件中去类型匹配,看看谁是Man实例:

   ```xml-dtd
    	// 这是第一个 Bena
   	<bean id="man" class="hello.demo.autowire.Man">
           <constructor-arg name="name" value="哇塞"/>
           <constructor-arg name="age" value="1"/>
       </bean>
   	// 这是第二个 Bean 
       <bean id="man2" class="hello.demo.Annontion.Man">
           <constructor-arg name="name" value="哇塞2"/>
           <constructor-arg name="age" value="12"/>
       </bean>
   	// 这是要被注入的 Bean
   	<bean id="person" class="hello.demo.autowire.Person" autowire="byType"/>
   ```

   看结果:

   ![image-20210904114445745](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904114445745.png)

   咦:为什么是第一个呢?难道第二个Bean的类型不就是Man吗?

   好把这个问题带到下面那个问题也就解开了:

   

   在Bean中也就是我们的`Person`类中:

   ![image-20210904110602602](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904110602602.png)

   有一个引用类型Man,全限定类名`hello.demo.autowire.Man`这就是它的类型,然后就会去在Spring容器中去找那些<bean>标签中class属性为"hello.demo.autowire.Man"的bean,那么找到之后就形成了类型匹配,Spring就会自动装配!

   ![image-20210904112117899](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904112117899.png)

   

   注:第一次画图,有一说一确实有点丑,不过意思就是这个意思,所谓的类型匹配就是将Bean中对应的属性的引用类型的全限定类名去Spring容器中去匹配,找到就注入!

   那思考一个问题:如果说在Spring容器中可能存在多个Class是一样的,比如:

   ```xml-dtd
   <bean id="man" class="hello.demo.autowire.Man"/>
   <bean id="man1" class="hello.demo.autowire.Man"/>
   ```

   而我们在找的过程中就会发现有两个bean都可以与之匹配,但是注入的话只能注入一个,不能同时注入多个对吧!所以这样子的就会报异常:

   ![image-20210904113632760](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904113632760.png)

   而且IDEA也给我们提示了:

   ![image-20210904113659164](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904113659164.png)

   这样应该懂了吧!

   

4. ## constructor

   这个叫构造函数自动装配,说白了就是自动装配到构造函数中,这个应该不常用,理解就好,重点是上面的byType和byName要理解,因为后面要是有注解的形式进行自动装配,所以得需要理解其装配的过程!

   这是Person:

   ![image-20210904115315660](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904115315660.png)

   然后xml:

   ```xml-dtd
   	// 这是 Man Bean
   	<bean id="man" class="hello.demo.autowire.Man">
           <constructor-arg name="name" value="哇塞"/>
           <constructor-arg name="age" value="1"/>
       </bean>
   	// 这是进行构造函数自动装配
   	<bean id="person" class="hello.demo.autowire.Person" autowire="constructor"/>
   ```

   结果:

   ![image-20210904115453442](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904115453442.png)

   其实也可以这样去理解:

   ```xml-dtd
   <bean id="person" class="hello.demo.autowire.Person" autowire="constructor"/>
   // 等同于 下面这种方式:
   	<bean id="person" class="hello.demo.autowire.Person">
           <constructor-arg type="hello.demo.autowire.Man" ref="man"/>
       </bean>
   ```

   其实也就看明白了,跟上面的`byType`道理是一样的,通过类型去匹配

**自动装配的优缺点**:

优点:

- 自动装配只需要较少的代码就可以实现依赖注入

缺点:

- 不能自动装配简单数据类型,比如String、boolean、int；
- 相比较显示装配，自动装配不受我们程序员控制！

### 2.5 注解装配Bean

JDK1.5之后为了解决应用中Bean的数量过多导致XML配置文件过于臃肿,从而给维护和升级带来一定的困难

使用注解必须在我们配置文件中进行添加元素进行启用扫描,不扫描的话即使你添加了那么也是徒劳的,也是没用的!

```xml-dtd
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"   // 这行很重要,要添加
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"
       xmlns:p="http://www.springframework.org/schema/p">
    <context:component-scan base-package="hello"/> // 这里指定包名,就是扫描哪些包下的注解
</beans>
```

Spring 中常用的注解如下:

1. @Component

   ```
   可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。
   ```

2. @Repository

   ```
   用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同
   ```

3. @Service

   ```
   通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。
   ```

4. @Controller

   ```
   通常作用在控制层（如 Struts2 的 Action、SpringMVC 的 Controller），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同
   ```

5. @Autowired

   ```
   可以应用到 Bean 的属性变量、属性的 setter 方法、非 setter 方法及构造函数等，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配
   ```

6. @Resource

   ```
   作用与 Autowired 相同，区别在于 @Autowired 默认按照 Bean 类型装配，而 @Resource 默认按照 Bean 实例名称进行装配。
   ```

7. @Qualifier

   ```
   与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定
   ```

接下来我会单独进行示例解释:

#### 2.5.1 @Component注解

意思就是将对象存放到Spring容器中,一般写在类的上方,如:

![image-20210903211119234](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903211119234.png)

这个等同于在Spring容器中构建<bean>标签,如:

```xml-dtd
<bean id="student" class="hello.demo.constructorSetter.Student"/>
```

其中`@Component`有三个子注解

- `@Controller`用于标注该类是控制层组件,web层
- `@Service`用于标注业务层,service层
- `@Repository`用于标注数据访问组件,DAO层

而@Component泛指组件,当组件不好归类的时候,我们可以使用这个注解进行标注!

**注意:**假如不写括号内的值(即name或id),默认使用类名首字母小写作为搜索

比如:Student类中使用了@Component,没有书写括号和值,那么默认搜索id或name为student

**理解:**

1. 不书写括号中的值:

   理解:默认不写括号就只是单纯打个注解,那么Spring容器在扫描这个注解的时候就会默认使用当前类名并把首字母小写创建bean,如:

   ```xml-dtd
   <bean id="student" class="hello.demo.constructorSetter.Student"/>
   ```

2. 指明括号中的值:

   理解:写了之后那么Spring容器在扫描注解创建组件Bean的时候就会把括号里值作为id或name,比如:
   
   @Component("s"),等同于以下标签:
   
   ```xml-dtd
   <bean id="s" class="hello.demo.constructorSetter.Student"/>
   ```
   
   所以在我们测试的时候获取Bean是能找得到的!不会报错!
   
   ![image-20210903213050718](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903213050718.png)

​    

​	![image-20210903213113886](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210903213113886.png)

   

> 其他的注解都是基于以上的注解而进行的,因为以上的注解的意思是是在Spring容器中创建了<bean id="xx" class="xxx">,而其他注解都在里面进行相关的操作

#### 2.5.2 值类型注入

@Value注解表示是值类型的注入,一般放Bean中属性的上面或者setter方法上

比如:

![image-20210904142217536](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904142217536.png)

这种加在属性上面后，**实际是通过反射field赋值**，这时默认要有一个无参构造方法：

执行效果：

![image-20210904144853772](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904144853772.png)



还有一种方式：

![image-20210904145013240](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904145013240.png)

使用这种方式等同于：

```xml-dtd
<bean id="person" class="hello.MessagePrinter">
	<property name="name" value="字段注入"/>
</bean>
```

效果:

![image-20210904145349448](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904145349448.png)

**注意:**这种只能是值注入,不能是引用类型的,否则会报异常!

> 如果两者若是同时存在的话那么执行顺序是以加在setter方法上为准,其实准确来讲就是**覆盖**!因为首先会通过反射field设值然后会调用setter方法设值,当然是后面的覆盖前面的

#### 2.5.3 引用类型注入

引用类型注入分为两个注解`@Autowired`和`@Resource`两种

共同点都是进行引用类型的自动装配,这里不理解的话可以去看Bean自动装配那一节我专门详细阐述了等同于<bean>标签使用的方式

##### 2.5.3.1 @AutoWired

**概念:**顾名思义就是自动装配,也就是依赖注入!

> @AutoWired默认按照类型匹配的方式,在容器查找匹配对应的Bean,有当且仅有一个匹配的Bean时,Spring将其注入@Autowired标注的变量中;

先来看使用方式:

Student:

```java
@Component
@ToString
public class Student {
    @Value("老师名字")
    private String teacher;
}
```

Teacher:

```java
@Component
@ToString
public class Teacher {
  @Autowired
  private Student student;
}
```

测试:

```java
@Test
public void test() throws Exception{
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    Object demo = context.getBean("teacher");
    System.out.println(demo);
}
```

运行:

![image-20210904160757665](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904160757665.png)

**结论**:从代码运行可以看出已经正常完成自动装配-依赖注入

**过程**:`@Autowired`是按照类型进行匹配,是什么意思呢?意思就是@Autowired所对应的`private Student student `,其中类型就是Student,Spring容器在查找的时候会将Student的全限定类名拿到去Spring容器中在class属性里面去匹配,匹配成功就把对应的bean注入到这个`student`属性里面!

**思考**:由于`<bean class="">中class的属性值`在多个Bean中是可以重复的,这个时候Spring容器就难以抉择,不知道该选什么?就会报异常让程序员给一个方案出来!

![image-20210904161531498](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904161531498.png)

所以解决方案是:

> 如果同类型有多个，可以使用@Autowired + @Qualifier表示按照名称注入。从类型匹配改为名称匹配

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807160824814)

##### 2.5.3.2 @Resource

@Resource注解与@Autowired注解作用非常相似，这个就简单说了，看例子：

```java
package com.spring.model;

import javax.annotation.Resource;

public class Zoo1 {
    
    @Resource(name="tiger")
    private Tiger tiger;
    
    @Resource(type=Monkey.class)
    private Monkey monkey;
    
    public String toString(){
        return tiger + "\n" + monkey;
    }
}
```

这里说一下`@Resource`的装配顺序:

1. `@Resouce`后面没有任何内容,默认通过属性名去匹配bean的name,找不到再按type类型去匹配class
2. 指定了name或者type则根据指定的类型去匹配bean
3. 指定了name和type则根据指定的name和type去匹配bean,任何一个不匹配都将报错!

##### 2.5.3.3 区别:

共同点:作用与Autowired相同,都是进行引用类型的自动装配

区别:

1. `@Autowired`默认按照Bean类型匹配,而`@Resouce`默认按照Bean实例名称进行匹配
2. `@Autowired`是Spring的注解,`@Resouce`是J2EE的注解,两者导入包都是不一致的!
3. 从功能上来讲`@Resouce`要比`@Autowired`要强大,因为前者首先会从实例名称去找bean,如果找不到那么就按类型去匹配,并且还有name和type还可以显示的指定查找,而后者如果依照type找不到或者出现多个的话就得依靠`@Qualifier`注解进行指定name才能进行匹配!

#### 2.5.4 初始和销毁注解

这个的话我就不敲代码演示了,这个的话在这里只是记录并不常用,需要的时候百度解决!

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807161040371" alt="img" style="zoom:50%;" />

#### 2.5.5 Spring注解测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")  
```

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807161406945" alt="img" style="zoom:50%;" />

### 2.6 总结

其实我们使用spring注解也好还是xml配置文件也好,都是为了省略了创建对象实例化步骤,就是没有new对象实例化也能调用两个类对象之间的关系,这也体现了Spring框架的核心**IOC**体系!

> 控制反转!回到最初的概念:将创建对象的权利交给Spring容器去管理!

注意:

1. 如果配置基本类的时候,注解和配置文件都同时使用的时候(注解和配置中bean的名字相同的时候),这个时候就会发生冲突,最终以`配置文件生效`

2. 如果配置基本类的时候,注解和配置文件都同时使用的时候(注解和配置中bean的名字不相同的时候),这个时候两个不发生冲突,**都能生效**。

## 3.AOP

### 3.1 简介

AOP-**面向切面编程**和OOP-**面向对象编程**类似,也是属于一种编程思想!

**概念:**AOP采取横向抽取机制(动态代理),取代了传统纵向继承机制的重复性代码,其应用主要体现在事务处理、日志管理、权限控制、异常处理等方面

**作用：**主要是作用是分离功能性需求和非功能性需求，使开发人员可以集中处理某一个关注点或者横切逻辑，减少了对业务代码的侵入，增强代码的可阅读性和可维护性

> 简单的说：AOP的作用就是保住开发者在不修改源代码的前提下，为系统中的业务组件添加某种通用功能，AOP就是代理模式的典型应用！

![image-20211008160719311](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008160719311.png)

目前流程的AOP框架主要有两个：

1. Spring AOP：

   - Spring AOP是基于AOP编程模式的一个框架,它能够有效的减少系统间的重复代码,达到松耦合的目的。

     **注意**：Spring AOP使用纯JAVA实现，不需要专门的编译过程和类加载器，在运行期间通过代理方式向目标类植入增强的代码

     主要有两种实现方式：

     - 基于接口的JDK动态代理
     - 基于继承的CGLIB动态代理

2. AspectJ:

   - AspectJ是一个基于Java语言的AOP框架,从Spring2.0开始,Spring AOP引入了对AspectJ的支持。AspectJ扩展了Java语言，提供了一个专门的编译器，在编译时提供横向代码的植入。

**理解：**AOP是横向切面编程，这里什么叫横向？既然提到横向那么就会提到纵向，首先纵向可以理解为从上往下的，而在Java代码中继承就是这样的例子，如果我们继承的类过于多，就会一层一层的往下堆积（子类可以继承父类祖父类），这就会使代码变得高耦合！-》这就是纵向，而所谓的横向我可以理解为是一块面包，是从往右的那种，而面向切面编程也就是说我们可以在这块面包上的左右两边可以切一刀放入其他东西比如番茄酱，而在Spring框架中可以动态切入，这样我们就可以不用一个类继承一个类这样子去实现某个功能点！需要时直接切入进去，这就是面向切面编程！

AOP理解：

![image-20211008160925600](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008160925600.png)

### 3.2 AOP术语

首先我们使用AOP那么得先理解它的相关术语，至少让我们看到某些词汇不要太过于陌生：

|       名称        |                             说明                             |
| :---------------: | :----------------------------------------------------------: |
| Joinpoint(连接点) | 指那些被拦截到的点,在Spring中,指可以被动态代理拦截目标类的方法;说白了就是目标对象中在每个方法中Spring允许通知的地方,可以说每个方的法前后都可以是连接点 |
| Pointcut(切入点)  | 指要对哪些Joinpoint进行拦截,即被拦截的连接点;也可以理解为在目标对象中对连接点方法之前或者之后或者抛出异常时干点什么 |
|   Advice(通知)    |     指拦截到Joinpoint之后要做的事情,即对切入点增强的内容     |
|   Target(目标)    |           指被代理对象,被通知的对象,被增强的类对象           |
|   Weaving(织入)   |              将通知应用到连接点形成切入点的过程              |
|    Proxy(代理)    |             将通知植入到目标对象之后形成代理对象             |
|   Aspect(切面)    |                      切入点和通知的结合                      |

Aspect:通知(Advice)说明了干什么的内容(即方法体代码)和什么时候干(什么时候通知过方法名中的before、after、around等就能知道),二切入点说明了在哪干(指定到底是哪个方法),切入点表达式等定义

这里对Advice(通知)总共有5种类型:

|              通知              |                说明                |
| :----------------------------: | :--------------------------------: |
|       before（前置通知）       |   通知方法在目标方法调用之前执行   |
|       after（后置通知）        | 通知方法在目标方法返回或异常后调用 |
| after-returning（返回后通知）  |   通知方法会在目标方法返回后调用   |
| after-throwing（抛出异常通知） | 通知方法会在目标方法抛出异常后调用 |
|       around（环绕通知）       |    通知方法会将目标方法封装起来    |



> 这里只是提出了一些AOP相关术语,看不懂没关系我也看不懂,接下来通过一些练习和讲解就能慢慢去理解!

### 3.3 Spring实现AOP的方案:

​	早期Spring是自己实现AOP，后期改回使用标准的aspects(面向切面编程),Spring使用动态代理织入。那么也就是Spring支持我们在方法的前后添加增强代码。

**注**:由咱们自己来完成动态代理确实是非常麻烦，不过并不用太过担心,因为Spring已经帮咱们把困难的部分完成，我们只需要做一些简单的配置就可以完成相应的功能;

配置的时候要记住三要素:`何时`、`何地`、`做什么事`

1. 何时:在执行方法之前/之后/有异常...
2. 何地:在哪些包中的哪些类的哪些方法上面执行
3. 做什么事:在UserServie中执行update方法之前添加日志

> AOP底层就是使用代理模式中的动态代理实现的

Spring实现AOP的方案:

​	Spring实现AOP有两种方案:**JDK**与**CGLIB**

如何去选择?

1. 若目标对象实现了若干接口
   - spring使用JDK的java.lang.reflect.Proxy类代理;
2. 若目标对象没有实现任何接口
   - spring使用CGLIB库生成目标对象的子类

```
注意事项:
	1.对接口创建代理优于对类创建代理,因为会产生更加松耦合的系统
		- 对类代理是让遗留系统或无法实现接口的第三方类库同样可以得到通知,这种方案应该是备用方案!
	2.标记为final的方法不能够被通知
	3.spring是为目标类产生子类,任何需要被通知的方法都被复写,将通知织入,final方法是不允许被重写的!
	4.AOP是一种思想,跟语言没什么关系,不是Spring独有的,可以理解为根spring本身没有什么关系!
```

### 3.4 JDK动态代理

Spring JDK 动态代理需要实现 InvocationHandler 接口，重写 invoke 方法，客户端使用 Java.lang.reflect.Proxy 类产生动态代理类的对象。

代码:

UserManager:

```java
package hello.demo.autoProxy;

/**
 * @program: springProject
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-04 19:32
 */
public interface UserManager {

  /**
   * 新增用户抽象方法
   *
   * @param userName 用户名
   * @param password 密码
   */
  void addUser(String userName, String password);

  /**
   * 删除用户抽象方法
   *
   * @param userName 用户名
   */
  void delUser(String userName);
}
```

UserManagerImpl:

```java
package hello.demo.autoProxy;

/**
 * @program: springProject
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-04 19:34
 */
public class UserManagerImpl implements UserManager {
  @Override
  public void addUser(String userName, String password) {
    System.out.println("正在执行添加用户的方法");
    System.out.println("用户名" + userName + " 密码:" + password);
  }

  @Override
  public void delUser(String userName) {
    System.out.println("用户正在执行删除方法");
    System.out.println("用户名称" + userName);
  }
}
```

MyAspect:(表示准备执行切入的方法)

```java
package hello.demo.autoProxy;

/**
 * @program: springProject
 * @description: 在切面中定义两个增强的方法
 * @author: MiaoWei
 * @create: 2021-09-04 19:36
 **/
public class MyAspect {
    public void myBefore(){
    System.out.println("执行方法之前!");
    }
    public void myAfter(){
    System.out.println("执行方法之后!");
    }
}
```

JdkProxy:(这里就是核心-用来定义切入目标方法的位置和利用动态代理机制实现)

```java
package hello.demo.autoProxy;

import org.junit.Test;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @program: springProject
 * @description: 这里使用JDK动态代理实现InvocationHandler接口
 * @author: MiaoWei
 * @create: 2021-09-04 19:41
 */
public class JdkProxy implements InvocationHandler {
  final MyAspect myAspect = new MyAspect();
  // 修改代理的目标对象
  private Object target;

  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    // 这里可以理解为切面方法
    myAspect.myBefore();
    Object result = method.invoke(target, args);
    myAspect.myAfter();
    return result;
  }

  // 定义获取代理对象方法
  private Object getJDKProxy(Object targetObject) {
    // 为目标对象target赋值
    this.target = targetObject;

    // JDK动态代理只能代理实现接口的类,从newProxyInstance函数所需的函数就可以看出来
    return Proxy.newProxyInstance(
        targetObject.getClass().getClassLoader(), targetObject.getClass().getInterfaces(), this);
  }

  @Test
  public void test() throws Exception {
    // 实例化JDKProxy对象
    JdkProxy proxy = new JdkProxy();
    // 获取代理对象
    UserManager manager = (UserManager) proxy.getJDKProxy(new UserManagerImpl());
    // 执行新增方法
    manager.addUser("张三", "123");
    // 执行删除方法
    manager.delUser("张三");
  }
}
```

实现效果:

![image-20210904200259054](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904200259054.png)

这里我编辑红线进行分开,从上面可以清楚看出执行每一个方法之前或者之后都会调用其他切入其他的代码!这就是动态代理!

**注意:**必须代理的目标类要实现一个或多个接口

### 3.5 CGLIB动态代理

从JDK动态代理可以看出使用起来确实要方便很多,但是有一个缺点就是目标类必须要实现一个或多个接口,这种具有一定的局限性,那假如我就是不想实现接口就是单纯的一个类而已那么则就会动态代理失效的情况!

所以`CGLIB代理`应运而生!

**概念:**CGLIB是一个高性能开源的代码生成包,它被许多AOP框架所会使用,其底层是通过一个小而快的字节码处理框架`ASM`(Java字节码操控框架)转换字节码并生成新的类!

**导包:**若是不用spring的话那么我们需要手动导包"asm-x.x.jar"和"CGLIB.XX.jar",但是如果导入Spring的核心包那么就已经集成了,就可以不用导包了!

代码:

由于其他的代码都是一样的我就只需要新建一个代理类:

```java
package hello.demo.autoProxy;

import org.junit.Test;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * @program: springProject
 * @description: 使用CGLIB动态代理,实现MethodInterceptor接口
 * @author: MiaoWei
 * @create: 2021-09-04 20:13
 */
public class CglibProxy implements MethodInterceptor {
  final MyAspect myAspect = new MyAspect();
  // 需要代理的目标对象
  private Object target;

  /**
   * 拦截方法
   *
   * @param o o
   * @param method 方法
   * @param objects 对象
   * @param methodProxy 方法的代理
   * @return {@link Object}
   * @throws Throwable throwable
   */
  @Override
  public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy)
      throws Throwable {
    // 在调用拦截方法之前执行方法
    myAspect.myBefore();
    // 执行目标方法
    Object invoke = method.invoke(target, objects);
    // 调用拦截方法之后知心方法
    myAspect.myAfter();
    return invoke;
  }

  /**
   * 获取cglib代理 定义获取代理对象方法
   *
   * @param objectTarget 目标对象
   * @return {@link Object}
   */
  public Object getCglibProxy(Object objectTarget) {
    // 为目标对象target赋值
    this.target = objectTarget;
    Enhancer enhancer = new Enhancer();
    // 设置父类,因为CGLIB是针对指定的类生成一个子类,所以需要指定父类
    enhancer.setSuperclass(objectTarget.getClass());
    // 设置回调
    enhancer.setCallback(this);
    // 创建并返回代理对象
    return enhancer.create();
  }

  @Test
  public void test() throws Exception {
    // 实例化CglibProxy对象
    CglibProxy proxy = new CglibProxy();
    // 获取代理对象
    UserManager cglibProxy = (UserManager) proxy.getCglibProxy(new UserManagerImpl());
    // 执行新增方法
    cglibProxy.addUser("张三", "123");
    // 执行删除方法
    cglibProxy.delUser("张三");
  }
}
```

结果:

![image-20210904202607916](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210904202607916.png)

跟之前的结果是一模一样的!

### 3.6 区别

我们使用`JDK动态代理`和`CGLIB动态代理`的执行效果都是一样的,那我们在使用的该如何去选择和判断两者之间有什么区别呢?

区别:

1. 实现方式不同
   - JDK动态代理是利用反射机制生成一个实现代理接口的匿名类,在调用具体方法钱调用`InvokeHandler`来处理!
   - CGLIB动态代理是利用ASM开源包进行加载代理对象类的class文件,通过修改其字节码生成子类来处理!
2. 针对方式不同:
   - JDK动态代理只能对接口的类生成代理,而不是针对类!
   - CGLIB是针对类实现代理,主要是对指定的类生成一个子类,覆盖其中的方法; 因为是继承所以该类或方法不能声明成final类型!

**JDK动态代理特点:**

- 代理对象必须实现一个或多个接口
- 以接口的形式接收代理实例,而不是代理类

**CGLIB动态代理特点:**

- 代理对象不能被final修饰
- 以类或接口形式接收代理实例

**JDK与CGLIB动态代理的性能比较:**

- 生成代理实例性能:JDK>CGLIB
- 代理实例运行性能: JDK>CGLIB

总结：

JDK动态代理：

```
有接口的类使用JDK的动态代理(JDK动态代理不支持没有接口的类)，如果有n个接口,必然有n个实现,只用写1个代理类JDKProxy就可以对所有有接口进行处理，如果有代理主题角色存在,必须修改调用方才能实现代理
```

CGLIB动态代理：

```
没有接口的类使用CGLIB动态代理(类有没有接口都可以支持)，只用写1个代理类CglibProxy就可以对所有没有接口的不能是final类都进行处理，如果有代理主题角色存在,必须修改调用方才能实现代理
```



### 3.7 AspectJ

之前学习JDK动态代理和CGLIB都是基于代理类的AOP实现,而现在又在它们之上又推出一个新的AOP方式:`AspectJ`

> AspectJ 是一个基于 Java 语言的 AOP 框架，它扩展了 Java 语言，提供了强大的 AOP 功能

而实现`AspectJ`有两种方式:XML配置文件形式、注解形式

#### 3.7.1 XML

在使用XML形式进行声明式定义切面、切入点及通知，而所有的切面和通知都必须定义在`<aop:config>`元素中!

而是由`<aop:config>`元素之前,我们需要导入Spring aop命名空间,如下:

```xml-dtd
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"  这是这行
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop   
       http://www.springframework.org/schema/aop/spring-aop.xsd">  这行
</beans>
```

还要导入相关的包:(我这里使用maven的形式):

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.wei</groupId>
    <artifactId>springProject</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <!--spring的核心依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.13.RELEASE</version>
        </dependency>
        <!--Spring的AOP包-->
<!--        <dependency>-->
<!--            <groupId>org.springframework</groupId>-->
<!--            <artifactId>spring-aop</artifactId>-->
<!--            <version>5.3.9</version>-->
<!--        </dependency>-->
        <!--使用AspectJ方式注解需要相应的包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.2.7.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjtools</artifactId>
            <version>1.9.5</version>
        </dependency>

        <dependency>
            <groupId>aopalliance</groupId>
            <artifactId>aopalliance</artifactId>
            <version>1.0</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.0</version>
        </dependency>

        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.3.0</version>
        </dependency>

        <dependency>
            <groupId>org.aopalliance</groupId>
            <artifactId>com.springsource.org.aopalliance</artifactId>
            <version>1.0.0</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>com.springsource.org.aspectj.weaver</artifactId>
            <version>1.7.2.RELEASE</version>
        </dependency>

        <!--log4j的日志文件-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <!--junit测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>compile</scope>
        </dependency>
        <!-- lombok插件-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.18</version>
        </dependency>
        <!--javax-->
        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>jsr250-api</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
</project>
```

> 可能用不了那么多但是我还是想全部提进去!貌似只有注解才会导包,使用xml配置文件只需要导入命名空间就可以了!

我们直接先上栗子:

Loggin:

```java
package hello.demo.AOP;

/**
 * @program: springProject
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-04 21:00
 */
public class Logging {
  /** 前置通知 */
  public void beforeAdvice() {
    System.out.println("前置通知");
  }

  /** 后置通知 */
  public void afterAdvice() {
    System.out.println("后置通知");
  }

  /**
   * 返回后通知
   *
   * @param retVal ret瓦尔
   */
  public void afterReturningAdvice(Object retVal) {
    System.out.println("返回值" + retVal.toString());
  }

  /**
   * 抛出异常通知
   *
   * @param e e
   *
   */
  public void afterThrowingAdvice(RuntimeException e) {
    System.out.println("这里的异常为:" + e.toString());
  }
    
  //环绕通知,这里调用,ProceedingJoinPoint是固定的
  public void around(ProceedingJoinPoint pjp){
        try {
            // 开启事务
            beforeAdvice();
            pjp.proceed();  // 相当于调用方法,这里就相当于模拟了在调用方法之前和之后所调用切面的方法
            // 提交事务
            afterReturningAdvice();
        } catch (Throwable e) {
            // 回滚
            afterThrowingAdvice(e);
        }finally {
            //  关闭
            afterAdvice();
        }

  }
}
```

Man:

```java
package hello.demo.AOP;

public class Man {
  private String name;
  private int age;

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public void throwException(Throwable e) {
    System.out.println("抛出异常");
    throw new RuntimeException(e.getMessage);
  }
}
```

来进行xml配置:

```xml-dtd
<!--配置AOP,何时,何地,做什么事-->
        <aop:config>
			<!--
				切面,何时
				ref:哪个bean切进去
			-->	
            <aop:aspect id="log" ref="loging">
				<!-- 
					切点,何地
					*:任意返回值
					..:任意参数
				-->
                <aop:pointcut id="selectAll" expression="execution(* hello.demo.AOP.*.*(..))"/>
				<!--做什么事-->
				<!--前置通知:在哪个方法前执行xx方法-->
                <aop:before method="beforeAdvice" pointcut-ref="selectAll"/>
				<!--最终通知:最终方法,整个方法执行结束完毕执行-->
                <aop:after method="afterAdvice" pointcut-ref="selectAll"/>
				<!--后置通知:在哪个方法之后执行xx方法-->
                <aop:after-returning method="afterReturningAdvice" pointcut-ref="selectAll" returning="retVal"/>
				<!--异常通知,method:切面的方法,pointcut-ref:目标方法,throwing:抛出异常后将异常传给切面的方法里去,这里e必须跟切面中的方法接收参数名保持一致!-->
                <aop:after-throwing method="afterThrowingAdvice" pointcut-ref="selectAll" throwing="e"/>

                <!--环绕通知,一个环绕能代替以上的通知-->
                <aop:around method="around" pointcut-ref="pointCut"></aop:around>
            </aop:aspect>
        </aop:config>
        <bean id="man" class="hello.demo.AOP.Man">
            <property name="name" value="张三"/>
            <property name="age" value="123"/>
        </bean>
    <bean id="loging" class="hello.demo.AOP.Logging"/>
```

这是理解图:

![image-20211008154005562](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008154005562.png)

测试:

```java
 @Test
  public void test() throws Exception {
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    Man demo = (Man) context.getBean("man");
    demo.getName();
    demo.getAge();
    demo.throwException();
  }
```

执行效果:

![image-20210905090904730](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210905090904730.png)

从这里可以看出每调用一个方法那么这里都会使用切面定义进行调用!

> 获取的时候必须是接口是父类!因为Aop底层是动态代理实现的,既然是代理我们就不能使用直接实现类去操作,所以我们需要使用它的父类或者接口去操作!

这里我详细介绍一下关于xml中AOP的配置:

1. ### 定义切面<aop:aspect>

   在Spring配置文件中,使用`<aop:aspect>`元素定义切面,该元素可以将定义好的Bean转换成切面Bean,所以使用<aop:aspect>之前需要先定义一个普通的Spring Bean

   ```xml-dtd
   <aop:config>
               <aop:aspect id="log" ref="loging">
   				...
               </aop:aspect>
   </aop:config>
   <bean id="loging" class="hello.demo.AOP.Logging"/>
   ```

   > 理解:这里可以理解为需要将一个bean定义一个切面Bean,什么意思呢?就是将一个bean定义好,哪里需要就用哪里!

   **注意:**其中，id 用来定义该切面的唯一表示名称，ref 用于引用普通的 Spring Bean。

2. ### 定义切入点<aop:pointcut>

   `<aop:pointcut>`用来定义一个切入点,当<aop:pointcut>元素作为<aop:config>元素的子元素定义时,表示该切入点是全局切入点,它可被多个切面所共享;但是如果当<aop:pointcut>元素作为<aop:aspect>元素的子元素时,表示该切入点只对当前切面有效

   ```xml-dtd
    <aop:config>
               <aop:pointcut id="selectAll" expression="execution(* hello.demo.AOP.*.*(..))"/>
   </aop:config>
   ```

   > 理解:切入点可以理解为从哪里进行切入,找准一个点!而上面这段话的意思就是说如果该标签位于<aop:config>里的子元素那么可以说该切入点被多个切面Bena共享,但是如果是<aop:aspect>的子元素的时候,那么只能被当前切入面所有效,说白了就是作用域问题 

   **注意**:其中，id 用于指定切入点的唯一标识名称，execution 用于指定切入点关联的切入点表达式

   execution 格式为：

   ```
   execution(modifiers-pattern returning-type-pattern declaring-type-pattern name-pattern(param-pattern)throws-pattern)
   ```

   其中:

   - returning-type-pattern、name-pattern、param-pattern 是必须的，其它参数为可选项。
   - modifiers-pattern：指定修饰符，如 private、public。
   - returning-type-pattern：指定返回值类型，`*`表示可以为任何返回值。如果返回值为对象，则需指定全路径的类名。
   - declaring-type-pattern：指定方法的包名。
   - name-pattern：指定方法名，`*`代表所有，`set*` 代表以 set 开头的所有方法。
   - param-pattern：指定方法参数（声明的类型），`(..)`代表所有参数，`(*)`代表一个参数，`(*,String)`代表第一个参数可以为任何值，第二个为 String 类型的值。
   - throws-pattern：指定抛出的异常类型。

   > 例如：`execution(* hello.demo.AOP.*.*(..))`表示匹配 hello.demo.AOP 包中任意类的任意方法。

3. ### 定义通知

   AspectJ 支持 5 种类型的 advice，如下。

   ```xml-dtd
   				 <aop:before method="beforeAdvice" pointcut-ref="selectAll"/>
                   <aop:after method="afterAdvice" pointcut-ref="selectAll"/>
                   <aop:after-returning method="afterReturningAdvice" pointcut-ref="selectAll" returning="retVal"/>
                   <aop:after-throwing method="afterThrowingAdvice" pointcut-ref="selectAll" throwing="e"/>
   ```

   通知的几种类型:

   1. 前置通知———目标方法运行之前调用
   2. 后置通知———目标方法运行之后调用(如果出现异常不调用)
   3. 环绕通知———目标方法之前和之后都调用
   4. 异常拦截通知———如果出现异常，就会调用
   5. 后置通知———目标方法运行之后调用(无论是否出现异常都会调用)

> 思考?如果在IO流进行方法处理的时候,必须抛出异常,那么这个时候我们不需要bean切面异常方法执行,那么我们就在service层进行捕获或者Controller进行捕获就可以了!就不会执行异常切面方法!

#### 3.7.2 注解

​	尽管我们可以使用XML配置文件可以实现AOP开发,但是如果所有的相关配置都集中在配置文件中,势必会导致XML配置文件过于臃肿,从而给维护和升级带来一定的困难,所以注解就出来了,Spring框架可以根据这些注解生成AOP代理:

这是相关注解介绍:

|      名称       |                        说明                         |
| :-------------: | :-------------------------------------------------: |
|     @Aspect     |                 用于定义一个切面。                  |
|    @Pointcut    |                用于定义一个切入点。                 |
|     @Before     |       用于定义前置通知，相当于 BeforeAdvice。       |
| @AfterReturning |   用于定义后置通知，相当于 AfterReturningAdvice。   |
|     @Around     |     用于定义环绕通知，相当于MethodInterceptor。     |
| @AfterThrowing  |        用于定义抛出通知，相当于ThrowAdvice。        |
|     @After      | 用于定义最终final通知，不管是否异常，该通知都会执行 |
| @DeclareParents |   用于定义引介通知，相当于IntroductionInterceptor   |

我们使用注解那么也要在配置文件启动注解:

```xml-dtd
//加入命名空间,命名空间上面 XML 已经写了!
<!--切面注解-->
<aop:aspectj-autoproxy>
```

直接看代码:

```java
package hello.demo.AOP;

import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

/**
 * @program: springProject
 * @description:
 * @author: MiaoWei
 * @create: 2021-09-04 21:00
 */
//Aspect相当于我们之前的<aop:aspect ref="xxx">
@Aspect
@Component
public class Logging {
   //切点,自定义方法, 
  @Pointcut("execution(* hello.demo.AOP.*.*(..))")
  private void mypointcut(){}


  /** 前置通知,参数是切点,用方法名代替使用 */
  @Before("mypointcut()")
  public void beforeAdvice() {
    System.out.println("前置通知");
  }

  /** 最终通知,整个方法执行完毕执行 */
  @After("mypointcut()")
  public void afterAdvice() {
    System.out.println("后置通知");
  }

  /**
   * 返回后通知,就是方法执行后执行通知
   *
   * @param retVal ret瓦尔
   */
  @AfterReturning(value = "mypointcut()",returning = "retVal")
  public void afterReturningAdvice(Object retVal) {
    System.out.println("返回值" + retVal.toString());
  }

  /**
   * 抛出异常通知
   *
   * @param e e
   *
   */
  @AfterThrowing(value = "mypointcut()",throwing = "e")
  public void afterThrowingAdvice(RuntimeException e) {
    System.out.println("这里的异常为:" + e.toString());
  }
    
    /**
     *  没用环绕通知之前：执行的顺序有问题
     *  环绕通知： 没有任何问题
     */
    @Around("mypointcut()")
    public void around(ProceedingJoinPoint pjp){
        try {
            // 开启事务,前置通知
            beforeAdvice();
            pjp.proceed();  // 相当于调用方法
            // 提交事务
            afterReturningAdvice();
        } catch (Throwable e) {
            // 回滚
            afterThrowingAdvice(e);
        }finally {
            //  关闭
            afterAdvice();
        }

    }
}
```

注意:我在需要定义的切面Bean里在类上添加注解`@Aspect`来表明该类是用来定义切面的!

然后:

```java
@Pointcut("execution(* hello.demo.AOP.*.*(..))")
  private void mypointcut(){}
```

解读:使用`@Pointcut`表示定义一个切入点,这里有一个要求:方法必须是private，返回值类型为void，名称自定义，没有参数;后面使用后置前置通知都需要这个方法名作为标识符!

> 这里定义切入点的格式中`hello.demo.AOP`意思是匹配包中任意类的任意方法。记住:必须是包下的类,不能层层嵌套,定义一个父包下还有子包那么是会报错的!



AOP注解解析:

1. @Before前置通知(Before advice):
   - 在某连接点(JoinPoint)-核心代码(类或者方法)之前执行的通知,但这个通知不能阻止连接点前的执行;为啥不能阻止线程进入核心代码呢?因为@Before注解的方法入参不能传`ProceedingJoinPoint`,而只能传入`JoinPoint`,要知道AOP走到核心代码就是通过调用ProceedingJoinPoint的`proceed()`方法,而JoinPoint没有这个方法.
2. @After后通知(After advice):
   - 当某连接点退出的时候执行的通知(无论是正常返回还是异常退出);
3. @AfterReturning返回后通知(After return advice):
   - 在某连接点正常完成后执行的通知,不包括抛出异常的情况
4. @Around环绕通知(Around advice):
   - 包围一个连接点的通知,类似于Servlet中的Filter的doFilter方法,可以在方法的调用前后完成自定义的行为,也可以选择不执行,这是AOP的最重要的也是最常用的注解;使用这个注解的方法入参传的是`ProceedingJinPoint pjp`,可以决定当前线程能否进入核心方法中-通过调用`pjp.proceed()`;
5. @AfterThrowing抛出异常后通知(After throwing advice):
   - 在方法抛出异常时退出执行的通知



在上面介绍@Before注解的时候牵扯两个类:`ProceedingJoinPoint`和`JoinPoint`

**说明**:ProceedingJoinPoint继承了JoinPoint,是在JoinPoint的基础上暴露出proceed这个方法,而proceed很重要,这是是**AOP代理链执行的方法**,暴露出这个方法,就能支持aop:around这种切面(而其他的几种切面只需要用到JoinPoint,这个跟切面类型有关),能决定是否走代理还是走自己拦截的其他逻辑,

**注意:**可以看一下JdkDynamicAopProxy的invoke方法,了解一下代理链的执行原理,这样你就能明白proceed方法的重要性



还有要注意环绕通知需要这样写:

```java
public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
	    //环绕方法执行前
            //proceedingJoinPoint.proceed();表示对拦截的方法进行放行
            //若注释proceedingJoinPoint.proceed()则不会执行被AOP匹配的方法
			proceedingJoinPoint.proceed();
            //环绕方法执行后
	}
```



### 3.8 总结

AOP是面向切面编程,是对咱们OOP的一个补充,Spring的AOP允许我们在方法的前后加上相应的功能进行增强!

> Spring的AOP核心使用的是代理模式中的动态代理模式完成的

代理模式分为两种:

1. 静态代理:一对一的那种,为一个类生成一个代理对象,我们基本不用,
2. 动态代理:众多类只用一个代理对象

而我们Spring底层使用动态代理模式解决方案有两种:**JDK动态代理**、**CGLIB动态代理**

而我们不用去考虑到底是选择第一个还是第二个,因为Spring底层会帮我们自动做选择:如果有接口的类那么默认选择JDK代理模式,反之CGLIB代理模式!

Spring使用简单的配置让我们完成AOP

1. XML
2. 注解

> 而我们在配置的时候只需要注意找到何时(方法前后,异常),何地(哪些类的哪些方法),做什么(安全中是添加事务)

### 3.9 代理模式

**定义**:代理模式的英文叫做Proxy或Surrogate，中文都可译为”代理“，所谓代理，就是一个人或者一个机构代表另一个人或者另一个机构采取行动。在一些情况下，一个客户不想或者不能够直接引用一个对象，而`代理对象可以在客户端和目标对象之间起到中介的作用`

开发术语:

- 抽象主题角色

  ```
  声明了真实主题和代理主题的共同接口，这样一来在任何可以使用真实主题的地方都可以是使用代理主题。
  ```

- 代理主题角色

  ```
  代理主题角色内部含有对真实主题的引用，从而可以在任何时候操作真实主题对象；代理主题角色提供一个与真实主题角色相同的接口，以便可以在任何时候都可以替代真实主题控制对真实主题的引用，负责在需要的时候创建真实主题对象（和删除真实主题对象）；代理角色通常在将客户端调用传递给真实的主题之前或之后(前置增强/通知，后置增强/通知)，都要执行某个操作，而不是单纯地将调用传递给真实主题对象。
  ```

- 真实主题角色

  ```
  定义了代理角色所代表地真实对象
  ```

理解:在之前使用XML方式进行AOP的时候,抽象主题角色就是接口,代理主题角色就是切面,真实主题角色就是要切入的类或者方法!

![image-20211008174251927](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008174251927.png)

具体生活中例子:

![image-20211008174316342](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211008174316342.png)

## 4.创建Bean的方式

### 4.1 第一种

**实例化一个有公共无参构造的方法**

```xml
<bean id="myBean" class="cn.itsource._05_createbean._01.MyBean"></bean>
```

xml中创建<bean>标签就可以创建一个bean

### 4.2 第二种

**集成静态简单工厂**

```java
MyBean类:
public class MyBean {}
MyBeanFactory类:
public class MyBeanFactory {

    public static MyBean getMyBean(){
        return new MyBean();
    }
}
```

XML:

```xml
<!--
 	 类名调用静态方法
-->
<bean id="myBean" class="cn.itsource._05_createbean._02.MyBeanFactory" factory-method="getMyBean"></bean>
```

使用工厂模式,class获取它的bean,然后创建factory-method:它的方法,通过static静态方法调用,这样我们在返回Bean的时候就会直接返回这个方法返回的对象,通过一个工厂模式就可以实现!

### 4.3 第三种

**集成简单实例工厂**

```java
MyBean类:
public class MyBean {}
MyBeanFactory类:
public class MyBeanFactory {
    
    public MyBean getMyBean(){
        return new MyBean();
    }
}
```

XML:

```xml
<!--
    先有对象
    MyBeanFactory myBeanFactory = new MyBeanFactory()；
-->
<bean id="myBeanFactory" class="cn.itsource._05_createbean._03.MyBeanFactory" ></bean>
<bean id="myBean" factory-bean="myBeanFactory" factory-method="getMyBean"></bean>
```

通过xml中factory-bean加载工厂bean进行调用然后在通过返回factory-method调用其方法作为返回对象,这是先有对象再有方法!

### 4.4 第四种

**使用FactoryBean**

一般都不自己写FactoryBean实例,而是使用spring已经写好的,FactoryBean也是用来生产Bean的。它主要是用于在集成一些框架的时候。框架中的核心类我们并没有提供公共无参的构造器

```java
MyBean类:
public class MyBean {}
MyBeanFactoryBean类:
/**
 * 写一个 类，实现FactoryBean 接口
 */
public class MyBeanFactoryBean implements FactoryBean<MyBean>{
    //最后返回的则是这一个对象
    @Override
    public MyBean getObject() throws Exception {
        return new MyBean();
    }
	//返回类型
    @Override
    public Class<?> getObjectType() {
        return MyBean.class;
    }

    /**
     * 是否是单例
     * @return
     */
    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

XML:

```xml
 <!--
       如果当前类实现了factoryBean接口，
       spring会自动调用getObject()方法，
       返回值就是我们要的bean
     -->
<bean id="myBean" class="cn.itsource._05_createbean._04.MyBeanFactoryBean"></bean>
```

工厂bean的方式,实现接口FactoryBean的方式,将我们要处理创建的类通过泛型的方式放进接口,然后我们在xml中用<bean>标签的形式进行创建它的实例-在创建对象之前要操作的事情就可以使用这个方式;spring底层就会自动去找类中是否存在并且调用getObject方法,返回值就是我们要的Bean

> 理解:使用第四种方式主要用于一些类无法提供公共无参构造方法,并且或者需要在创建对象之前操作一些东西!

#### 4.4.1 面试题

说一下BeanFactory与FactoryBean的区别?

```java
BeanFactory接口是顶层父工厂,也是一个容器,主要用于获取Bean的实例
FactoryBean接口,主要用于实例化不能通过默认无参构造方法获取的Bean,通过子类的getObject方法来返回实例,比如Spring整合mybatis中的SessionFactory,使用SQlSessionFactoryBean

   注:如果一个类没有构造方法,但是可以用其他方式拿到它的对象,就可以用FactoryBean,主要多用于Spring的框架集成!
```



## 5.JDBC

  spring中提供了一个可以操作数据库的对象，对象封装了jdbc技术 ————JDBCTemplate JDBC模板对象，而JdbcDaoSupport则对JdbcTemplate进行了封装，所以要操作JdbcTemplate，或只需要继承JdbcDaoSupport即可

这是实现类:

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807174204755" alt="img" style="zoom:50%;" />

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/2018080717424540" alt="img" style="zoom:50%;" />

 依赖关系配置：

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807175348552" alt="img" style="zoom:50%;" />

测试:

<img src="https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180807175618666" alt="img" style="zoom:50%;" />

## 6. 总结

我们最后再来体会一下用Spring创建对象的过程:

​	通过`ApplicationContext`这个IOC容器的入口,用它的两个具体的实现子类,从class Path或者file path中读取数据,用`getBean()`获取具体的bean instance

那使用Spring到底省略了我们什么工作?

> 答:new的过程;把new的过程交给第三方来创建、管理；这就是“解耦”

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmWUCpCfC5jcxPJXwoMhcjMXTQ0tWBz1RLWaK9RicHcdSLYqYD6t4zffdHVaasI74AbGtrt52wVMug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这里思考几个关键性问题:

1. 何为控制（spring容器）,控制的是什么?

   - 是bean的创建、管理的权利，控制bean的整个生命周期

2. 何为反转,反转了什么?

   - 把这个权利交给了Spring容器，而不是自己去控制，这就是反转。由之前的自己主动创建对象，变成现在被动接收别人给我们的对象的过程，这就是反转。

3. 何为依赖,依赖什么?

   - 程序运行需要依赖外部的资源，提供程序内对象的所需要的数据、资源

4. 何为注入,注入什么?

   - 配置文件把资源从外部注入到内部，容器加载了外部的文件、对象、数据，然后把这些资源注入给程序内的对象，维护了程序内外对象之间的依赖关系

   > 所以说。控制反转是通过依赖注入实现的。但是也有差别的，像是从不同角度描述的同一件事
   >
   > 1.IOC是设计思想，DI是具体的实现方式
   >
   > 2.IOC是理论。DI是实践
   >
   > 从而实现对象之间的解耦，当然，IOC也可以通过其他方式来实现，而DI只是Spring的选择而已，其中IOC和DI也并非Spring框架提出来的，Spring只是应用了这个设计思想和理念到自己的框架中去！

5. 为什么要用IOC这种思想?IOC能带来什么好处?

   **解耦！**

   ​	在项目中，底层的实现都是由很多个对象组成的，对象之间彼此合作实现项目的业务逻辑。但是，很多很多对象紧密结合在一起，一旦有一方出问题了，必然会对其他对象有所影响，所以才有了解藕的这种设计思想。

   ![img](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmWUCpCfC5jcxPJXwoMhcjMF203QicHAFo667MzoMW75e9jW8wwufT7Fb97OhfcaoUxfrwnnicSibpmg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmWUCpCfC5jcxPJXwoMhcjMXTQ0tWBz1RLWaK9RicHcdSLYqYD6t4zffdHVaasI74AbGtrt52wVMug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

​	如上图所示，本来 ABCD 是互相关联在一起的，当加入第三方容器的管理之后，每个对象都和第三方法的 IoC 容器关联，彼此之间不再直接联系在一起了，没有了耦合关系，全部对象都交由容器来控制，降低了这些对象的亲密度，就叫“**解藕**”。

Spring也是用的`set()`方法,它只不过提供了一套更加完善的实现机制而已!

而说到底,,底层的原理并没有很复杂,只是为了提高扩展性、兼容性；Spring提供了丰富的支持，所以才觉得源码比较难，这是因为框架是要给各种各样的用户来使用的，它们考虑更多的是扩展性，如果让我们来实现，或许三五行就能搞定，但是我们实现的不完善、不完整、不严谨、总之不高大上，所以它写三五十行把框架设计的尽可能的完善，提供了丰富的支持，满足不同用户的需求，才能占领更大的市场！
