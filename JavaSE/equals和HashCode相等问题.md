# 探讨equals和HashCode之间关于相等的问题

## 1. 问题

写这篇文档是为了记录自己学习遇到的问题,这个问题据说也是面试经常问到的!在网上找了几篇博客根据自己理解记录一下,问题如下:

当两个对象比较的时候:

1. 如果equals相等的时候,那么HashCode是否存在相等?
2. 如果HashCode相等的时候,那么equals是否相等呢?
3. 为什么重写equals时必须重写HashCode方法?

根据网上总结的答案来说:

- equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。
- hashCode()相等的两个对象他们的equals()不一定相等，也就是hashCode()不是绝对可靠的。

但是也存在一个问题,如果面试的时候就这样两句话给面试老师说了,估计面试老师还不太高兴甚至心里没什么变化,因为他要的可能不只是答案那么简单,他需要的是你对这个问题的过程的理解;比如你说了答案他就可能反问你:为什么equals相等而对应的hashCode就一定相等,给我理由?为什么hashMap进行判断重复的原则的时候就要首先比较hashCode而不是首先进行比较equals()方法?这时你就手无足措了!

## 2. hashCode()和equals()是什么?

作用:都是在Java里对比两个对象是否相等一致

而在Object类中有这两个方法:

```java
public native int hashCode();

public boolean equals(Object obj) {
        return (this == obj);
}
```

1. hashCode():

   - 作用:用于判断两个对象是否相等
   - 实现:通过获取[哈希码](哈希码也称散列码,实际上就是返回一个int整数)来确定该对象在哈希表中的[索引位置](HashMap中使用hash作为indexFor的输入);

   - 这是一个本地方法，用来返回对象的哈希值（一个整数）。在 Java 程序执行期间，对同一个对象多次调用该方法必须返回相同的哈希值。

2. equals():

   - 作用：用于判断两个对象是否相等；
   - 实现：如果对象重写了equals()方法，重写的比较逻辑一般是比较两个对象的内容是否相等；如果没有重写，那就是比较两个对象的地址是否相同，等价于“==”；
   - 对于任何非空引用 x 和 y，当且仅当 x 和 y 引用的是同一个对象时，equals 方法才返回 true。

但是这样想两者之间并没有任何联系啊?

但是在官方的文档中是这样写道:

1. 如果两个对象调用 equals 方法返回的结果为 true，那么两个对象调用 hashCode 方法返回的结果也必然相同	——来自 hashCode 方法的 doc 文档
2. 每当重写 equals 方法时，hashCode 方法也需要重写，以便维护上一条规约

## 3.HashCode在HashMap中的实现:

既然HashCode是获取哈希值而最典型代表就是HashMap,因为它存储的就是键值对,能够快速的检索出对应的值

-> 这是 HashMap 的 get 方法，通过键来获取值的方法

```java
public V get(Object key) {
    HashMap.Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

然后通过getNode方法:

```java
final HashMap.Node<K,V> getNode(int hash, Object key) {
    HashMap.Node<K,V>[] tab; HashMap.Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof HashMap.TreeNode)
                return ((HashMap.TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

这里我们就可以得出:

在没有发生哈希冲突的情况下:

​	`first = tab[(n - 1) & hash]`得到的就是key对应的value,按照时间复杂度来讲就是O(1);

那如果发生哈希冲突的情况下:

​	`if ((e = first.next) != null) {}`代码块中,可以看到如果节点不是红黑树的时候，会通过 do-while 循环语句判断键是否 equals 返回对应值的。按照时间复杂度来讲就是O(n)

```java
判断红黑树:
 if (first instanceof HashMap.TreeNode){
     return ((HashMap.TreeNode<K,V>)first).getTreeNode(hash, key);
 }
循环进行equals进行比较
do {
  if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k)))){
      return e;
  }
} while ((e = e.next) != null);    //因为要循环查找所以这里的复杂度是O(n)
```

而如果出现了哈希冲突在HashMap内部底层是这样子的:

​	HashMap 是通过拉链法来解决哈希冲突的，也就是如果发生哈希冲突，同一个键的坑位会放好多个值，超过 8 个值后改为红黑树，为了提高查询的效率。从链表变为红黑树结构

**结论:**从时间复杂度上来看的话 O(n) 比 O(1) 的性能要差，这也正是哈希表的价值所在。

> O(n):时间复杂度的一种表示方法,简单理解为n就代表代码执行n次
>
> O(1):也是时间复杂度的一种表示方法,也可以理解为1就代表只需要执行1次



HashMap从底层代码可以看出本质上是通过数组来进行实现的,而我们如果获取某个值时,实际上就是获取数组中某个位置的元素,而位置是通过[键](这里的键的位置准确来讲是通过hashCode来决定的)来决定的

```java
Node<K,V>[] tab; //底层就是一个键值对的数组
```

试想一下HashMap中存入是怎么操作的?

​	这是 HashMap 的 put 方法，会将键值对放入到数组当中

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

​	在调用的时候首先就会对key进行获取哈希值

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

​	然后调用putVal方法:

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    HashMap.Node<K,V>[] tab; HashMap.Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        // 拉链
    }
    return null;
}
```

这里就可以看出`HashMap.Node<K,V>[]`就是存储的键值对数组保存数据

通常情况下，`p = tab[i = (n - 1) & hash])` 就是key对应的值。而数组的索引 `(n - 1) & hash` 正是基于 hashCode 方法计算得到的。



## 4.为什么要重写HashCode方法?

先看一段代码:

```java
public class Test {
    public static void main(String[] args) {
        Student s1 = new Student(18, "张三");
        Map<Student, Integer> scores = new HashMap<>();
        scores.put(s1, 98);

        Student s2 = new Student(18, "张三");
        System.out.println(scores.get(s2));
    }
}
 class Student {
    private int age;
    private String name;

     public Student(int age, String name) {
         this.age = age;
         this.name = name;
     }

     @Override
     public boolean equals(Object o) {
         Student student = (Student) o;
         return age == student.age &&
                 Objects.equals(name, student.name);
     }
 }
```

这里我首先创建了一个内部类Student(为了方便)然后重写了equals方法,在测试中我new了一个对象然后放入了HashMap中,然后通过另一个对象来获取

这里你可能有疑问我为什么要用另一个对象去获取?

​	那是因为如果两个学生的年纪和姓名相同，我们就认为是同一个学生，虽然很离谱，但我们就是这么草率。

最后答案是:

```java
null
```

原因:

首要原因就是 **在重写equals方法的时候没有重写hashCode方法**

> 你可以这样理解:equals 方法虽然认定名字和年纪相同就是同一个学生，但它们本质上是两个对象，hashCode 并不相同。

![image-20210805211259712](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210805211259712.png)

解决办法:

可以直接调用 Objects 类的 hash 方法

```java
 @Override
 public int hashCode() {
     return Objects.hash(age, name);
 }
```

`Objects.hash()`方法可以针对不同数量的参数生成新的哈希值,源码如下:

```java
public static int hash(Object... values) {
        return Arrays.hashCode(values);
    }
```

而hash方法调用的是Arrays.hashCode方法:源码如下;

```java
public static int hashCode(Object a[]) {
        if (a == null)
            return 0;

        int result = 1;

        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());

        return result;
    }
```

而我们模拟刚刚上面的例子来讲:

```java
第一次循环:
result = 31*1 + Integer(18).hashCode();
第二次循环:
result = (31*1 + Integer(18).hashCode()) * 31 + String("张三").hashCode();
```

> 针对姓名年纪不同的对象，这样计算后的哈希值很难很难很难重复的；针对姓名年纪相同的对象，哈希值保持一致。

此时再执行:

```java
98
```

为什么现在就就可以获取了呢?难道是hashCode值一样了吗?

哈哈哈,当然啦肯定是一样的了:此时 s1 和 s2 对象的哈希值都为 776408。

![image-20210805212359877](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210805212359877.png)

总结:

> 每当重写 equals 方法时，hashCode 方法也需要重写，原因就是为了保证：如果两个对象调用 equals 方法返回的结果为 true，那么两个对象调用 hashCode 方法返回的结果也必然相同



## 5.两者区别?

下边从两个角度介绍了他们的区别：一个是**性能**，一个是**可靠性**。他们之间的主要区别也基本体现在这里。

1. equals()既然已经能实现对比的功能了，为什么还要 hashCode()呢？

   - 因为重写的equals（）里一般比较的比较全面比较**复杂**，这样**效率就比较低**，而利用hashCode()进行对比，则只要生成一个hash值进行比较就可以了，**效率很高**。

2. hash Code()既然效率这么高为什么还要 equals()呢？

   因为hashCode()**并不是完全可靠**，有时候不同的对象他们生成的Hashcode也会一样（生成hash值得公式可能存在的问题），所以hashCode()只能说是大部分时候可靠，并不是绝对可靠，所以我们可以得出

   - equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。
   - hashCode()相等的两个对象他们的equals()不一定相等，也就是hashCode()不是绝对可靠的。

## 6. 两者使用注意事项:

1. 对于需要大量并且快速的对比的话如果都用equals()去做显然效率太低，所以解决方式是，每当需要对比的时候，首先用hashCode()去对比，如果hashCode()不一样，则表示这两个对象肯定不相等（也就是不必再用equals()去再对比了）,如果hashCode()相同，此时再对比他们的equals()，如果equals()也相同，则表示这两个对象是真的相同了，这样既能大大提高了效率也保证了对比的绝对正确性！

> 这个问题就解决了为什么Set判断重复标准的时候先用HashCode而不是equals方法

![image-20210805214211654](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210805214211654.png)

2. 这种大量的并且快速的对象对比一般使用的Hash容器中，比如HashSet,HashMap,HashTable等等，比如HashSet里要求对象不能重复，则他内部必然要对添加进去的每个对象进行对比，而他的对比规则就是像上面说的那样，先hashCode()，如果hashCode()相同，再用equals()验证，如果hashCode()都不同，则肯定不同，这样对比的效率就很高了

3. 然而hashCode()和equals()一样都是基本类Object里的方法，而和equals()一样，Object里hashCode()里面只是返回当前对象的地址，如果是这样的话，那么我们相同的一个类，new两个对象，由于他们在内存里的地址不同，则他们的hashCode（）不同，所以这显然不是我们想要的，所以我们必须重写我们类的hashCode()方法，即一个类，在hashCode()里面返回唯一的一个hash值

   比如:

   ```java
   @Override
   public int hashCode() {
       return Objects.hash(age, name);
   }
   ```

## 7.拓展

我在阿里巴巴java详尽手册泰山版里查询到,人家是做出来了一个规定->注意是**强制性**:

![image-20210805214645898](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210805214645898.png)

1. 那我们思考下我们什么时候重写呢?

​	 你可以这样去记:只有当类需要放在HashTable、HashSet、HashMap等等hash结构的集合时就要重写hashCode

2. 我们又反过来再思考我们为什么要重写hashCode?

​	如果你重写了equals，比如说是基于对象的内容实现的，而保留hashCode的实现不变，那么很可能某两个对象明明是“相等”，而hashCode却不一样。这样，当你用其中的一个作为键保存到hashMap、hasoTable或hashSet中，再以“相等的”找另一个作为键值去查找他们的时候，则根本找不到。

3. 回到我们最开始开始的为什么equals()相等,hashCode就一定相等?而hashCode相等,却不要求equals相等?

   - 因为是按照hashCode来访问小内存块，所以hashCode必须相等。
   - HashMap获取一个对象是比较key的hashCode相等和equals等于true。

   之所以hashCode相等，却可以equal不等，就比如ObjectA和ObjectB他们都有属性name，那么hashCode都以name计算，所以hashCode一样，但是两个对象属于不同类型，所以equals为false。

4. 为什么需要hashCode?

   - 通过hashCode可以很快的查到小内存块
   - 通过hashCode比较比equals方法**快**，当get时先比较hashCode，如果hashCode不同，直接返回false。

## 8.时间复杂度

**概念:**时间复杂度是用评估代码执行的效率的重要标准

**思考:**试想既然评估代码执行效率为何不在前后设置计数,然后进行时间差不就可以?

​	实际上来说这种评估方法可以得出非常准确的数值，但也有很大的局限性

1. 测试结果会受到测试环境的影响。比如说，同样的代码，在本地的电脑上执行的时间和在其他人的电脑上抛出的时间可能就差别很大。
2. 测试结果会受到测试数据的影响。你比如说，一个排序后的数组和一个没有排序后的数组，调用了同一个查询方法，得出来的结果可能会差别特别大。

**总结:**因此，我们需要这种不依赖于具体测试环境和测试数据就能粗略地估算出执行效率的方法，时间复杂度就是其中的一种，还有一种是空间复杂度

来看一下代码:

```java
public static int sum(int n) {
    int sum = 0; // 第 1 行
    for (int i=0;i<n;i++) { // 第 2 行
        sum = sum + 1; // 第 3 行
    } // 第 4 行
    return sum; // 第 5 行
}
```

这段代码非常简单，方法体里总共 5 行代码，包括“}”那一行。每段代码的执行时间可能都不大一样，但假设我们认为每行代码的执行时间是一样的，比如说 unit_time，那么这段代码总的执行时间为多少呢？

答:第 1、5 行需要 2 个 unit_time，第 2、3 行需要 2*n*unit_time，总的时间就是 2(n+1)*unit_time。

也就是说一段代码的执行时间 T(n) 和总的执行次数成正比，也就是说，代码执行的次数越多，花费的时间就越多。”我总结道，“这个规律可以用一个公式来表达：
$$
T(n) = O(f(n))
$$
f(n) 表示代码总的执行次数，大写 O 表示代码的执行时间 T(n) 和 f(n) 成正比。这也就是大 O 表示法，它不关心代码具体的执行时间是多少，它关心的是代码执行时间的变化趋势，这也就是时间复杂度这个概念的由来。对于上面那段代码 `sum()` 来说，影响时间复杂度的主要是第 2 行代码，其余的，像系数 2、常数 2 都是可以忽略不计的，我们只关心影响最大的那个，所以时间复杂度就表示为 `O(n)`。

**分类:**

1. `O(1)`

   解释:代码的执行时间，和数据规模 n 没有多大关系。

   比如:括号中的 1 可以是 3，可以是 5，可以 100，我们习惯用 1 来表示，表示这段代码的执行时间是一个常数级别。比如说下面这段代码：

   ```java
   int i = 0;
   int j = 0;
   int k = i + j;
   ```

   实际上执行了 3 次，但我们也认为这段代码的时间复杂度为 `O(1)`。

2. `O(n)`

   解释:时间复杂度和数据规模 n 是线性关系。换句话说，数据规模增大 K 倍，代码执行的时间就大致增加 K 倍。

3. `O(login)`

   解释:时间复杂度和数据规模 n 是对数关系。换句话说，数据规模大幅增加时，代码执行的时间只有少量增加。

   代码示例:

   ```java
   public static void logn(int n) { 
       int i = 1;
       while (i < n) {
           i *= 2;
       }
   }
   ```

   换句话说，当数据量 n 从 2 增加到 2^64 时，代码执行的时间只增加 64 倍。

   ```
   遍历次数 |   i
   ----------+-------
       0     |   i
       1     |  i*2
       2     |  i*4
      ...    |  ...
      ...    |  ...
       k     |  i*2^k 
   ```

   

**理解:**第一个可理解为代码执行一次就找到了,第二个则表示代码执行N次才执行完成,算是比较复杂的一种,第三个则表示查询规模大但代码执行时间增幅不大
