# JDBC

![image-20210831092047309](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210831092047309.png)

## 1.概念:

**理解**:JDBC就是Java代码与数据库之间建立桥梁,实现两者之间的通信操作

学习JDBC就需要了解一个概念:**持久化**

```
持久化:把数据保存到可掉电式存储设备(断电之后，数据还在，比如硬盘，U盘)中以供之后使用。大多数情况下，特别是企业级应用，数据持久化意味着将内存中的数据保存到硬盘上加以”固化”，而持久化的实现过程大多通过各种关系数据库来完成。(mysql  oracle)
```

那些可以做持久化:

1. 记事本:通过流保存到记事本里面，但是记事本里面删除数据不容易，很少使用
2. XML:把数据存在XML，但是XML查找，删除，修改 也是比较麻烦
3. 数据库:把数据存在数据库，可以直接通过SQL取到,比较容易

结论:持久化的主要应用是**将内存中的数据存储在关系型数据库中**，当然也可以存储在磁盘文件、XML数据文件中，只是比较麻烦。所以在Java代码中要把数据保存到数据库就得通过JDBC去访问数据库

**注意:**在Java中,数据库存取技术`只能`通过JDBC访问数据库;

而访问数据库的形式有两种:

1. 直接使用JDBC的API访问数据库服务器(比如MySQL/Oracle)->这是属于原生操作
2. 间接地使用JDBC的API去访问数据库服务器->比如框架mybatis的底层就是使用的JDBC,它使我们的操作使用更简单

> JDBC是Java访问数据库的基石,其他技术都是对jdbc的封装,并且持久化到数据库的技术,唯一的只有JDBC,其他的实现都是基于JDBC!

### 1.1 JDBC概述

JDBC(JAVA Data Base Connectivity)指java数据库连接;是一种用于执行SQL语句的Java API;

**特点:**

1. 可以为多种关系数据库提供统一访问,它由一组用Java语言编写的类和接口组成,是JAVA访问数据库的**标准规范**;
2. JDBC提供了一种基准,据此可以构建更高级的工具和接口,使数据库开发人员能够快速编写数据库应用程序。
3. JDBC需要连接驱动,驱动是两个设备要进行通信,满足一定通信数据格式,数据格式由设备提供商规定,而设备提供商为设备提供驱动软件.通过软件可以与该设备进行通信;
4. 我们使用的是MySQL的驱动`mysql-connector-java-5.1.39-bin.jar`;
5. JDBC规范定义接口,具体的实现由各大数据库厂商决定
6. JDBC是接口，驱动是接口的实现，没有驱动将无法完成数据库连接，从而不能操作数据库！每个数据库厂商都需要提供自己的驱动，用来连接自己公司的数据库，也就是说驱动一般都由数据库生成厂商提供。

**使用JDBC的好处:**

1. 程序员如果要开发访问数据库的程序,只需要会调用JDBC接口中的方法即可,不用关注类是如何实现的!
2. 使用用一套Java代码,进行少量的修改就可以访问其他JDBC支持的数据库!

图解:

![img](https://pic1.zhimg.com/v2-6e8dff2c548e355cfbae0821d385bb3c_r.jpg)

**结论:**以后我们不管使用哪一种数据库，都需要去导入这个数据库给我们提供的jdbc实现的jar包;

**注意:**这里具体的实现类就是厂商提供的Jar包(驱动包),就是人家写好了具体的实现类然后打包给我们用

> JDBC之所以说是一种标准规范,就好比是Java定义一个接口,而在里面定义有关操作数据库的方法,只不过这里只是一个抽象方法并没有实现具体,然后各大厂商会根据这个抽象方法然后去实现这个接口,所以各大厂商都会为了自己的数据库去实现这个抽象方法,所以我们在写JDBC的时候首先会加载驱动,而这个加载驱动说白了就是加载别人厂商对此Java接口的实现,然后我们就直接调我们Java写的接口中的方法就行了,所以才说JDBC是一个标准规范,对于我们来说不需要去考虑具体实现的类,只考虑如何去调用即可!

## 2.使用JDBC

### 2.1 开发步骤和例子

开发步骤:

1. 注册驱动(要引入驱动jar包)
2. 获得连接
3. 创建执行sql语句的对象
4. 执行sql语句,处理结果
5. 关闭资源

先来例子:

由于之前都是用导jar包的形式,由于我一直采用的是maven的形式,所以我这里就直接贴maven了

```xml
		<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.18</version>
        </dependency>
```

数据库:

![image-20210820174416155](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820174416155.png)

代码实现:

```java
package org.javaweb.jdbcdemo;

import com.mysql.cj.jdbc.Driver;
import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Objects;
import java.util.StringJoiner;

/**
 * @program: YmsdLearn
 * @description: 数据库连接
 * @author: MiaoWei
 * @create: 2021-08-20 16:32
 */
public class SqlConnection {

  @Test
  public void demo() throws Exception {
    // 注册驱动
    DriverManager.registerDriver(new Driver());
    //    Class.forName("com.mysql.cj.jdbc.Driver");
    // 连接配置
    String url =
        "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
    // 账号和密码
    String user = "root";
    String password = "root";
    // 获取连接
    Connection connection = DriverManager.getConnection(url, user, password);
    // 创建执行SQL语句对象
    Statement statement = connection.createStatement();
    // 执行SQL,处理结果
    String sql = "select * from emp";
    ResultSet query = statement.executeQuery(sql);
    StringJoiner joiner;
    while (query.next()) {
      joiner = new StringJoiner(" , ", "[", "]");
      joiner
          .add(query.getObject(1).toString())
          .add(query.getObject(2).toString())
          .add(query.getObject(3).toString())
          .add(query.getObject(4) != null ? query.getObject(4).toString() : "NULL")
          .add(query.getObject(5).toString())
          .add(query.getObject(6).toString())
          .add(query.getObject(7) != null ? query.getObject(8).toString() : "NULL")
          .add(query.getObject(8).toString());
      System.out.println("joiner = " + joiner);
    }
    // 关闭资源
    if (Objects.nonNull(query)) {
      query.close();
    }
    if (Objects.nonNull(statement)) {
      statement.close();
    }
    if (Objects.nonNull(connection)) {
      connection.close();
    }
  }
}
```

**注意:**

 1. 由于我使用的SQL驱动已经是8.0,所以之前的`com.mysql.jdbc.Driver;`已经被废弃了,现在采用`com.mysql.cj.jdbc.Driver;`这种加载驱动,不然就会报异常:

    ![image-20210820175054702](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820175054702.png)

 2. 使用了8.0后的版本在连接数据库配置的时候,就需要指定时区了,不然也会因为时区问题而报错:

    ![image-20210820175232835](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820175232835.png)

3. 在执行查询语句的时候,如果查询字段为空那么就得做处理,不然也会报空指针异常,因为`(query.getObject(7) `返回就是`null`,然后用null去调用toString就会报异常!
4. 驱动版本达到8.0了之后,就不需要显式的指定注册驱动了,也就是`DriverManager.registerDriver(new Driver());`也可以注释了

结果:

![image-20210820175455039](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820175455039.png)

### 2.2 加载驱动-Driver

我们在使用JDBC的时候首要工作就是要加载对应数据库的驱动

DriverManager作用:

1. 管理和注册驱动
2. 创建数据库的连接

创建方式:

1. 创建驱动对象,再手动进行注册

   ```java
   DriverManager.registerDriver(new Driver());
   ```

   使用registerDriver(Driver driver) ;注册驱动，但是我们查看Driver源码类中包含一个静态代码块:

   ![image-20210820180754987](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820180754987.png)

   我们发现这会导致Driver会new两次,所以我们推荐使用反射来进注册驱动

2. 反射读取，自动注册(建议使用)

   ```java
   Class.forName("com.mysql.cj.jdbc.Driver");//8.0之前是com.mysql.jdbc.Driver
   ```

### 2.3 获取连接-connection

接口的实现在数据库驱动中。所有与数据库交互都是基于连接对象的。

获取链接是通过DriverManager类中的静态方法:

```java
1.Connection getConnection (String url, String user, String password)
    通过连接字符串，用户名，密码来得到数据 库的连接对象
2.Connection getConnection (String url, Properties info)
    通过连接字符串，属性对象来得到连接对象
```

栗子:

```mysql
String url =
        "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
    // 账号和密码
    String user = "root";
    String password = "root";
    // 获取连接
    Connection connection = DriverManager.getConnection(url, user, password);
```

其中连接数据库的URL地址格式:

```
协议名:子协议://服务器名或 IP 地址:端口号/数据库名?参数=参数值
如:
jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8
```

参数说明:

```java
1.jdbc:协议;
2.mysql:子协议;
3.localhost:3306:主机:端口;
4.testhome:数据库名;
```

> 连接简写:jdbc:mysql:///数据库名 ,有个前提必须是本地服务器且端口号为3306

Connection接口中方法:

1. createStatement() ;创建执行sql语句对象
2. prepareStatement(String sql) ;创建预编译执行sql语句的对象

### 2.4 获取语句对象-statement

有语句对象才可以操作执行SQL(DDL,DML,DQL);

表示代表一条语句对象，用于发送 SQL 语句给服务器，用于执行静态 SQL 语句并返回它所生成结果的对象。

接口方法:

|                方法                |                             说明                             |
| :--------------------------------: | :----------------------------------------------------------: |
| ResultSet executeQuery(String sql) | 用于发送 DQL 语句，执行查询的操作。select参数：SQL 语句返回值：查询的结果集 |
|   int executeUpdate(String sql)    | 用于发送 DML 语句，增删改的操作，insert、update delete参数：SQL 语句返回值：返回对数据库影响的行数 |
|    boolean execute(String sql)     | 此方法可以执行任意sql语句。返回boolean值，表示是否返回的是ResultSet结果集。仅当执行select语句，且有返回结果时返回true, 其它语句都返回false; |

代码:

```java
Statement st = conn.createStatement();
```

#### 2.4.1 查询结果集-resultSet

封装结果集,查询结果表的对象

- 提供一个游标，默认游 指向结果集第一行之前。
- 调用一次next()，游标向下移动一行。
- 提供一些get方法。

接口常用API:

|               方法                |                       描述                       |
| :-------------------------------: | :----------------------------------------------: |
|         boolean next() ;          |           将光标从当前位置向下移动一行           |
|     int getInt(int colIndex)      |   以int形式获取ResultSet结果集当前行指定列号值   |
|    int getInt(String colLabel)    |   以int形式获取ResultSet结果集当前行指定列名值   |
|   float getFloat(int colIndex)    |  以float形式获取ResultSet结果集当前行指定列号值  |
|  float getFloat(String colLabel)  |  以float形式获取ResultSet结果集当前行指定列名值  |
|  String getString(int colIndex)   | 以String 形式获取ResultSet结果集当前行指定列号值 |
| String getString(String colLabel) | 以String形式获取ResultSet结果集当前行指定列名值  |
|  Date getDate(int columnIndex);   |  以Date 形式获取ResultSet结果集当前行指定列号值  |
| Date getDate(String columnName);  |  以Date形式获取ResultSet结果集当前行指定列名值   |

图解:

![image-20210822153848414](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210822153848414.png)

说明:当我们查询后返回的是一个`ResultSet`类型,而这个里面装的是一个集合,而`ResultSet对象.next()`是指当前指针指的查询结果集的头部,也就是如上图的字段那个位置,如果我们不使用`next`移动指针直接去获取字段,就会获取失败,所以我们需要将指针指向下一块区域,才能获取,如果获取的是最后的区域,已经表示没有了,那么`next`就会返回false

### 2.5释放资源-close

我们使用连接connection都是属于资源操作,所以我们要及时的关闭资源,避免资源浪费

释放资源的对象:

- ResultSet结果集
- Statement 语句
- Connection 连接

释放的原则:

```
先开的后关，后开的先关。ResultSet > Statement > Connection
```

## 3.JDBCUtil

jdbcUtil并不是jdk自带的工具类,而是我们手动创建的一个类,也就是将JDBC中用户名和密码和一些配置进行抽取出来,避免每个地方使用都要去创建,这样就会造成代码冗余和资源的一些浪费

```mysql
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/java0827?useUnicode=true&characterEncoding=utf8
username=root
password=123456
```

放入以下的环境里:

![image-20210820191436661](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210820191436661.png)

代码:

```java
package org.javaweb.jdbcdemo;

import java.io.IOException;
import java.sql.*;
import java.util.Properties;
/** 一个JDBC的工具类 使用静态方法/单例模式(懒汉,饿汉,枚举) */
public class JDBCUtil {
  // 在静态代码块中使用，声明必需是静态的
  private static Properties props = null;

  static {
    // 一.加载驱动只需要执行一次 只需要执行一次的代码写在静态代码块中
    try {
      // 创建一个Properties对象
      props = new Properties();
      // 加载(读取资源文件夹中的jdbc.properties文件)
      props.load(
          Thread.currentThread().getContextClassLoader().getResourceAsStream("jdbc.properties"));
      // 加载驱动
      Class.forName(props.getProperty("driver"));
    } catch (ClassNotFoundException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

  /**
   * 获取连接 二.涟:每次操作数据库都要获取一个新的连接对象 提供一个静态方法
   *
   * @return {@link Connection}
   */
  public static Connection getConnection() {
    try {
      return DriverManager.getConnection(
          props.getProperty("url"), props.getProperty("username"), props.getProperty("password"));
    } catch (SQLException e) {
      e.printStackTrace();
    }
    return null;
  }

  /**
   * 关闭 三.操作完成之后要关闭资源 注意:把要关闭的对象传过来，不能自己创建
   *
   * @param rs rs
   * @param st 圣
   * @param conn 康涅狄格州
   */
  public static void close(ResultSet rs, Statement st, Connection conn) {
    // 每个关闭都在finally中
    // 保证每个需要关闭的资源一定会关闭
    try {
      if (rs != null) {
        rs.close();
      }
    } catch (SQLException e) {
      e.printStackTrace();
    } finally {
      try {
        if (st != null) {
          st.close();
        }
      } catch (SQLException e) {
        e.printStackTrace();
      } finally {
        try {
          if (conn != null) {
            conn.close();
          }
        } catch (SQLException e) {
          e.printStackTrace();
        }
      }
    }
  }
}

```

## 4.SQL注入问题

何为SQL注入:用大白话来讲就是输入的字段在SQL语句拼接中破坏了原有的SQL语句操作

比如:

这是定义SQL语句:

```mysql
select * from user where name='?' and password='?'
```

我们正常规规矩矩输入"张三","123456"拼接在SQL中,这是没什么问题的,可是难免有些有坏心思的人使用其他想法破坏数据库的使用就不太友好了:

比如输入:"张三","123 or 1=1"那么这句话就有问题的:

```mysql
select * from user where='张三' and password='123' or 1=1
```

那么结果就是无论输入什么都是正确的,这就破坏了数据的严谨性! 其中1=1就是true :

```mysql
select * from user where true; //查询了所有记录
```



所以:我们让用户输入的密码和 SQL 语句进行字符串拼接。用户输入的内容作为了SQL语句语法的一部分，改变了原有SQL真正的意义，以上问题称为SQL注入。要解决SQL注入就不能让用户输入的密码和我们的 SQL 语句进行简单的字符串拼接。

### 4.1 SQL注入问题解决：preparedStatement

**概念:**预编译对象， 是Statement对象的子类。

**特点:**

- 性能要高
- 会把sql语句先编译,格式固定好
- sql语句中的参数会发生变化，过滤掉用户输入的关键字。

**结构:**

![image-20210821084231250](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821084231250.png)

PreparedStatement 是 Statement 接口的子接口，继承于父接口中所有的方法。它是一个预编译的 SQL 语句

### 4.2 执行原理

**什么是预编译SQL语句:**

​	预编译语句PreparedStatement是java.sql中的一个接口,它是Statement的子接口。通过Statement对象执行SQL语句时,需要将SQL语句发送给DBMS,由DBMS首先进行编译后再执行。预编译语句和Statement不同,在创建PreparedStatement对象时就指定了SQL语句,该语句立即发送给DBMS进行编译。当该编译语句被执行时，DBMS直接运行编译后的SQL语句，而不需要像其他SQL语句那样首先将其编译在执行

**区别:**

1. PreparedStatemnt的速度比Statement更快
2. PrepareStatement是预处理语句

Api示例:

```java
PreparedStatement pstmt=con.prepareStatement("update employees set salary=? where id=?");
pstmt.setBigDecimal(1,12312);
pstmt.setInt(2,123);
```

**注意:**?上面不要添加引号,不要这样写"?",后面会自动知道该?表示的是什么类型;比如`pstmt.setInt(2,123);`这里可以设置int值,它会自动知道;

图解:

![img](https://pic3.zhimg.com/v2-07054649882dd037e42727bb31f730f2_r.jpg)

**理解:**也就是使用之前的Statement对象抛开SQL注入问题单单就说执行问题,那么它就是每执行一条SQL语句就会编译然后执行,那么我假如我插入一千条一万条它就会编译1万条执行1万条,这样太耗内存资源了,而PrepareStatemenet会将多个SQL语句集中一起编译,然后再执行,也就说1万条Insert语句,只需要编译1万次,而执行就1次,这样极大的提高了开发效率

**功能:**

1. 因为有预先编译的功能，提高 SQL 的执行效率。
2. 可以有效的防止 SQL 注入的问题，安全性更高。

**思考:**我把一条sql发送到数据库之后，数据库会做什么事情?

![image-20210821100235077](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821100235077.png)

### 4.3 使用

直接上代码看:

```javascript
package org.javaweb.jdbcdemo;

import com.mysql.cj.jdbc.Driver;
import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Objects;
import java.util.StringJoiner;

/**
 * @program: YmsdLearn
 * @description: 数据库连接
 * @author: MiaoWei
 * @create: 2021-08-20 16:32
 */
public class SqlConnection1 {

  @Test
  public void demo() throws Exception {
    // 注册驱动
    DriverManager.registerDriver(new Driver());
    //    Class.forName("com.mysql.cj.jdbc.Driver");
    // 连接配置
    String url =
        "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
    // 账号和密码
    String user = "root";
    String password = "root";
    // 获取连接
    Connection connection = DriverManager.getConnection(url, user, password);
    // 执行SQL,处理结果
    String sql = "select * from emp where `DEPTNO`=?";
    // 创建执行SQL语句对象
    PreparedStatement statement = connection.prepareStatement(sql);
    // 传值
    statement.setInt(1, 20);
    // 执行
    ResultSet query = statement.executeQuery();
    StringJoiner joiner;
    while (query.next()) {
      joiner = new StringJoiner(" , ", "[", "]");
      joiner
          .add(String.valueOf(query.getInt("EMPNO")))
          .add(query.getObject(2).toString())
          .add(query.getString("ENAME"))
          .add(query.getString("JOB") != null ? query.getString("JOB") : "NULL")
          .add(query.getString("MGR"))
          .add(query.getDate("HIREDATE").toString())
          .add(query.getString("SAL"))
          .add(query.getString("COMM") != null ? query.getString("COMM") : "NULL")
          .add(String.valueOf(query.getInt("DEPTNO")));
      System.out.println("joiner = " + joiner);
    }
    // 关闭资源
    if (Objects.nonNull(query)) {
      query.close();
    }
    if (Objects.nonNull(statement)) {
      statement.close();
    }
    if (Objects.nonNull(connection)) {
      connection.close();
    }
  }
}
```

结果:

![image-20210821092047887](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821092047887.png)

我这里虽然还使用之前的代码,但是我想意思你也看明白了,这并不难,这是多了SQL语句的占位符罢了!

再来个demo:

```java
String sql = "select *from user where username =? and password =?";
        //创建prepareStatement
        statement = connection.prepareStatement(sql);
        //设置参数
        statement.setString(1, username);
        statement.setString(2, password);
		//执行查询操作
        resultSet = statement.executeQuery();
```

### 4.4 思考

1. 既然写好了SQL语句,而在执行操作的时候为什么还要指定具体的操作呢?

   ```java
   // 执行SQL,处理结果
       String sql = "select * from emp where `DEPTNO`=?";
   // 创建执行SQL语句对象
       PreparedStatement statement = connection.prepareStatement(sql);
   // 传值
       statement.setInt(1, 20);
   //执行更新操作
   int update = statement.executeUpdate();
   ```

   试想一下会发生什么?

   ![image-20210821092633587](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821092633587.png)

   这时就会发生报错,就会提示不能为select做出更新操作,那么我们有的时候我们换个角度思考,我们站在程序的角度上我们并不知道SQL语句里面到底是select还是update,这些编写SQL语句都是程序员而进行操作的,那么我们每次就不能手动去选择`executeUpdate()`和`executeQuery()`操作,所以我们只需换个方法使用`execute`,但是也有缺点:

   ![image-20210821093155954](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821093155954.png)

   判断是否执行成功,返回对应的Boolean值,所以后阶段一些框架mybatis也对此做了一些封装和修改

2. 既然说PrepareStatement是防注入,那具体在哪?怎么防注入的?

   我们来做一个实验(代码依然是上面贴的代码,这里只是展示其中某个部分):

   先来组正常的:

   ```java
    	// 执行SQL,处理结果
       String sql = "select * from emp where `ENAME`=?";
       // 创建执行SQL语句对象
       PreparedStatement statement = connection.prepareStatement(sql);
       // 传值
       statement.setString(1, "SMITH");
   ```

   结果:

   ![image-20210821093646082](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821093646082.png)

   修改下:

   ```java
   // 执行SQL,处理结果
       String sql = "select * from emp where `ENAME`=?";
       // 创建执行SQL语句对象
       PreparedStatement statement = connection.prepareStatement(sql);
       // 传值
       statement.setString(1, "'SMITH' or ENAME='JONES'");
   ```

   看断点:

   ![image-20210821094007466](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821094007466.png)

   很明显失败了因为它这个针对占位符使用了""引号括起来的,我们对应这个SQL再进行改变一下(结果就不贴图了因为什么都没有,因为失败了肯定没查出啊):

   ```mysql
   statement.setString(1, "SMITH or ENAME=JONES");
   ```

   ![image-20210821094433320](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821094433320.png)

   .....

   我已经连续试了好几遍了,都是无情的打回来了,所以我的结论是:真的可以防止SQL注入

### 4.5 注意事项

总结一下注意事项看看有哪些?

1. 问号（？）--相当于占位符（SQL里面的问题），我们使用的时候有没有区分类型?--没有-字符串也不需要特殊加上引号

2. 在拿到语句对象的时候就使用SQL

   ```java
   Statement st = conn.createStatement();
   PreparedStatement st = conn.prepareStatement(sql)；//预编译sql
   ```

3. 在执行的时候不需要传sql；

   ```java
   PreparedStatement statement = connection.prepareStatement(sql);
   // 执行
   ResultSet query = statement.executeQuery();
   ```

4. 所有占位符，需要给它添加相应的值

   ```java
    PreparedStatement对象.setXxxx(位置(从1开始)，相应的值)
   ```

5. ？ 只能放在特定的地方（不能替换关键字表名）--不能放在 select * from ?

PreparedStatement容易出错的地方:

- 占位符字符串千万不要加上引号
- 加上相应的值的时候，位置，类型，个数必须要完全一致;
- 执行的时候，不要SQL放进来;
  - 比如:`st.executeUpdate()`执行的是编译之后的语句，如果你把sql放进去，是编译前的sql，里面就会出现问题;

### 4.6 总结

使用PreparedStatement的好处:

1. prepareStatement()会先将 SQL 语句发送给数据库预编译。PreparedStatement 会引用着预编译后的结果。
2. 可以多次传入不同的参数给 PreparedStatement 对象并执行。减少 SQL 编译次数，提高效率。
3. 安全性更高，没有 SQL 注入的隐患。
4. 提高了程序的可读性
5. 速度更快,效率更高

### 4.7 Statement 和 PreparedStatement的区别?

1. PreparedStatement继承自Statement,对其功能的扩展和增强
2. PreparedStatement更加安全,因为它能防止SQL注入,而Statement则不能
3. 性能方面PreparedStatement执行效率更高,
   - PreparedStatement则会将SQL解析后编译成二进制就放进**预编译区**,等待最后所有的SQL解析完全放在编译区再一并发送到SQL执行
   - Statement则直接编译完二进制后直接去执行了,有多少条SQL就会执行多少次

## 5. 事务

**概念:**程序中一组操作,要么都成功,要么都失败(同生共死)

**理解:**事务说白了就是保持同步性,比如一个经典案例:银行转账或者支付宝转账,假如用户A和用户B转账,转账500,在开始转的过程中,因为服务器的一些特殊原因,突然崩了,这时用户A确实已经转过去了,而用户B也收到了,但是用户A的账户余额还没减去500,而用户B的账户却增加了500,这就造成了银行经济上的损失!

**事务的操作:**先定义开始一个事务,然后对数据作修改操作,这时如果提交(commit),这些修改就永久地保存下来,如果回退(rollback),数据库管理系统将放弃您所作的所有修改而回到开始事务时的状态。

为确保数据库中数据的一致性,数据的操纵应当是离散的成组的逻辑单元;当每个逻辑操作单元全部完成时,数据的一致性可以保持,而当这个单元中的一部分操作失败,整个事务应全部视为错误,所有从起始点以后的操作应全部回退到开始状态。 

### 5.1 四大特性(ACID)

1. 原子性(Atomicity):原子性是指事务是一个不可分割的工作单位,事务中的操作要么都发生,要么都不发生
2. 一致性(Consistency):事务前后数据的完整性必须保持一致.
3. 隔离性(Isolation):事务的隔离性是多个用户并发访问数据库时,数据库为每一个用户开启的事务,不能被其他事务的操作数据而干扰,多个并发事务之间要相互隔离
4. 持久性(Durability):持久性是指一个事务一旦被提交,它对数据库中数据的改变就是永久性,接下来即使数据库发生故障也不应该对其有任何影响

### 5.2 事务操作

**事务处理:**保证所有事务都作为一个工作单元来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都被提交(commit)，要么整个事务回滚(rollback)到最初状态

**处理事务的两个动作:**

1. 提交:commit:
   - 当整个事务中,所有的逻辑单元都正常执行成功.  ---->提交事务.---数据已经提交,不能更改.
2. 回滚:rollback:
   - 当整个事务中,有一个逻辑单元执行失败,     ---->回滚事务.---撤销该事务中的所有操作--->恢复到最初的状态.

API介绍:

|                  方法                  |                             描述                             |
| :------------------------------------: | :----------------------------------------------------------: |
| void setAutoCommit(boolean autoCommit) | 参数是 true 或 false(如果设置为 false，表示关闭自动提交，相当于开启事务) |
|             void commit()              |                           提交事务                           |
|            void rollback()             |                           回滚事务                           |

示例:

先看数据库:

![image-20210821104612984](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821104612984.png)

上代码:

```java
package org.javaweb.jdbcdemo;

import com.mysql.cj.jdbc.Driver;
import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Objects;

/**
 * @program: YmsdLearn
 * @description: 数据库连接
 * @author: MiaoWei
 * @create: 2021-08-20 16:32
 */
public class SqlConnection2 {

  @Test
  public void demo() throws Exception {
    // 注册驱动
    DriverManager.registerDriver(new Driver());
    //    Class.forName("com.mysql.cj.jdbc.Driver");
    // 连接配置
    String url =
        "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
    // 账号和密码
    String user = "root";
    String password = "root";
    // 获取连接
    Connection connection = DriverManager.getConnection(url, user, password);
    // 开启事务
    connection.setAutoCommit(false);

    PreparedStatement statement = null;
    try {
      System.out.println("======================================================");
      // 执行SQL,给SMITH减去200
      String sql = "update emp set SAL=SAL-? where ENAME=?";
      // 创建执行SQL语句对象
      statement = connection.prepareStatement(sql);
      // 传值
      statement.setInt(1, 200);
      statement.setString(2, "SMITH");
      // 执行
      statement.executeUpdate();

//      // 制造出现异常
//      int i = 0;
//      if (i == 0) {
//        throw new RuntimeException("出现不可描述的异常!");
//      }

      System.out.println("======================================================");
      //给ALLEN增加200
      String sql1 = "update emp set SAL=SAL + ? where ENAME=?";
      // 创建执行SQL语句对象
      PreparedStatement statement1 = connection.prepareStatement(sql1);
      // 传值
      statement1.setInt(1, 200);
      statement1.setString(2, "ALLEN");
      // 执行
      statement1.executeUpdate();

      int i = 0;
      if (i == 0) {
        throw new RuntimeException("出现不可描述的异常!");
      }

      // 提交事务
      connection.commit();
      System.out.println("双方操作成功!");
    } catch (Exception throwables) {
      // 遇到异常就回滚
      connection.rollback();
      System.out.println("操作失败!");
      throwables.printStackTrace();
    } finally {
      // 关闭资源
      if (Objects.nonNull(statement)) {
        statement.close();
      }
      if (Objects.nonNull(connection)) {
        connection.close();
      }
    }
  }
}
```

可以看出我在commit之前我就抛出异常了,然后就捕获对其回滚

看结果:

![image-20210821104748046](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821104748046.png)

看数据库的变化:

![image-20210821104807555](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821104807555.png)

好了你学废了吗?

### 5.3 注意事项

总的来说开发步骤:

1. 获取连接
2. 开启事务
3. 获取到 PreparedStatement
4. 使用 PreparedStatement 执行两次更新操作
5. 正常情况下提交事务
6. 出现异常回滚事务
7. 最后关闭资源

在JDBC中事务是默认提交的,就是执行一条SQL语句那么就提交事务了,所以我们在执行SQL之前就得去设置事务为手动提交

```java
connection对象.setAutoCommit(false);//设置事务为手动提交.
```

然后是手动提交事务

```java
connection对象.commit();
```

若出现异常必须回滚事务:

```java
connection对象.rollback();
```

注意:回滚事务,总余额依然是正确的. 若不回滚事务,不会释放数据库资源.

> 总的来说一般使用事务,都对其try-catch-finally,只要产生异常那么就捕获异常然后回滚

**注意:**

1. MyISAM:不支持事务
2. InnoDB:支持外键,支持事务

## 6.获取主键

通常我们在做开发的时候,是要同时操作多张表的插入,比如我现在新增一个用户"张三",然后我现在对User表进行插入,插入成功后我现在又要去另一张表account账户表,既然是账户表肯定需要user表的id来作为关联关系,那么此时我必须就要获取到这个新插入的id主键,并且这之间还得处于事务的开启中,得同时成功同时失败!

这里涉及一下:

依然使用`exceuteUpdate()`来进行设置

![image-20210821112155626](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112155626.png)

这就是参数值

![image-20210821112248334](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112248334.png)

而获取则通过:

![image-20210821112337158](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112337158.png)

你肯定还有点小疑惑,那么我们直接贴代码直接看或许就知道怎么回事了!

```java
package org.javaweb.jdbcdemo;

import com.mysql.cj.jdbc.Driver;
import org.junit.Test;

import java.sql.*;
import java.util.Objects;

/**
 * @program: YmsdLearn
 * @description: 数据库连接
 * @author: MiaoWei
 * @create: 2021-08-20 16:32
 */
public class SqlConnection3 {

  @Test
  public void demo() throws Exception {
    // 注册驱动
    DriverManager.registerDriver(new Driver());
    // 连接配置
    String url =
        "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
    // 账号和密码
    String user = "root";
    String password = "root";
    // 获取连接
    Connection connection = DriverManager.getConnection(url, user, password);
    // 执行insert插入SQL
    String sql = "insert into tb1(`name`,`email`,`num`,`no_index`) values (?,?,?,?)";
    // 创建执行SQL语句对象,加入参数表示可获取主键
    PreparedStatement statement = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
    // 传值
    statement.setString(1, "MiaoDa威");
    statement.setString(2, "110@qq.com");
    statement.setInt(3, 6);
    statement.setInt(4, 7);
    // 执行
    statement.executeUpdate();

    // 拿到插入成功后的主键值
    ResultSet keys = statement.getGeneratedKeys();
    while (keys.next()) {
      System.out.println("keys = " + keys.getInt(1));
    }

    // 关闭资源
    if (Objects.nonNull(statement)) {
      statement.close();
    }
    if (Objects.nonNull(connection)) {
      connection.close();
    }
  }
}
```

这是之前的数据库:

![image-20210821112553828](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112553828.png)

然后这是执行之后的数据库和控制台打印的主键:

![image-20210821112640492](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112640492.png)

![image-20210821112650119](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821112650119.png)

对吧没那么难,所以我总结出:

首先在获取SQL执行对象的时候设置可获取主键:

```java
PreparedStatement statement = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
```

然后执行SQL语句然后获取主键

```java
// 执行
statement.executeUpdate();
// 拿到插入成功后的主键值
ResultSet keys = statement.getGeneratedKeys();
    while (keys.next()) {
      System.out.println("keys = " + keys.getInt(1));
 }
```

**注意:**这里获取主键返回的是一个`ResultSet`对象,这里依然要进行`keys.next()`移动指针,然后这里必须得获取列号,不能通过字段来获取,因为这里并没有写select nid from表名，所有不能通过rs.geeLong（"nid"）来获取,否则就会报异常:

![image-20210821113214561](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821113214561.png)

**注意:**这里只展示PrepareStatement用法,而Statement也是一样的获取,只不过Statement则把设置是否能获取主键放在了executeUpdate里面,而PrepareStatement则是在获取SQL执行对象的时候就设置,然后获取的两者都是一样的!

```java
Statement: 在这里
	statement.executeUpdate(sql,Statement.RETURN_GENERATED_KEYS);//执行SQL语句的时候设置
PrepareStatement:
	PreparedStatement statement = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);//创建SQL执行对象的时候设置
```

> 我个人认为这里获取主键还是挺麻烦的还要移动指针啥的,不像mybatis直接通过实体类的字段直接获取,较为方便;

## 7. 数据库连接池

### 7.1 概念和引入

**概念**顾名思义,就是装连接对象的容器，可以极大的提升咱们操作数据库的性能

**理解:**连接池就是将在应用程序和数据库之间开辟一块区域,这块区域正如名字一样专门放连接用的,而这个"连接"就是指连接数据库的资源,因为连接数据库是一个非常耗费内存资源的操作,假如这是在淘宝,淘宝有1千万个用户同时在线,那么每个人都会去创建连接然后跟数据库,如果有些人出现故障导致没有及时关闭,那么是不是也就意味着资源泄漏,所以出现了"连接池"来控制,而这个连接池就是用来装这个连接对象的容器!

一般来说,Java应用程序访问数据库的过程是:

1. 装载数据库驱动程序
2. 通过JDBC建立数据库连接
3. 访问数据库,执行SQL语句
4. 断开数据库连接

如:

```java
Public void FindAllUsers(){
              //1、装载sqlserver驱动对象
              DriverManager.registerDriver(new SQLServerDriver());             
              //2、通过JDBC建立数据库连接
              Connection con =DriverManager.getConnection("jdbc:sqlserver://192.168.2.6:1433;DatabaseName=customer", "sa", "123");            
              //3、创建状态
              Statement state =con.createStatement();           
              //4、查询数据库并返回结果
              ResultSet result =state.executeQuery("select * from users");           
              //5、输出查询结果
              while(result.next()){
                     System.out.println(result.getString("email"));
              }            
              //6、断开数据库连接
              result.close();
              state.close();
              con.close();
        }
```

分析:

​	程序开发过程中，存在很多问题：首先，每一次web请求都要建立一次数据库连接。建立连接是一个费时的活动，每次都得花费0.05s～1s的时间，而且系统还要分配内存资源。这时就在想为什么不能用同一个连接呢?反正大家都要连接?对吧!但是是不行的!因为使用同一个连接必然会造成线程安全问题!比如说线程A正准备新增一条记录,线程B反手就是把连接给关了,那线程A插一半就插不了了,这就在想谁TM给我关了!再比如说在网站中登录验证账号和密码,这也需要去数据库去比对,那么这个连接数据库的操作必然也会花费时间,这个时间对于一次或几次数据库操作，或许感觉不出系统有多大的开销。可是对于现在的web应用，尤其是大型电子商务网站，同时有几百人甚至几千人在线是很正常的事。

​	在这种情况下，频繁的进行数据库连接操作势必占用很多的系统资源，网站的响应速度必定下降，严重的甚至会造成服务器的崩溃。不是危言耸听，这就是制约某些电子商务网站发展的技术瓶颈问题。其次，对于每一次数据库连接，使用完后都得断开,否则，如果程序出现异常而未能关闭，将会导致数据库系统中的内存泄漏，最终将不得不重启数据库。还有，这种开发不能控制被创建的连接对象数，系统资源会被毫无顾及的分配出去，如连接过多，也可能导致内存泄漏，服务器崩溃。

假如现在就有1000个人访问:

![image-20210821163253478](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821163253478.png)

这里面就会造成不断的连接和断开,然后我们都知道“数据库连接”是一种稀缺的资源，为了保障网站的正常使用，应该对其进行妥善管理。其实我们查询完数据库后，如果不关闭连接，而是暂时存放起来，当别人使用时，把这个连接给他们使用。就避免了一次建立数据库连接和断开的操作时间消耗。，所以现在的线程池就出来了：

![image-20210821163634972](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821163634972.png)

**数据库连接池的基本思想：**给数据库建立一个“缓冲池”。就是说预先在缓冲池中放入一定数量的连接，当需要建立数据库连接时，只需要从“缓冲池”中取出一个，使用完毕之后再放回去。从而避免了频繁的向数据库申请资源，释放资源带来的性能损耗。我们可以通过设定连接池最大连接数来防止系统无尽的与数据库连接。更重要的是我们可以通过连接池的管理机制监视数据库的连接的数量、使用情况，为系统开发、测试及性能调整提供依据。

![image-20210821164313928](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821164313928.png)

再来一张连接池的概述图:

![image-20210823155030543](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210823155030543.png)

我们了解了连接池的基本思想那我们思考几个问题(并且我采用火车站的问题来回答):

1. 连接池一出来就有连接嘛？

   ```html
   有,需要提前设置好连接数,有的人用了会还回来，我下一个人在用就行了;就跟火车站窗口一样不管有没有人，都需要留一个两个窗口，--有个初始窗口,比如说初始容量为5
   ```

2. 如果我拿10个连接，你只有5个，怎么办？

    ```
    需要增加连接数,不过也不能乱加,当人数连接比较多的时候就设置最大连接数量,比如说10 
    ```

3. 连接池如果放过多的连接，浪费，并且占用资源，对不对？

    ```
    所以一般都会有最小连接容量,比如说2
    ```

4. 如果5个人一直连接没有关闭，怎么办？

    ```
    在火车站如果一个人一直占着窗口不放,那么保安就会强制请他出去,所以这里会设置一个最大连接时间
    ```

5. 如果已经达到最大连接数,后面还有人在排队怎么办?

    ```
    如果等待过长，断开连接，请求超时;
    ```

### 7.2 手写迷你数据库连接池

数据库连接池:

1. 数据库连接池中存放的就是数据库操作管道,不仅仅是存放,而且应该是管理这些管道;
2. 应该提供外部配置文件去初始化数据库连接池;
3. 如果一个数据库操作管道已经被占用,那么其他请求是否应该得到这个管道,也就是说我们要考虑多线程并发下,管道的分配问题;
4. 如果做到管道的复用?放回池子中,标示可用,并不是真正的去关闭管道;

先来一个总的工程目录:

![image-20210821170827800](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821170827800.png)

- DBConfigXML:是一个接口,用于存放配置文件信息,只是这里图方便采用常量的形式来进行配置
- demo:程序执行的入口
- IMyPool:是一个接口，对外提供数据库连接池的基本服务，比如得到一个数据库操作管道。
- MyDefaultPool:是IMyPool的实现,也是对连接池的一些操作和初始化
- MyPooledConnection:数据库连接管道,就是执行SQL和关闭管道
- MyPoolFactory:运用单例模式来获取到MyDefaultPool的对象来进行调用;

DBConfigXML:

```java
package org.javaweb.jdbcSource;

/**
 * @program: YmsdLearn
 * @description: 数据库连接配置-为了简化本应放在资源文件夹下来进行统一配置
 * @author: MiaoWei
 * @create: 2021-08-21 14:29
 */
public interface DBConfigXML {
  // 连接信息
  String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
  String JDBC_URL =
      "jdbc:mysql://localhost:3306/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false";
  String JDBC_USER_NAME = "root";
  String JDBC_PASSWORD = "root";

  // 数据库连接池初始化大小
  int INIT_COUNT = 10;
  // 连接池不足的时候增长的步进值
  int STEP = 2;
  // 连接池的最大数量
  int MAX_COUNT = 50;
}
```

MyPooledConnection:这个类主要用于建立和数据库之间的连接,然后执行SQL的查询操作,然后是关闭通道

```java
package org.javaweb.jdbcSource;

import lombok.Data;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * @program: YmsdLearn
 * @description: 数据库连接管道-就是对JDBC Connection进行封装而已
 * @author: MiaoWei
 * @create: 2021-08-21 14:36
 */
@Data
public class MyPooledConnection {
  private Connection connection;
  private boolean isBusy;

  public MyPooledConnection(Connection connection, boolean isBusy) {
    this.connection = connection;
    this.isBusy = isBusy;
  }

  /** 关闭-注意isBusy的这个标示。对管道的关闭，实际上只是标示的改变而已！ */
  public void close() {
    this.isBusy = false;
  }

  /**
   * 查询-操作
   *
   * @param sql sql
   * @return {@link ResultSet}
   */
  public ResultSet query(String sql) {
    Statement statement;
    ResultSet resultSet = null;
    try {
      statement = connection.createStatement();
      resultSet = statement.executeQuery(sql);
    } catch (SQLException throwables) {
      throwables.printStackTrace();
    }
    return resultSet;
  }
}
```

IMyPool:

```java
package org.javaweb.jdbcSource;

/**
 * 对连接池的一个基本管理API接口 要可以得到数据库操作的管道/要可以创建数据库管道
 *
 * @author MiaoDaWei
 * @date 2021/08/21
 */
public interface IMyPool {
  /**
   * 获取连接池连接
   *
   * @return {@link MyPooledConnection}
   */
  MyPooledConnection getMyPooledConnection();

  /**
   * 连接数据库连接池管道连接大小
   *
   * @param count 数
   */
  void createMyPooledConnection(int count);
}
```

MyDefaultPool:

```java
package org.javaweb.jdbcSource;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.concurrent.CopyOnWriteArrayList;

/**
 * @program: YmsdLearn
 * @description: 连接池->加载外部配置进行初始化->加载数据库驱动->考虑采用什么集合对连接管道进行储存
 * @author: MiaoWei
 * @create: 2021-08-21 14:51
 */
public class MyDefaultPool implements IMyPool {
  private static String jdbcUrl;
  private static String jdbcUserName;
  private static String jdbcPassword;
  private static int initCount;
  private static int step;
  private static int maxCount;

  // 我这里为了线程安全采用CopyOnWriteArrayList
  private final CopyOnWriteArrayList<MyPooledConnection> myPooledConnectionsList =
      new CopyOnWriteArrayList<>();

  public MyDefaultPool() {
    // 初始化数据库连接池配置
    init();
    // 加载驱动-我个人认为应放在static静态代码块里,只需要加载一次
    try {
      Class.forName(DBConfigXML.JDBC_DRIVER);
    } catch (ClassNotFoundException e) {
      e.printStackTrace();
    }
    // 初始化连接数据库连接池管道
    createMyPooledConnection(initCount);
  }
  /** 初始化 */
  private void init() {
    jdbcUrl = DBConfigXML.JDBC_URL;
    jdbcUserName = DBConfigXML.JDBC_USER_NAME;
    jdbcPassword = DBConfigXML.JDBC_PASSWORD;
    initCount = DBConfigXML.INIT_COUNT;
    step = DBConfigXML.STEP;
    maxCount = DBConfigXML.MAX_COUNT;
  }

  /**
   * 获取我的连接池连接
   * 解读:
   * 1.简单的数量判断
   * 2.从连接管道的集合中获取连接管道
   * 3.如果为空那么就表明集合已经没有了,所以就创建新的连接池添加到连接池集合中
   * 4.返回一个连接管道
   *
   * @return {@link MyPooledConnection}
   */
  @Override
  public MyPooledConnection getMyPooledConnection() {
    if (myPooledConnectionsList.size() < 1) {
      throw new RuntimeException("连接池初始化错误!");
    }
    MyPooledConnection myPooledConnection = null;
    try {
      myPooledConnection = getRealConnectionFromPool();
      while (myPooledConnection == null) {
        createMyPooledConnection(step);
        myPooledConnection = getRealConnectionFromPool();
        return myPooledConnection;
      }

    } catch (SQLException e) {
      e.printStackTrace();
    }

    return myPooledConnection;
  }

  /**
   * 解读:这个接口主要是创建连接池的大小
   * 1.首先会对配置文件中的最大连接容量进行判断
   * 2.循环获取新的连接然后创建新的连接管道
   * 3.将新的连接管道添加进集合中
   *
   * @param count 数量
   */
  @Override
  public void createMyPooledConnection(int count) {
    if (myPooledConnectionsList.size() > maxCount
        || myPooledConnectionsList.size() + count > maxCount) {
      throw new RuntimeException("连接池已经满了!");
    }
    for (int i = 0; i < count; i++) {
      try {
        Connection connection = DriverManager.getConnection(jdbcUrl, jdbcUserName, jdbcPassword);
        MyPooledConnection pooledConnection = new MyPooledConnection(connection, false);
        myPooledConnectionsList.add(pooledConnection);
      } catch (SQLException e) {
        e.printStackTrace();
      }
    }
  }

  /**
   * 返回连接管道-这里使用synchronized避免多线程环境下产生问题
   * 1.遍历整个连接管道的集合
   * 2.isBusy:true:代表关闭;false:开启
   * 3.将开启了的连接管道在时间范围内进行判断是否有效并未关闭
   * 4.那么设置连接管道有效标志并且就结束并返回整个连接通道
   * 5.如果不满足条件则创建新的管道并返回
   * 6.这里其实就是随机获取,只不过这个随机是从上往下去查关闭了的连接管道
   *
   * @return {@link MyPooledConnection}
   * @throws SQLException sqlexception异常
   */
  private synchronized MyPooledConnection getRealConnectionFromPool() throws SQLException {
    for (MyPooledConnection myPooledConnectionItem : myPooledConnectionsList) {
      if (!myPooledConnectionItem.isBusy()) {
        //isValid:如果连接尚未关闭并且仍然有效，则返回true。等待数据库操作用于验证连接完成的时间（以秒为单位）。如果超时时间在操作完成之前到期，则此方法返回false。
        if (myPooledConnectionItem.getConnection().isValid(3000)) {
          myPooledConnectionItem.setBusy(true);
          return myPooledConnectionItem;
        } else {
          Connection connection = DriverManager.getConnection(jdbcUrl, jdbcUserName, jdbcPassword);
          myPooledConnectionItem.setConnection(connection);
          myPooledConnectionItem.setBusy(true);
          return myPooledConnectionItem;
        }
      }
    }
    return null;
  }
}
```

MyPoolFactory:

```java
package org.javaweb.jdbcSource;

/**
 * @program: YmsdLearn
 * @description: 数据库线程池工厂
 * @author: MiaoWei
 * @create: 2021-08-21 15:17
 */
public class MyPoolFactory {

  public static IMyPool getInstance() {
    return CreatePool.myPool;
  }

  private static class CreatePool {
    public static IMyPool myPool = new MyDefaultPool();
  }
}
```

demo:

```java
package org.javaweb.jdbcSource;

import org.junit.Test;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.StringJoiner;

/**
 * @program: YmsdLearn
 * @description: 测试
 * @author: MiaoWei
 * @create: 2021-08-21 15:20
 */
public class demo {
  private static final IMyPool MY_POOL = MyPoolFactory.getInstance();

  /**
   * 解读:
   * 1.获取到唯一的实例后首先会从数据库连接池集合中随机找到已经关闭了的连接管道
   * 2.开启连接管道
   * 3.拿到连接管道后就开始查询SQL操作
   * 4.关闭管道
   *
   */@Test
  public void test() {
    for (int i = 0; i < 1000; i++) {
      MyPooledConnection connection = MY_POOL.getMyPooledConnection();
      ResultSet resultSet = connection.query("select * from tb1 where nid=10");
      try {
        StringJoiner joiner;
        while (resultSet.next()) {
          joiner = new StringJoiner(", ");
          joiner
              .add("name: " + resultSet.getString("name"))
              .add("email " + resultSet.getString("email"))
              .add("使用管道:" + connection.getConnection());
          System.out.println("joiner = " + joiner);
        }
      } catch (SQLException e) {
        e.printStackTrace();
      } finally {
        connection.close();
      }
    }
  }
}
```

结果:

![image-20210821181311292](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821181311292.png)

结论:这里我循环1000次管道都是一样的,是因为我每使用一个连接管道后我及时的设置关闭的标识,然后下一次管道就会从已经关闭了的管道中去获取然后进行开启,然后我将连接池用集合来进行存储,可以实现同样的功能!

> 在写的过程进行思考,就能理解数据库连接池的原理及其过程,这让我们对数据库连接池的基本思想是有益处的!

### 7.3 连接池的使用

**概述:**在JAVA中,连接池使用`java.sql.DataSource`接口来表示连接池,而这里的DataSource就是连接池。

DataSource是接口,和JDBC是一样的,是Sum公司开发的一套接口,需要各大厂商实现;所以需要导入相应的包;

常用的DataSource的实现有下面多种方式:

1. DBCP:DBCP更新速度很慢，基本处于不活跃状态;是tomcat自带的连接池,扩展性不强
2. C3P0:Hibernate推荐的(早期)（Hibernate框架已经集成C3P0）,好久不在维护了,比较老的连接池;历史悠久，代码及其复杂，不利于维护
3. boneCP:淘汰了
4. Hikari:非常牛批的连接池,因为高效,简洁,追求性能,更新维护快,扩展性一般
5. Druid:阿里的,综合性能方便都比较好,性能优秀,扩展性好,自带拦截器实现

再来个综合比较图：

![综合比较](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20200214155722813.png)

我这里直接贴最后一个Druid的maven:

```xml
		<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.8</version>
        </dependency>
```

直接上代码:

```java
package org.javaweb.jdbctest.jdbcDurid;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidPooledConnection;
import org.junit.Test;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * @program: YmsdLearn
 * @description: 连接池实现
 * @author: MiaoWei
 * @create: 2021-08-21 19:07
 */
public class DuridDemo {
  @Test
  public void test() throws SQLException {
    // 数据源配置
    DruidDataSource dataSource = new DruidDataSource();
    dataSource.setUrl(
        "jdbc:mysql://localhost/testhome?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&serverTimezone=Asia/Shanghai&useSSL=false");
    dataSource.setUsername("root");
    dataSource.setPassword("root");

    // 可选配置
    // 初始连接数
    dataSource.setInitialSize(10);
    // 最大连接数
    dataSource.setMaxActive(30);
    // 最小闲置数
    dataSource.setMinIdle(10);
    // 获取连接的最大等待时间,单位毫秒
    dataSource.setMaxWait(2000);
    // 缓存PreparedStatement,默认false
    dataSource.setPoolPreparedStatements(true);
    // 缓存PreparedStatement的最大数量,默认为-1(不缓存),大于0时会自动开启缓存PreparedStatement,所以可以省略上一局代码
    dataSource.setMaxOpenPreparedStatements(20);

    // 获取连接
    DruidPooledConnection connection = dataSource.getConnection();

    // 使用Statement接口
    Statement statement = connection.createStatement();
    String sql = "insert into tb1 (name,email,num,no_index) values ('李二狗','12312312@qq.com',7,8)";
    int executeUpdate = statement.executeUpdate(sql, Statement.RETURN_GENERATED_KEYS);
    if (executeUpdate > 0) {
      System.out.println("Statement插入成功");
      ResultSet resultSet = statement.getGeneratedKeys();
      while (resultSet.next()) {
        System.out.println("Statement插入成功后的主键为:" + resultSet.getString(1));
      }
    } else {
      System.out.println("Statement插入失败");
    }

    // 使用PreparedStatement接口
    String sql2 = "insert into tb1 (name,email,num,no_index) values ('李二狗','12312312@qq.com',8,9)";
    PreparedStatement preparedStatement =
        connection.prepareStatement(sql2, Statement.RETURN_GENERATED_KEYS);
    int update = preparedStatement.executeUpdate();
    if (update > 0) {
      System.out.println("PreparedStatement插入成功");
      ResultSet resultSet = preparedStatement.getGeneratedKeys();
      while (resultSet.next()) {
        System.out.println("PreparedStatement插入成功后的主键为:" + resultSet.getString(1));
      }
    } else {
      System.out.println("PreparedStatement插入失败");
    }

    // 关闭连接
    connection.close();
  }
}
```

看结果:

![image-20210821194500892](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210821194500892.png)

> 这里我就不贴数据库变化了,因为这里控制台已经给出答案了!

**注意**:我这里直接使用数据源,因为我图方便,所以最好将这些数据源统一放在资源文件夹下的文件里统一保管,然后使用Properties进行获取set到DruidDataSource里面去

我举个例子吧(这是伪代码demo):

```java
Properties properties=new Properties();
InputStream is = 类名.class.getResourceAsStream(/文件路径/文件名);
properties.load(is);
DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);//这种方式对properties的key有严格要求，必须值指定的字符串，容易写错。
```



### 7.5连接池的优势

使用连接池和不使用连接池的区别在哪里?

1. 不使用连接池:

   ```java
   Connection对象由DriverManager获取;
   如:
   Connection conn=DriverManager.getConnection(url,username,password);
   ```

2. 使用连接池:

   只有一个不同点,现在的连接需要从DataSource里面拿,DataSource需要从数据库拿,所以我们需要把用户名和密码给连接池,连接池帮我们从数据库中拿连接,而最后使用连接池的时候,需要释放资源:`Connection对象.close()`;

   **注意:**是把Connection放回给连接池,而不是和数据库断开,只是连接放回到连接池中,

这里直接说应用程序和数据交互:

1. 首先通过TCP协议的三次握手和数据库服务器建立连接,然后发送数据库用户账号和密码,等待数据库验证用户身份
2. 提交SQL语句到数据库执行(可能还有SQL的预编译)
3. 连接关闭,关闭连接需要和数据库通信,通知断开连接了然后再TCP四次挥手最后完成关闭

> 连接池技术的思想采用的是空间还时间的概念;预先完成多个数据库连接对象,在执行大多数sql时,可以省略第一步和第三步,有效提升SQL效率

### 7.6 思考

用连接池可不是直接用那么简单,还要考虑更多的问题

1. 并发问题

   为了便连接管理服务具有更大的通用性,必须考虑多线程环境,即并发问题。这个问题相对比较好解决，因为Java语言自身提供了对并发管理的支持，使用`synchronized`关键字即可确保线程是同步,使用方法为直接在类方法前面加上`synchronized`关键字,如:

   ```java
   public synchronized connection getconnection ()
   ```

2. 多数据库服务器和多用户

   对于大型的企业级应用，常常需要同时连接不同的数据库（如连接oracle和sybase）。如何连接不同的数据库呢？

   ```
   	我们采用的策略是：设计一个符合单例模式的连接池管理类，在连接池管理类的唯一实例被创建时读取一个资源文件，其中资源文件中存放着多个数据库的url地址等信息。根据资源文件提供的信息，创建多个连接池类的实例，每一个实例都是一个特定数据库的连接池。连接池管理类实例为每个连接池实例取一个名字，通过不同的名字来管理不同的连接池。对于同一个数据库有多个用户使用不同的名称和密码访问的情况，也可以通过资源文件处理，即在资源文件中设置多个具有相同url地址，但具有不同用户名和密码的数据库连接信息。
   ```

3. 事务处理

   ```
   我们都知道在数据库事务中,具有原子性也就是"同生共死"。在Java语言中，`connection`类本身提供了对事务的支持,可以通过设置connection的autoCommit属性为false,然后显示的调用commit或rollback方法来实现。但要高效的进行connection复用,就必须提供相应的事务支持机制,可采用每一个事务独占一个连接来实现,这种方法可以大大降低事务管理的复杂性。
   ```

4. 连接池的分配与释放

   ```
   连接池的分配与释放，对系统的性能有很大的影响。合理的分配与释放，可以提高连接的复用度，从而降低建立新连接的开销，同时还可以加快用户的访问速度。
   	对于连接的管理可使用空闲池。即把已经创建但尚未分配出去的连接按创建时间存放到一个空闲池中，每当用户请求一个连接时，系统首先检查空闲池内有没有空闲连接，如果有就把建立时间最长（通过容器的顺序存放来决定的)的那个连接分配给他(实际是先做连接是否有效的判断,如果可用就分配给用户,如不可用就把这个连接从空闲池删掉,重新检测空闲池是否还有连接);如果没有则检查当前所开连接池是否达到连接池所允许的最大连接数,如果没有达到,就新建一个连接,如果已经达到,就等待一定的时间(timeout)
   ```

5. 连接池的配置与维护

   连接池中到底应该放置多少连接，才能使系统的性能最佳？

   ```
   	系统可采取设置最小连接数(minconn）和最大连接数（maxconn）来控制连接池中的连接。最小连接数是系统启动时连接池所创建的连接数,如果创建过多,则系统启动就慢,但创建后系统的响应速度就会很快;如果创建过少,则系统启动的很快.响应起来却慢,这样,可以在开发时设置较小的最小连接数,开发起来会快,而在系统实际使用时设置较大的,因为这样对访问客户来说速度会快些,最大连接数是连接池中允许连接的最大数目,具体设置多少,要看系统的访问量,可通过反复测试,找到最佳点
   ```

   如何确保连接池中的最小连接数呢？

   ```
   有动态和静态两种策略。动态即每隔一定时间就对连接池进行检测，如果发现连接数量小于最小连接数，则补充相应数量的新连接以保证连接池的正常运转。静态是发现空闲连接不够时再去检查。
   ```

### 7.7 工作机制

数据库连接池在初始化时将创建一定数量的数据库连接放到连接池中，这些数据库连接的数量是由最小数据库连接数来设定的。无论这些数据库连接是否被使用，连接池都将一直保证至少拥有这么多的连接数量。连接池的最大数据库连接数量限定了这个连接池能占有的最大连接数，当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列中

> 以上这些可通过我写的迷你连接池也可理解其机制

**理解**:所谓的数据库连接池就是将连接数据库的连接管道统一的放在一起管理,可理解为容器,在连接池初始化的时候可设置初始连接管道的连接数量来控制,每一次连接使用完就关闭连接管道,注意这里的关闭不是跟数据库断开连接,而是将连接重新放回连接池中去,也可理解为相当于设置了标识;所以可通过一系列参数来设置关于连接池中连接管道的数量的参数,如果访问请求连接超过了最大连接数量,那么就只能加入等待队列中,如果有些连接管道出现空闲,那么连接池会把空闲连接最长的那个连接管道分配给他,如果最后都没有达到条件的话,那就只能处于等待一定的时间
