# Script笔记

> 不是很完整就是简简单单做一个小笔记

## 1. 简介

> 轻量级的解释性的脚本语言，运行在浏览器，客户端面向对象

1. JavaScript是一种`解释性脚本语言`（代码不进行预编译），由浏览器解释执行，用来向页面添加交互行为；
2. Java Script由三个主要组成部分：`ECMAScript`（核心），`BOM`（浏览器对象模型），`DOM`（文档对象模型）;

![image-20210910105622328](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910105622328.png)

**注意**:

1. ECMAScript:
   - 描述了Js的基本语法：类型、语句、关键字、保留字、运算符和对象等
2. DMO:
   - 是把整个文档中的所有节点解析成一个一个对象，并且这些对象之间会形成一个层级关系。通过DOM我们可以对文档中所有节点做CRUD操作
3. BOM:
   - 是浏览器的一种特性，它可以对浏览器窗口进行访问和操作，例如移动、关闭窗口，调整窗口的大小等

> 从以上可以知道使用JavaScript语言是一种脚本语言,也就是说可以不用像java那样需要编译为class文件才能运行而是可以直接在浏览器上直接运行的,而javaScript的核心就那三个:一个是定义本身语言基本的语法知识,一个是针对整个HTML中所有节点的支持功能,还有一个是针对浏览器进行操作的功能

## 2.三种写法和变量

JavaScript的写法与我们之前的CSS本身并无太大差别:

写法:

1. 行内JS:

   ```javascript
   语法：<a href="javascript:alert('点你就点你');">点我，点我...</a>
   // 行内JS就是在我们写的HTML代码中进行嵌套使用,这个所用也只能针对单个的标签,
   //而上面的href在执行的时候就不是跳转而是执行其中的JS代码,这是因为javascript: 是一种伪协议,其目的作用就是告诉浏览器这个不是要跳转而是要执行JS代码!
   其他使用方式:
   1.<a href="javascript:alert('点你就点你');">点我，点我...</a>   // 不推荐 冗余写法
   2.<a href="javascript:;"   onclick="abc()">钢筋2</a> // 推荐 标签不执行js 作为触发源头,触发js的执行
   3.<a href="#"   onclick="abc()">钢筋3</a> //不推荐 修改地址栏,弹到地址栏
   4.<a href="javascript:void(0)" onclick="abc()">点击一下呢</a>  // 不推荐,void未做任何定义的意思
   ```

2. 内部JS:

   ```javascript
   语法：
   <script type="text/javascript">
   	JS代码
   </script>
   // 页面内的JS是写在一组<script>标签内，这组标签可以写在页面内的任意位置
   ```

3. 外部JS:

   ```javascript
   引入外部JS文件：
   <script type="text/javascript" src="JS文件位置"></script>
   //这个就更好理解就是引入外部写好的JS文件
   ```

**注意**:如果我们在引入js标签内部写了内容,那么引入的外部js文件就会覆盖标签内部的内容

> 在定义JS的时候,要注意位置,浏览器打开页面,自上而下,一般推荐放在</body>和</html>之间

变量:

在js中声明变量用"var"来声明,不管是什么类型的数据都可以用var来定义声明

```javascript
var var1=true;
```

这里还提出另外两种定义变量声明`let`,`const`

区别:

1. var定义的变量可以修改，如果不初始化会输出undefined，不会报错,var是全局作用域!函数内使用var外面依然可以进行访问!

   ```javascript
   var a = 1;
   // var a;//不会报错
   console.log('函数外var定义a：' + a);//可以输出a=1
   function change(){
   a = 4;
   console.log('函数内var定义a：' + a);//可以输出a=4
   }
   change();
   console.log('函数调用后var定义a为函数内部修改值：' + a);//可以输出a=4
   ```

2. let是块级作用域，函数内部使用let定义后，对函数外部无影响。在同级作用域下不能出现声明重复的let

   ```javascript
   let c = 3;
   console.log('函数外let定义c：' + c);//输出c=3
   function change(){
   let c = 6;
   console.log('函数内let定义c：' + c);//输出c=6
   }
   change();
   console.log('函数调用后let定义c不受函数内部定义影响：' + c);//输出c=3
   ```

3. const定义的变量不可以修改，而且必须初始化

   ```javascript
   const b = 2;//正确
   // const b;//错误，必须初始化 
   console.log('函数外const定义b：' + b);//有输出值
   // b = 5;
   // console.log('函数外修改const定义b：' + b);//无法输出
   ```

   > 全局变量是作用于window的属性,原本是window.属性只不过这里window.进行省略了而已

### 2.1 ESM6

​	var let 区别:

​		var:是全局作用域,可以被再次定义,在函数内部被定义那么函数外依然可以正常访问到!如果在同级作用域下出现一样的var 变量,那么后者覆盖前者!

​		let:是块级作用域,是块级元素,函数内被声明,外部不能进行访问,可以在外部和函数内部同时定义相同的let,但是两者是完全不同的变量!

```html
  <script type="text/javascript">

        /**
         *  var: 可以被再次定义
         *  let: 不可以被二次定义   let是块级元素，外部不可以使用。
         */

        // es5：var
        var name = "lei";
        var age = 20;
        var age = 30;
        console.debug(name,age);

        // es6 方式
        let sex = "女";
        //let sex = "男";
        console.debug(sex)
        if(true){
            var aa = "aa";
            let bb = "bb";
        }
        console.debug(aa); //"aa"
        console.debug(bb); //undefind
    </script>
```

​	const:声明常量!一旦声明不可被第二次赋值!

```html
 <script type="text/javascript">
        var name = "tom";
        name = "jack";

        let age = 20;
        age = 30;
        console.debug(name)
        console.debug(age)

        // const: 申明常量， 不可以被二次赋值。
        const sex = "未知";
        // sex = "ads";  不可以再次赋值

    </script>
```

解构表达式:

​	解构数组:将声明的数组的变量转为赋值一个数组变量,按照顺序意义对应即可!,然后我们就能拿到对应的数组元素!

​	解构对象:将声明的对象的变量作为另一个赋值的结果,变量名按照对象中的key一一对应,然后我们就能拿到里面的对象元素!

```html
<script type="text/javascript">
        let arr = ["王天霸","力很弱","阮经天"];
        console.debug(arr[1]) // "力很弱"

        // 解构数组
        let [a,b,c,d] = arr;
        console.debug(a,b,c,d)//"王天霸","力很弱","阮经天",underfind

        // 解构对象
        let obj = {"id":10,"name":"雷","sex":"男"};
        // es6方式
        let {id,name,sex} = obj;
        console.debug(id,name,sex) //10,"雷","男"

    </script>
```

箭头函数:

```html
<script type="text/javascript">
        let obj = {
            f1:function () { // es5方式
                console.debug("f1.....")
            },
            f2(){ // es6简写方式
                console.debug("f2...,,")
            },
            f3:()=>{ // 箭头函数
                console.debug("f3......")
            },
            f4:name=>console.debug("f4.....")  // 如果只有一个参数可以省略括号，函数体中只有一句代码可以省略方法体。
        };

        let {f1,f2,f3,f4} = obj;
        f1();f2();f3();f4(); //依次执行,但是要加上括号,因为这是函数方法是调用的那种

    </script>
```

promise:Promise是异步编程的一种解决方案, 所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果,从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。过Promise的构造函数来创建Promise对象，并在内部封装一个异步执行的结果。axios是一个发送http请求工具，是通过promise对Ajax进行封装。

```html
<script type="text/javascript">
        const p = new Promise((resolve, reject) =>{
            // 这里我们用定时任务模拟异步
            setTimeout(() => {
                const num = Math.random();
                // 随机返回成功或失败
                if (num < 0.5) {
                    resolve("成功！num:" + num)
                } else {
                    reject("出错了！num:" + num)
                }
            }, 300)
        })
        // 调用promise
        p.then(function (msg) {  //首先执行这个方法体,then中!如果失败了就执行catch中,他们分别对应上诉Promise中的参数,
            console.log(msg); //异步成功的回调
        }).catch(function (msg) {
            console.log(msg); //异步失败的回调
        })
    </script>
```

模块化导入导出:我写在Vue.md中的Vue-Cli中的安装Webpack上一点点,这个模块化呢是ES6的写法,在VUE中也是比较重要的特性,但是由于有些浏览器不支持ES6,只支持ES5,所以这个时候我们会在VUE中使用webpack进行打包,将ES6自动转换为ES5的写法给浏览器看;

理论:

```
模块化是一种思想,这种思想在前端有多种规范，常见的规范有commonJs(nodeJS),cmd/amd(可以在浏览器中使用),es6(可以在浏览器中使用).
模块化就是把代码进行拆分，方便重复利用。类似java中的导包：要使用一个包，必须先导包。而JS中没有包的概念，换来的是模块。es6模块功能主要由两个命令构成：export和import。
- export命令用于规定模块的对外接口，
- import命令用于导入其他模块提供的功能。
```



## 3.数据类型

在js中的原始数据类型与Java中的基本数据类型都是一样的,只不过在js是一门弱类型语言所以所有的类型都可用"`var`"来定义!

当然我们可以检测其属于哪种数据类型:

```javascript
	var var1=true;
    alert(typeof var1 )
//等同于
	alert(typeof (var1) )
```

### 3.1 无穷数

在JS中,1/0并不会报错，因为JS语法中0是取值了一个无限趋近于0的一个数字

比如:

```javascript
<script type="text/javascript">
    alert(1 / 0);
</script>
```

结果:

![image-20210910142202708](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910142202708.png)

解读:在JS语法中，数字是有取值边界的，超出最大数取值为Infinity【正无穷】，超出最小数取值为-Infinity【负无穷】，无穷数不能用于数值运算，一般使用`isFinite()`函数对一个数字进行判断是否是有穷的【有限的】，若传入的数据是无穷数，那么返回false，若传入的数据是有穷数，则返回为true;

如:

```javascript
 <script type="text/javascript">
    alert(isFinite(1 / 0)); //是无穷数返回false
  </script>
```

结果:

![image-20210910142434212](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910142434212.png)

### 3.2 非数

在JS中可以用`isNan`来判断当前字符是否是数字,是数字返回"false",不是数字返回"true"

```javascript
<script type="text/javascript">
    let var1 = "";
    console.log(isNaN(var1)); //返回false
	var1="1";
	console.log(isNaN(var1)); //返回false
	var1="/"
	console.log(isNaN(var1)); //返回true
	var1="w"
	console.log(isNaN(var1)); //返回true
</script>
```

经过以上测试发现其中 ""空字符也会判定为数值类型的,也会返回false

## 4.运算符

### 4.1 赋值运算符:

![image-20210910145115594](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910145115594.png) 

### 4.2 算术运算符

![image-20210910145203702](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910145203702.png)

这个跟Java的运算符也是一致的,其中的"+"在同时数字类型与字符类型的时候也是会演变成字符串的拼接

### 4.3 比较运算符

比较运算符用于判断两个变量大小关系：>、<、==、<=、>=、===、!=、!===，其中:

1. ==:代表相等(它只比较内容，不比较类型)
2. ===：绝对相等(先比较类型，再比较内容)
3. !==：绝对不等

> 这里因为md语法所以显示有问题,第一个是双等号,第二个是等号,第三个是双等号前加!

例如:

```javascript
<script type="text/javascript">
  let var1 = 1;
  let var2 = "1";
  console.log(var1 == var2) //true
  console.log(var1 === var2) //false
  console.log(var1 !== var2); //true
</script>
```

> 绝对相等的话不仅类型相等并且内容也要相等,两者条件都要满足!

### 4.4 逻辑运算符

1. `&&` ：逻辑AND运算符，一假遍假
2. `||` ：逻辑OR运算符 ，一真遍真
3. `! ` ：逻辑NOT运算符，真为假，假为真

**注意**:在比较&&的时候,两边都是非0的,值为后者,如果有0则为0

## 5.流程控制

JS中同Java一样存在流程控制语句，用法一样:

1. 分支:
   - if语句
   - switch语句
2. 循环:
   - while语句
   - do-while语句
   - for循环
3. break/continue语句
4. 三目表达式

## 6.函数

js中的函数就跟Java中定义方法是一样的!只不过区别就是js在定义函数前需要声明`function`,并且不需要声明什么访问权限!

```javascript
<script type="text/javascript">
  let test1 = test("1");
  console.log(test1)


  function test(var1) {
    console.log("哇塞,我进来了")
    console.log(var1)
  }
</script>
```

结果:

![image-20210910151907566](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910151907566.png)

从以上可以发现:

1. 定义的函数没有返回值那么在调用函数时变量接收的话就是`underfined`;
2. 函数在传值的时候,在接收参数的时候直接用变量名接收就可以了,不管是什么类型的,都会默认加上`var`类型,注意名称不能相同

> underfined表示未定义,就是声明了变量但是没有赋值

> 函数内部声明的变量要用"var"来声明定义才表明这是局部变量,不然默认不加的话就是全局变量!

###  6.1 匿名函数

匿名函数就是没有名字就定义的函数,但是得需要返回参数名作为函数名来进行调用!

如:

```javascript
<script type="text/javascript">
   let acion=function(){
     console.log("前方高能,注意脚下!");
   }
   console.log(acion())
</script>
```

如果没有声明返回名称,那么就会发生异常:

![image-20210910153615207](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910153615207.png)

## 7.对象

js的对象有三大类:`内部对象`、`宿主对象`、`自定义对象`

解释:

1. 内部对象就分为`本地对象`、`内置对象`
   - 本地对象可以new实例,方法或函数式通过实例调用的
   - 内置对象不能使用new关键字创建实例，方法调用也不需要通过实例去调用
2. 宿主对象指的是BOM和DOM中所有对象;
3. 自定义对象就是开发人员自己定义的对象

> 理解:内部对象就是指已经存在定义好的对象,其中有些对象使用其中的方法是需要实例来调用是需要new的,而有些对象中的方法是通过类似于java中的静态方法调用不需要new

### 7.1 内部对象

这里做个笔记关于对象的`插件机制`的描述:

在JS中我们可以为一个对象绑定一个方法->函数进行使用:比如:

```javascript
<script>
  let date = new Date();
  date.format=function (){
    console.log("这是固定了的");
  }

  date.format();
  let date1 = new Date();
  date1.format();
</script>
```

**说明**:以上就是定义一个匿名函数然后用变量跟Date实例中的方法进行绑定,这样无论在哪都可以使用这个方法!但是这种只是为一个实例进行绑定,如果此时重新new了一个实例,那么这个new的实例不再享受绑定

结果:

![image-20210911174400913](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210911174400913.png)

当然也有解决办法:就是将对应的函数给声明给全局作用域:

```javascript
<script>

  Date.prototype.format=function (){
    console.log("这是固定了的");
  }

  let date = new Date();
  date.format();
  let date1 = new Date();
  date1.format();
</script>
```

**说明**:给Date这个对象定义变量然后跟匿名函数绑定,这样我们无论new几个实例那么都会享受这个方法! -> **插件机制**

结果:

![image-20210911174828046](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210911174828046.png)

#### 7.1.1 本地对象

本地对象就是`ECMA-262 定义的类（引用类型）`,而我们只需要new来创建对应的实例来调用其中的方法

这里举出一些典型例子:

1. ## Date 日期对象

   先看API:

   ![image-20210910154937864](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910154937864.png)

   ![image-20210910154948635](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910154948635.png)

   使用案例:

   ```javascript
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <title>测试</title>
   </head>
   <body>
   <h1 id="time"></h1>
   <script type="text/javascript">
   
     var time = function time() {
       const date = new Date();
       let year = date.getFullYear();//获取年
       let month = date.getMonth() + 1; //获取月份
       let day = date.getDate(); //获取一月中天数
       let hours = date.getHours();//获取一天中小时数
       let minutes = date.getMinutes(); //获取分钟数
       let seconds = date.getSeconds(); //获取秒数
       let timeDate = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds;
       document.getElementById("time").innerText = timeDate;//这行意思是:获取获取文档DOM对象获取节点time然后插入文本
     }
     setInterval("time()", 100)
   
   </script>
   </body>
   </html>
   ```

   以上就是一个实时显示时间的js,在页面呈现的效果如下:

   ![image-20210910161047018](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910161047018.png)

   > 注意:其中的月份由于是从0开始的所以要+1!

2. ## String 字符串对象

   API:

   |                            方法名                            |                             说明                             |
   | :----------------------------------------------------------: | :----------------------------------------------------------: |
   |                           split()                            |             方法用于把一个字符串分割成字符串数组             |
   |                        toLowerCase()                         |                  方法用于把字符串转换为小写                  |
   |                        toUpperCase()                         |                  方法用于把字符串转换为大写                  |
   |                         substring()                          | 方法用于提取字符串中介于两个指定下标之间的字符，不包括结束位置的字符 |
   |                      charCodeAt(index)                       |                   返回指定位置的unicode码                    |
   | fromCharCode() -- 相当于静态方法，用法：String.fromCharCode(65); |        可接受一个指定的 Unicode 值，然后返回一个字符         |
   |                            concat                            |                          拼接字符串                          |

   这里可以发现跟Java的String用法一模一样,所以就不贴代码了!
   
3. ## 数组

   ```javascript
   <script type="text/javascript">
     let array1 = new Array(); //创建一个空的数组
     let array2 = new Array("1", 2); // 创建数组的时候初始化参数的数组
     let array3 = new Array(8);  // 创建初始容量的数组
     //数组可以在创建的时候指定初始参数和容量,但是不受容量影响,依然正常运行
     let array = [];
     array[6] = "这是第六";
     console.log(array);
   
     //遍历第一种方式: 只会遍历有值
     for (let key in array) {
       console.log(array[key]);  // key为索引
     }
   
     //遍历第二种方式: 遍历每一个数组元素,没有值也会打印为undefined
     for (let i = 0; i < array.length; i++) {
       console.log(array[i]);
     }
   </script>
   ```

#### 7.1.2 内置对象

内置对象也是 ECMA-262 提供的,不需要new来创建实例就能直接使用的,包括`Global`、`Math`

1. ### Global 全局对象

   API:

   ![image-20210910162030270](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910162030270.png)

   代码示例:

   ```javascript
   <script type="text/javascript">
     var url = "https://www.baidu.com";
     console.log(encodeURI(url)); //将字符串编为URL路径
     console.log(decodeURI("%E7%99%BE%E5%BA%A6")); //进行解码操作
     console.log(encodeURIComponent(url)); //将其编码为URL组件
     console.log(decodeURIComponent("www.baidu.com%3Farg%3D%E7%99%BE%E5%BA%A6")); //进行解码
   
     var a = "console.log('绕了我吧')";
     eval(a); //将其字符串作为JS脚本进行执行
   
     var b = '123';
     console.log(parseInt(b));
     b = "123aa";
     console.log(parseInt(b));
     b = "aa123";
     console.log(parseInt(b));
      //表示会从左往右读取,一旦读取非数字相关那么就会立即放弃继续读下去
   </script>
   ```

   结果:

   ![image-20210910163713514](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910163713514.png)

2. ### Math 数学对象

   API:

   ![image-20210910163932873](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910163932873.png)

### 7.2 宿主对象

宿主对象分为`BOM(浏览器对象模型)`和`DOM(文档对象模型)`

#### 7.2.1 BOM

BOM简称为浏览器对象模型:

![image-20210910165743944](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910165743944.png)

其中:

1. 打开一个窗口就是一个`window`对象
2. 窗口里面使用`location`表示地址栏;
3. 窗口的历史记录使用`history`来表示;
4. 浏览器的信息使用`navigator`来表示;
5. 窗口里面的内容使用`document`来表示;

> 可以理解为BOM浏览器对象模型就是操作浏览器相关的对象!

1. ## window

   是BOM中顶级对象,在调用其函数的时候可以默认省略`window.`

   > 使用window对象依赖于浏览器而言的

   API:

   ![image-20210910170716285](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910170716285.png)

   

   来个示例:

   ```javascript
   <script type="text/javascript">
     let prompt1 = prompt("请输入关键词");
     console.log(prompt1);
   </script>
   ```

   页面:

   ![image-20210910175821824](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910175821824.png)

   console:

   ![image-20210910175853280](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910175853280.png)

2. ## location

   **当前访问的网址的相关信息**

   这是API:

   ![image-20210910175933792](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910175933792.png)

   代码示例:

   ```javascript
   <script type="text/javascript">
     let href = window.location.href;
     console.log(href);
     let protocol = window.location.protocol;
     console.log(protocol);
   </script>
   ```

   结果:

   ![image-20210910180443126](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910180443126.png)

   > 这里window.location.href是获取当前浏览器地址栏,可以给地址栏设值,让其改变

#### 7.2.2 DOM

**概念**:DOM【Document Object Model】 ：文档对象模型。直白的讲就是通过程序解析结构化文档（xml，html）的时候，在内存中生成的包含当前结构化文档中所有内容的一个对象模型。文档中的每一个节点都会生成一个对象，这些对象与对象之间会形成一个层级关系，像一个树形结构，所以称之为DOM树!

![image-20210910182046935](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910182046935.png)



DOM模型就是结构化文档在内存中的另外一种表现形式:

![image-20210910182158345](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910182158345.png)

再解释一下DOM树:

![image-20210910182326903](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210910182326903.png)

> 我们在使用DOM中的添加节点之类的,都是在在内存中进行操作,下一次重新加载不会修改硬盘上的数据

**总结**:

1. 每个载入浏览器的HTML文档都会成为一个Document对象，文档中都是节点;
2. 文档中的标签就是元素节点（Element）,标签的内容就是文本节点（Text）,标签的属性就是属性节点（Attribute）
3. 由于document对象属于window对象，所有BOM包括DOM。获取document对象直接使用window.document获取，而且window.是可以省略的

> Dom的核心就是操作这三种节点，以及元素节点之间层次关系。我们可以使用JavaScript对HTML页面中的所有节点进行访问

**理解**:其实就是说白了整个HTML就是一个DOM树,而我们可以通过`document`对象获取每一个标签节点进行相关的操作

**理解**:浏览器在加载HTML的时候,会加载到内存中然后生成一个`document`对象,而HTML中每个节点都生成一个对应的对象进行一定的层级关系连接起来,然后我们就可以方便的相关操作

##### 1. 事件

**概念**:事件是可以被JavaScript侦测到的行为，例如单击某一个按钮，我们可以使用js提供的API监听到单击行为，并可以对该行为作出回应!

**理解**:说白了就是针对DOM文档中某一节点进行于JS绑定,比如用户点击按钮就可以单击,然后此时JS进行监听然后触发相关操作!

API:

|         方法          |                             说明                             |
| :-------------------: | :----------------------------------------------------------: |
|        onabort        |                       用户终止页面加载                       |
|        onblur         |                    用户离开对象。失去焦点                    |
|       onchange        |                       用户改变对象的值                       |
|        onclick        |                         用户点击对象                         |
|      ondblclick       |                         用户双击对象                         |
| onfocus(和onblur相反) |                         用户获得焦点                         |
|       onkeydown       |                           按下键盘                           |
|      onkeypress       |                           按压键盘                           |
|        onkeyup        |                           松开键盘                           |
|        onload         | 页面完成加载。注释：在 Netscape 中，当页面加载时会发生该事件 |
|      onmousedown      |                        用户按鼠标按钮                        |
|      onmouseover      |                     鼠标指针移动到对象上                     |
|      onmouseout       |                       鼠标指针移出对象                       |
|       onmouseup       |                       用户释放鼠标按钮                       |
|        onreset        |                         用户重置表单                         |
|       onselect        |                     用户选取页面上的内容                     |
|       onsubmit        |                         用户提交表单                         |
|       onunload        |                         用户关闭页面                         |

我们需要在HTML中与JS进行绑定事件进行触发:

- 元素上直接绑定吧

- 元素.事件名

1. ## 元素绑定

   这种就是在HTML上的节点与JS中的其中一个函数进行绑定,当触发了HTML中的绑定节点,那么JS中函数也会随之触发;

   ```javascript
   <input type="button" value="计算" onclick="danji()"/>
   <span id="echo"></span>
   <script>
     function danji() {
       alert("我好想出来啊")
       document.getElementById("echo").innerText = "哎我就出来了!";
     }
   </script>
   ```

   > 缺点:这种就是JS与HTML代码写在一起了;优点:如果多个函数被多个节点所共享那么可以考虑使用这种方式

2. ## 通过DOM.事件去绑定

   这种就是在JS里通过获取DOM中的节点与函数进行绑定操作:

   ```javascript
   <input type="button" value="计算" id="ss"/>
   <span id="echo"></span>
   <script>
     let btn2 = document.getElementById("ss");
     btn2.onmouseover=function danji() {
       alert("我好想出来啊")
       document.getElementById("echo").innerText = "哎我就出来了!";
     }
   </script>
   ```

   **注意**:使用这种方式的时候再跟函数进行绑定的时候再触发事件前加"`on`",比如要触发点击事件,那么需要在前加on然后才是click

   > 在使用事件的时候,名称不要用一些关键字之类的,这样不会触发事件

##### 2. 操作

我们都知道整个HTML在浏览器加载的时候会生成文档树,那么我们如何去获取其中某个节点呢?

1. ## 获取节点

   获取节点分为这几个:

   - getElementById("id值")->通过标签元素的ID获取到标签对象
   - getElementsByName()->通过标签元素的name属性获取到标签对象，返回的是数组
   - getElementsByTagName()->通过标签名称获取到标签对象，返回的是数组
   - parentNode:->父节点，返回Node,获取指定元素的父节点
   - document.body:获取body节点  <body>

2. ## 文本操作

   API:

   - innerHTML
     - 设置或者获取双标签中的的元素
   - innerText
     - 设置或者获取双标签中的纯文本

   > 第一个一般用于向指定节点插入样式相关的,第二个就是纯文本

3. ## 属性和样式操作

   API:

   ```javascript
   1.setAttribute("type","button") 等价于  node.type="button"
   	设置属性
   2.getAttribute("type") 等价于 node.type
   	获取属性值
   3.removeAttribute()
   	删除某属性；
   4.node.style.样式 -- 单个样式设置
   	设置样式
   5.node.className -- 统一设置样式，多个样式方式，需要结合css代码
   	设置样式名
       例如：o.className = "样式类名称";
   	.样式类名称{
   	}
   常见样式：
         node.style.color 可以设置节点的字体颜色
   node.style.fontSize 可以设置节点的字体大小
   node.style.backgroundColor 用于设置节点的背景颜色
   node.style.width ='300px'用于设置节点的宽度
   node.style.height ='200px'用于设置节点的高度
   node.style.background="url('image/red.png')";
   ```

   node:意思是节点

4. ## 元素操作

   ```javascript
   创建和添加:
   1.document.createElement(); //创建新的元素
   2.insertBefore(newnode, refnode); //在childNodes中的refnode之前插入newnode，返回Node
   3.appendChild();//父节点追加子节点
   删除:
   1. removeChild();//父节点删除子节点
   ```

### 7.3 自定义对象

​	定义函数和定义类本质上都是要一样的,只不过两者都是使用`function`来表示,那如何区分呢?

> 定义类的话用"首字母大写当成类",否则就当成函数

# AJAX



## 1. 概念

AJAX即Asynchronous Javascript And XML（异步JavaScript和XML），是**改善用户体验的网页开发技术**；

> 也就是说AJAX是用于发送HTTP请求的!是用来跟后端数据进行交互的!

## 2.交互方式

我们之前的传统的交互方式是用于一个超链接或者form表单的形式进行提交数据到后端去,这种方式造成每次请求数据后后端就会重新返回整个页面,也就是每次请求都会重新加载整个页面,这种方式就会造成浪费网络资源传输浪费;

传统交互图:

![image-20210911190956206](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210911190956206.png)

**缺点**:

1. 发送的请求是一个同步请求，销毁页面就不能继续操作;
2. 修改的数据仅仅是页面中一小部分，然后响应的却是整个完整页面，浪费网络传输资源

而现在新的技术是`AJAX`,也是属于发起请求到后端,但是这种的好处就在于只返回数据文本对局部数据进行重新修改,而不会对整个页面进行销毁加载:

AJAX交互图:

![image-20210911191228531](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210911191228531.png)

**优点**:

1. 异步请求，发送请求的同时还可以继续操作页面。页面不销毁;
2. 返回部分数据，减少不必要的数据传输，减少网络资源。页面不刷新，而是更新页面部分数据

> 传统方式是同步请求方式,AJAX是异步请求方式;AJAX能更好的提升用户使用体验避免不必要的数据传输!

**同步与异步的区别:**

1. 同步:你先做完我再做，后一步的操作必须要等待前一步操作的结果
2. 各做各的相互不干扰（效率高)

## 3.运行

我们使用AJAX的步骤就是先获取AJAX对象,然后创建请求,然后设置回调函数,然后发送请求!

发送请求实现步骤:

1. 获取ajax对象(已经获取)

2. 准备发送请求:xhr.open("get","/xx/add?name="+参数值)【**get请求**】

   准备发送请求：xhr.open("post","/xx/add")【**post请求**】

3. 设置回调函数(主要是获取服务器返回的正确数据):

   xhr.onreadystatechange=function(){

   ​	...

   }

4. 发送请求:xhr.send()【**get请求,无参**】

   发送请求:xhr.send("参数名="+参数值&"参数名="+参数值);【**post请求**】



来个实例:

```javascript
<script>
  //获取AJAX对象
  let httpRequest = new XMLHttpRequest();
  //创建发送请求信息
  httpRequest.open("post", "http://localhost:8080/demo/show1");
  //设置回调函数,进行监听,注意这个回调函数必须在创建发送请求和发送请求之间,避免没监听不到
  httpRequest.onreadystatechange = function () {
      //判断当前请求状态是否是处于请求发送成功了
    if (httpRequest.readyState === 4 && httpRequest.status === 200) {
       //解析返回文本并解析为JSON类型,然后通过对象的方式去获取其中的数据 
      let responseText = httpRequest.responseText;
      console.log(JSON.parse(responseText).data[0].ename);
    }
  }
  //发送请求
  httpRequest.send();
</script>
```

**注意**:由于我是用webStrom写请求而后端是用idea导致两个服务器端口不一样,所以这里请求就会导致出现一个问题"**跨域**",所以为了解决这个问题,我是在后端在响应头里设置了跨域,这样就能避免了这类问题,但是也只是针对单个请求,所以这个问题可以用过滤器进行针对每个请求都进行"跨域"过滤!

```java
// 指定允许其他域名访问
resp.addHeader("Access-Control-Allow-Origin","*");  //可以设置过滤器进行设置
//以下是可选设置
// 响应类型
resp.addHeader("Access-Control-Allow-Methods","POST");
// 响应头设置
resp.addHeader("Access-Control-Allow-Headers","x-requested-with,content-type");
//响应内容支持中文
resp.setContentType("text/html;charset=utf-8");
httpServletResponse.setHeader("Access-Control-Max-Age", "3600");
```

> 要自定义过滤器的话就单独的类然后实现"Filter"接口并覆写方法就可以了!

## 4.API

关于AJAX的相关API的使用方法这里可以通过一个网站进行访问:

```http
https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/open
```

然后这里列举一张常用API使用图:

![image-20210912172052469](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210912172052469.png)
