# 笔记

## 1. 概念

简介：Mybatis-Plus是一个Mybatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

特性：

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

官网地址：https://baomidou.com/pages/24112f/#%E7%89%B9%E6%80%A7

## 2.快速开始

> 步骤

1. 新建数据库“mybatis_plus_miao”，导入官网的SQL表及其数据：

   ```sql
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
       id BIGINT(20) NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   );
   ```

   ```mysql
   DELETE FROM user;
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

2. 新建一个工程：

   1. 导入依赖：

      ```xml
      <!--数据库连接驱动-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
      </dependency>
      <!--mybatis-plus-->
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus-boot-starter</artifactId>
          <version>3.0.5</version>
      </dependency>
      //这是重要的依赖，其他都是基本的依赖就不粘贴了！
      ```

      注意：在导入mybatis-plus要避免同时和mybatis同时导入，这会引起版本的差异等其他问题！

   2. 连接数据库：

      ```yml
      # DataSource Config
      spring:
        datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          # useSSL:使用安全配置 serverTimezone=GMT%2B8 时区配置
          url: jdbc:mysql://localhost:3306/mybatis_plus_miao?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
          username: root
          password: root
      ```

   3. 新建mapper：

      ```java
      //在对应的mapper上面继承基本的类BaseMapper
      @Repository  //代表持久层
      public interface UserMapper extends BaseMapper<User> {
          //这里面就有基本的CRUD的代码了
      }
      ```

      这里我们点击BaseMapper就可以发现已经提供了基本的CRUD的代码，并且我们只需要传递一个泛型就行

      ![image-20220213141639387](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131416503.png)

   4. 实体类和在主启动类上添加@MapperScan("mapper的路径")省略

   5. 测试：

      ```java
      @SpringBootTest
      class SpringBootMybatisPlusDemoApplicationTests {
      
          @Autowired
          private UserMapper userMapper;
          @Test
          void contextLoads() {
              //这里条件参数是Wrapper，表示这是条件构造器
              //这里表示没有条件，就直接查询全部用户
              List<User> users = userMapper.selectList(null);
              users.forEach(System.out::println);
          }
      }
      ```

   6. 结果：

      ![image-20220213141917957](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131419996.png)

## 3.配置日志

在我们的开发过程中，我们需要知道这段SQL在数据库中它是如何执行的，所以需要查看日志并打开它！

```yaml
# 配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl  # 这是控制台打印日志
```

这是其他日志：

![image-20220213142652310](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131426363.png)

如果要导入例如：Log4J2或者Slf4J那么就需要导入对应的依赖即可！

这是结果：

![image-20220213142759280](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131427358.png)



但是要注意：项目上线的话就要关闭这个打印日志的功能，不然会浪费时间的！



## 4.CURD扩展

### 4.1 插入及相关主键生成策略

我们先测试一下插入，看看会发生什么样的变化

```java
@Test
    void testInsert(){
        User user = new User();
        user.setAge(3);
        user.setName("miao");
        user.setEmail("不告诉你");
		int row=userMapper.insert(user); //受影响的行数
        System.out.println(user);
    }
```

看结果：

![image-20220213144713173](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131447225.png)

> 咦？为什么这个id这么长，难道不应该自增+1么？吓得我赶紧去看了下数据库表果然没有设置自增，那么点击自增然后重新运行看看呢？

![image-20220213144830906](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131448945.png)

可以发现两者自增也是不规律啊！这个其实是这样子的：

> 数据库插入的id的默认值为：全局的唯一id

-----

找到User表的id，添加注解@TableId，可以看到type的默认是`IdType.ID_WORKER`为全局的唯一id

这是相关分布式系统唯一id生成：https://www.cnblogs.com/haoxinyue/p/5208136.html

**雪花算法**

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！

> AUTO自增策略

1. 实体类字段上`@TableId(type = IdType.AUTO)`

2. 数据库字段一定是要自增，否则就会发生异常！

3. 启动重试插入测试：

   ![image-20220213150523789](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131505840.png)

> INPUT手动输入

1. 实体类字段上`@TableId(type = IdType.INPUT)`

2. 意思就是这个id需要我们手动输入

3. 测试：

   ```java
   @Test
   void testInsert(){
       User user = new User();
       user.setAge(3);
       user.setName("miao");
       user.setEmail("不告诉你");
       user.setId(111L);
       int insert = userMapper.insert(user);
       System.out.println(insert);
       System.out.println(user);
   }
   ```

4. 结果：

   ![image-20220213151533706](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131515746.png)

​	注意：如果你这里也没有给id，那么数据库如果有自增那么它就会接着继续自增！

> 其他

```java
public enum IdType {
    AUTO(0),  //数据库id自增
    NONE(1),  //未设置主键
    INPUT(2), //手动输入
    ID_WORKER(3), //默认的全局唯一id
    UUID(4),      //全局唯一的id uuid
    ID_WORKER_STR(5); //ID_WORKER 字符串表示法
}
```

###  4.2 更新及自动填充

更新操作：

```java
@Test
void testUpdate(){
    User user = new User();
    user.setId(111L);
    user.setName("这是我新名字");
    userMapper.updateById(user);
}
```

![image-20220213153318945](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131533984.png)

然后追加一下我再更新一个字段：

`user.setEmail("wwww")`

![image-20220213153736013](https://gitee.com/miawei/pic-go-img/raw/master/imgs/202202131537049.png)

可以发现：这个自动拼接SQL是非常的方便了，按照我们以前的Mybatis来讲就得写if判断，可以说节省了很多的时间了！

> 自动填充









