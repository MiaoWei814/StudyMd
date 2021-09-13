# XML

## 1.概念:

**概述**:XML是可扩展标记语言（Extensible Markup Language）的缩写

**作用**:主要用于提供数据表述格式，适用于不同应用程序间的数据交换，而且这种交换不以预先定义的一组数据结构为前提，增强了可扩展性

**特点:**

XML在各个行业、各类应用中广泛使用，作为数据存储、传输交互等重要作用，具有很强的扩展性:

1. 跨平台
2. 跨语言
3. 灵活度高，可扩展

## 2.XML结构证明

一个基本的XML文档通常是由序言和文档元素两部分组成。序言中可以包括XML声明、处理指令和注释，但这3项不是必须的，XML文档中的元素以XML文档中的元素以树形结构排列，一个元素可以嵌套在另一个元素中XML文件的声明虽然不是必须，但是规范性一般都建议加上：

- version：用于指定遵循ⅩML规范的版本号，必须在第一个出现。
- encoding：用于指定ⅩML文档中使用的编码（u18或gbk最为常用）

## 3.XML文档语法规范

Xml虽然可以自由扩展标签，但是必须遵循如下规范：

**第一行用了定义xml文件的，写版本号和字符集**

1. 注释不能放在第一行
   - 必须有声明且声明必需在第一行
   - 文档声明的字符编码必需和文档本身的编码一致
2. 标签不能交叉嵌套使用（标签名字随便定义）
3. 有开始有结束
4. 不数字开头，只能以字母或下划线开头
5. 只能由一个根标签
6. 严格区分大小写
7. 一个标签不能有两个相同属性（属性名随便定义）
   - 属性直接写在头标签内，格式：属性名=“属性值”
8. 特殊符号需要转义 或者可以放到CDATA区 `[![CDATA[这里写特殊符号]]]` HTML特殊符号,转义符号
9. 编码格式统一:
   - 开发环境:项目环境、系统环境
   - 当前文件内 encoding字符集
10. 不能以xml(或者XML,Xml等)开头,W3C保留日后使用
11. 名称字符之间不能有空格或者制表符,例如<四川 省>
12. 名称字符之间不能使用冒号;<xml:xx></xml:xx>

### 3.1文档声明:

- 在编写ⅫML文档时，需要先使用文档声明来声明XML文档。且必须岀现在文档的第一行。
  - 最简单的语法：<？xml version=“1.0">

- 用 encod ing属性说明文档所使用的字符编码，默认为UIF3。保存在磁盘上的文件编码要与声明的编码一致。
  - 如：<？xml version=“1.0" encoding=UTF8？>
- 用 standalone属性说明文档是否独立，即是否依赖其他文档。
  - 如：<？xml version=“1.0" standalone=yes"？>

**注意**:

1.  XML文件可以使任意的文本编辑器来写
2. 可以使用浏览器打开：
   - 如果符合xml的语法，可以看到内容，否则页面上面报错;
   - 不同的浏览器显示的效果可能有细微的差别
3. 必需有声明，声明必需在第一行。
4. 文档声明的字符编码必需和文档本身的编码一致

### 3.2 元素定义

- XML元素指XML文件中出现的标签。一个标签分为起始和结束标签（不能省略）。一个标签有如下几种书写形式：

  - 包含标签主体：<name><name>：如`<b>`加粗`</b>`
  - 不含标签主体：<name/>：如<h/>

- 一个标签中可以嵌套若干子标签，但所有标签必须合理的嵌套，不允许有交叉嵌套。

  ```xml
  <name><age></name></age> //错误的
  <name><age></age></name> //正确的
  ```

- 一个XML文档必须有且仅有一个根标签,其他标签都是这个根标签的子标签或孙标签

**注意:**

1. 有开始必需有结束 <input><input />

2. 不允许交叉嵌套

3. 有且只有一个根标签

4. XML中不会忽略主体内容中出现的空格和换行

   ```xml
   比如:
   <name>Will</name>
   再看这个
   <name>\r\n
       \tWill\r\n
   </name>
   //所以这是不一样的
   ```

### 3.3 元素的标记名称规范

元素(标签)的名称可以包含字母、数字、减号、下划线和英文句点，但且必须遵守下面的一些规范

1. 严格区分大小写；<p></p>
2. 只能以字母或下划线开头; 如:abc _abc
3. 不能以xml(XML、Xml等)开头-----W3C保留日后使用
4. 名称字符之间不能有空格或制表符;a b
5. 名称字符之间不能使用冒号; (有特殊用途)

### 3.4 属性定义

1. 一个元素可以有多个属性,每个属性都有他自己的名称和取值,例如:

   ```css
   <font color="red" size=24> xxx</font>
   ```

2. 属性值一定要用引号(单引号或双引号)引起来

3. 属性名称的命名规范与元素的命名规范相同

4. 元素中的属性是不允许重复的

5. 在XML技术中,标签属性所代表的信息也可以被改成用子元素的形式来描述:

   ```xml
   <person>
       <name>Will</name>	<person name="Will"/>
   </person>
   ```

**注意:**

- 属性必需有值
- 值必需用引号引起来
- 同一个标签中的属性名不能相同;但是一个标签可以有多个同名的子标签

### 3.5 注释

- 用"`<!--注释-->`"这种格式加入注释

- XML声明之前不能带有注释

- 注释不能嵌套

  ```xml
  <!-- 大段注释
  	....
  	<!-- 局部注释 -->
  	....
  -->
  ```

### 3.6 特殊字符

对于一些特殊字符,若要在元素主体内容中显示,必须进行转义。

| 特殊字符 | 替代符号 |
| :------: | :------: |
|    &     | `&amp;`  |
|    <     |  `&lt`   |
|    >     |  `&gt;`  |
|    "     | `&quot'` |
|    '     | `&apos;` |

### 3.8 CDATA区

1. CDATA是Character Data的缩写

2. 作用:把标签当做普通文本内容

3. 语法:

   ```xml
   <![CDATA[数据内容]]>
   ```

4. 作用:

   - 解析器不会对CDATA区中的内容进行解析,而是将这些数据原封不动地交给程序去处理

5. CDATA区中的起始和结束处有和没有空格和换行符是有区别的,例如:

   ```xml
   <![CDATA[xxx]]>
   和
   <![CDATA[
   	xxx
   ]]>
   //<![CDATA[<><><><><>&^*(^*(^&*(3sdfjpwrjp2024372394j*(*<>]]>
   CDATA段中的内容原样输出
   ```

   

### 3.9语法

1. 文档声明必须写在第一行,且只能由一个根标签
2. Xml中空格和换行都表示数据,所以请严格区分大小写
3. XML中特殊字符表示的数据需要使用特殊字符编码和HTML一样
4. CDATA区中的数据不会被识别为语法

## 4.XML的约束

XML的约束有两种方式：DTD(.dtd)与Schema(.xsd)

其中的Schema的约束是基于dtd的替代者,也是描述XML文档的结构

```java
DTD:语法自成一派， 早起就出现的。 可读性比较差.
Schema：其实就是一个xml ， 使用xml的语法规则， xml解析器解析起来比较方便 ， 是为了替代DTD 。
但是Schema 约束文本内容比DTD的内容还要多。 所以目前也没有真正意义上的替代DTD   
```

使用:

- DTD

  - 格式

    ```dtd
    	<!ELEMENT stus (stu+)> stus根标签下面有至少一个标签 stu,定义根标签
        <!ELEMENT stu (name,age)>  stu下面有两个元素 name,age顺序必须 name-age 定义标签
        <!ELEMENT name (#PCDATA)> 
        <!ELEMENT age (#PCDATA)>
        <!ATTLIST stu id CDATA #IMPLIED> stu有一个属性 文本类型， 该属性可有可无
    ```

  - 引入:

    ```dtd
    1. 引入网络上的DTD
    
       <!-- 引入dtd 来约束这个xml -->
    
              <!--    文档类型  根标签名字 网络上的dtd   dtd的名称   dtd的路径
    
              <!DOCTYPE stus PUBLIC "//UNKNOWN/" "unknown.dtd"> -->
    
    2. 引入本地的DTD
    
          <!-- 引入本地的DTD  ： 根标签名字 引入本地的DTD  dtd的位置 -->
          <!-- <!DOCTYPE stus SYSTEM "stus.dtd"> -->
    
    3. 直接在XML里面嵌入DTD的约束规则
    
       <!-- xml文档里面直接嵌入DTD的约束法则 -->
    
              <!DOCTYPE stus [
                  <!ELEMENT stus (stu)>
                  <!ELEMENT stu (name,age)>
                  <!ELEMENT name (#PCDATA)>
                  <!ELEMENT age (#PCDATA)>
              ]>
    
              <stus>
                  <stu>
                      <name>张三</name>
                      <age>18</age>
                  </stu>
              </stus>
    ```

- Schema

  - 基本格式:

    ```scheme
    <!-- xmlns  :  xml namespace : 名称空间 /  命名空间
        targetNamespace :  目标名称空间 。 下面定义的那些元素都与这个名称空间绑定上。 
        elementFormDefault ： 元素的格式化情况。  -->
        <schema xmlns="http://www.w3.org/2001/XMLSchema" 
            targetNamespace="http://www.w3.org/teacher" 
            elementFormDefault="qualified">  //表示启用自定义命名空间
            
            <element name="teachers">
                <complexType>
                    <sequence maxOccurs="unbounded">
                        <!-- 这是一个复杂元素 -->
                        <element name="teacher">
                            <complexType>
                                <sequence>
                                    <!-- 以下两个是简单元素 -->
                                    <element name="name" type="string"></element>
                                    <element name="age" type="int"></element>
                                </sequence>
                            </complexType>
                        </element>
                    </sequence>
                </complexType>
            </element>
        </schema>
    
    实例文档：
        <?xml version="1.0" encoding="UTF-8"?>
        <!-- xmlns:xsi : 这里必须是这样的写法，也就是这个值已经固定了。
        xmlns : 这里是名称空间，也固定了，写的是schema里面的顶部目标名称空间
        xsi:schemaLocation : 有两段： 前半段是名称空间，也是目标空间的值 ， 后面是约束文档的路径。
         -->
        <teachers
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns="http://www.w3.org/teacher"
            xsi:schemaLocation="http://www.itheima.com/teacher teacher.xsd"
        >
            <teacher>
                <name>zhangsan</name>
                <age>19</age>
            </teacher>
            <teacher>
                <name>lisi</name>
                <age>29</age>
            </teacher>
            <teacher>
                <name>lisi</name>
                <age>29</age>
            </teacher>
        </teachers>
    ```

  - 名称空间的作用:

    一个xml如果想指定它的约束规则， 假设使用的是DTD ，那么这个xml只能指定一个DTD ， 不能指定多个DTD 。 但是如果一个xml的约束是定义在schema里面，并且是多个schema，那么是可以的。简单的说： 一个xml 可以引用多个schema约束。 但是只能引用一个DTD约束。

    > 名称空间的作用就是在 写元素的时候，可以指定该元素使用的是哪一套约束规则。 默认情况下 ，如果只有一套规则，那么都可以这么写

    ```scheme
    <teachers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aa="http://www.w3.org/teacher" xmlns:bb="http://www.w3.org/teacher"xsi:schemaLocation="http://www.itheima.com/teacher teacher.xsd" >
    <name>张三</name>
    
    <aa:name></aa:name>
    <bb:name></bb:name>
    ```

## 5.Dom4J解析

首先我们得引入Dom4J,因为这是其他人写的,然后这里引入采用maven的方式:

```xml
 <!-- dom4j -->
 <dependency>
     <groupId>dom4j</groupId>
     <artifactId>dom4j</artifactId>
     <version>1.6.1</version>
</dependency>
```

**说明:**这里解析方式不止一个,有多个:

> xml解析方式：https://www.cnblogs.com/longqingyang/p/5577947.html【扩展】

示例:

```java
package com.cxx.xml;
import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.File;
import java.util.Iterator;
import java.util.List;

/**
 * @Author: cxx
 * Dom4j解析xml
 * @Date: 2018/5/30 12:21
 */
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        //1.创建Reader对象
        SAXReader reader = new SAXReader();
        //2.加载xml
        Document document = reader.read(new File("src/main/resources/demo.xml"));  //拿到了Document对象
        //3.获取根节点
        Element rootElement = document.getRootElement(); //拿到根节点
        Iterator iterator = rootElement.elementIterator(); //通过根节点获取一个节点的文本
        while (iterator.hasNext()){
            Element stu = (Element) iterator.next();
            List<Attribute> attributes = stu.attributes();
            System.out.println("======获取属性值======");
            for (Attribute attribute : attributes) {
                System.out.println(attribute.getValue());
            }
            System.out.println("======遍历子节点======");
            Iterator iterator1 = stu.elementIterator();
            while (iterator1.hasNext()){
                Element stuChild = (Element) iterator1.next();
                System.out.println("节点名："+stuChild.getName()+"---节点值："+stuChild.getStringValue());
            }
        }
    }
}
```

结果:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20180530123713879)

### 5.1 Dom4J的结构与语法:

![image-20210815152850586](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210815152850586.png)

分别对应右图的结构,也叫文档树,整个文档树就是Document对象

这里先介绍在解析过程中常用的方法:

```java
dom4j中使用到的方法：
		1.创建SaxReader对象
			SAXReader reader = new SAXReader();
		
		*******************************这是使用约束的用法************************************	
		2.设置命名空间	（如果XML使用约束且属性elementFormDefault值是：qualified，则必须要设置）【简化自定义命名空间使用过程】
			HashMap<String, String> hashMap = new HashMap<String, String>();
			map.put("myKey", "自定义命名空间名");// key的名字自己取
        	reader.getDocumentFactory().setXPathNamespaceURIs(map);	
		*******************************************************************	
		
		3. SAXReader对象调用read方法，将当前XML文件，转换为Document对象
			获取字节流InputStream
			用缓冲流包装一下InputStream
			Document document = reader.read(缓冲流);
			
		4. 获取根节点
			root = document.getRootElement();
			
		5. 通过父签添加子标签（元素）
			Element element = root.addElement("标签名");//返回值就是要添加的元素对象
			
			给子标签添加值
			element.setText("标签值");
			
		6. 给当前标签添加属性:xxx ，值是：xxx
			Element attribute = linkman.addAttribute("属性名","值");	
			
			通过当前元素获取属性对象
				element.attribute("属性名");
				
			通过属性对象attribute获取属性值
				attribute.setText("值");
				
			通过属性对象attribute获取属性值
				String 值 = attribute.getText();
			
		7. 获取当前元素标签名
				String name = e.getName();
				
			获取当前元素标签值
				String text = e.getText();
		
		8. 获取指定名字的子标签（元素）
		   	 	当前标签.element(String name);	
		   	 	
		9. 获取所有子标签（元素）
		   	 	当前标签.elements();	
			
		10. 删除子元素，必须通过父元素remove(子元素对象)完成	
				父元素.remove(子元素对象);
			
		10.在dom4j里面提供了两个方法，用来支持xpath
       　　　　	selectNodes("xpath表达式")，获取当前名字的多个节点
　　　　　　 	selectSingleNode("xpath表达式")，获取一个节点	
			【注意：】
				在多层级的xpath使用的语法：//命名空间:a[@id='b1']/命名空间:元素
```

案例:

这是定义的XML:

```xml
<?xml version="1.0" encoding="UTF-8" ?>  <!-- 声明当前文件为XML文件,且版本是1.0,字符集是UTF-8,在该代码之前不能写任何其他代码,包括注释-->
<contacts>
    <linkman id="2334">
        <name>gg</name>
        <email>ggditsource cn</email>
        <address>成都</address>
        <group>源代码教育</group>
    </linkman>
    <linkman id="45">
        <name>mm</name>
        <email>lanyotechdgg com</email>
        <address>阿斯顿发送到</address>
        <group>源代码教育</group>
    </linkman>
</contacts>
```

代码:

这是只是查询我们想要的:

```java
package org.javase.work_0815;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.io.File;
import java.util.List;

/**
 * @program: YmsdLearn
 * @description: 解析xml
 * @author: MaoWei
 * @create: 2021-08-15 15:17
 */
public class ParseXml {
  @Test
  // 查找id=45的的节点
  public void test() throws Exception {
    //    final File file = new File("F:\\源码时代\\YmsdLearn\\src\\main\\resources\\test.xml");
    final File file = new File("src/main/resources/test.xml");
    if (!file.exists()) {
      throw new RuntimeException("123");
    }
    parseXMLFile(file);
  }

  /**
   * 解析xml文件
   *
   * @param file 文件
   */
  public void parseXMLFile(File file) throws Exception {
    SAXReader reader = new SAXReader();
    // 获取doument文档树
    final Document document = reader.read(file);
    // 获取根节点【<contacts>】
    final Element rootElement = document.getRootElement();
    // 遍历根节点下的所有标签【<linkman>】
    final List<Element> elements = rootElement.elements();

    // 第一种方式
    for (Element element : elements) {
      // 获取每个标签的指定属性【id】
      Attribute idAttribute = element.attribute("id");
      // 筛选指定属性值
      if ("45".equals(idAttribute.getText())) {
        // 获取指定元素
        Element group = element.element("group");
        System.out.println(group.getText()); //源代码
      }
    }

    // 第二种方式
    String name =
        elements.stream()
            .filter(x -> "45".equals(x.attribute("id").getText()))
            .map(x -> x.element("group").getText())
            .findFirst()
            .orElseThrow();
    System.out.println("name = " + name); //源代码
  }
}

```

### 5.2 Xpath的语法与解析

支持xpath解析，就可以可以直接输入一个路径查找：
由于DOM4J在解析XML时只能一层一层解析，所以当XML文件层数过多时使用会很不方便，
结合XPATH就可以直接获取到某个元素

这是语法:

```java
使用dom4j支持xpath的操作的几种主要形式【提高查询元素的效率的】
			https://blog.csdn.net/sidihuo/article/details/41310727
			xpath表达式：
			   　　第一种形式
			        　　　　/a/b/c： 表示一层一层的，a下面 b下面的c
			    　  第二种形式
			       　　　　 //b： 表示和这个名称相同，表示只要名称是b，都得到
			         第三种形式
			        　　　　/* : 所有元素
			         第四种形式
			       　　　　a[1]：　表示第一个a元素
			        　  　　a[last()]：表示最后一个a元素
			         第五种形式
			        　　　　//a[@id]： 表示只要a元素上面有id属性，都得到
			         第六种形式
			        　　　　//自定义命名空间:a[@id='b1'] 表示元素名称是a,在a上面有id属性，并且id的属性值是b1【如果使用约束文件，则写自定义命名空间】
			        　　　　//a[@id='b1'] 表示元素名称是a,在a上面有id属性，并且id的属性值是b1【如果不使用约束文件】
			
					如果查询id属性值是b1的a元素下一级元素d，则可以这样写：
						//自定义命名空间:a[@id='b1']/自定义命名空间:d 表示元素名称是a,在a上面有id属性，并且id的属性值是b1【使用了约束文件，且属性值是：qualified】
						//a[@id='b1']/d 表示元素名称是a,在a上面有id属性，并且id的属性值是b1【如果不使用约束文件】
			
			使用dom4j支持xpath具体操作
		         默认的情况下，dom4j不支持xpath，如果想要在dom4j里面是有xpath，
		         第一步需要，引入支持xpath的jar包，如下：
				jaxen-1.1-beta-6.jar
		       
		         在dom4j里面Xpath解析，提供了两个方法，用来支持xpath
		       　　　　selectNodes("xpath表达式")，获取多个节点
		　　　　　　selectSingleNode("xpath表达式")，获取一个节点
	解析XML步骤：
			1. 在项目下创建一个lib文件夹folder
			2. 复制dom4j的jar包到lib文件夹中
			3. 选中jar右键buildpath
			4. 在测试类中使用dom4j解析XML。（今天开始学习别人的api）
```

这里是优化:

xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- 为了测试简单先不用约束 -->
<!-- <contacts>  -->
<!-- 使用约束文件  -->
<contacts xmlns="http://itsource.cn" xmlns:xs="http://www.w3.org/2001/XMLSchema-instance" xs:schemaLocation="http://itsource.cn  contactsSchema.xsd">  
  <linkman id="2334"> 
    <name>gg</name>  
    <email>gg@itsource.cn</email>  
    <address>成都</address>  
    <group>源代码教育</group> 
  </linkman>  
  <linkman id="45"> 
    <name>mm</name>  
    <email>lanyotech@qq.com</email>  
    <address>阿斯顿发送到</address>  
    <group>源代码教育</group> 
  </linkman>  
  <linkman id="69"> 
    <name>菁涵小宝贝</name>  
    <email>chengDuHot@qq.com</email>  
    <address>成都高新区菊花台路</address>  
    <group>菊花残集团</group> 
  </linkman> 
</contacts>

```

优化后的查询:

```java
package org.javase.work_0815.homewrok;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.HashMap;

/**
 * @program: YmsdLearn
 * @description: 解析xml
 * @author: MiaoWei
 * @create: 2021-08-15 18:16
 */
public class parseXmlTest3 {
  // 使用XPath解析
  @Test
  public void test() throws Exception {
    // 1.创建SaxReader对象
    SAXReader reader = new SAXReader();
    // 2.使用自动关流try
    try (BufferedReader bufferedReader =
        new BufferedReader(
            new FileReader("src/main/java/org/javase/work_0815/homewrok/contacts.xml"))) {
      // 3.因为设置了约束,所以这里需要设置命名空间
      HashMap<String, String> hashMap = new HashMap<>();
      hashMap.put("myKey", "http://itsource.cn");
      reader.getDocumentFactory().setXPathNamespaceURIs(hashMap);
      // 4.read对象调用read方法读取缓冲字符流,获取Document对象
      Document document = reader.read(bufferedReader);
      // 5.获取根节点
      Element rootElement = document.getRootElement();
      // 6.通过XPath得到节点
      Node node = rootElement.selectSingleNode("//myKey:linkman[@id='69']/myKey:name"); //linkman[@id='69']： 表示只要linkman元素上面有id属性为69的，都得到
      System.out.println("node.getText() = " + node.getText()); //获取值
    }
  }
}
```

注意:因为我这里使用的maven,所以在使用XAPath语法,就会导致在`selectSingleNode`就会发生找不到包,所以我这里经过测试可以引入下面的包:

```xml
 <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.2.0</version>
        </dependency>
```

这样就可以完美的解决了!

> 还有我这里的xml使用了约束,所以我这里采用命名空间的方式来解决!

然后光查询绝对是远远不够的,所以我们还得需要增删改查的操作!

#### 5.2.1 增删改查

这是xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- 为了测试简单先不用约束 -->
<!-- <contacts>  -->
<!-- 使用约束文件  -->
<contacts xmlns="http://itsource.cn" xmlns:xs="http://www.w3.org/2001/XMLSchema-instance" xs:schemaLocation="http://itsource.cn  contactsSchema.xsd">  
  <linkman id="2334"> 
    <name>gg</name>  
    <email>gg@itsource.cn</email>  
    <address>成都</address>  
    <group>源代码教育</group> 
  </linkman>  
  <linkman id="45"> 
    <name>mm</name>  
    <email>lanyotech@qq.com</email>  
    <address>阿斯顿发送到</address>  
    <group>源代码教育</group> 
  </linkman>  
  <linkman id="69"> 
    <name>菁涵小宝贝</name>  
    <email>chengDuHot@qq.com</email>  
    <address>成都高新区菊花台路</address>  
    <group>菊花残集团</group> 
  </linkman> 
</contacts>

```

代码:

```java
package org.javase.work_0815.homewrok;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.OutputFormat;
import org.dom4j.io.SAXReader;
import org.dom4j.io.XMLWriter;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.util.HashMap;

/**
 * @program: YmsdLearn
 * @description: 解析xml -使用XPath解析
 * @author: MiaoWei
 * @create: 2021-08-15 18:16
 */
public class parseXmlTest3 {
  // 这是文档树
  private Document document;
  // 这是根节点
  private Element rootElement;

  /** 开始配置 */
  @Before
  public void stratDeploy() throws Exception {
    // 1.创建SaxReader对象
    SAXReader reader = new SAXReader();
    // 2.使用自动关流try
    try (BufferedReader bufferedReader =
        new BufferedReader(
            new FileReader("src/main/java/org/javase/work_0815/homewrok/contacts.xml"))) {
      // 3.因为设置了约束,所以这里需要设置命名空间
      HashMap<String, String> hashMap = new HashMap<>();
      hashMap.put("myKey", "http://itsource.cn");
      reader.getDocumentFactory().setXPathNamespaceURIs(hashMap);
      // 4.read对象调用read方法读取缓冲字符流,获取Document对象
      document = reader.read(bufferedReader);
      // 5.获取根节点
      rootElement = document.getRootElement();
    }
  }

  /** 最终部署 */
  @After
  public void endDeploy() throws Exception {
    // 我们的读写如果不写进文件中那么一切都是空谈,我们创建的都是在内存中没有写入硬盘中,所以现在就来进行改变
    XMLWriter xmlWriter = null;
    // 创建字符流
    try (BufferedWriter bufferedWriter =
        new BufferedWriter(
            new FileWriter("src/main/java/org/javase/work_0815/homewrok/contacts.xml")); ) {
      // 在写入文件中要对格式进行修改,如果不修改那么就会直接打印成一行的格式
      OutputFormat prettyPrint = OutputFormat.createPrettyPrint();
      // 创建XMLWriter对象,字符流作为参数,作用就是将文档树document写入硬盘中
      xmlWriter = new XMLWriter(bufferedWriter, prettyPrint);
      // 开始写入
      xmlWriter.write(document);
      System.out.println("修改成功");
    } finally {
      // 这里进行手动关流
      if (xmlWriter != null) xmlWriter.close();
    }
  }
  /**
   * 查询
   *
   * @throws Exception 异常
   */
  @Test
  public void query() throws Exception {
    // 通过XPath得到节点
    Node node = rootElement.selectSingleNode("//myKey:linkman[@id='69']/myKey:name");
    System.out.println("node.getText() = " + node.getText());
  }

  /**
   * 添加
   *
   * @throws Exception 异常
   */
  @Test
  public void add() throws Exception {
    // 由于我们的before注解的类得到根节点后,我们只需要在根节点下创建一个标签
    Element linkman = rootElement.addElement("linkman");
    // 对这个标签进行增加属性
    linkman.addAttribute("id", "200");
    // 对这个节点下的子节点进行创建元素
    linkman.addElement("name").setText("缪威");
    linkman.addElement("email").setText("2439135122@qq.com");
    linkman.addElement("address").setText("成都市");
    linkman.addElement("group").setText("分租");
  }

  /**
   * 编辑
   *
   * @throws Exception 异常
   */
  @Test
  public void edit() throws Exception {
    // 首先得找到修改的地方
    Node node = rootElement.selectSingleNode("//myKey:linkman[@id='200']/myKey:group");
    // 然后进行set新的值
    node.setText("新分组123");
  }

  /** 删除 */
  @Test
  public void remove() {
    // 删除也是得首先找到要删除的节点,不能删除标签内的元素,因为约束,所以我们这里删除标签
    Node childNode = rootElement.selectSingleNode("//myKey:linkman[@id='200']");
    // 删除的语法是:父标签.remove(子标签)
    boolean remove = rootElement.remove(childNode);
    System.out.println("删除是否成功:" + (remove));
  }
}

```

