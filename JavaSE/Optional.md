# JAVA8新特性Optional

> 这是java8新特性,用于解决我们日常开发中一层一层获取值,但是不确定中间性会不会是空"null",造成NullPointerException

## 1.回顾之前

之前在开发过程中经常会遇见要通过一层一层的获取某个值,比如:

```java
String isocode = user.getAddress().getCountry().getIsocode().toUpperCase();
```

这是如果中间出现了空,比如getCountry()为空,那么此时引用获取后面的值,那么此时就会抛出空指针异常.

按照以前我们就会一层一层进行判断:

```java
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        Country country = address.getCountry();
        if (country != null) {
            String isocode = country.getIsocode();
            if (isocode != null) {
                isocode = isocode.toUpperCase();
            }
        }
    }
}
```

但是这样的代码显得很臃肿,很繁琐,也是很不美观

终于，Java 8 看不下去了，就引入了 Optional，以便我们编写的代码不再那么刻薄呆板。

```java
Optional.ofNullable(user).map(Address::getAddress()).map(...).orElse(null);
```



## 2.Optional创建对象

1. 可以使用静态方法 `empty()` 创建一个空的 Optional 对象

```java
Optional<Object> empty = Optional.empty();
System.out.println("empty = " + empty); //empty = Optional.empty
```

2. 可以使用静态方法 `of()` 创建一个非空的 Optional 对象

```java
String name = "缪威";
Optional<String> optOrNull = Optional.of(name);
System.out.println(optOrNull); // 输出：Optional[缪威]
```

**注意:**如果此时name为null,就会抛出空指针异常

3. 可以使用静态方法 `ofNullable()` 创建一个即可空又可非空的 Optional 对象

```java
String name = "缪威";
Optional<String> optOrNull = Optional.ofNullable(name);
System.out.println(optOrNull); // 输出：Optional[缪威]
```

**注意:**如果此时name为null,那么此时会返回`Optional.empty`,不会抛出异常

​	之所以这里的`ofNullable`不会返回null,是因为:

![image-20210727200526729](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210727200526729.png)

进行了三元表达式,在确定不为null就选择of()来创建Optinonal对象

## 3.判断值是否存在

可以通过方法 `isPresent()` 判断一个 Optional 对象是否存在，如果存在，该方法返回 true，否则返回 false——取代了 `obj != null` 的判断。

```java
String name = null;
Optional<String> optOrNull = Optional.ofNullable(name);
System.out.println(optOrNull.isPresent()); //false
```

jdk11出了一个isEmpty(),也可实现此功能,不过这个判断为空,则返回true

## 4.非空表达式

Optional 类有一个非常现代化的方法——`ifPresent()`，允许我们使用函数式编程的方式执行一些代码，因此，我把它称为非空表达式。如果没有该方法的话，我们通常需要先通过 `isPresent()` 方法对 Optional 对象进行判空后再执行相应的代码;

其中`ifPresent()`跟`isPresent()`非常相似,这里做下解释: 前者会判断该Optional是否为空,为空则不执行,不为空则执行括号里的表达式操作,而后者会判断该Optional对象是否为空返回对应的Boolean值

1. 不为空

```java
String name = "miaowei";
Optional<String> optOrNull = Optional.ofNullable(name);
optOrNull.ifPresent(x-> System.out.println("x.length() = " + x.length()));//7
```

2. 为空

```java
String name = null;
Optional<String> optOrNull = Optional.ofNullable(name);
optOrNull.ifPresent(x-> System.out.println("x.length() = " + x.length()));//
```

不会执行括号里的操作,直接跳过了!

java 9 后还可以通过方法 `ifPresentOrElse(action, emptyAction)` 执行两种结果，非空时执行 action，空时执行 emptyAction。

语法:`ifPresentOrElse(action,emptyAction)`

```java
		String name = null;
        Optional<String> opt = Optional.ofNullable(name);
        opt.ifPresentOrElse(str -> System.out.println(str.length()), () -> System.out.println("为空")); //为空
```

## 5.设置获取默认值

有时候，我们在创建（获取） Optional 对象的时候，需要一个默认值，`orElse()` 和 `orElseGet()` 方法就派上用场了。还有一个`ororElseThrow()`抛出异常.

所谓默认值就是该Optinal对象为null,就会执行orElase这些默认值

示例类:

```java
public class OrElseOptionalDemo {
    public static String getDefaultValue() {
        System.out.println("getDefaultValue");
        return "123";
    }
}
```

1. `orElse()` 方法用于返回包裹在 Optional 对象中的值，如果该值不为 null，则返回；否则返回默认值。该方法的参数类型和值得类型一致。

   ```java
   String name = null;
   Optional<String> opt = Optional.ofNullable(name);
   String orElse = opt.orElse(OrElseOptionalDemo.getDefaultValue());
   System.out.println("orElse = " + orElse);//123
   ```

2. `orElseGet()` 方法与 `orElse()` 方法类似，但参数类型不同。如果 Optional 对象中的值为 null，则执行参数中的函数。函数返回类型要与值类型要一致!

   ```java
   String name = null;
   Optional<String> opt = Optional.ofNullable(name);
   String elseGet = opt.orElseGet(OrElseOptionalDemo::getDefaultValue);//如也可使用()->{}进行修改,如:(pwd->执行方法)
   System.out.println("elseGet = " + elseGet);//123
   ```

3. `orElaseThrow()`如果Optional为空那么就会自定义抛出一个异常来显示

   ```java
   String name = null;
   Optional<String> opt = Optional.ofNullable(name);
    opt.orElseThrow(()->new Exception("自定义异常"));
   ```

![image-20210727204733947](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210727204733947.png)

以上都是建立在name值为null的基础上,可是如果name不为空,那么就不会执行以上这些方法.

## 6.获取值

直观从语义上来看，`get()` 方法才是最正宗的获取 Optional 对象值的方法，但很遗憾，该方法是有缺陷的，因为假如 Optional 对象的值为 null，该方法会抛出 NoSuchElementException 异常。这完全与我们使用 Optional 类的初衷相悖。

```java
public class GetOptionalDemo {
    public static void main(String[] args) {
        String name = null;
        Optional<String> optOrNull = Optional.ofNullable(name);
        System.out.println(optOrNull.get());
    }
}
```

这段程序在运行时会抛出异常：

![image-20210727205347740](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210727205347740.png)

显然获取为空的话是会抛出异常的,这时可以推荐上面的获取默认值,也就是判断为空的话执行默认值.1.orElse 2.orElseGet()



## 7.过滤值

`filter()` 方法的参数类型为 Predicate（Java 8 新增的一个函数式接口），也就是说可以将一个 Lambda 表达式传递给该方法作为条件，如果表达式的结果为 false，则返回一个 EMPTY 的 Optional 对象，否则返回过滤后的 Optional 对象。

```java
public class FilterOptionalDemo {
    public static void main(String[] args) {
        String password = "12345";
        Optional<String> opt = Optional.ofNullable(password);
        System.out.println(opt.filter(pwd -> pwd.length() > 6).isPresent()); //false
    }
}
```

在实际开发中也可以增加一些难度在上例中，由于 password 的长度为 5 ，所以程序输出的结果为 false。假设密码的长度要求在 6 到 10 位之间，那么还可以再追加一个条件。来看增加难度后的代码。

```java
Predicate<String> len6 = pwd -> pwd.length() > 6; //这里使用的函数式接口然后实现用lambda表达式完成
Predicate<String> len10 = pwd -> pwd.length() < 10;

password = "1234567";
opt = Optional.ofNullable(password);
boolean result = opt.filter(len6.and(len10)).isPresent();
System.out.println(result);
```

如果使用if-else那么此时也会显得有些冗余

## 8.转换值

`map()` 方法，该方法可以按照一定的规则将原有 Optional 对象转换为一个新的 Optional 对象，原有的 Optional 对象不会更改。

```java
public class OptionalMapDemo {
    public static void main(String[] args) {
        String name = "沉默王二";
        Optional<String> nameOptional = Optional.of(name);
        Optional<Integer> intOpt = nameOptional
                .map(String::length);

        System.out.println( intOpt.orElse(0));
    }
}
```

在上面这个例子中，`map()` 方法的参数 `String::length`，意味着要 将原有的字符串类型的 Optional 按照字符串长度重新生成一个新的 Optional 对象，类型为 Integer。



综合案例:过滤值跟转换值的使用:

```java
public class OptionalMapFilterDemo {
    public static void main(String[] args) {
        String password = "password";
        Optional<String>  opt = Optional.ofNullable(password);

        Predicate<String> len6 = pwd -> pwd.length() > 6;
        Predicate<String> len10 = pwd -> pwd.length() < 10;
        Predicate<String> eq = pwd -> pwd.equals("password");

        boolean result = opt.map(String::toLowerCase).filter(len6.and(len10 ).and(eq)).isPresent();
        System.out.println(result);
    }
}
```

这里进行利用函数式接口然后追加一些判断条件,然后使用map()转换成新的Opitonal对象,然后进行过滤出符合条件的,然后使用isPresent()进行判断是否为空

# <? extends T>和 <? super T>



- <? extends T>:是指"上界通配符"
- <? super T>:是指"下界通配符"

今天我在学习Optinonal的时候刚好看到了这个,就学习记录一下:

![image-20210728101345341](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210728101345341.png)

这里就不说多了,直接上例子:

以List为例
List<? extends T> 表示List中存放的都是T或者T的子类型

```java
List<? extends Number> foo = new ArrayList<Number>();
List<? extends Number> foo = new ArrayList<Integer>();
List<? extends Number> foo = new ArrayList<Double>();
```

**说明:**extends确定T就是边界,获取的是T类型及他之下的子类.

foo确定了上界，所以可以从foo中读取到Number对象
而不知道下界，所以foo可能是存储Number子类型的List，不能写入Number对象，同时不知道是哪一种子类型（Integer写Double或Double写Integer），所以写哪一种对象都是不合适的。
List<? super T>表示List中存放的都是T或者T的父类型

```java
List<? super Integer> foo = new ArrayList<Integer>();
List<? super Integer> foo = new ArrayList<Number>();
List<? super Integer> foo = new ArrayList<Object>();
```

**说明:**foo确定了下界但是不知道上界，所以读取的话只能保证读到的是Object对象。而可以确定下界，所以可以向foo中写入Integer和Integer子类型对象。

总结:

1. extends指的T的类及其子类,而super值的T的类及其父类
2. extends 可用于返回类型限定，不能用于参数类型限定（换句话说：? extends xxx 只能用于方法返回类型限定，jdk能够确定此类的最小继承边界为xxx，只要是这个类的父类都能接收，但是传入参数无法确定具体类型，只能接受null的传入）。
3. super 可用于参数类型限定，不能用于返回类型限定（换句话说：? supper xxx 只能用于方法传参，因为jdk能够确定传入为xxx的子类，返回只能用Object类接收）。
4. ? 既不能用于方法参数传入，也不能用于方法返回。

解读上面使用:首先传入的参数必须是T类型或者是T的父类,这里的Predicate<? super T> 是指泛型 ,表明这个参数必须满足这个条件,然后局部变量是predicate,然后返回的是T类型,而这个这个T类型是指谁调用的就是哪个类型.

看apache parquet源码时，发现代码各种泛型嵌套，有必要系统整理一下关于泛型的各种知识，在此做一总结。

1. `List<String>` —- 参数化的类型 
2. `List<E>` —- 泛型 
3. `List<?>` —- 无限制通配符类型 
4. `<E extends SomeClass>` —- 有限制类型参数 
5. `List <? extends SomeClass>`—- 有限制通配符类型 
6. `<T extends Comparable<T>>` —– 递归类型限制 
7. `static <E> List<E> asList(E[] a)` —- 泛型方法

# 泛型中通配符

## 1.常用的T、E、K、V、？

本质上这些个都是通配符，没啥区别，只不过是编码时的一种约定俗成的东西。比如上述代码中的 T ，我们可以换成 A-Z 之间的任何一个 字母都可以，并不会影响程序的正常运行，但是如果换成其他的字母代替 T ，在可读性上可能会弱一些。**通常情况下，T，E，K，V，？是这样约定的：**

1. ？表示不确定的 java 类型
2. T (type) 表示具体的一个java类型
3. K V (key value) 分别代表java键值中的Key Value
4. E (element) 代表Element 元素

## 2. ？无界通配符：

无界不确定性的，代表传进来值是不知道的

```java
static int countLegs (List<? extends Animal > animals ) {
    int retVal = 0;
    for ( Animal animal : animals )
    {
        retVal += animal.countLegs();
    }
    return retVal;
}

static int countLegs1 (List< Animal > animals ){
    int retVal = 0;
    for ( Animal animal : animals )
    {
        retVal += animal.countLegs();
    }
    return retVal;
}

public static void main(String[] args) {
    List<Dog> dogs = new ArrayList<>();
     // 不会报错
    countLegs( dogs );
    // 报错
    countLegs1(dogs);
}
```

所以，对于不确定或者不关心实际要操作的类型，可以使用无限制通配符（尖括号里一个问号，即 ），表示可以持有任何类型。像 countLegs 方法中，限定了上届，但是不关心具体类型是什么，所以对于传入的 Animal 的所有子类都可以支持，并且不会报错。而 countLegs1 就不行。

这里countLegs()里表明传值是任意类型但是要必须是Animal类或者及其Animal子类才能符合List<>泛型要求,而countLegs1()则直接限定了list<Animal>固定的泛型类型,意思就是说传进来的参数类型也必须是Animal,其他类型都不行!

## 3. 上界通配符<? extendds E>

在类型参数中使用 extends 表示这个泛型中的参数必须是 E 或者 E 的子类，这样有两个好处：

- 如果传入的类型不是 E 或者 E 的子类，编译不成功
- 泛型中可以使用 E 的方法，要不然还得强转成 E 才能使用

```java
private <K extends A, E extends B> E test(K arg1, E arg2){
    E result = arg2;
    arg2.compareTo(arg1);
    //.....
    return result;
}
```

**注意:**类型参数列表中如果有多个类型参数上限，用逗号分开

## 4. 下界通配符<? super E>

在类型参数中使用 super 表示这个泛型中的参数必须是 E 或者 E 的父类。

```java
private <T> void test(List<? super T> dst, List<T> src){
    for (T t : src) {
        dst.add(t);
    }
}

public static void main(String[] args) {
    List<Dog> dogs = new ArrayList<>();
    List<Animal> animals = new ArrayList<>();
    new Test3().test(animals,dogs);
}
// Dog 是 Animal 的子类
class Dog extends Animal {

}
```

用 super 进行声明，表示参数化的类型可能是所指定的类型，或者是此类型的父类型，直至 Object

用于判断传进来的参数类型->"?"是否是调用的T类型的同类型或及其父类,如果都满足那么就进方法往下执行,如果不满足,那么在外面就会报编译错误!

说明:dst 类型 “大于等于” src 的类型，这里的“大于等于”是指 dst 表示的范围比 src 要大，因此装得下 dst 的容器也就能装 src 。

## 5. ?跟T的区别:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/v2-8fdbb04cddfbcf7cb25496c28086ef9b_720w.jpg)



？和 T 都表示不确定的类型，区别在于我们可以对 T 进行操作，但是对 ？不行，比如如下这种 ：

```text
// 可以
T t = operate();

// 不可以
？car = operate();
```

总结:

​	T 是一个 确定的 类型，通常用于泛型类和泛型方法的定义，？是一个 不确定 的类型，通常用于泛型方法的调用代码和形参，不能用于定义类和泛型方法。一般在使用的过程中,?用于形参接收参数判断是否符合要求,而T用于返回类型一些实际的参数

