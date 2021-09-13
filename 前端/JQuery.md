# JQuery笔记

## 1.概念:

**概念**:JQuery是继prototype之后的一个优秀的开源的JavaScript代码库(也可以成JavaScript框架),它封装了JavaScript常用的功能代码;

> 理解:JQuery就是一个JavaScript框架,里面封装了很多JS供我们使用

**特点:**

1. 具有独特的链式语法,可以把多个操作写在一行代码里,更加简洁
2. 拥有大量的选择器,不仅有高效灵活的CSS选择器,并且还可以对CSS选择器进行扩展
3. 拥有便捷的插件扩展机制和丰富的插件(比如:树形菜单,日期控件,图片切换插件,弹出窗口等);
4. JQuery兼容各种主流浏览器(包括IE);

**核心思想**:写得更少,做得更多;

## 2.简单使用

由于这是第三方框架不是其本身所自带的,所以需要引入JS包:

![image-20210913102417562](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913102417562.png)

而我们使用的话就直接引入外部JS就行:

```javascript
<script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script> //注意标签内部不能写内容不然就会被覆盖
```

之前我们获取文档对象中某个节点是用:

```javascript
document.getElementById("myDiv");
```

而使用JQuery之后就可以用$("选择器")来表示:

```javascript
$("#myDiv").html("这是JQuery修改后的"); //这里是选择为ID是myDIV,所以是#myDIV,如果是class选择器的话就是.myDiv
```

综合小案例:

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>搞一把</title>
</head>
<body>
<div class="myDiv">yyy</div>
<div id="myDiv">yyy</div>
<div id="my">试试</div>
</body>
<script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script>
<script type="text/javascript">
  alert("点开就知道了");
  $(".myDiv").html("这是Class选择器修改的");
  $("#myDiv").html("这是ID选择器修改的");
  document.getElementById("my").innerText = "这是原生JS获取并修改";
</script>
</html>
```

结果:

![image-20210913103604444](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913103604444.png)

**注意**:在JQuery中都是用$来表示使用,那么这两者是等价也就是说我们可以用`jQuery("#myDiv")`来进行使用,但是也要注意大小写问题!

**注意**:一般情况下为了避免dom对象与jquery对象混淆，可以将jquery对象的变量名用加上$前缀,并且不能随意调用不同的属性方法,也就是说jqeury对象不能调用dom对象的属性和方法

这里介绍一下DOM对象与JQuery对象互换:

```javascript
<script type="text/javascript" src="jquery/jquery-2.1.3.min.js"></script>
<script type="text/javascript">
		$(function(){
			//根据id获取jquery对象
			var mydiv = $("#mydiv");
			/*把jQuery对象转为dom节点：
				1.var dom = jQuery对象.get(0);
				2.var dom = jQuery对象[0]  推荐使用(简写)
			*/		
			var dom = mydiv[0];
			console.debug("dom对象:"+dom)
			/*	
			     把dom节点转为jQuery对象:
				1.var jqy = $(dom节点)
			*/
			var jqy = $(dom);
			console.debug("jQuery对象:"+jqy)			
		});
</script>
</head>
<body>
	<div id="mydiv">这是我的div</div>
</body>
```

> $(".myDiv")也能跟原生JS一样获取节点是因为JQuery函数内部对其原生的DOM对象进行了封装然后产生的对象,可理解为其底层就是操作DOM对象!

### 2.1 加载页面:

加载页面的方式有两种方式:

```javascript
1.$(document).ready(function(){     //获取DOM文档对象然后执行函数内容
    console.log("加载页面1");
});
2.window.onload=function(){    //从DOM中的顶级对象window在浏览器加载才执行函数内容
    console.log("加载页面2");
}
```

**共同点**:都是用于在浏览器加载页面的时候执行!

**区别**:

1. `window.onload = function(){}`:
   - 整个页面加载完毕之后再执行,也就是页面内容渲染完**HTML+CSS+图片**完毕之后,才会执行匿名函数中的内容;
2. `ready()`:
   - 整个页面的元素读取完毕之后就执行{}里面的代码,也可理解为是在页面结构**HTML**加载完毕之后就执行匿名函数中的内容;
3. 性能:由于ready()是在加载完HTML结构就开始执行函数内容,而另一个则要把所有资源都要读取完毕才会执行该函数内容,所以**ready()的性能更优**;

**注意**:第一种方式`$(document).ready(function(){...}`可以简写为`$(function(){...});`!



## 3.选择器

**概念**:JQuery在原本的选择器上面进行封装和增加让其JQuery的选择器更加的强大和易操作!

### 3.1 基本选择器

这里列举几个基本的选择器:

|            选择器             |    说明    |                 使用                  |
| :---------------------------: | :--------: | :-----------------------------------: |
|              #id              |  id选择器  |      根据id获取jQuery对象(单个)       |
|            element            | 元素选择器 |    根据标签名获取jQuery对象(多个)     |
|            .class             | 类型选择器 |    根据类型名获取jQuery对象(多个)     |
| selector1,selector2,selectorN | 组合选择器 | 根据多个选择器组合在一起,同时选择多个 |

案例:

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>基本选择器</title>
  <script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script>
</head>
<body>
<div id="myDiv" class="one">div1</div>
<div class="one">div2</div>
</body>
<script type="text/javascript">
  //id选择器
  let innerText = $("#myDiv").text();
  console.log(innerText);
  //class选择器
  let text = $(".one").text();
  console.log(text);
  //标签选择器
  let $div = $("div").text();
  console.log($div)
  //组合选择器
  let text1 = $("#myDiv,.one").text();
  console.log(text1);
</script>
</html>
```

结果:

![image-20210913134637875](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913134637875.png)

### 3.2 层次选择器

1. 祖先+空格+后代:**在给定的祖先元素下匹配所有的后代元素**

   ![image-20210913135717462](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913135717462.png)

2. 父亲+空格+儿子:**在给定的父元素下匹配所有的子元素**

   ![image-20210913135804101](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913135804101.png)

3. 元素+加号+紧接下一个元素:**匹配所有紧接在 prev 元素后的 next 元素**

   ![image-20210913140019373](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913140019373.png)

4. ~:**匹配前一个元素同一辈分的元素**

   ![image-20210913140150086](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913140150086.png)

$("input:button"):表示获取input框中带有button元素的节点

## 4.for循环

场景：例如有多个input按钮，获取每个按钮的value值

第一种：传统的遍历方式：

```javascript
var $objs = $("input:button");
for(var i = 0 ; i < $obj.length ; i++){
	console.debug( $obj[i]) );//注意$obj[i]是一个dom对象
}
```

第二种：jQuery提供的each函数

```javascript
//$obj：jquery集合对象，index遍历元素的下标（从0开始），obj表示每一个节点
$obj.each(function(index , obj){
	alert($(obj).val());
});
```

## 5.绑定事件

在使用原生的JS进行绑定事件的时候,通常有这几种方式:

1. ```javascript
   <a href="javascript:void(0);" onclick="functionName()";/>
   ```

   - 优点:简单,支持所有浏览器
   - 缺点:结构与事件没有分开,耦合度太高,只能添加一个事件,添加之后,事件不能取消;

2. ```javascript
   Document.getElementById("id").onclick=function(){...}
   ```

   - 优点:简单,支持所有浏览器
   - 缺点:只能添加一个事件,添加之后,事件可以取消

3. ```javascript
   元素.addEventListener()->所有浏览器
   元素.attachEvent(IE浏览器)
   ```

   - 优点:功能很强大
   - 缺点:麻烦,需要自己做兼容

现在介绍JQuery注册事件的四种方式:

1. 第一种:

   ```javascript
   语法格式:
   jQuery对象.事件名称(function(){...})
   比如:
   $("#myDiv").click(function(){
       console.log("事件1")
   })
   ```

2. 第二种:

   ```javascript
   语法格式:
   jQuery对象.bind("事件名",function(){...})
   比如:
   $("#myDiv").bind("click",function(){
       console.log("事件2")
   })
   ```

3. 第三种:**兼容性好,功能强大,支持事件委托机制**

   ```javascript
   语法格式:
   jQuery对象.on("事件名",[选择器],function(){...})
   比如:
   $("#myDiv").on("click",function(){
       console.log("事件3")
   })
   ```

4. 第四种:**使用一次之后自动注销使用**

   ```javascript
   语法格式:
   jQuery对象.one("事件名",function(){...})
   比如:
   $("#myDiv").one("click",function(){
       console.log("事件4")
   })
   ```

然后是注销方法(两种):

1. 第一种:

   ```javascript
   语法格式:
   jQuery对象.off("事件名称")
   比如:
   $("#btn1").off(); //没指定就是注销所有
   ```

2. 第二种:

   ```javascript
   语法格式:
   jQuery对象.unbind("事件名称")
   ```

### 5.1 事件委托机制

```html
<input type="button" value="动态添加按钮" id="btn">
<div id="filediv">
	<input type="button" code="button" value="按钮"><br/>
</div>
```

JS:

```javascript
 $(function() {
    $("#btn").on("click", function () {
      //追加子节点
      $("#filediv").append('<input type="button" code="button" value="按钮"><br/>');
    })
    //点击事件,选择所有input框并且带有code元素的
     $("input[code]").on("click",function(){
       alert("注册成功!");
     })
});
```

说明:这种有个缺点:这种注册方式，只会在加载结构的时候，满足条件的才会注册事件，动态追加的，就没法注册成功,也就是说只会对现有的结构有效,那么动态追加的都不生效!

而**事件委托机制**就可以解决这种问题:就是当对一个选择器进行触发的时候将绑定事件委托给其他选择器!

修改:

```javascript
$(function() {
    $("#btn").on("click", function () {
      //追加子节点
      $("#filediv").append('<input type="button" code="button" value="按钮"><br/>');
    })
    //当点击id="fileDiv"这个节点的时候将点击事件委托给了input[code]这个选择器,那么也就是说当我点击filediv下的input框带有code元素的节点就会触发点击事件
    $("#filediv").on("click","input[code]",function(){
      alert("注册成功!!");
    })
});
```

## 6.方法

### 6.1 操作文本值

这里获取html和文本和value值分别是:`html()`,`text()`,`val()`

综合案例:

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>基本选择器</title>
  <script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script>
</head>
<body>
    <div id="myDiv">div</div>
    <input value="1212" id="testDiv">
</body>
<script type="text/javascript">
  let html = $("#myDiv").html("<h2>真的</h2>");  //设置html,可以带标签,不带的话就是获取文本
  console.log(html);

  let text = $("#myDiv").text(); //获取纯文本
  console.log(text);

  let val = $("#testDiv").val(); //获取value值
  console.log(val);
</script>
</html>
```

console:

![image-20210913150647931](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913150647931.png)

页面:

![image-20210913150656774](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913150656774.png)

### 6.2 操作元素和节点

添加:append()，appendTo()，prepend()，after()，before()

删除：remove()，empty()

复制：clone(true)

```javascript
<body>
    <div id="myDiv">div</div>
    <input value="1212" id="testDiv">
</body>
<script type="text/javascript">
  $("#myDiv").append("<h2>这是append</h2>"); //这是添加一个带标签的节点添加到指定选择器的子节点中
  $("#myDiv").appendTo($("#testDiv")) ;//将节点添加到指定节点的子节点中
  $("#myDiv").prepend("<h2>这是prepend</h2>");  //将指定内容添加到指定标签的上一个节点
</script>
```

其他的就不用写了,大概也知道什么意思!

这里注意拷贝:

```javascript
$("input[code]:first").clone(true).appendTo($("#filediv")); //浅克隆仅仅克隆标签 Jquery对象.clone(true)  浅克隆标签及事件
```

### 6.3 操作属性和css

#### 6.3.1 操作属性

操作属性的方法:attr()，removeAttr(),data();

其中`attr()`跟`data()`是获取和设置属性,而`removeAttr()`则表示删除属性

那这两者都是获取值的**区别**:

1. attr():获取的都是字符串格式的,并且如果在修改这个属性的时候如果没有这个属性那么默认自动添加属性
2. data():获取的值会转为对应的类型,并且使用data获取值有要求必须属性名的格式为`data-xxx`这种格式才能获取

使用案例:

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>基本选择器</title>
  <script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script>
</head>
<body>
    <div id="myDiv" data-role="22">div</div>
    <input value="1212" id="testDiv">
</body>
<script type="text/javascript">
  const $myDiv = $("#myDiv");
  //获取id的属性值
  let $id = $myDiv.attr("id");
  console.log($id);
  //获取id1的属性值
  let $id1 = $myDiv.attr("id1");
  console.log($id1);
  //修改id1的属性值
  $myDiv.attr("id1","原本是修改现在是添加了");
  let $id2 = $myDiv.attr("id1");
  console.log($id2);
  //获取data值
  let attr = $myDiv.data("id1");
  console.log(attr);
  //获取data值
  let role = $myDiv.data("role");
  console.log(role);
</script>
</html>
```

console:

![image-20210913153723543](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913153723543.png)

#### 6.3.2 操作CSS

操作CSS的方法:css(),addClass(),removeClass()

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>基本选择器</title>
  <script type="text/javascript" src="jQueryLibrary/jquery-2.1.3.js"></script>
</head>
<body>
    <div id="myDiv" data-role="22">div</div>
    <input value="1212" id="testDiv">
</body>
<script type="text/javascript">
  const $myDiv = $("#myDiv");
  $myDiv.css("background-color", "green"); //追加CSS样式
  $myDiv.addClass("className");  //追加Class选择器
</script>
</html>
```

页面:

![image-20210913154208812](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913154208812.png)

### 6.4 循环

```html
<ul>
  <li>李寻欢</li>
  <li>方世玉</li>
  <li>雷老虎</li>
  <li>大朗吃药了</li>
</ul>
```

使用JQuery对象的遍历方式:

```javascript
<script type="text/javascript">
  $("ul>li").each(function(index,obj){
    console.log(obj.innerText)
  })
</script>
//还有另一种写法:
$.each(result, function(i, field){
      $("div").append(field + " ");
});
```

console:

![image-20210913155905982](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913155905982.png)

其中:index遍历元素的下标（从0开始），obj表示每一个节点

## 7.AJAX

$.get("url",data,callback,type);

$.post("url",data,callback,type);

url：表示请求

data：请求参数，格式{key1:value1,key2:value2}

callback：回调函数function(data){}，其他data表示服务器返回的数据

type：指定服务器返回数据的类型，如果是字符串，基本值用text，如果是对象，或者集合用json

实例:

```javascript
<script type="text/javascript">
    $.get("http://localhost:8080/demo/show1",function (data) {
      let obj = data.data;
      $.each(obj, function (i, data) {
          console.log(data);
      });
    })
</script>
```

结果:

![image-20210913163004305](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913163004305.png)

在来看一下获取数据的断点:

![image-20210913163110075](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913163110075.png)

说明:可以看出在接收参数的时候返回是一个对象,然后通过对象.的方式获取到data数组,而数组中每个元素都是对象,然后通过循环的方式展示出来!

这里展示一下在JS端发送数据到服务器:

```javascript
var canshu={username:"缪威", password: 123}
    $.get("http://localhost:8080/demo/show1",canshu,function (data) {
      let obj = data.data;
      $.each(obj, function (i, data) {
          console.log(data);
      });
    })
```

我在后端接收并返回给JS,所以这里接收是接收成功了,这里发送数据就必须按照这种格式,无论get还是post

![image-20210913164830434](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20210913164830434.png)

这里还可以进行简写:

```javascript
var canshu={username:"缪威", password: 123}
$.ajax({
    type:"get", //这里不写是默认是GET请求,如果是POST则需要显示指定一下请求类型
    url: "http://localhost:8080/demo/show1",
    data:canshu,
    success: function(data){  //这里data是回传的内容
      let obj = data.data;
      $.each(obj, function (i, data) {
        console.log(data);
      });
    },
    dataType:"json"
  })
```

> 这样更显得代码更加优雅

## 8.创建对象的三部曲

1. 创建对象

   - 使用new XXx()创建对象
   - 使用JSON创建对象

2. 设置属性

   - 普通设置   对象.属性名=值
   - 动态设置值  对象["属性名"]=值

3. 删除属性

   - delete 对象.属性名  或者 delete 对象["属性名"]

   