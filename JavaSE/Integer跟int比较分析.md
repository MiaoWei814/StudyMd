 

# Integer跟int比较分析

前几天在做项目过程中对数值类型的比较上让我有很大的兴趣去探究一下,毕竟稍不注意就会容易犯错。

**int是基本类型、Integer是包装类型。基本类型不能为null而包装类型可以为null。**

自动装箱:Integer.valueOf(); 自动拆箱:Integer.intValue();

直接看demo：
```java
public class Test {
public static void main(String[] args) {
	
	Integer a1 = 6;
	Integer a2 = 6;
	int a11 = 6;
	
	System.out.println(a1 == a2); //true
	System.out.println(a1 == a11); //true
	
	System.out.println("----------------");
	
	Integer a3 = 128;
	Integer a4 = 128;
	int a33 = 128;
	
	System.out.println(a3 == a4); //false
	//Integer会自动拆箱为int，所以为true
	System.out.println(a3 == a33); //true
	System.out.println(a3.equals(a4)); //true
	
	System.out.println("----------------");
	
	Integer a5 = new Integer(6);
	Integer a6 = new Integer(6);
	
	System.out.println(a5 == a6); //false
	System.out.println(a5.equals(a6)); //true

	}
}
```



其中：

1. int跟int比较是属于基本类型比较：值比较

2. Integer跟Integer比较:

   如:Integer a3=128;在编译时会被编译成Integer a3=Integer.valueOf(128);进行自动装箱的操作;再来看源码:
```java
   public static Integer valueOf(int i) {
       assert IntegerCache.high >= 127;
       if (i >= IntegerCache.low && i <= IntegerCache.high)
           return IntegerCache.cache[i + (-IntegerCache.low)];
       return new Integer(i);
   }

```

由以上源码就会发现，对于-128~127之间的数，会进行缓存，Integer a1 = 6时，会将6进行缓存，下次再写Integer a2 = 6;时，就会直接从缓存中取，也就不用new一个对象了，所以a1和a2比较时就为true。a3和a4是超过范围，会new一个对象，==是进行地址和值比较，是比较两个对象在JVM中的地址，这时a3和a4虽然值相同但地址是不一样的，所以比较就为false了。

3. 通过new出来的Integer会在堆内存中开辟一个内存,所以地址都不相同。



> 包装类对象之间值的比较，建议使用equals比较或者使用compareTo()比较



**总结:**

- int跟int比较的是值
- int跟integer比较,数值相同,用==比较为true(Integer会自动拆箱为基本类型)
- Integer跟Integer比较,值在-128~127之间比较的是值true,范围外会new一个对象为false
