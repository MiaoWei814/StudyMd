# File与IO流

介绍:

1. 文件包括文件里面的内容和文件基本属性
2. 文件基本属性：名称、大小、扩展名、修改时间等

## 1. File类

概念:就是java中的一个文件或者文件夹。如果没有后缀就表示是一个文件夹，如果有后缀就表示文件。

作用:为了IO流提供操作的基础

**路径:**

1. 相对路径:相对于当前Java项目
2. 绝对路径:从根目录或者盘符开始 /都表示从根目录或者盘符开始

**注意:**File不涉及到具体的文件内容,只涉及相关属性



构造方法:

```java
File(String pathname) 根据文件名创建一个File对象
    pathname:（如果有后缀就是一个文件对象，如果没有后缀会默认为是一个文件夹对象）
        1.相对路径：相对于当前Java类的项目
        2.绝对路径：从根目录或者盘符开始	/都表示从根目录或者盘符开始
File(String parent, String child) 在父级目录下创建一个文件或文件夹
        parent：父级目录
        child：当前目录或者当前文件
File(File parent, String child) 在父级目录下创建一个文件或文件夹
```

> 注意: 创建一个File对象并没有在磁盘上面创建一个文件（文件夹） 需要调用File中的特殊的方法。

常用方法:

```java
获取子文件夹或者子文件方法
1.String[] list()获取当前文件夹下所有的子文件夹或者子文件的名;	//文件没有下一级
2.File[] listFiles() 获取当前文件夹下所有的子文件夹或者子文件对象;
3.static File[] listRoots() 列出可用的文件系统根(操作系统上面的盘符)
有过滤功能
3.String[] list(FilenameFilter filter) //获取根据文件名过滤后的文件名 FilenameFilter：文件名过滤器接口
4.File[] listFiles(FilenameFilter filter); //获取根据文件名过滤后的文件对象 FilenameFilter：文件名过滤器接口
5.File[] listFiles(FileFilter filter); //获取根据文件过滤后的文件对象 FileFilter：文件过滤器接口
```

注意:如果获取当前文件夹因为人为原因不存在了,那么就会返回null,或者当前文件夹里没有子文件,那么就会返回空数组

​	一般判断的话就用if(list==null || list.length==0)

```java
1.boolean exits(); //判断File对象是否存在
2.boolean mkdir(); //创建由此抽象路径名命名的目录。(创建的是目录而不是文件)
3.boolean isDirectory(); //测试此抽象路径名表示的文件是否为目录。
4.boolean createNewFile(); // 创建新文件（非目录）,可能会磁盘不足或者需要权限问题所以报错
5.boolean	isFile();//判断是否是文件
6.String	getName();//返回文件或者文件目录名称
7.String	getPath();//返回路径
8.String	getParent();//返回父路径字符串,如果没有命名则为空[只能获取创建对象的时候的父级目录]
9.long	length();//返回文件长度,文件夹不能直接获取字节数,获取文件占用空间大小(返回的是字节)
10.long	lastModified();//返回文件上次修改时间
11.Boolean delete();//删除文件或者目录 这里要注意:文件可以直接删除，如果是文件夹，该文件夹必须是空文件夹，才能删除成功
12.boolean renameTo(File dest);// 重命名当前文件或文件夹对象 【当前File对象和dest路径要一致】
13.static File createTempFile(String prefix, String suffix, File directory);//在指定的目录directory中创建一个新的空文件，使用给定的前缀和后缀字符串生成其名
14.static File createTempFile(String prefix, String suffix);//在默认临时文件目录中创建一个空文件，使用给定的前缀和后缀生成其名称。
```

示例:

```java
 public static void main(String[] args) throws Exception{
    //直接输入文件名则是相对路径,也就是相对当前项目的路径下
    File file = new File("testFile/abc.txt");
    if (!file.exists()) {
      //      file.mkdir(); //判断是否存在,如果不存在则创建
      file.createNewFile(); //创建新的文件
    }
    System.out.println("判断是否是目录:"+file.isDirectory());

    // 输出相关属性
    System.out.println("是否是文件:"+file.isFile());
    System.out.println("返回文件名:"+file.getName());
    System.out.println("返回文件父路径:"+file.getParent());
    System.out.println("文件大小:"+file.length());
    System.out.println("文件最后一次修改时间:"+file.lastModified());
  }
//
判断是否是目录:false
是否是文件:true
返回文件名:abc.txt
返回文件父路径:testFile
文件大小:0
文件最后一次修改时间:1628297111566
```



文件和文件名过滤器

File中提供了可以达到过滤效果的方法，我们可以在调用的时候指定具体的过滤器来得到我们想要的一个目录下面的东西。由于我们想要什么东西Java本身不知道的，所有它仅仅给我们提供了一个过滤器的接口（FilenameFilter,  FileFilter）而已,需要我们自己自定义实现过滤器接口来定义过滤器类，具体的使用查阅File中的如下方法：

```java
String[] list(FilenameFilter filter) //返回的是文件名字组成的String[]数组,比如:[test.txt]
File[] listFiles(FileFilter filter)  //可以根据文件的其他属性进行过滤
File[] listFiles(FilenameFilter filter) 
```

示例:FilenameFilter

```java
      File[] files = dir.listFiles(
              (dir1, name) -> {
                  System.out.println("dir = " + dir1);
                  System.out.println("name = " + name);
                  return name.endsWith(".txt");
              });
    System.out.println("Arrays.toString(files = " + Arrays.toString(files));
//打印
dir = D:\测试删除文件
name = api_access.2021-04-15.log
dir = D:\测试删除文件
name = test.txt
dir = D:\测试删除文件
name = 新建文件夹
Arrays.toString(files = [D:\测试删除文件\test.txt]
```

说明:经过测试得出dir是父文件名 name是子文件名,所返回的也是符合要求的FIle对象,只包含符合要求的,文件夹不会返回

示例:FileFilter

```java
File[] files1 = dir.listFiles(
              new FileFilter() {
                  @Override
                  public boolean accept(File pathname) {
                      System.out.println("pathname = " + pathname);
                      return pathname.getName().endsWith(".txt");
                  }
              });
    System.out.println("Arrays.toString(files1 = " + Arrays.toString(files1));
打印:
pathname = D:\测试删除文件\api_access.2021-04-15.log
pathname = D:\测试删除文件\test.txt
pathname = D:\测试删除文件\新建文件夹
Arrays.toString(files1 = [D:\测试删除文件\test.txt]
```

**注意:**这里要注意一个地方,在使用过滤器的时候,它只会在第一层子文件里查找,如果子文件有个文件夹那么它不会去子文件里的子文件里去找(我这里新建文件夹里还有一个文件它这里完全就没打印出来,所以就可以得出结论了),所以这里确实是个要注意的地方!所以要实现子文件的子文件过滤那么就要手动进行修改一下

> 一旦使用过滤器和list都是只查找当前文件夹下的子文件一层,如果子文件下的子文件存在是不会走到那一层的

这里给出一个文件夹把所有符合要求的文件返回提供一个思路:

​	通过根据点遍历子文件,判断是文件那么装到list中的File[]中,然后判断是文件夹那么进行递归又重新调用本方法,然后继续执行放到File[]中然后一直这样就可以了,这里的LinkedList<File[]> file=new LinkedList<>();可以先add添加进去,然后添加完后获取size作为数组大小,然后循环添加到新的数组中去



这里我记录一下我思考了许久的问题:如何将List<File[]> list=new ArrayList<>();合并为一个数组

经过我多次思考想过多个方案,也曾想过试着循环list遍历然后装到新的数组中去,但是那个过程太繁琐且不优雅,所以我在研究Stream的过程中,发现了好东西:

```java
 ArrayList<File[]> list2 = new ArrayList<>();
    list2.add(files2);
    list2.add(files1);
    list2.add(listFiles);

File[] files3 = list2.stream().flatMap(Arrays::stream).toArray(File[]::new);//完美解决多个数组合并到一个数组中去
```

看结果:

![image-20210808162750820](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210808162750820.png)

解释一下:首先将list转为流的形式然后对list多个数组进行转为多个流合并为一个流,然后转为数组,我真的叹呼Stream的强大之处!

### 1.1 递归

简单理解: 一般就是一个方法内部自己调自己,是一种思想,也是一种算法.

作用:就是简化有相似的业务逻辑（循环）代码。

使用规律特点:

1. 通过数值带入的方式，查找逻辑规律：将所有的已知条件写出来,就是有相似运算逻辑的代码
2. 通过条件判断（条件/循环）的方式，在满足条件的时候根据规律调用自己的方法。 如果不满足就不会自己的方法，这就是出口

使用场景:

1. 树状结构数据、菜单结构、上下级关系等的数据查询
2. 有相似逻辑运算的结构
3. 适用于层级结构少，数据量小的场景。（最重要）如果层级多，且数据量大用循环。

解决了哪些问题:

1. 数据的定义是按递归定义的。（Fibonacci函数：斐波拉契数列）
2. 问题解法按递归算法实现。(汉诺塔问题) 这类问题虽则本身没有明显的递归结构，但用递归求解比迭代求解更简单，如Hanoi问题。
3. 数据的结构形式是按递归定义的。如二叉树、广义表等，由于结构本身固有的递归特性，则它们的操作可递归地描述。

注意:

1. 递归必须 有出口

2. 能用循环用循环，不要用递归，因为[效率极其低下](方法的调用都是创建新栈帧，入栈，而栈层级是有限制的,StackOverflowError : 栈内存溢出错误)

   

示例:

```java
public static Long fileSizeSum(File file) {
    long fileSize = 0L;
    if (!file.exists()) {
      throw new RuntimeException("该目录或文件无效不存在!");
    }
    // 判断是否文件
    if (file.isFile()) {
      return countFileSize(file.length(),false);
    }else{
      File[] listFiles = file.listFiles();
      for (File listFile : listFiles) {
        //判断是文件
        if (listFile.isFile()) {
          fileSize += listFile.length();
        }else{
          // 目录
          fileSize+= fileSizeSum(new File(listFile.getPath()));
        }
      }
    }

    return fileSize;
  }
```

画图理解:

![image-20210807103230021](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210807103230021.png)

从图可以看出,每次递归都会创建新的栈帧进行在栈帧中入栈,然后当方法调用完毕就会出栈,而如果一直没返回那么就会一直入栈创建新的栈帧,就会造成栈帧溢出,因为栈帧的内存是有限的,这是去取决于电脑内存的,然后性能方面肯定是不好的,所以少用!

## 2.IO

### 2.1 概念

**概念**:IO：Java对数据的操作是通过流的方式，IO流用来处理设备之间的数据传输，上传文件和下载文件，Java用于操作流的对象都在IO包中。

**分类:**

![这里写图片描述](https://gitee.com/miawei/pic-go-img/raw/master/imgs/20160522165107051)

流的概念与作用:

​	流是一组有顺序的,有起点和终点的字节集合,是对数据传输的总称和抽象。即数据在两设备间的传输称为流，而流的本质就是`数据传输`，根据数据传输特性将流抽象为各种类，方便直观的进行数据操作。

流的分类：

- 根据处理数据类型的不同分为：字符流和字节流
- 根据数据流向不同分为：输入流和输出流

流的特性:

- 先进先出：最先写入输出流的数据最先被输入流读取到。
- 顺序存取：可以一个接一个地往流中写入一串字节，读出时也将按写入顺序读取一串字节，不能随机访问中间的数据。（`RandomAccessFile`除外）
- 只读或只写：每个流只能是输入流或输出流的一种，不能同时具备两个功能，输入流只能进行读操作，对输出流只能进行写操作。在一个数据传输通道中，如果既要写入数据，又要读取数据，则要分别提供两个流。

字节流跟字符流的区别：

​	字符流的由来： 因为数据编码的不同，而有了对字符进行高效操作的流对象。本质其实就是基于字节流读取时，去查了指定的码表。

1. 读写单位不同：字节流以字节（8bit）为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节。
2. 处理对象不同：字节流能处理所有类型的数据（如图片、avi等），而字符流只能处理字符类型的数据
3. 字节流：一次读入或读出是8位二进制
4. 字符流：一次读入或读出是16位二进制。
5. 字节流本身没有缓冲区,缓冲字节流相对于字节流,效率提升比较大。而字符流本身就带有缓冲区，缓冲字符流相对于字符流效率提升就没有那么大

结论：设备上的数据无论是图片或者视频，文字，它们都是以二进制存储。二进制的最终都是以一个8位为数据单元进行体现，所以计算机中的最小数据单元就是字节，也就是说意味着 **字节流以处理设备上的所有数据，所以字节流一样可以处理字符数据**

> 理解：只要是处理纯文本数据，就优先考虑使用字符流，除此之外都使用字节流

### 2.2 IO流的分类

从三个不同的维度：

1. 按照流的方法（输入和输出都是以程序内部所在的内存的角度进行划分）

   - 输入流：只能从中读取数据【主要是由InputStream和Reader作为基类】-》可理解为读取文件

   - 输出流:只能向其写入数据【主要是由outputStream和Writer作为基类】-》可理解为向文件里写东西

     ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-9482617e5e4192a41995b36c8c53441e_720w.jpg)

2. 按照流的操作颗粒度进行划分:

   - 字节流：以字节为单元，可操作任何数据【主要由InputStream和outPutStream作为基类】
   - 字符流：以字符为单元，只能操作纯字符数据，比较方便【主要由Reader和Writer作为基类】

3. 按照流的角色进行划分:

   - 节点流:可以从/向一个特定的IO设备(如磁盘,网络)读/写数据的流.也叫"低级流";比如`FileInputStream`
   - 高级流:用于对一个已存在的流进行连接和封装,通过**封装**后的流来实现数据的读/写功能,也叫"高级流";比如`BufferedInputStream`

   ![preview](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-e1afd7f4862e6608cf4103fe5425e7a4_r.jpg)

   ![image-20210807135917047](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210807135917047.png)

   可以看出上面左边的流也就是我们平时主要用的流,叫"低级流",而右边处理流就是在基础的字节流上，进行了封装，增加了特定的功能，使得传输更适合特定的场景。

   最重要的还是 **缓冲流:**

   ​	我们知道，程序与磁盘的交互相对于内存运算是很慢的，容易成为程序的性能瓶颈。减少程序与磁盘的交互，是提升程序效率一种有效手段。缓冲流，就应用这种思路：普通流每次读写一个字节，而缓冲流在内存中设置一个缓存区，缓冲区先存储足够的待操作数据后，再与内存或磁盘进行交互。这样，在总数据量不变的情况下，通过提高每次交互的数据量，减少了交互次数

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-b06fea615630981d1ed47bc79c7d5e17_720w.jpg)

完整的IO分类图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-625373ac62037f328eb6e8b034f512c8_720w.jpg)

### 2.3 FileInputStream

操作文件的输入流:

构造方法:

```java
FileInputStream(String name); name:表示(绝对路径、相对路径)文件名
FileInputStream(File)
```



常用方法:

```java
------- 从头往尾执行的-----------------------------
1. int read();//从输入流中读取一个字节的二进制数据。
2.int read(byte[] b);//从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b中
3.int read(byte[] b, int off, int len);//从输入流中读取长度为 len 的数据，从数组 b 中下标为 off 的位置开始放置读入的数据，读完返回读取的字节数。
4.void close();//关闭此输入流并释放与该流关联的所有系统资源。
5.int available();//返回目前可以从数据流中读取的字节数（但实际的读操作所读得的字节数可能大于该返回值）
6.long skip(long l);//跳过数据流中指定数量的字节不读取，返回值表示实际跳过的字节数。
--------以反方向读取,则需要回推操作-----------------------
1.boolean markSupported(); //用于测试数据流是否支持回推操作，当一个数据流支持 mark() 和 reset() 方法时，返回 true，否则返回 false。
2.void mark(int readlimit);//用于标记数据流的当前位置，并划出一个缓冲区，其大小至少为指定参数的大小
3.void reset();//将输入流重新定位到对此流最后调用 mark() 方法时的位置。
```



常见的FileInputStream有很多子类如图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/bba1cd11728b4710312b3d5ec1018bfbfd032375.jpeg)

1. ByteArrayInputStream:字节数组输入流
   - 该类的功能就是从字节数组byte[]中进行以字节为单位的读取,也就是将资源文件都以字节形式存入到该类中的字节数组中去,我们拿数据也是从这个字节数组中拿。
2. PipedInputStream:管道字节输入流
   - 它和PipedOutputStream一起使用,能实现多线程之间的管道通信
3. FilterInputStream:
   - 装饰者模式中充当装饰者的角色,具体的装饰者都要继承它,所以在该类的子类下都是用来装饰别的流的,也就是处理类
4. BufferedInputStream:缓冲流
   - 对处理流进行装饰、增强,内部会有一个缓冲区,用来存放字节,每次都是将缓冲区存满然后发送,而不是一个字节或两个字节这样发送,效率更高
5. DataInputStream:数据输入流
   - 用来装饰其他输入流,它允许通过数据流来读写java基本数据类型
6. FileInputStream:文件输入流
   - 通常用于对文件进行读取操作
7. File:对指定目录的文件进行操作
8. ObjectInputStream:对象输入流
   - 用来提供对"基本数据或对象"的持久存储,通俗的来讲:就是能直接传输java对象(序列化、反序列化用)

示例:

第一个案例是一个一个读然后打印出来:

```java
import java.io.*;
public class TestFileImportStream {

	public static void main(String[] args) {
		int b=0;
		FileInputStream in = null;
		try {
		in =new FileInputStream("C:\\Users\\41639\\Desktop\\java\\FileText\\src\\TestFileImportStream.java");
		}catch(FileNotFoundException e){
			System.out.println("file is not found");
			System.exit(-1);
		}
		try {
			long num=0;
			while ((b=in.read())!=-1) {  //一次读取一个字节,返回-1表示读取结束
				System.out.println((char)b); //不转的话那么就会输出ASCII码,因为字符在底层存储的时候就是存储的数值
				num++;
			}
            if(in!=null){  //这里进行判空.这是因为避免空指针调用
                in.close(); //关闭流资源,因为这是内存与硬盘之间的交互,是非常消耗性能资源的
            }
			System.out.println();
			System.out.println("共读取了"+num+"个字节");
		}catch(IOException e) {
			System.out.println("IO异常，读取失败");
			System.exit(-1);
		}
	}
    //读取中文会乱码,乱码原因是在读取的时候一次性只能读取一个字节,但是中文不止一个字节,在UTF-8里的汉字占用3个字节,而在GBK里的汉字占用2个字节
```

- 可以看出用read只能一次性读取一个字节,而字节需要转字符需要转char字符对应ASCII码表

**注意:**声明代码每次读取一个字节数据，如果文件较大，那么性能极其低下。所以不用。

- 而用read([])则一次性读取数组大小的字节,

第二个则使用读取到指定数组里然后再打印出来:

```java
public class TestFileImportStream {
  public static void main(String[] args) {
    FileInputStream in = null;
    //建立连接
    try {
      in = new FileInputStream("F:\\studyMarkDown\\javaSE\\序列化与反序列化.md");
    } catch (FileNotFoundException e) {
      System.out.println("file is not Found");
      System.exit(-1);
    }

    //读取字节文件
    try {
      byte[] bytes = new byte[in.available()];//存储最大范围则根据获取到流的字节数
      int count;
        while (count=in.read(bytes)>0) {  // 一次读取一个字节数组,存放在缓冲区
        System.out.println(new String(bytes, StandardCharsets.UTF_8)); //将btye数组转为字符串
      	System.out.println("一次性读取"+count+"字节");
        }
      //关闭
        if(in!=null){
            in.close(); //必须关闭
        }
    } catch (Exception e) {
      System.out.println("IO异常,读取失败");
      System.exit(-1);
    }
  }
}
//这里使用in.available()一次性获取最大的字节数来获取,所以这里可以不用while来循环,因为只循环一次,
//所以可以一次性少读一点比如byte[] bytes = new byte[1024]大小,然后用while循环读取
```

> 一次读取一个字节数组，提高了操作效率,IO流使用完毕一定要关闭

试想一下如果数组大小采用固定,然后使用while循环,那么容易遇到一个问题,比如:

文本数据:abcide你好

```
byte[] bytes = new byte[1024],初始化固定长度,然后使用while循环in.read(bytes),每次读取指定数组大小的字节,然后第二次打印的话容易出现原本10个字节的空间,然后只获取了2个字节空间,也就是还剩下8个字节空间没用,造成浪费;所以"abcide你好"第一次打印:abcide",然后第二次打印"你好de";看吧第二次循环就把de打印了这明显上一次的打印内容,
```

来看图解:

![Snipaste_2021-08-09_10-51-59](https://gitee.com/miawei/pic-go-img/raw/master/imgs/Snipaste_2021-08-09_10-51-59.png)

**结论:**每次循环读取一个数组大小那么就会覆盖之前的数组,所以会造成有些数组的元素还是之前的,

所以解决办法就是每次打印的时候:

```java
String str=new String(b,0,count);//将每次读取的 count个字节数组b中数据转换为 String
System.out,println(str);
//也可以用这样显式的指定字符集
 System.out.println(new String(bytes, 0,count,StandardCharsets.UTF_8));   //解码
```

这里推荐使用这种指定数组大小的方式,最好不要用[available()](返回从此输入流中可以读取（或跳过）的剩余字节数的估计值，而不会被下一次调用此输入流的方法阻塞)。这个方法只是获取输入流中的字节的估计值,注意这是估计值,所以有的时候存在不确定性,所以这里建议采用数组固定大小,一般推荐1024,然后无论读取打印还是利用输出流输出到另一个文件中的话,只需要每次读取获取的字节大小带进特定的方法就可以了,比如打印`System.out.println(new String(bytes, 0,count,StandardCharsets.UTF_8))`,获取bytes数组然后从0开始,总共读取count个字节,然后指定字符集;如果是输出流,那么也可以用`void write(byte[] b, int off, int len);`也可以指定每次读取字节的大小



**注意:**程序中打开的文件IO资源不属于内存里的资源,垃圾回收机制无法回收资源,所以应该显式关闭IO资源

### 2.4 FileOutputStream

字节流基类是OutputStream的基类,这是文件字节流

构造方法:

```java
// 创建文件输出流以写入由指定的 File对象表示的文件。
    FileOutputStream(File file)
// 创建文件输出流以写入由指定的 File对象表示的文件。append表示是否追加末尾
    FileOutputStream(File file, boolean append)
// 创建文件输出流以指定的名称写入文件。
    FileOutputStream(String name)
// 创建文件输出流以指定的名称写入文件.append表示是否追加末尾
    FileOutputStream(String name, boolean append)
```

常用方法:

```java
1. void write(byte[] b);//将 b.length 个字节从指定的 byte 数组写入此输出流
2.void write(byte[] b, int off, int len);//将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流
3.abstract void write(int b);// 将指定的字节写入此输出流
4.void close();//关闭此输出流并释放与此流有关的所有系统资源
5.void flush();// 刷新此输出流并强制写出所有缓冲的输出字节
```

这里的刷新缓冲flush()方法:

​	为了加快数据传输速度，提高数据输出效率，又是输出数据流会在提交数据之前把所要输出的数据先暂时保存在内存缓冲区中，然后成批进行输出，每次传输过程都以某特定数据长度为单位进行传输，在这种方式下，数据的末尾一般都会有一部分数据由于数量不够一个批次，而存留在缓冲区里，调用 flush() 方法可以将这部分数据强制提交。

如图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/e1fe9925bc315c60aed5e98f8f7e831548547767.jpeg)

再循环write()这里只是将数据写到内存的缓冲区,只有缓冲区满了,才能将这些数据写出去

> 刷新方法作用:将内存区缓存的字符数据全部写出到硬盘【关流前,会自动调用flush刷新方法】

试想这里内存中的缓存区到底多大呢?才会满了才去写

```java
public void write(String str, int off, int len) throws IOException {
    synchronized (lock) {
        char cbuf[];
        if (len <= WRITE_BUFFER_SIZE) {
            if (writeBuffer == null) {
                writeBuffer = new char[WRITE_BUFFER_SIZE];
            }
            cbuf = writeBuffer;
        } else {    // Don't permanently allocate very large buffers.
            cbuf = new char[len];
        }
        str.getChars(off, (off + len), cbuf, 0);
        write(cbuf, 0, len);
    }
}
```

**结论:**首先缓存区也就是char数组,然后在writer的时候,会判断len字节大小,如果没有那么默认为1024大小,如果len不为空那么就设置为缓存区大小



示例:

```java
public class TestFileWriteDemo {
  public static void main(String[] args) {
    TestFileWriteDemo writeDemo = new TestFileWriteDemo();
    	writeDemo.writeFile("testFile\\abc.txt");
  }

  /**
   * 写文件
   *
   * @param filePath 文件路径
   */
  public void writeFile(String filePath) {
    OutputStream os = null;
    try {
      os = new FileOutputStream(filePath,true);//这里写true表明每次对文件写入不会覆盖原本文件的内容而只是进行在末尾追加
      // 写文件
      os.write(65);
      os.write('A');
      os.write('B');
      os.write("缪威".getBytes(StandardCharsets.UTF_8)); //将字符串通过getBytes进行转为byte数组进行输出 ,也叫编码
      os.write("\r\n".getBytes()); //输出换行符  
    } catch (Exception e) {
      System.out.println("e.getMessage() = " + e.getMessage());
    } finally {
      if (os != null) {
        try {
          os.close();
        } catch (Exception e) {
          System.out.println("e.getMessage() = " + e.getMessage());
        }
      }
    }
  }
}
如果想写字符串之类的,那么得将字符串进行转换为byte数组然后还要进行设置字符集
```

演示一下输入和输出的综合案例:也叫复制文件

```java
public class CopyFiles {
  public static void main(String[] args) {
    CopyFiles copyFiles = new CopyFiles();
    boolean file =
        copyFiles.copyFile(
            "C:\\Users\\MiaoDaWei\\Desktop\\day019-File和递归 笔记.txt",
            "C:\\Users\\MiaoDaWei\\Desktop\\复制过后的文件.txt");
    System.out.println(file ? "复制成功" : "复制失败");
  }

  /**
   * 复制文件
   *
   * @param oldFile 老文件
   * @param newFile 新文件
   * @return boolean
   */
  private boolean copyFile(String oldFile, String newFile) {
    boolean flag;
    InputStream in = null;
    OutputStream on = null;

    try {
      // 建立连接
      in = new FileInputStream(oldFile);
      on = new FileOutputStream(newFile,true); //这里加true,表示写入文件不会覆盖,直接追加末尾,如果是false,那么程序就是清空文件,等待所有操作完毕再进行复制操作,而不是循环一次													写入文件一次

      // 读取操作  
      byte[] buffer = new byte[in.available()]; //定义一个输入流读取的字节数的数组
      int read = in.read(buffer);//读取到指定数组中,返回读取字节个数
        
      // 写入操作
      on.write(buffer, 0, read); //开始写入,将指定数组中的0下标开始读,指定读取字节个数
      on.flush(); //刷新缓冲区,将还没有写入文件的进行强制提交
        
      //另一种写法:
      byte[] buffer = new byte[1024];
      int read;
      while((read=in.read(buffer))!=-1) {
          on.write(buffer,0,read);
      } 
      on.flush(); //刷新缓存
        
        
        
      flag = true;
    } catch (Exception e) {
      return false;
    } finally {
      try {
          //关闭流
        if (in != null) {
          in.close();
        }
        if (on != null) {
          on.close();
        }
      } catch (IOException e) {
        System.out.println("失败");
      }
    }

    return flag;
  }
}
我认为这里的移动文件也是一个道理,复制过后进行删除原来的文件利用File的delete
```

**注意:**

1. 在实际的项目中，所有的IO操作都应该放到子线程中操作，避免堵住主线程。
2. `FileInputStream`在读取文件内容的时候，我们传入文件的路径（`"D:/abc.txt"`）, 如果这个路径下的文件不存在，那么在执行`read()`方法时会报`FileNotFoundException`异常。
3. `FileOutputStream`在写入文件的时候，我们传入文件的路径（`"D:/abc.txt"`）, 如果这个路径下的文件不存在，那么在执行`write()`方法时, 会默认给我们创建一个新的文件。还有重要的一点，不会报异常

> 输出的目的地文件不存在，则会自动创建，不指定盘符的话，默认创建在项目目录下;
>
> 输出换行符时一定要写\r\n不能只写\n,因为不同文本编辑器对换行符的识别存在差异性

### 关于读写乱码问题-字符编码

概念:产生的原因无非就是编码和解码规则不一致

1. GBK:
   - 中文2个字节
   - 英文、数字：1字节
2. UTF-8:
   - 中文3~6个字节
   - 英文、数字：1字节

编码:将字符串转换为字节

```java
byte[] getBytes() 使用平台的默认字符集将此 String 编码为 byte 序列 
byte[] getBytes(Charset charset) 使用指定的字符编码来编码字符串
byte[] getBytes(String charsetName) 使用指定的字符编码来编码字符串
```

解码:将字节转换为字符

```java
String(byte[] bytes) 通过使用平台的默认字符集解码指定的 byte 数组
String(byte[] bytes, Charset charset) 使用指定的字符集来解码指定的byte数组
String(byte[] bytes, String charsetName) 使用指定的字符集来解码指定的byte数组
```

乱码产生的问题:

1. 因为字符集不统一，即编码和解码new String(b,0,read,"gbk")字符集不一致
2. 因为字节流读取汉字的时候，字节数组长度不够，将一个汉字拆开了



常见的字符集编码

1. ASCII码:用来表示英文，它使用1个字节表示，其中第一位规定为0，其他7位存储数据，一共可以表示128个字符
2. 拓展ASCII码:用于表示更多的欧洲文字，用8个位存储数据，一共可以表示256个字符
3. GBK/GB2312/GB18030:简称国标，表示汉字。GB2312表示简体中文，GBK/GB18030表示繁体中文,其实就是几个不同的版本而已。
4. Unicode:包含世界上所有的字符，是一个字符集
5. UTF-8:是Unicode字符的实现方式之一，它使用1-4个字符表示一个符号，根据不同的符号而变化字节长度
6. ISO8859-1:是单字节编码，向下兼容ASCII,**不支持中文**！

> 实际web开发  前台网页  后台Java程序  数据库  多个地方都会涉及到字符编码-> 一定要一致

注意:关流必须在finally中,关流原则:先开后关,在关流必须判断是否为null,避免空指针异常

### 2.5 缓冲流(高效流)

所谓的缓冲字节流其实就是在`FileInputStream`和`FileOutputStream`进行设计的,其目的就是为了提高效率而开发

**概念:**为了提高数据读写的速度,JavaAPI提供了带缓冲功能的流类,在使用这些流类时,会创建一个内部缓冲区的数组,而缓冲流要"套接"在相应的节点流之上,对读写的数据提供了缓冲的功能,提高了读写的效率,同时增加了一些新的方法,提高读取和写出效率的流【指的是读取和写出单个字节、字符】,缓冲流可以使用标记和回读方法，缓冲流还有预读功能。

**说明:**如果不设置缓冲的话那么CPU取一个存一个,缓冲区就是内存里的一块区域，把数据先存内存里，然后一次性写入，类似数据库的批量操作，显然效率比较高。还节省CPU的使用

**总结:**使用缓冲处理流包装就是一堆一堆的干活，还能不用CPU多次处理数据转换，只是设置一下数据转换成功后的文件,不使用缓冲处理流包装就是CPU傻傻的一个字节一个字节循环来干活存储写入文件中，相比可见效率明显变慢

**使用场景:**适用于大文件的上传和下载，自己不创建数组的场景。如果处理文本数据，建议使用字符缓冲流。

**作用:**标记和回读,如果只是复制文件，建议使用字节流，然后用自己创建大的数组方式完成。

> 对于输出的缓冲流,写出的数会现在内存中缓存.使用flush()将会使内存中的数据立刻写出

缓冲流就分为:

- 缓存字节流:

  比如:BufferedInputStream、BufferedOutputStream

- 缓存字符流:

  比如:BufferedReader、BufferedWriter

这里先介绍缓冲字节流:

构造方法:

```java
1.BufferedInputStream:
	// 创建一个 BufferedInputStream并保存其参数，即输入流in，以便将来使用
	BufferedInputStream(InputStream in)
    // 创建具有指定缓冲区大小的 BufferedInputStream并保存其参数，即输入流in以便将来使用
    BufferedInputStream(InputStream in, int size)
2.BufferedOutputStream
    // 创建一个新的缓冲输出流，以将数据写入指定的底层输出流
     BufferedOutputStream(OutputStream out)
     // 创建一个新的缓冲输出流，以将具有指定缓冲区大小的数据写入指定的底层输出流
     BufferedOutputStream(OutputStream out, int size)    
```

> 缓冲字节流是为高效率而设计的，真正的读写操作还是靠`FileOutputStream`和`FileInputStream`，所以其构造方法入参是这两个类的对象也就不奇怪了

示例:

```java
public class BufferedStreamDemo {
  public static void main(String[] args) {
        BufferedStreamDemo copyFiles = new BufferedStreamDemo();
    boolean file =
        copyFiles.copyFile(
            "F:\\源码时代\\开班课件\\2021-07-10 课件\\2021-06-05 java基础进阶-IO\\resource\\test.jpg",
            "C:\\Users\\MiaoDaWei\\Desktop\\test.jpg");
        System.out.println(file ? "复制成功" : "复制失败");
  }

  /**
   * 复制文件
   *
   * @param oldFile 老文件
   * @param newFile 新文件
   * @return boolean
   */
  private boolean copyFile(String oldFile, String newFile) {
    boolean flag=false;
    // 定义缓存流
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;

    try {
      // 定义输入输出流
      FileInputStream inputStream = new FileInputStream(oldFile);
      FileOutputStream outputStream = new FileOutputStream(newFile);

      // 定义缓存流,缓冲字节流，提高了效率
      bis = new BufferedInputStream(inputStream);//先缓冲,再读取
      bos = new BufferedOutputStream(outputStream);//先缓冲,再写入

      // 定义缓存数组,
      byte[] bytes = new byte[bis.available()];
      int off = 0;

      // 输入操作
      off = bis.read(bytes);
      // 输出操作
      bos.write(bytes, 0, off);

      //关闭流
      bis.close();
      bos.close();
      flag = true;
    } catch (IOException e) {
      System.out.println("发生异常原因:"+e.getMessage());
      return false;
    }

    return flag;
  }
}
```

字符缓冲流(高效流)

1. BufferedReader

   BufferedReader：字符缓冲流，从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。

   构造方法:

   ```java
    // 创建一个使用默认大小输入缓冲区的缓冲字符输入流
       BufferedReader(Reader in)
       // 创建一个使用指定大小输入缓冲区的缓冲字符输入流
       BufferedReader(Reader in, int sz)
   ```

   特有方法:

   ```java
    // 读取一个文本行
       String readLine()
   ```

   案例:

   ```java
   //生成字符缓冲流对象
           BufferedReader reader = new BufferedReader(new InputStreamReader(new FileInputStream("test.txt")));
           String str;
           //一次性读取一行
           while ((str = reader.readLine()) != null) {
               System.out.println(str);// 爱生活，爱Android
           }
   
           //关闭流
           reader.close();
   ```

2. BufferedWriter

   BufferedWriter：字符缓冲流，将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。

   构造方法

   ```java
   // 创建一个使用默认大小输出缓冲区的缓冲字符输出流
       BufferedWriter(Writer out)
       // 创建一个使用给定大小输出缓冲区的新缓冲字符输出流
       BufferedWriter(Writer out, int sz)
   ```

   特有方法:

   ```java
   // 写入一个行分隔符
       void newLine() 
   ```

字符缓冲流的完整例子:

```java
public class IOTest {
	
	public static void write(File file) throws IOException {
		// BufferedWriter fw = new BufferedWriter(new OutputStreamWriter(new
		// FileOutputStream(file, true), "UTF-8"));
		// FileWriter可以大幅度简化代码
		BufferedWriter bw = new BufferedWriter(new FileWriter(file, true));

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		bw.write(string);
		bw.close();
	}

	public static String read(File file) throws IOException {
		BufferedReader br = new BufferedReader(new FileReader(file));
		// 用来接收读取的字节数组
		StringBuilder sb = new StringBuilder();

		// 按行读数据
		String line;
		// 循环取数据
		while ((line = br.readLine()) != null) {
			// 将读取的内容转换成字符串
			sb.append(line);
		}
		// 关闭流
		br.close();

		return sb.toString();
	}
}
```





### 2.6 字符流(转换流)

**概念:**转换流提供了在字节流和字符流之间的转换

**注意:**目前唯一一个可以设置字符集的流

**使用场景:**别人给提供了字节流，而需要处理文本数据，这时候，就需要用转换流转换一下，更方便处理文本数据

**作用:**就是以字符流的方式读取或写出文本相关的数据。

JavaAPI提供了两个转换流:

- InputStreamReader
- OutputStreamWriter

> 字节流中的数据都是字符时,转成字符流操作更高效

**字符流适用于文本文件的读写**，`OutputStreamWriter`类其实也是借助`FileOutputStream`类实现的，故其构造方法是`FileOutputStream`的对象

示例:

```java
public class InputStreamReaderDemo {
  public static void main(String[] args) throws IOException {
    write(new File("C:\\Users\\MiaoDaWei\\Desktop\\test.txt"));
    String read = read(new File("C:\\Users\\MiaoDaWei\\Desktop\\test.txt"));
    System.out.println("read = " + read);
  }

  public static void write(File file) throws IOException {
    // OutputStreamWriter可以显示指定字符集，否则使用默认字符集
    OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(file, true), "UTF-8");

    // 要写入的字符串
    String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
    osw.write(string);
    osw.close();
  }

  public static String read(File file) throws IOException {
    InputStreamReader isr = new InputStreamReader(new FileInputStream(file), "UTF-8");
    // 字符数组：一次读取多少个字符
    char[] chars = new char[1024];
    // 每次读取的字符数组先append到StringBuilder中
    StringBuilder sb = new StringBuilder();
    // 读取到的字符数组长度，为-1时表示没有数据
    int length;
    // 循环取数据
    while ((length = isr.read(chars)) != -1) {
      // 将读取的内容转换成字符串
      sb.append(chars, 0, length);
    }
    // 关闭流
    isr.close();

    return sb.toString();
  }
}

```



转换图解:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/1363376-20201118224838460-266023909.png)







1. InputStreamReader

   用于将字节流中读取到的**字节按照指定字符集解码成字符**,需要和"InputStream"进行套接

   构造方法:

   ```java
     // 创建一个使用默认字符集的 InputStreamReader
   InputStreamReader(InputStream in)
       // 创建使用给定字符集的 InputStreamReader
   InputStreamReader(InputStream in, Charset cs)
       // 创建使用给定字符集解码器的 InputStreamReader
   InputStreamReader(InputStream in, CharsetDecoder dec)
       // 创建使用指定字符集的 InputStreamReader
   InputStreamReader(InputStream in, String charsetName)
   ```

   特有方法:

   ```java
   //返回此流使用的字符编码的名称 
       String getEncoding() 
   ```

   普通方法:

   ```java
   public int read(char[] cbuf);
   int read(char[] cbuf, int offset, int length)  
   ```

   案例:

   ```java
   //使用默认编码        
           InputStreamReader reader = new InputStreamReader(new FileInputStream("test.txt"));
           int len;
           while ((len = reader.read()) != -1) {
               System.out.print((char) len);//爱生活，爱JAVA
   
           }
           reader.close();
   ```

2. OutputstreamWriter

   字节流转字符流,将字节输出流包装一下,让其更适合写出文本文件

   构造方法:

   ```java
   // 创建使用默认字符编码的 OutputStreamWriter
       OutputStreamWriter(OutputStream out)
       // 创建使用给定字符集的 OutputStreamWriter
       OutputStreamWriter(OutputStream out, Charset cs)
       // 创建使用给定字符集编码器的 OutputStreamWriter
       OutputStreamWriter(OutputStream out, CharsetEncoder enc)
       // 创建使用指定字符集的 OutputStreamWriter
       OutputStreamWriter(OutputStream out, String charsetName)
   ```

   特有方法:

   ```java
   //返回此流使用的字符编码的名称 
       String getEncoding() 
   ```

   普通方法:
   
   ```java
   void write(char[] cbuf, int off, int len);  
   ```
   
   

> 理解:字节流转字符流应用场景就在于转换String类型的时候会指定字符集编码



字符便捷类:

```java
FileReader：InputStreamReader类的直接子类，用来读取字符文件的便捷类，使用默认字符编码。
FileWriter：OutputStreamWriter类的直接子类，用来写入字符文件的便捷类，使用默认字符编码。
```

意思就是就是简化了InputStreamReader与InputStreamWriter的使用:

```java
public class IOTest {
	
	public static void write(File file) throws IOException {
		FileWriter fw = new FileWriter(file, true);

		// 要写入的字符串
		String string = "松下问童子，言师采药去。只在此山中，云深不知处。";
		fw.write(string);
		fw.close();
	}

	public static String read(File file) throws IOException {
		FileReader fr = new FileReader(file);
		// 一次性取多少个字节
		char[] chars = new char[1024];
		// 用来接收读取的字节数组
		StringBuilder sb = new StringBuilder();
		// 读取到的字节数组长度，为-1时表示没有数据
		int length;
		// 循环取数据
		while ((length = fr.read(chars)) != -1) {
			// 将读取的内容转换成字符串
			sb.append(chars, 0, length);
		}
		// 关闭流
		fr.close();

		return sb.toString();
	}
}
```

​	

### 2.7 对象流

**分类:**

- ObjectInputStream
- ObjectOutStream

**作用:**用于存储和读取对象的处理流。它的强大之处就是把java中的对象写入到数据源中，也能把对象从数据源中还原回来

> 所谓的对象流就是把类和对象写入文件中进行对象传输

**序列化：**用ObjectOutputStream类将一个java对象写入IO流

**反序列化:**用ObjectInputStream类从IO流中恢复该java对象

注意:ObjectOutputStream和ObjectInputStream不能序列化 **static**和 **transient**修饰的成员变量

对象流其实就是一种特殊的处理流水，也是在基础的字节流上去作封装。【可以应用于游戏存盘】

```java
import java.io.*;
public class ObjectStreamTest {
	public static void main(String[] args) throws Exception{
		try {
			Person P=new Person("Jeccica",26);
			FileOutputStream fos=new FileOutputStream("C:\\Users\\admin\\Desktop\\Java\\temp\\22.txt");
			ObjectOutputStream oos=new ObjectOutputStream(fos);
			oos.writeObject(P);
			oos.flush();
			oos.close();
		}catch(FileNotFoundException e) {
			e.printStackTrace();
		}catch(IOException e) {
			e.printStackTrace();
		}			
		
		FileInputStream fis=new FileInputStream("C:\\Users\\admin\\Desktop\\Java\\temp\\22.txt");
		ObjectInputStream ois=new ObjectInputStream(fis);
		Person P2=(Person)ois.readObject();
		System.out.println(P2.name+"的年龄为"+P2.age);
	}

}

class Person implements Serializable{
	String name=null;
	int age=0;
	Person(String _name,int _age){
		name=_name;
		age=_age;
	}
}
```



### 2.8 高效流效率对比

现在就有题目:读取f盘下的一个视频文件到项目中：文件大小29.5 MB

读取方式一:采用字节流每读取一个字节就向输出流写入

```java
  FileInputStream inputStream = new FileInputStream("f://滑板//HEEL_FLIP.mp4");
        FileOutputStream outputStream = new FileOutputStream("HEEL_FLIP.mp4");
        int len;
        // 开始时间
        long begin = System.currentTimeMillis();
        // 一次读取一个字节
        while ((len = inputStream.read()) != -1) {
            outputStream.write(len);
        }
        // 用时毫秒
        System.out.println(System.currentTimeMillis() - begin);// 213195
        //关闭流释放资源
        inputStream.close();
        outputStream.close();
```

读取方式二:采用字节流流读取到字节数组中然后一次性向输入流写入

```java
  FileInputStream inputStream = new FileInputStream("f://滑板//HEEL_FLIP.mp4");
        FileOutputStream outputStream = new FileOutputStream("HEEL_FLIP.mp4");
        int len;
        byte[] bs = new byte[1024];
        // 开始时间
        long begin = System.currentTimeMillis();
        // 一次读取一个字节数组
        while ((len = inputStream.read(bs)) != -1) {
            outputStream.write(bs, 0, len);
        }
        // 用时毫秒
        System.out.println(System.currentTimeMillis() - begin);// 281

        inputStream.close();
        outputStream.close();
```

读取方式三:使用缓冲流进行方式二的读取写入

```java
  FileInputStream inputStream = new FileInputStream("f://滑板//HEEL_FLIP.mp4");
        BufferedInputStream bis = new BufferedInputStream(inputStream);
        FileOutputStream outputStream = new FileOutputStream("HEEL_FLIP.mp4");
        BufferedOutputStream bos = new BufferedOutputStream(outputStream);
        int len;
        byte[] bs = new byte[1024];
        // 开始时间
        long begin = System.currentTimeMillis();
        while ((len = bis.read(bs)) != -1) {
            bos.write(bs, 0, len);
        }
        // 用时毫秒
        System.out.println(System.currentTimeMillis() - begin);// 78

        bis.close();
        bos.close();
```



经过测试发现所耗时最少的就是缓冲流,这样节省了时间还提高了效率

> 由此可以看出高效缓冲流读写速度是非常快的，建议使用。



### 2.9数据流

**概念:**为了方便操作Java语言的基本数据类型的数据,可以使用数据流

**分类:**用于读取和写出基本数据类型的数据

- DataInputStream和DataOutputStream
- 分别"套接"在InputStream和OutputStream节点流上

DataInputStream中的方法:

```java
boolean readBoolean();
char readChar();
double readDouble();
long readLong();
String readUTF();
byte readByte();
float readFloat();
short readShort();
int readInt();
void readFully(byte[] b)
```

示例:

```java
@Test
    public void test4() throws Exception {//数据流：用来处理基本数据类型、String、字节数组的数据:DataInputStream DataOutputStream
        DataOutputStream dataOutputStream = new DataOutputStream(new FileOutputStream("data.txt"));
        dataOutputStream.writeUTF("我爱你，而你不知道");
        dataOutputStream.writeBoolean(true);
        dataOutputStream.writeInt(12);
        dataOutputStream.close();//会乱码，但是再次读的时候会恢复
        DataInputStream dataInputStream = new DataInputStream(new FileInputStream(new File("data.txt")));
//        byte[] b =new byte[10];
//        int len;
//        while((len = dataInputStream.read(b))!=-1){
//            System.out.println(new String(b,0,len));
//        }
        System.out.println(dataInputStream.readUTF());
        System.out.println(dataInputStream.readBoolean());
        System.out.println(dataInputStream.readInt());
        dataInputStream.close();
    }
```



### 字符流

**概念:**使用字节流来操作中文的时候非常不方便,容易产生乱码,他不管你是哪种编码方式都按照一个字符一个字符的方式读取数据

字符流用于处理文本相关的数据

```java
基类:Reader抽象类
	子类:InputStreamReader
	子类:FileReader：适用于读取字符相关的文件
		构造方法:
			1.FileReader(File file)
			2.FileReader(String fileName)	fileName：文件名（相对路径/绝对路径）
		读取方法:
			1.int read(char[] c)读取字符到char数组中		最常用
			2.int read()读取一个字符
			3.int read(char[] c,int start,int length)读取制定长度的字符到char数组中
基类:Writer 抽象类
    子类:OutputStreamWriter（输出转换流）
    子类:FileWriter：适用于写出字符相关的文件
        构造方法:
			1.FileWriter(File file) 	默认是false
            2.FileWriter(File file, boolean append)
                append：表示是在文件末尾追加还是从头覆盖，如果true追加，false覆盖，默认是false
            3.FileWriter(String fileName) 	默认是false
            4.FileWriter(String fileName, boolean append)
                fileName：文件名（相对路径/绝对路径）
         读写方法:
			// append方法底层就是write()方法。可以写出字符串序列【StringBuffer、StringBuilder、String】可以连续调用append方法；append().append()
            1.public Writer append(CharSequence csq, int start, int end) ;//表示追加的意思,每次循环一次追加一次
            2.public Writer append(char c)
            3.public Writer append(CharSequence csq)
            4.public void write(char[] c)
            5.public void write(String str)
            6.public void write(String str,int start,int end)    
```

**分类:**

1. FileReader

   其中的构造方法和方法和字节流输入流FileInputStream非常类似,差别就是单位是char

   ```java
   Reader r = new FileReader("D:/123.txt");
   char[] cbuf = new char[3];
   int len;
   while((len = r.read(cbuf))!=-1){
   	System.out.println(new String(cbuf,0,len));
   }
   r.close();
   ```

   

2. FileWriter

   特有的方法: 可以直接写字符串

   ```java
   void write(String str)  写入字符串。 
   void write(String str, int off, int len) 写入字符串的一部分
   ```

   

综合案例:

```java
/**
 * @program: YmsdLearn
 * @description: 字符流
 * @author: MiaoWei
 * @create: 2021-08-09 14:58
 */
public class ReaderWriterDemoTest {
  public static void main(String[] args) {
    // 字符流
    ReaderWriterDemoTest demoTest = new ReaderWriterDemoTest();
    File readFile = new File("C:\\Users\\MiaoDaWei\\Desktop\\day020-io笔记.txt");
    File writerFile = new File("C:\\Users\\MiaoDaWei\\Desktop\\day020-io笔记2.txt");
    demoTest.readWriter(readFile,writerFile);
  }

  public void readWriter(File readFile, File writerFile) {
    // 校验文件是否存在,写的文件不需要进行检查,因为不存在就会自动创建
    if (!readFile.exists()) {
      throw new RuntimeException("读写文件不存在,请重新检查再试");
    }

    FileReader reader = null;
    FileWriter writer = null;

    try {
      // 创建字符对象
      reader = new FileReader(readFile);
      writer = new FileWriter(writerFile);

      // 开始读-写
      char[] chars = new char[1024];
      int count;
      while ((count = reader.read(chars)) != -1) {
        writer.write(chars, 0, count);
        writer.append("\r\n").append("这是append追加方法!!!!").append("\r\n");
      }

      //刷新缓存区数据,强制提交
      writer.flush();
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 关流
      try {
        if (reader != null) {
          reader.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }

      // 关流
      try {
        if (writer != null) {
          writer.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
    System.out.println("读写成功!");
  }
}
```



### 关流新方式

jdk7关流的新方式：

用新结构可以自动关流
前提：该流必须是实现了一个接口	`AutoCloseable`

语法:

```java
try (
			// 只能写创建流的操作，不能写其他业务操作【eclipse可能自动提示失效】
		) {
			// 可能有异常的代码或者业务代码
		} catch (异常类型1 e) {
			
		} ...不需要finally关流了
```



### 流的使用场景推荐:

1. 处理**文本**相关数据用**字符流和缓冲流**【Reader、Writer】 -> FileReader、FileWriter
2. 处理**非文本**相关数据，例如：复制各种类型(txt、avi、mp3、exe等)文件 用**字节流**【InputStream、OutputStream】 -> FileInputStream、FileOutputStream
3. 如果别人提供了字节流，但是要处理文本相关数据，则用转换流【InputStreamReader、OutputStreamWriter】，再使用缓冲流处理。
4. 只要**标记和回读**，都要用对应【字节、字符】**缓冲流**
5. 如果要多线程下载或者上传文件、断点续传，用RandomAccessFile类【扩展】

# BIO、NIO、AIO

知识点:

1. 同步与异步: 
   - **同步**是一种可靠的有序运行机制,当我们进行同步操作时,后续的任务是等待当前调用返回,才会进行下一步;
   - **异步**跟同步相反,其他任务不需要等待当前调用返回,通常依靠、回调等机制来实现任务间次序关系
2. 阻塞与非阻塞:
   - **阻塞**是在进行阻塞操作时,当前线程会进入阻塞状态,无法从事其他任务,只有当条件就绪才能继续,比如ServerSocket新连接建立完毕,或者数据读取、写入操作完成;
   - **非阻塞**则是不管IO操作是否结束,直接返回,相应操作在后台继续处理

拿到IO中来讲:

- 同步与异步就是**实际的IO操作**
  - 同步是用户线程发起IO请求后需要等待或者轮询内核IO操作完成后才能继续执行
  - 异步是用户线程发起IO请求后仍需要继续执行,当内核IO操作完成后通知用户线程,或者调用用户线程注册的回调函数（异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知（异步的特点就是通知，Java将IO读写委托给OS处理，需要将数据缓冲区地址和大小传给OS）
- 阻塞与非阻塞就是**发起IO请求**
  - 阻塞是指IO操作需要彻底完成后才能返回用户空间（当试图对该文件描述符进行读写时, 如果当时没有东西可读,或者暂时不可写, 程序就进入等待 状态, 直到有东西可读或者可写为止）
  - 非阻塞是指IO操作被调用后立即返回一个状态值,无需等IO操作彻底完成（如果没有东西可读, 或者不可写, 读写函数马上返回, 而不会等待）

**理解：**

1. 阻塞：强迫症，必须这件事做完才能做其他事情，否则一直等待
2. 非阻塞：先取号，可以去做其他事情，叫号后，过来完成该事情
3. 同步：亲力亲为且专注于一件事
4. 异步：指示别人帮自己做事，完成后得到通知

## 1.概述:

所谓的IO流它是基于流模型实现的,交互方式是**同步**、**阻塞**的方式[解释](在读取输入流或者写入输出流时,在读、写工作完成之前，线程会一直阻塞在那里，它们之间的调用是可靠的线性顺序)

IO流的优缺点：

- 优点：代码简单、直观
- 缺点：IO效率和扩展性存在局限性，容易成为应用性能的瓶颈

```
每个流的交互方式:
IO:同步阻塞
BIO:同步阻塞
NIO:同步非阻塞
AIO:异步非阻塞
```

## 2.IO模型的知识

1. JAVA BIO:同步并阻塞

   ```
   这是jdk1.4之前都是采用的BIO模式,首先服务器实现模式为一个连接一个线程。即客户端有连接请求时服务器端就需要启动一个线程并处理，如果这个连接不做任何事情会造成不必要的开销，当然可以通过线程池改善。
   ```

2. JAVA NIO：同步非阻塞

   ```
   服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有IO请求时才启动一个线程进行处理
   ```

3. JAVA AIO：异步非阻塞

   ```
   服务器实现模式为一个有效请求一个线程，客户端的IO请求都是由OS先完成了再通知服务器应用启用启动线程进行处理；
   ```

使用场景：

1. BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，jdk1.4以前的唯一选择，让程序直观简单理解
2. NIO方式适用于连接数目比较多且连接短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，jdk1.4开始支持
3. AIO方式适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，jdk7开始支持

> BIO 是面向流的，NIO 是面向缓冲区的；BIO 的各种流是阻塞的。而 NIO 是非阻塞的；BIO的 Stream 是单向的，而 NIO 的 channel 是双向的。

## 3.三者比较

BIO、NIO、AIO三者比较：

| 名称 |      说明      |                           应用场景                           |                  补充                  |        优点        |                 缺点                 |
| :--: | :------------: | :----------------------------------------------------------: | :------------------------------------: | :----------------: | :----------------------------------: |
| BIO  |   同步阻塞IO   |          Apache、Tomcat。主要是并发量要求不高的场景          |       连接数目比较小且固定的架构       | 模型简单、编码简单 |       性能瓶颈低、不适合高并发       |
| NIO  |  同步非阻塞IO  |            Nginx、Netty。主要是高并发量要求的场景            | 连接数目多且连接比较短（轻操作）的架构 |     性能瓶颈高     | 模型复杂、编码复杂、需要处理半包问题 |
| AIO  | 非同步非阻塞IO | 还不是特别成熟，底层也基本是多线程模拟，所以应用场景不多，Netty曾经用了,但又放弃了 |  连接数目多且连接比较长(重操作)的架构  |                    |                                      |

这样去理解:假设有这么一个场景，有一排水壶（客户）在烧水。

- AIO的做法是，每个水壶上装一个开关，当水开了以后会提醒对应的线程去处理。
- NIO的做法是，叫一个线程不停的循环观察每一个水壶，根据每个水壶当前的状态去处理。
- BIO的做法是，叫一个线程停留在一个水壶那，直到这个水壶烧开，才去处理下一个水壶。

> 得出:可以看出AIO是最聪明省力，NIO相对省力，叫一个人就能看所有的壶，BIO最愚蠢，劳动力低下。



进程中的IO调用步骤:

1. 进程向操作系统请求数据 ;
2. 操作系统把外部数据加载到内核的缓冲区中; 
3. 操作系统把内核的缓冲区拷贝到进程的缓冲区 ;
4. 进程获得数据完成自己的功能 ;

当操作系统在把外部数据放到进程缓冲区的这段时间（即上述的第二，三步），如果应用进程是挂起等待的，那么就是同步IO，反之，就是异步IO，也就是AIO 。



**Nio与Aio的区别？**

AIO是发出IO请求后，由操作系统自己去获取IO权限并进行IO操作；NIO则是发出IO请求后，由线程不断尝试获取IO权限，获取到后通知应用程序自己进行IO操作。

- AIO只是帮助你从内核中将数据复制到用户空间中，并调用你传入的回调方法。 
- NIO 是需要程序自己从内核中将数据复制到用户空间中，并需要程序自己调用相应的处理逻辑。

**Nio线程处理流程**

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/d87a237af3ba4080f598a536e5dcede2.png)

